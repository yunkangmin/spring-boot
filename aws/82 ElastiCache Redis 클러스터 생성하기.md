# 정리  
- 쓰기 활동의 경우 애플리케이션을 직접 기본에 연결하는 대신 기본 엔드포인트에  
연결하는 것이 좋다. 왜?   
기본 엔드포인트는 기본 노드의 DNS 이름이다. 즉, 기본 노드는 원래 ip이다.    
보통 원래 ip로 접속을 하지 않고 DNS 주소로 하기도 하고 레디스에서 쓰기    
작업을 할 때도 DNS 주소로 하기 때문이다.  

- sudo  
sudo는 관리자 권한으로 실행함을 의미한다. 즉, 관리자의 권한이 필요한 경우  
예를 들어, 접근이 제한되어 있는 파일이나 폴더 등을 수정, 삭제하는 등의   
제한된 권한이 사용된 곳에 sudo를 입력하게 된다.    
일반적으로 새로운 애플리케이션을 설치하거나 OS 설정 등을 변경할 때 필요할  
수 있다. sudo를 사용하는 것은 root 사용자의 권한을 얻는 것과 동일하다. 

---

ElastiCache에서 Redis 클러스터를 생성하고 사용해보자. ElastiCache 캐시 클러스터  
목록(Amazon ElastiCache -> Cache Clusters)에서 위쪽 Launch Cache Cluster 버튼을  
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157166885-968819c4-8034-4b5e-b17a-3092db1b4d05.png" width="50%" height="50%"/>      
Redis 클러스터를 생성한다.   
- Name: 클러스터 이름이다. exampleredis를 입력한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157167464-898a2700-a41a-4b89-b8b1-f82a4f9afd8d.png" width="50%" height="50%"/>  
  
- Cache Port: Redis 포트이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157167516-0093613d-dd1d-4a6e-836e-d64db354a9c2.png" width="50%" height="50%"/>   
  
- Number of Nodes: 생성할 캐시 노드이다. Redis는 아직까지 클러스터에 캐시 노드를   
추가할 수 없으므로 값을 설정할 수 없다(현재는 안보임).  
- Node Type: 캐시 노드 유형이다. 프리티어에서 무료로 사용할 수 있는 마이크로 캐시  
노드(cache.t2.micro)를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157167638-8482ee1d-0623-4cf4-aec8-68c7bb5facf9.png" width="50%" height="50%"/>  
  
- Topic for SNS Notification: 알람을 받을 SNS 토픽이다. 여기서는 알람은 받지  
않을 것이므로 Disable Notifications를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157167755-140b6fee-1ea8-4b03-982c-dc50fb71e616.png" width="50%" height="50%"/>   
  
- S3 Snapshot Location: S3에 저장된 Redis RDB(Redis Database) 파일의 위치이다.   
ElastiCache Redis 클러스터를 생성하면 이 RDB 파일의 내용이 캐시 노드에 복원된다.  
이미 Redis를 사용하고 있다가 ElastiCache로 이전(Migrate)하고 싶을 때 사용한다.   
RDB 파일은 Redis에서 BGSAVE, SAVE 명령으로 생성할 수 있다. 기본값 그대로 비워둔다.  
  <img src="https://user-images.githubusercontent.com/33191974/157168060-14b0b64f-636a-4497-a476-319253712cc9.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
보안패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들면 Redis의 경우  
2.8.6를 사용하고 있는데 2.8.7이 나오면 2.8.7 버전으로 업데이트하게 된다.  
기본값 그대로 사용한다(현재는 안보임).  
- Engine: 사용할 캐시 엔진이다. redis를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157168487-e9e3d795-d95d-424e-a895-79bc3cb46698.png" width="50%" height="50%"/>  
     
- Engine Version: Redis 버전이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157168524-d8d05485-60bf-4110-9c63-12db82016445.png" width="50%" height="50%"/>  
  
- Preferred Zone: 클러스터가 위치할 가용 영역(AZ)이다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157168655-197129e3-54a9-411c-ba86-f39211db7e37.png" width="50%" height="50%"/>  
     
