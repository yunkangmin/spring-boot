Auto Scaling으로 EC2 인스턴스가 많아지면 일일이 소스를 업데이트하기가 어려워  
진다. 이번에는 Auto Scaling 그룹의 EC2 인스턴스에 소스를 배포하는 방법을  
알아보자. 다음 예제 코드는 필자의 GitHub 저장소에 있는 예제 코드를 받아서    
사용한다.   
```
var AWS = require('aws-sdk')
  , fs = require('fs')
  , sshclient = require('sshclient')
  , async = require('async')
AWS.config.loadFromPath('./config.json');

var ec2 = new AWS.EC2({ region: 'ap-northeast-2' })
  , autoscaling = new AWS.AutoScaling({ region: 'ap-northeast-2' });

//SSH로 EC2 인스턴스에 접속할 수 있도록 pem 파일의 경로를 설정한다. 이 파일  
//은 키 쌍(Key Pair)을 생성할 때 다운로드했다.  
var privateKey = fs.readFileSync('../../awskeypair.pem');

//Auto Scaling 그룹에 속한 EC2 인스턴스의 ID를 얻어오는 함수이다.   
//AutoScalingGroupNames에 Auto Scaling 그룹의 이름을 설정한다.     
function getInstanceIds(callback) {
  autoscaling.describeAutoScalingGroups({
    AutoScalingGroupNames: ['ExampleAutoScalingGroup'],
  }, function (err, data) {
    instanceIds = [];
    if (!err)
      data.AutoScalingGroups[0].Instances.forEach(function (e) {
        instanceIds.push(e.InstanceId);
      });
    }
    callback(instanceIds);
  });
}

//SSH로 접속하려면 IP 주소가 필요하다. EC2 인스턴스 ID로 IP 주소를 얻어오는  
//함수이다.  
function getInstanceIpAddress(instanceId, callback) {
  ec2.describeInstances({
    InstanceIds: [ instanceId ]
  }, function (err, data) {
    if (err)
      console.log(err, err.stack);
    callback(data.Reservations[0].Instances[0].PublicIpAddress);
  });
}

//소스를 배포하는 함수이다.  
//SSH로 EC2에 접속할 수 있도록 username을 설정한다.  
//Amazon Linux: ec2-user
//Ubuntu Linux: ubuntu 
//SSH로 EC2 인스턴스에 접속하여 aws s3 sync 명령을 실행한다. S3 버킷에서  
//소스를 가져오는 방식이다. S3 버킷 주소와 소스 경로는 자신의 상황에 맞게   
//설정한다.  
function deploy(host) {
  sshclient.session({
    host: host,
    port: 22,
    username: 'ec2-user',
    privateKey: privateKey
  }, function (err, ses) {
    async.series([
      function (callback) {
        ses.exec('aws s3 sync --region=ap-northeast-2 s3://example.src/ExampleWebServer /home/ec-user/ExampleWebServer', function (err, stream) {
          callback(err, stream);
        });
      }
    ], function (err, results) {
      console.log(results);
      ses.quit();
    });
  });
}
getInstanceIds(function (instanceIds) {
  instanceIds.forEach(function (e) {
    getInstanceIpAddress(e, function (ipAddress) {
      console.log(ipAddress);
      deploy(ipAddress);
    });
  });
});
```
다음과 같은 명령으로 스크립트를 실행하면 된다. Linux, Windows, Mac OS X에서  
사용할 수 있다.  
```
node deploy.js
```

S3 버킷에서 소스를 가져오지 않고 Git이나 Subversion을 사용하려면 다음과 같이   
작성한다. git pull과 svn update 명령을 사용하기 때문에 처음에 저장소를 클론  
/체크아웃해 놓아야 한다.   
Git  
```
function deploy(host) {
  sshclent.session({
    host:host,
    port: 22,
    username: 'ec2-user',
    privateKey: privateKey
  }, function (err, ses) {
    async.series([
      function (callback) {
        ses.exec('cd /home/ec2-user/ExampleWebServer; git pull', function (err, stream) {
          callback(err, stream);
        });
      }
    ], function (err, results) {
      console.log(results);
      ses.quit();
    });
  });
}
```
  
Subversion  
```
function deploy(host) [
  sshclient.session({
    host: host,
    port: 22,
    username: 'ec2-user',
    privateKey: privateKey
  }, function (err, ses) {
    async.series([
      function (callback) [
        ses.exec('cd /home/ec2-user/ExampleWebServer; svn update', function (err, stream) {
          callback(err, stream);
        });
      }
    ], function (err, results) {
      console.log(results);
      ses.quit();
    });
  });
}
```
필자는 Node.js로 작성했지만 AWS API를 사용해 Java, Python, Ruby 등 여러 가지  
언어로 작성할 수 있다. 그리고 SSH로 접속하지 않고 Chef 또는 Docker로 구현해도  
된다.  

























