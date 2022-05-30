필요한 AWS 리소스를 생성하고 설정했다. 이제 Node.js로 게임 서버를 작성해보자.   
다음 예제 코드는 필자의 GitHub 저장소에 있는 예제 코드를 받아서 사용한다.   
app.js   
```
var express = require('express')
  //body-parser 모듈은 HTTP POST, PUT 메서드로 오는 데이터를 사용하기 편리  
  //하게 req.body에 넣어준다.  
  , bodyParser = require('body-parser')
  //express-validator 모듈은 HTTP POST, PUT 메서드로 받을 필수적인 데이터를  
  //지정할 수 있다. 지정한  데이터가 빠졌을 때 에러를 발생시킨다.  
  //app.use 함수로 body-parser, express-validator 모듈을 활성화한다.  
  , expressValidator = require('express-validator')
  , AWS = require('aws-sdk')
  , redis = require('redis')
  , Sequelize = require('sequelize')
  , moment = require('moment')
  , http = require('http')
  , fs = require('fs')
  , app = express()
  , server = http.createServer(app)
  , dynamodb = new AWS.DynamoDB({ region: 'ap-northeast-2' });
  
//ElastiCache 캐시 노드 엔드포인트 주소(Redis), RDS DB 인스턴스 엔드포인트  
//주소(MySQL)와 연결 설정, DynamoDB 테이블 이름은 여러분들이 생성한 AWS  
//리소스의 정보를 입력한다.  
var redisEndpoint = {
  host: 'redis url',
  port: 6379
};

var rdsEndpoint = {
  host: 'rds url',
  port: 3306
};

var dynamoDbTable = 'ExampleGameLog';

var redisClient = redis.createClient(redisEndpoint.port, redisEndpoint.host);

//MySQL DB 이름, 계정, 암호  
var sequelize = new Sequelize('examplegame', 'admin', '18263628', {
  host: rdsEndpoint.host,
  port: rdsEndpoint.port
};

//유저 테이블 정의
//비밀번호 부분은 향후 로그인을 구현할 때 사용하면 된다. 요즘은 직접 로그  
//인보다는 카카오톡, 라인, 페이스북을 연동을 많이 한다. 카카오톡, 라인,  
//페이스북 연동에 필요한 데이터도 유저 테이블에 저장한다.  
var User = sequelize.define('User', {
  userId: { type: Sequelize.STRING, allowNull: false, unique: true },
  //password.Sequelize.STRING,
  topScore: Sequelize.INTEGER,
  coin: Sequelize.INTEGER,
  itemSlot1: Sequelize.STRING,
  itemSlot2: Sequelize.STRING,
  itemSlot3: Sequelize.STRING
});

//예제 유저 데이터 생성
//테스트에 필요한 예제 유저 데이터를 생성한다.  
User.count().error(function (error) {
  if (error.code == 'ER_NO_SUCH_TABLE') {
    sequelize.sync().success(function () {
      User.create({
        userId: 'john',
        topScore: 0,
        coin: 1000,
      });
      User.create({
        userId: 'maria',
        topScore: 0,
        coin: 1000,
      });
    });
  }
});

//아이템 데이터
//아이템 데이터는 보통 JSON 파일로 따로 빼서 정의한다. 아래 코드는 예제  
//이므로 JavaScript에서 오브젝트 형태로 직접 정의했다.  
var itemTable = {
  '101': { name: 'Bomb', price: { coin: '100' }},
  '102': { name: 'Time Bonus', price: { coin: '150' }}
};

app.use(bodyParser.urlencoded())
app.use(bodyParser.json());
app.use(expressValidator());

//DynamoDB에 로그 저장
//고정 매개변수인 action과 비정형 데이터인 data를 받는다. 
//data는 DynamoDB 매개변수 형식에 맞게 변환한다.
//AWS API를 이용하여 DynamoDB 테이블에 로그를 저장한다.  
function writeLog(action, data) {
  var params = {
    Item: {
      action: { S: action },
      date: { S: moment().format('YYYY-MM-DD HH:mm:ss') }
    },
    TableName: dynamoDbTable
  };
  
  for (var key in data) {
    var attribute = data[key];
    if (isNaN(attribute))
      params.Item[key] = { S: attribute };
    else
      params.Item[key] = { N: String(attribute) };
  }
  
  dynamodb.putItem(params, function (err, data) { console.log(err) });
}

//ELB 로드 밸런서 헬스 체크용
//앞의 예제들은 웹 서버이기 때문에 모두 인덱스 페이지가 있었다. 
//게임 서버는 따로 인덱스 페이지가 없으므로 ELB 로드 밸런서에서 헬스   
//체크를 할 수 있도록 /, /index.html 경로에서 빈 내용을 출력한다.  
app.get(['/', '/index.html'], function (req, res) {
  res.send();
});

//유저 정보 얻기
//GET 메서드이며 userId를 받는다.
//Sequelize 모듈로 MySQL에서 유저 정보를 가져와서 출력한다.   
//클라이언트에는 꼭 필요한 정보만 전달한다.  
app.get('/users/:userId', function (req, res) {
  req.assert('userId').notEmpty();
  var errors = req.validationErrors();
  if (errors) {
    res.send({ error: -1, data: errors });
    return;
  }
  
  var userId = req.params.userId;
  
  User.find({ where: { userId: userId }}).success(function (user) {
    var data = {};
    data.userId = user.userId;
    data.topScore = user.topScore;
    data.coin = user.coin;
    data.itemSlot1 = user.itemSlot1;
    data.itemSlot2 = user.itemSlot2;
    data.itemSlot3 = user.itemSlot3;
    res.send({ error: '', data: data });
  }).error(function (error) {
    res.send({ error: 'db error' });
  });
});

//클라이언트에서 점수받기
//클라이언트에서 점수를 받는 API이다.  
//POST 메서드이며 userId와 score를 받는다.  
//zadd 함수로 Redis의 Sorted Set에 점수를 저장한다.  
//Redis에 점수 저장이 성공하면 MySQL에서 유저 정보를 가져온다.  
//그리고 유저의 최고 점수와 현재 점수를 비교하여 현재 점수가 높으면
//유저의 최고 점수를 업데이트한다.  
//writeLog 함수로 userId와 점수를 저장한다.  
app.post('/users/:userId/scores', function (req, res) {
  req.assert('userId').notEmpty();
  req.checkBody('score').notEmpty();
  var errors = req.validationErrors();
  if (errors) {
    res.send({ error: -1, data: errors });
    return;
  }
  
  var userId = req.params.userId;
  var score = req.body.score;
  
  redisClient.zadd('leaderboard', score, userId, function (err, reply) {
    if (!err) {
      User.find({ where: { userId: userId }}).success(function (user) {
        if (score > user.topScore) {
          user.topScore = score;
          user.save().success(function () {
            res.send({ error: '' });
          }).error(function (error) {
            res.send({ error: 'db error' });
          });
        } 
        else 
          res.send({ error: '' });
        
        writeLog('game', {
          category: 'score',
          userId: userId,
          score: score
        });
      });
    }
    else
        res.send({ error: 'cache error' });
    });
});

//유저 현재 순위 얻기
//유저의 현재 순위와 전체 순위 정보를 얻는 API이다.  
//GET 메서드이며 userId를 받는다.  
//zrank 함수로 Redis에서 현재 유저의 순위를 얻는다.  
app.get('/users/:userId/rank', function (req, res) {
  req.assert('userId').notEmpty();
  var errors = req.validationErrors();
  if (errors) {
    res.send({ error: -1, data: errors });
    return;
  }
  
  var userId = req.params.userId;
  
  redisClient.zrank('leaderboard', userId, function (err, reply) { 
    if (!err) 
      res.send({ error: '', data: { rank: reply }});
    else
      res.send({ error: 'cache error' });
  });
});

//전체 순위 정보 얻기
//zrevrange 함수로 처음(0)부터 끝(-1)까지 점수 순으로 정렬된 정보를   
//얻는다. 0, -1 대신 특정 구간의 정보를 얻을 수도 있다. withscores를   
//설정하면 점수도 함께 얻는다. zrevrange 함수에서 출력된 정보는 userId,   
//점수순으로 된 배열이다.  
app.get('/leaderboard', function (req, res) {
  redisClient.zrevrange('leaderboard', 0, -1, 'withscores', function (err, reply) {
    if (!err) {
      var data = [];
      for (var i = 0, rank = 1; i < reply.length; i += 2, rank++) {
        data.push({ rank: rank, userId: reply[i], score: reply[i + 1] });
      }
      res.send({ error: '', data: data });
    }
    else {
      res.send({ error: 'cache error'});
    }
  });
});

//아이템 구입하기
//아이템을 구입하는 API이다.   
//POST 메서드이며, userId, itemId, itemSlot을 받는다.  
//MySQL에서 유저 정보를 가져온 뒤 유저가 가지고 있는 돈(coin)을 확인한다.   
//아이템을 살 수 있으면 아이템 테이블에 설정된 대로 아이템 값을 차감하고,  
//유저의 아이템 슬롯에 아이템을 넣어준 뒤 MySQL에 저장한다.  
//클라이언트에는 아이템 구입 결과를 보낸다.  
//writeLog 함수로 userId, itemSlot, ItemId를 저장한다.   
app.post('/users/:userId/items', function (req, res) {
  req.assert('userId').notEmpty();
  req.checkBody('itemId').notEmpty();
  req.checkBody('itemSlot').notEmpty();
  var errors = req.validationErrors();
  if (errors) {
    res.send({ error: -1, data: errors });
    return;
  }
  
  var userId = req.params.userId;
  var itemId = req.body.itemId;
  var itemSlot = req.body.itemSlot;
  
  User.find({ where: { userId: userId }}).success(function (user) {
    if (user.coin > itemTable[itemId].price.coin) {
      user['itemSlot' + itemSlot] = itemId;
      user.coin -= itemTable[itemId].price.coin;
      user.save().success (function () {
        var data = {};
        data['itemSlot' + itemSlot] = itemId;
        res.send({ error: '', data: data});
        writeLog('shop', {
          category: 'item',
          userId: userId,
          itemSlot: itemSlot,
          itemId: itemId
        });
      }).error(function (error) {
        res.send({ error: 'db error' });
      });
    }
    else
      res.send({ error: 'not enough coin' });
  }).error(function (error) {
    res.send({ error: 'db error' });
  });
});

server.listen(80);
```
  
다음은 app.js에서 사용한 모듈들의 버전을 정의한 파일이다.  
package.json  
```
{
  "name": "ExampleGameServer",
  "version": "0.0.1",
  "description": "ExampleGameServer",
  "dependencies": {
    "express": "4.4.x",
    "express-validator": "2.3.x",
    "body-parser": "1.3.x",
    "aws-sdk": "2.0.x",
    "redis": "0.10.x",
    "sequelize": "1.7.x",
    "mysql": "2.3.2",
    "moment": "2.7.x"
  }
}
```
앞에서 생성한 `<프로젝트 이름>.src` S3 버킷에 ExampleGameServer라는 디렉  
터리를 생성하고 app.js, package.json 파일을 올린다.  
<img src="https://user-images.githubusercontent.com/33191974/160267197-8980c6d6-b1f8-4431-89ee-156b61949b90.png" width="50%" height="50%"/>  