- Cache Subnet Group: 캐시 노드가 위치할 서브넷이다. 생성한 Subnet Group이 있어  
야 선택할 수 있다(memcached 생성시 사용한 서브넷을 사용한다).  
  <img src="https://user-images.githubusercontent.com/33191974/157168842-c91d6389-81b1-4d92-b361-55d46b251caa.png" width="50%" height="50%"/>   
  
- VPC Security Group: 방화벽 설정인 Security Group이다. 기본값 그대로 사용한다.   
이 Security Group은 나중에 Redis 클러스터 전용으로 따로 생성해야 한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157169013-9588254f-6b63-4185-8e61-eec6e22d7366.png" width="50%" height="50%"/>  
  
- Cache Parameter Group: Redis를 실행할 때 필요한 매개변수 집합이다. 캐시 노드  
생성 후 Cache Parameter Group을 추가할 수 있다(redis.conf 파일을 생성하는 것과  
동일하다). 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157169341-68cf1b0e-54aa-42ff-9c0c-8e3cbd0ffc1f.png" width="50%" height="50%"/>   
  
- Maintenance Window: 점검시간이다. 기본값은 No Preference이다. 여기서는 Start  
Time을 Monday, `00:00`, Duration을 1로 설정한다. UTC기준으로 00시 00분에 점검이  
시작되며 시간은 1시간이다. 점검은 Duration에 설정한 시간보다 일찍 끝날 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/157169902-8fff1ccf-bb28-4904-8608-ce670cb87739.png" width="50%" height="50%"/>  
   - 이 시간에 Auto Minor Version Upgrade를 설정했다면 Redis 버전 업데이트 또는  
   패치가 적용된다. Redis 버전 업데이트 또는 패치는 필수적인 것(보안 패치)만   
   적용되며 자주 발생하지 않고 몇 달에 한 번 발생한다. Redis 업데이트 또는 패치가  
   적용되는 시간 동안에는 캐시 노드의 실행이 중지된다.  
   - 캐시 노드의 유형을 변경했다면 이 시간에 적용된다. 캐시 노드의 유형을 변경하는  
   동안에는 캐시 노드의 실행이 중지된다.   

> #### Enable Automatic Backups   
> Node Type을 cache.m1.small 이상으로 설정하면 자동 백업 기능을 사용할 수 있다  
> (cache.t1.micro로 설정하면 자동 백업 설정을 사용할 수 없다. 현재는 되는 걸로  
> 확인됨).   
> - Enable Automatic Backups: 자동 백업 기능이다. Yes로 설정하면 설정한 백업  
> 시간에 스냅샷을 생성한다.  
> - Backup Retention Period: 백업 데이터 유지 기간이다. 최대 35일까지 설정할  
> 수 있다. 여기서 지정한 날짜 이전까지 되돌릴 수 있다.   
> - Backup Window: 백업 시간이다. 기본값은 No Preference이다. 설정한 시간에  
> 매일 스냅샷이 생성된다. 설정 가능한 백업시간은 1시간이다.   
>   <img src="https://user-images.githubusercontent.com/33191974/157252114-852ddd40-e17c-49fc-baed-8cd4f49d2225.png" width="50%" height="50%"/>  

지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Launch Cache Cluster  
버튼을 클릭한다.   
  
ElastiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)에서 방금  
설정한 Redis 클러스터가 생성되고 있다. 완전히 생성되기까지 약 5분 정도 소요된다.   
<img src="https://user-images.githubusercontent.com/33191974/157252738-2f87ae72-b9fd-410a-a9ac-9a3156ab86c6.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160226053-871c7f6d-6741-4ada-ac3d-3d98f1ac7a63.png" width="50%" height="50%"/> 

아래는 예전 버전  

---
  
