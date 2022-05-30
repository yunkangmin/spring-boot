# DNS란 무엇인가
Domain Name System의 약자로 인터넷 주소창에 Host Domain Name을 입력했을 때  
(예) naver.com, google.com 등) 해당 문자를 IP 주소로 변환해 주는 시스템을   
말한다.  
  
필자는 URL 창에 Host Domain Name을 입력했을 때 어떤 식으로 해당 IP주소를  
받아오는지, DNS 서버의 구조는 어떻게 되어있는지, 좀 더 효율적인 방법을  
위해 어떤 걸 사용하는지 등에 대해 상세히 적어보고자 한다.  

# 1. 기지국 DNS 서버(Local DNS Server)   
URL에 Domain Name을 입력했을 때 해당 IP를 찾기위해 가장 먼저 찾는 DNS  
서버이다.   
기본적으로 컴퓨터의 LAN 선을 통해 인터넷이 연결되면, 인터넷을 사용할  
수 있게 IP를 할당해주는 통신사(KT, SK, LG등)에 해당되는 통신사의 DNS  
서버가 등록된다.   
  
KT를 사용하는 집이면 KT DNS가 되고, SK 통신사를 사용하는 집이면 SK DNS  
가 자동으로 세팅된다.   
<img src="https://user-images.githubusercontent.com/33191974/159441961-b1253d96-5cab-4b86-b81b-8063b4ed30e0.png" width="50%" height="50%"/>  
  
각 통신사에 대한 DNS 서버의 IP 주소이다.  
그럼 클라이언트는 해당 DNS 서버에 묻는다.  
예를 들어 "hwan.co.kr"이라는 도메인 주소를 입력했다고 하면, 다음과 같이   
응답한다.  
  
나: "hwan.co.kr"이라는 도메인 이름의 IP 주소좀 알려줘
 
찾는 도메인 주소가 있다면  
기지죽 DNS 서버(Local DNS Server): 해당 IP 주소는 1.1.1.100이야.   
  
찾는 도메인 주소가 없다면   
기지국 DNS 서버(Local DNS Server): Root DNS 서버에 물어볼게 기다려봐  
  
이렇게 반응하게 된다.  
  
여기서 도메인 네임에 대한 정보를 갖고 있으면서, 해당 도메인에 네임에  
해당하는 IP 주소를 갖고 있는 서버를 Authoritative DNS Server라고 한다.   
즉, 모두 다 알고 있는 서버를 말한다.   
  
반면에 두개 중에 하나만 알고 있는 서버를 Non-Authoritative DNS Server라고  
한다.  
  
여기서 의문점이 들 수 있는 부분이 "도메인 이름을 알고 있으면 IP 주소도  
알고 있는거 아니냐??"라고 생각할 수 있는데, 이는 아래 부분에 대한 설명을   
보면 이해가 될 것이다.  
   
또 한가지, 방금 필자가 클라이언트가 DNS 서버에게 IP 주소 아냐고 묻는다고  
했다. 이 때 물어보는 역할을 하는 것이 리졸버(Resolover)라고 하는 녀석이  
수행하게 된다.  
리눅스 기준으로 하면 `/etc/resolv.conf`라는 경로를 생각하면 될 것이다.  
  
# Root DNS Server   
위에서 기지국 DNS에게 처음 해당 도메인 네임에 대한 IP 주소가 있는지  
물어봤다.   
  
하지만 기지국 DNS 서버에 해당 도메인에 대한 IP 주소가 없을 때는 해당  
DNS 서버는 Root DNS 서버에게 물어보게 된다.  
  
Root DNS는 최상위 DNS 서버로 해당 DNS부터 시작해서 아래 딸린 node DNS  
서버에게로 차례차례 물어보게 된다.   
  
트리구조로 되어 있으며 사진은 아래와 같다.   
<img src="https://user-images.githubusercontent.com/33191974/159466885-8466f395-36c0-433d-b04e-104a1a7e5ac5.png" width="50%" height="50%"/>    
  
요런식으로 구성되어 있다.   
  
