AWS CLI로 S3를 사용할 수 있지만 이것보다 좀 더 편리한 s3cmd라는 툴이 있다.   
s3fs는 파일 시스템처럼 사용할 수 있어서 편리하지만 속도가 느린 것이 단점이다.   
s3cmd는 속도도 느리지 않고 다양한 옵션을 제공한다. 특히 로컬에 있는 많은  
파일을 S3 버킷과 동기화하고 바뀐 파일은 CloudFront에 갱신할 때 유용하다.  
그리고 s3cmd는 Linux, Windows, Mac OS X에서 모두 사용할 수 있다.   
  
http://s3tools.org/s3cmd
  
먼저 python-dateutil 패키지를 설치한다.  
   
Amazon Linux, CentOS   
```
sudo yum install -y python-dateutil
```
Ubuntu Linux   
```
sudo apt-get install -y python-dateutil
```
Windows에서는 다음 링크에서 pip-Win을 받는다. 그리고 Command에 pip install  
python-dateutil을 입력하고 Run 버튼을 클릭하면 python-dateutil 패키지를  
설치할 수 있다.  
  
https://sites.google.com/site/pydatalog/python/pip-for-windows  
  
pip-Win으로 python-dateutil 설치   
<img src="https://user-images.githubusercontent.com/33191974/160344327-d7d11f5e-fa33-4290-bd3e-6e289779907f.png" width="50%" height="50%"/>  
  
GitHub에서 소스 파일을 받은 뒤 압축을 해제하고 Python으로 설치한다.  
- Windows: 탐색기에서 압축을 해제한다. 명령 프롬프트나 PowerShell을 실행  
하고, s3cmd-master 디렉터리에서 python setup.py install 명령을 실행한다.  
- Mac OS X: Finder에서 압축을 해제한다. 터미널을 실행하고 s3cmd-master   
디렉터리에서 sudo python setup.py install 명령을 실행한다.  
  
```
wget https://github.com/s3tools/s3cmd/archive/master.zip
unzip master.zip
cd s3cmd-master
sudo python setup.py install
```
s3cmd --configure 명령으로 액세스 키와 시크릿 키를 설정한다.   
- Access Key: 액세스 키를 입력한다.  
- Secret Key: 시크릿키를 입력한다. 액세스 키와 시크릿 키를 생성하는 방법은   
'API와 툴 사용을 위한 액세스 키 생성하기'를 참조하기 바란다.   
- 나머지 옵션은 그냥 엔터키를 입력한다.   
- Test access with supplied credentials? [Y/n]: y를 입력한다.  
- Save settings? [y/N]: y를 입력한다.   
  
```
[ec2-user@ip-172-31-25-8 s3cmd-master]$ s3cmd --configure

Enter new values or accept defaults in brackets with Enter.
Refer to user manual for detailed description of all options.

Access key and Secret key are your identifiers for Amazon S3. Leave them empty for using the env variables.
Access Key: AKIA5K5N3sdfsdfAJ4
Secret Key: 1qm5+LWltMsdfsdfsd5LznmaoFesdfcjDN/+
Default Region [US]:

Use "s3.amazonaws.com" for S3 Endpoint and not modify it to the target Amazon S3.
S3 Endpoint [s3.amazonaws.com]:

Use "%(bucket)s.s3.amazonaws.com" to the target Amazon S3. "%(bucket)s" and "%(location)s" vars can be used
if the target S3 system supports dns based buckets.
DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]:

Encryption password is used to protect your files from reading
by unauthorized persons while in transfer to S3
Encryption password:
Path to GPG program [/usr/bin/gpg]:

When using secure HTTPS protocol all communication with Amazon S3
servers is protected from 3rd party eavesdropping. This method is
slower than plain HTTP, and can only be proxied with Python 2.7 or newer
Use HTTPS protocol [Yes]:

On some networks all internet access must go through a HTTP proxy.
Try setting it here if you can't connect to S3 directly
HTTP Proxy server name:

New settings:
  Access Key: AKIA5K5N3cvbcvbcvAJ4
  Secret Key: 1qm5+LWltMBkbeRLNf5LvbcvbcjDN/+
  Default Region: US
  S3 Endpoint: s3.amazonaws.com
  DNS-style bucket+hostname:port template for accessing a bucket: %(bucket)s.s3.amazonaws.com
  Encryption password:
  Path to GPG program: /usr/bin/gpg
  Use HTTPS protocol: True
  HTTP Proxy server name:
  HTTP Proxy server port: 0

Test access with supplied credentials? [Y/n]
Please wait, attempting to list all buckets...
Success. Your access key and secret key worked fine :-)

Now verifying that encryption works...
Not configured. Never mind.

Save settings? [y/N] Y
Configuration saved to '/home/ec2-user/.s3cfg'
```
s3cmd로 파일을 받는 방법은 다음과 같다.  
- `--recursive`: 하위 디렉터리까지 받는다.  
```
mkdir exampledir
cd exampledir
s3cmd get --recursive s3://s3cmdtest02
s3cmd get s3://s3cmdtest02/test02.jpg
```
  
s3cmd로 파일을 올리는 방법은 다음과 같다.   
  
- `--recursive`: 하위 디렉터리까지 올린다.  

```
cd exampledir
s3cmd put --recursive . s3://s3cmdtest02
s3cmd put ./hello.txt s3://s3cmdtest02
```
  
s3cmd로 파일을 동기화하는 방법은 다음과 같다. 로컬의 파일들과 S3 버킷을  
비교하여 바뀐 파일만 업데이트한다.   
- `--recursive`: 하위 디렉터리까지 동기화한다.   
```
s3cmd sync --recursive . s3://s3cmdtest02
s3cmd sync --recursive ./hello/ s3://s3cmdtest02/hello
```
s3cmd로 파일을 동기화한 뒤 CloudFront를 갱신하는 방법은 다음과 같다.   
- `--recursive`: 하위 디렉터리까지 동기화한다.   
- `--cf-invalidate`: 바뀐 파일만 S3 버킷에 연결된 CloudFront에 갱신한다.  
- `--cf-invalidate-default-index`: S3 정적 웹사이트 호스팅에서 설정한 In  
dex Document 파일을 갱신한다.    
- `--default-mime-type="text/html"`: 확장자가 없는 파일은 MIME 타입을 HT  
ML로 설정한다.   
- `--guess-mime-type`: 확장자에 따라서 MIME 타입을 자동으로 설정한다.   

```
s3cmd sync --recursive --cf-invalidate --cf-invalidate-default-index --default-mime-type="text/html" --guess-mime-type ./exampledir/ s3://s3cmdtest02 
```
다음 기타 옵션이다.   
- `--dry-run`: 테스트 실행 옵션이다.   
- `--acl-public`: 파일을 공개 상태로 설정한다.   
- `--delete-removed`: sync 명령으로 동기화할 때 로컬에서 삭제된 파일은 S3  
버킷에서도 삭제한다.   
  
자세한 사용 방법은 다음 링크를 참조하기 바란다.  
  
http://s3tools.org/usage

  