Redis 클러스터 생성이 완료되었으면 1 node 링크를 클릭한다(클러스터 모드를  
말하는 듯(하나의 노드로 인식되게하는 것)).  
  
  
캐시 클러스터에 속한 캐시 노드의 목록이 표시된다. Endpoint 부분에 exam  
pleredis-001.jyqn3e.0001.apn2.cache.amazonaws.com처럼 캐시 노드에 접속할 수   
있는 엔드포인트 주소가 표시된다. 이후 이 엔드포인트 주소를 통해 캐시 노드에  
접속하면 된다.  
<img src="https://user-images.githubusercontent.com/33191974/157253967-992bf1c2-2cd8-4112-8dca-451cedc6525f.png" width="50%" height="50%"/>  

---

# 노드 엔드포인트 찾기
클러스터가 사용 가능한 상태이고 사용자가 이 클러스터에 액세스할 수 있는 권  
한을 부여받았다면 Amazon EC2 인스턴스에 로그인하여 클러스터에 연결할 수  
있다. 이를 수행하려면 먼저 엔드포인트를 결정해야 한다.   

## Redis(클러스터 모드 비활성화됨) 클러스터의 엔드포인트 찾기(콘솔)   
Redis(클러스터 모드 비활성화됨) 클러스터에 노드가 하나 뿐이면 노드의 엔드  
포인트가 읽기와 쓰기에 모두 사용된다. 클러스터에 여러 노드가 있는 경우 세  
가지 유형의 엔드포인트(기본 엔드포인트, 리더 엔드포인트 및 노드 엔드포인트  
)가 있다.  
  
기본 엔드포인트는 항상 클러스터의 기본 노드로 확인되는 DNS 이름이다. 기본  
엔드포인트는 읽기 전용 복제본을 기본 역할로 승격하는 것과 같은 클러스터 변  
경의 영향을 받지 않는다. **쓰기 활동의 경우 애플리케이션을 직접 기본에 연결  
하는 대신 기본 엔드포인트에 연결하는 것이 좋다.**     
Redis(클러스터 모드 비활성화됨) 클러스터에서는 모든 쓰기 작업에 기본 엔드  
포인트를 사용한다. 리더 엔드포인트를 사용하여 모든 읽기 전용 복제본 사이  
에 수신 연결을 고르게 분할한다. 읽기 작업에는 개별 노드 엔드 포인트를 사  
용하자.  
  
리더 엔드포인트는 ElastiCache for Redis 클러스터의 모든 읽기 전용 복제본  
간에 엔드포인트에 대한 수신 연결을 고르게 분할한다. 애플리케이션이 연결을   
생성하는 시기 또는 애플리케이션에서 연결을 다시 사용하는 방법과 같은 추가  
요소가 트래픽 분산을 결정한다. 리더 엔드포인트는 복제본이 추가 또는 제거   
되는 클러스터의 변경 사항을 실시간으로 반영한다. ElastiCache for Redis  
클러스터의 여러 읽기 전용 복제본을 다양한 AWS 가용 영역(AZ)에 두어 리더   
엔드포인트의 가용성을 높일 수 있다.   

> #### 참고
> 리더 엔드포인트는 로드 밸런서가 아니다. 라운드 로빈 방식으로 복제본 노드  
> 중 하나의 IP주소로 확인되는 DNS 레코드이다.  

읽기 활동의 경우 애플리케이션은 클러스터의 어떤 노드에도 연결할 수 있다.   
기본 엔드포인트와 달리, 노드 엔드포인트는 특정 엔드포인트로 확인된다. 복제  
본을 추가하거나 삭제하는 것과 같이 클러스터를 변경하면 애플리케이션에서  
노드 엔드포인트를 업데이트해야 한다.  

#### Redis(클러스터 모드 비활성화됨) 클러스터의 엔드포인트를 찾으려면   
1. AWS Management Console에 로그인하고 https://console.aws.amazon.com/  
elasticache/에서 ElastiCache 콘솔을 연다.  
2. 탐색창에서 Redis를 선택한다.  
Redis(클러스터 모드 비활성화됨) 및 Redis(클러스터 모드 활성화됨) 클러스터  
의 목록이 있는 클러스터 화면이 나타난다.   
3. 클러스터의 기본 엔드포인트 또는 리더 엔드포인트를 찾으려면 클러스터   
이름 왼쪽의 상자를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/160225102-d77366be-aa80-4631-bcd8-4cc607b55c74.png" width="50%" height="50%"/>   

