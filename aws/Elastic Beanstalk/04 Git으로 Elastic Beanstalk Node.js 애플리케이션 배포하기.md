# 정리  
[명령어 설명 참고](https://github.com/yunkangmin/spring-boot/tree/main/aws/etc) 

---

분산형 버전 관리 시스템인 Git으로 Elastic Beanstalk Node.js 애플리케이션을   
Elastic Beanstalk 환경에 배포해보자.   
  
Git을 이용한 Elastic Beanstalk 작업 흐름   
<img src="https://user-images.githubusercontent.com/33191974/158306076-8b63daf0-3236-41da-aa4a-69fb50db6ca7.png" width="50%" height="50%"/>  
Git을 설치하지 않았다면 Git(http://git-scm.com)을 설치한다.
  
Amazon Linux, CentOS  
```
sudo yum install git
```
Ubuntu Linux  
```
sudo apt-get install git
```

# Linux에 Python, pip 및 EB CLI 설치  
EB CLI에는 Python 2.7, 3,4 또는 그 이상이 필요하다. Python이 기본으로 설치되어  
있지 않았거나 이전 버전과 함께 제공된 경우 pip 및 EL CLI를 설치하기 전에   
Python을 설치한다.   
#### Linux에 Python 3.7을 설치하려면  
1. Python이 이미 설치되어 있는지 확인한다.   
```
$ python --version
```
2. Python 2.7 이상이 설치되어 있지 않은 경우 배포의 패키지 관리자를 사용하여   
Python 3.7을 설치한다. 다음과 같이 명령과 패키지이름이 다르다.   
- Ubuntu와 같은 Debian 계여려 시스템에는 APT를 사용한다.  
```
$ sudo apt-get install python3.7
```
- Red Hat 및 계열 시스템에는 yum을 사용한다.  
```
$ sudo yum install python37
```
- SUSE 및 계열 시스템에는 zypper를 사용한다.   
```
$ sudo zypper install python3-3.7
```
3. Python이 올바르게 설치되었는지 확인하려면 터미널 또는 쉘을 열고 다음 명령을  
실행한다.   
```
$ python3 --version
```
Python Packaging Authority에서 제공하는 스크립트를 사용하여 pip를 설치한 후   
EB CLI를 설치한다.   
#### pip 및 EB CLI를 설치하려면  
1. pypa.io에서 설치 스크립트를 다운로드한다.  
```
$ curl -O https://bootstrap.pypa.io/get-pip.py
```
이 스크립트는 최신 버전의 pip와 setuptools라는 다른 필수 패키지를 다운로드하고  
설치한다.  
2. Python을 사용하여 스크립트를 실행한다.   
```
$ python3 get-pip.py --user
Collecting pip
  Downloading pip-8.1.2-py2.py3-none-any.whl (1.2MB)
Collecting setuptools
  Downloading setuptools-26.1.1-py2.py3-none-any.whl (464kB)
Collecting wheel
  Downloading wheel-0.29.0-py2.py3-none-any.whl (66kB)
Installing collected packages: pip, setuptools, wheel
Successfully installed pip setuptools wheel 
```
python 대신 python3 명령을 사용하여 Python 버전 3을 직접 호출하면 이전 버전의   
Python이 시스템에 있어도 pip가 적절한 위치에 설치된다.   
3. 실행경로 `~/.local/bin`을 `PATH` 변수에 추가한다.  
`PATH` 변수를 수정하려면(Linux, Unix 또는 macOS)
   a. 사용자 폴더에서 셸의 프로파일 스크립트를 찾는다. 어떤 셸을 가지고 있는지   
   잘 모르는 경우 `echo $SHELL`을 실행한다.   
   ```
   $ ls -a ~
   .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
   ```
   - Bash - .bash_profile, .profile 또는 .bash_login
   - Zsh - .zshrc
   - Tcsh - .tcshrc, .cshrc 또는 .login  
   b. 내보내기 명령을 프로파일 스크립트에 추가한다. 다음 에제에서는 LOCAL_PATH로   
   표현되는 경롤르 현재 PATH 변수에 추가했다.   
   ```
   $ export PATH=~/.local/bin:$PATH
   ```
   c. 첫 번째 단계에서 설명한 프로파일 스크립트를 현재 세션에 로드한다. 다음  
   예제에서는 PROFILE_SCRIPT로 표현되는 프로파일 스크립트를 로드한다.  
   ```
   $ source ~/.bash_profile
   ```
4. pip가 올바르게 설치되었는지 확인한다.  
```
$ pip --version
pip 8.1.2 from ~/.local/lib/python3.7/site-packages (python 3.7)
```
5. pip를 사용하여 EB CLI를 설치한다.   
```
$ pip install awsebcli --upgrade --user
```
6. EB CLI가 올바르게 설치되었는지 확인한다.   
```
$ eb --version
EB CLI 3.14.8 (Python 3.7)
```

※ 최신 버전으로 업그레이드하려면 설치 명령을 다시 실행한다.   
```
$ pip install awsebcli --upgrade --user
```

# EB CLI 구성
EB CLI를 설치한 후 eb init를 실행하면 프로젝트 디렉터리 및 EB CLI를 구성할 준비가   
완료된다.   
다음은 이름이 eb init인 프로젝트 폴더에서 eb를 처음 실행할 경우의 구성 단계를   
보여주는 예이다.  
#### EB CLI 프로젝트를 시작하려면   
1. 먼저 EB CLI가 리전을 선택하라는 메시지를 표시한다. 사용할 리전에 해당하는  
번호를 입력한다.   
```
[ec2-user@ip-172-31-9-172 ~]$ eb init

Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
4) eu-west-1 : EU (Ireland)
5) eu-central-1 : EU (Frankfurt)
6) ap-south-1 : Asia Pacific (Mumbai)
7) ap-southeast-1 : Asia Pacific (Singapore)
8) ap-southeast-2 : Asia Pacific (Sydney)
9) ap-northeast-1 : Asia Pacific (Tokyo)
10) ap-northeast-2 : Asia Pacific (Seoul)
11) sa-east-1 : South America (Sao Paulo)
12) cn-north-1 : China (Beijing)
13) cn-northwest-1 : China (Ningxia)
14) us-east-2 : US East (Ohio)
15) ca-central-1 : Canada (Central)
16) eu-west-2 : EU (London)
17) eu-west-3 : EU (Paris)
18) eu-north-1 : EU (Stockholm)
19) eu-south-1 : EU (Milano)
20) ap-east-1 : Asia Pacific (Hong Kong)
21) me-south-1 : Middle East (Bahrain)
22) af-south-1 : Africa (Cape Town)
(default is 3): 10
```
2. 그런 다음 EB CLI가 리소스를 관리할 수 있도록 액세스 키와 보안 키를 입력한다.   
액세스 키는 AWS Identity and Access Management 콘솔에서 생성된다.  
```
You have not yet set up your credentials or your credentials are incorrect
You must provide your credentials.
(aws-access-id): A5K5NERWERSUGBMFQAJ4
(aws-secret-key): 1qm5+LRWSDGWltMBkbeRLNSDFfeOzilSDFSDF/aLcjDN/+
```
3. 1번을 선택하여 기존에 있는 애플리케이션을 선택한다.  
```
Select an application to use
1) exampleapp
2) [ Create new Application ]
(default is 2): 1
```

> Cannot setup CodeCommit because there is no Source Control setup,  
> continuing with initialization(Source Control 설정이 없기 때문에   
> CodeCommit을 설정할 수 없으며 초기화를 계속합니다.)    
> 위와 같은 메시지가 나오는데 CodeCommit 서비스를 이용하지 않기 때문에   
> 무시하면 된다.  
> eb init으로 설정을 잘못했다면 eb init --interactive 명령어로 기본 설정을   
> 지우고 다시 시작하면 된다.   

이제 간단한 웹 페이지를 작성할 차례이다. 메모장이나 기타 텍스트 편집기를 열고   
다음과 같이 작성한 뒤 app.js로 저장한다.   
app.js   
```
var express = require('express')
   , http = require('http')
   , app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('Hello Elastic Beanstalk - Git');
});

http.createServer(app).listen(process.env.PORT || 3000);
```
Node.js npm 패키지 사용을 위해 다음과 같이 작성한 뒤 package.json으로 저장한다.   
package.json  
```
{
  "name": "hello",
  "description": "Hello Elastic Beanstalk",
  "version": "0.0.1",
  "dependencies": {
    "express": "4.4.x"
  }
}
```

# EB CLI와 Git 사용  
EB CLI는 Git과 통합된다. 이 단원에서는 Git을 EB CLI와 함께 사용하는 방법을 간략히  
살펴본다.   
#### Git을 설치하고 Git 리포지토리를 시작하려면   
1. http://git-scm.com으로 이동하여 최신 Git 버전을 다운로드한다.  
2. 다음을 입력하여 Git 리포지토리를 시작한다.   
```
$ git init
```
EB CLI가 이제 애플리케이션이 Git으로 설정되었음을 인식한다.  
3. eb init을 아직 실행하지 않은 경우 지금 실행한다.   
```
$ eb init
```
# Git 브랜치와 Elastic Beanstalk 환경 연결   
다양한 환경을 코드의 각 브랜치와 연결할 수 있다. 브랜치를 체크아웃하면 연결된   
환경에 변경 사항이 배포된다. 예를 들어 다음을 입력하여 프로덕션 환경을 메인라인   
브랜치와 연결하고 별도의 배포 환경을 배포 브랜치와 연결할 수 있다.   
```
$ git checkout master
$ eb use Exampleapp-env
```

Git 저장소에 app.js, package.json 파일을 커밋한다.  
```
$ git add app.js
$ git add package.json
$ git commit -m "Hello Elastic Beanstalk"
$ eb deploy
```

---

#### 커밋하지 않고 변경사항을 배포하려면   
1. 스테이징 영역에 새 파일과 변경된 파일을 추가한다.   
```
$ git add .
```
2. eb deploy를 사용하여 스테이징된 변경 사항을 배포한다.   
```
$ eb deploy --staged
```

---

Elastic Beanstalk 환경 페이지로 이동한다. 잠시 기다리면 Health가, Updating에서  
Green으로 바뀌고, Elastic Beanstalk 애플리케이션 배포가 완료된다. 그리고   
Running Version에는 git-<리비전>으로 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/158359461-2b6913b3-1468-4d26-ac51-54f8ec069f95.png" width="50%" height="50%"/>    
웹 브라우저에서 Elastic Beanstalk URL에 접속하면 app.js의 내용이 표시된다.  
<img src="https://user-images.githubusercontent.com/33191974/158359599-2b1dab57-3cc0-49be-94a0-41a6ee68fa09.png" width="50%" height="50%"/>    
이처럼 Git으로 Elastic Beanstalk 애플리케이션을 배포할 수 있다.   





















