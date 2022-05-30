# 정리  
- Amazon Linux 2에는 기본으로 CLI가 설치되어 있는듯?
- Linux x86과 Linux ARM  
x86은 인텔 cpu용이고, arm은 arm cpu용이다. 본인이 사용하는 컴퓨터의 cpu에  
맞는 바이너리를 받으면 된다.    
  
- 바이너리  
바이너리 형식과 BIN 형식이란 말은 같은 뜻이다.    
BIN이란 BINARY의 약어로서 이진코드라는 뜻이다.   
이진코드라는 것은 컴퓨터 내부적으로 이용되는 코드로서 0과 1로만 표현되는 것을 뜻한다.   
쉽게 말하자면, 바이너리 형식의 파일이란, 텍스트 형식의 파일이 아닌 모든 형식  
즉, 실행파일이나 혹은 이미지파일 등의 파일로 보면 된다.    
  
---
  
# 설치방법 
명령줄에서 다음 단계에 따라 Linux에 AWS CLI를 설치한다.  
  
1. curl 명령 사용 - `-o` 옵션은 다운로드한 패키지가 기록되는 파일 이름을 지정  
한다. 다음 예제 명령의 옵션을 사용하면 다운로드한 파일이 로컬 이름 `awscliv2.  
zip`으로 현재 디렉터리에 기록된다.  

```
sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

2. 설치 관리자의 압축을 푼다. Linux 배포에 기본 제공 unzip 명령이 없는 경우   
이와 동등한 명령을 사용하여 압축을 푼다. 다음 명령 예제는 패키지의 압축을 풀고  
현재 디렉터리 아래에 aws라는 디렉터리를 만든다.  
```
unzip awscliv2.zip  
```
3. 설치 프로그램을 실행한다. 설치 명령은 새로 압축을 푼 install 디렉터리의  
aws이라는 이름의 파일을 사용한다. 기본적으로 파일은 모두 `/usr/local/aws-  
cli`에 설치되고 `/usr/local/bin`에 심볼 링크가 생성된다. 이 명령은 해당  
디렉터리에 대한 쓰기 권한을 부여하는 sudo를 포함한다.  
```
sudo ./aws/install
```
이미 쓰기 권한이 있는 디렉터리를 지정하는 경우 sudo없이도 설치할 수 있다.   
install 명령에 대해 다음 지침에 따라 설치 위치를 지정한다.  
- `-i`및 `-b` 파라미터에 입력하는 경로의 볼륨 이름이나 디렉터리 이름에   
공백이나 기타 공백 문자가 없어야 한다. 공백이 있으면 설치가 실패한다.  
- `--install-dir` 또는 `-i`이 옵션은 모든 파일을 복사할 디렉터리를 지정한다.   
기본값은 `/usr/local/aws-cli`이다.  
- `--bin-dir` 또는 `-b` 이 옵션은 설치 디렉터리의 기본 aws 프로그램에 대한  
심볼 링크를 지정된 경로의 aws 파일에 연결하도록 지정한다. 지정된 디렉터리에  
대한 쓰기 권한이 있어야 한다. 이미 경로에 있는 디렉터리에 대한 symlink를  
만들면 설치 디렉터리를 사용자의 `$PATH` 변수에 추가할 필요가 없다.   
  
기본값은 /usr/local/bin이다.  
4. 다음 명령을 사용하여 설치를 확인한다.  
```
aws --version
aws-cli/2.4.27 Python/3.8.8 Linux/5.10.102-99.473.amzn2.x86_64 exe/x86_64.amzn.2 prompt/off
```
만약 aws 명령을 찾을 수 없으면 터미널을 재시작해보자.   

---

# CLI 구성 기본사항
이 섹션에서는 AWS Command Line Interface(AWS CLI)가 AWS와 상호작용하는데   
사용하는 기본 설정을 간편하게 구성하는 방법에 대해 설명한다. 여기에는 보안   
자격 증명, 기본 출력 형식 및 기본 AWS 리전이 포함된다.   

## aws configure를 통한 빠른 구성
일반적인 용도에서 aws configure 명령은 AWS CLI 설치를 설정할 수 있는 가장   
빠른 방법이다. 이 명령을 입력하면 AWS CLI가 네 가지 정보를 입력하라는  
메시지를 표시한다.  
- 액세스 키 ID
- 보안 액세스 키
- AWS 리전  
- 출력 형식   
```
$ aws configure
AWS Access Key ID [None]: AKIA5K5N3XSSGFSGDFQA
AWS Secret Access Key [None]: 1qm5+LWltMBkbsdfsdfsdeRLNf5Lznmal/aLcjDN/+
Default region name [None]: ap-northeast-2
Default output format [None]: json
(설정파일 저장 형식인듯)  
```