클러스터에 노드가 하나 뿐이면 기본 엔드포인트가 없으며 다음 단계에서 계속   
할 수 있다.  
   
4. Redis(클러스터 모드 비활성화됨) 클러스터에 복제본 노드가 있으면 클러스  
터 이름을 선택하여 클러스터의 복제본 노드 엔드포인트를 찾을 수 있다.  
노드 화면에 클러스터의 각 노드, 기본 및 복제본이 엔드포인트와 함께 나열된  
다.   
  <img src="https://user-images.githubusercontent.com/33191974/160225203-a30febb1-58f0-437c-b294-009f6dc05370.png" width="50%" height="50%"/>   
  
5. 엔드포인트를 클립보드에 복사하려면  
   1. 한 번에 하나씩 엔드포인트를 찾고 복사할 엔드포인트를 선택한다.  
   2. 강조 표시한 엔드포인트를 마우스 오른쪽 버튼으로 클릭한 후 컨텍스트  
   메뉴에서 `[copy]`를 선택한다.  
  
## Redis 클러스터 또는 복제 그룹에 연결(Linux)  
이제 필요한 엔드포인트가 있으므로 EC2 인스턴스에 로그인하여 클러스터 또는  
복제 그룹에 연결할 수 있다. 다음 예제에서는 `redis-cli` 유틸리티를 사용  
하여 클러스터에 연결한다. 최신 버전의 redis-cli는 암호화/인증이 활성화된   
클러스터에 연결할 수 있도록 SSL/TLS도 지원한다.  

## redis-cli 다운로드 및 설치   
1. 선택한 연결 유틸리티를 사용하여 Amazon EC2 인스턴스에 연결하자.   
2. 다음 명령을 실행하여 redis-cli 유틸리티를 다운로드하고 설치한다.  
Amazon Linux 2  
```
$ sudo amazon-linux-extras install epel -y
# gcc 컴파일러 설치
$ sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y  
$ sudo wget http://download.redis.io/redis-stable.tar.gz
$ sudo tar xvzf redis-stable.tar.gz
$ cd redis-stable
# 컴파일
$ make
# redis-cli를 bin에 추가해 어느 위치에서든 사용가능하게 등록한다.  
$ sudo cp src/redis-cli /usr/bin
$ sudo make BUILD_TLS=yes
```
> #### 참고  
> 연결하려는 클러스터가 암호화되지 않은 경우 Build-TLS=yes 옵션이 필요하지  
> 않다.
  
## 클러스터 모드가 비활성화된 암호화되지 않은 클러스터에 연결   
1. 다음 명령을 실행하여 클러스터에 연결하고 `cluster-endpoint` 및 `port
number`를 클러스터의 엔드포인트 및 포트 번호로 바꾼다(Redis의 기본 포트  
는 6379이다).  
```
redis-cli -h [cluster-endpoint] -c -p [port number]
```
> #### 참고  
> 위 명령에서 -c 옵션은 클러스터 모드를 활성화한다.
나타나는 Redis 명령 프롬프트는 다음과 유사하다.  
```
[cluster-endpoint]:port number]
```
2. 이제 Redis 명령을 실행할 수 있다. 리디렉션은 -c 옵션을 사용하여 활성화  
했기 때문에 발생한다.   
```
set x Hi
-> Redirected to slot [16287] located at 172.31.28.122:6379
OK
set y Hello
OK
get y
"Hello"
set x Bye
-> Redirected to slot [8157] located at 172.31.9.201:6379
OK
get z
"Bye"
get x
-> Redirected to slot [16287] located at 172.31.28.122:6379
"Hi"
```










  





