모든 DNS 서버(기지국 DNS 서버)들은 이 Root DNS Server의 주소를 기본적  
으로 갖고 있다. 그리고 모르는 Domain name이 온다면 Roote DNS에게 물어  
보게 된다.     
  
하지만 Root DNS Server의 목록에도 해당 Domain Name의 IP가 없을 수 있다.  
  
그럼 Root DNS Server는 기지국 DNS(Local DNS)에게 없다고 전해준다.  
단, 한가지 정보를 실어서 전달해준다.  

# 최상위 도메인(Top-Level Domain)   
Root DNS Server는 기지국 DNS(Local DNS)서버에게 없다고 알려준다고 했다.   
그와 동시에 이렇게 말한다.  
  
Root DNS Server: 나한테 해당 도메인 주소가 없다. 대신 hwan.co.kr의   
주소 중 .kr의 주소를 알고 있으니, kr DNS 주소에게 물어봐라.  
  
그럼 저희 기지국 DNS(Local DNS)서버는 kr DNS 서버에게 다시 물어보게 된다.   
이하 반복...
  
다음 설명에 앞서 최상위 도메인에 대해 좀 더 알아보자. 
  
최상위 도메인은 다음과 같이 나뉜다.  
1. 국가 코드 최상위 도메인(Country Code Top-Level Domain, ccTLD)   
(ex, .kr, .jp, .CN, .US 등...)  
2. 일반 최상위 도메인(generic top-level domain, gTLD)   
(ex, .com, .net, .org 등...)   
  
이 두개가 대표적이며, 그 밖에 신규 일반 최상위 도메인, 국제화 일반 최상위  
도메인 등 다양한 도메인들이 존재한다.  
  
본론으로 돌아와서,  
여기서 다음과 같은 사실을 알 수 있다.   
1. "hwan.co.kr"이라는 주소를 뜯어보면   
<img src="https://user-images.githubusercontent.com/33191974/159468585-e67b0974-38d9-4410-bb21-7fa33b648622.png" width="50%" height="50%"/>  
이런 식으로 구성되어 있는 걸 알 수 있다.  
2. 재귀적으로 순환을 반복한다.  
기지국 DNS(Local DNS)에서 Root DNS Server로 도메인 주소를 물으면,   
Root DNS Server에서 다 알아서 찾아주는 것이 아니라, Root DNS Server  
자신에 등록되어 있는 최상위 도메인(TLD)에서 해당 도메인에 붙어있는  
TLD 주소를 찾아서 기지국 DNS(Local DNS)에게 준다.  
그럼 기지국 DNS(Local DNS)는 Root DNS Server에게 받은 TLD Server에게  
다시 물어본다.   
이하 찾을 때까지 반복한다.  
<img src="https://user-images.githubusercontent.com/33191974/159469281-b9f1fc84-2e04-445b-af97-21f8da71a5ee.png" width="50%" height="50%"/>    
  
이런 식으로 말이다.  
이렇게 받아오는 것을 Recursive Query라고 한다.  
  
4. DNS Cache  
위 과정을 통해 우리 PC는 "hwan.co.kr"의 IP주소를 성공적으로 받아왔다고   
가정해보자.   
  
그럼 몇 분 후 다시 "hwan.co.kr"에 방문하려고 했을 때, 또 다시 위와 같은  
과정을 반복해야하는, 비효율적인 과정을 반복해야 한다.   
  
그렇게 되면 느리기도 하거니와 DNS 서버에 부담을 줄 수밖에 없다.  
  
때문에, PC에는 DNS Cache라는 Cache를 활용해 Cache안에 자주쓰는 Domain  
Name 주소를 저장해 놓는다.   
  
이를 확인하는 방법은 Window 기준 다음과 같다.  
```
cmd -> ipconfig /displaydns  
```
<img src="https://user-images.githubusercontent.com/33191974/159470350-269ee0e4-3dc8-44dc-90d4-1c5e4ecba9f7.png" width="50%" height="50%"/>    
이런 Cache 정보가 수십개 나오게 된다.  
이 정보를 가지고 PC는 찾는 과정없이 막바로 IP주소를 찾을 수 있게 된다.   
  
# 마지막으로   
DNS 설명부분에서 누락된 부분






  



























