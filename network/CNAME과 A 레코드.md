## DNS
인터넷을 구성하고 있는 IP 주소는 IPv4의 경우 192.168.0.1 같이 숫자로 구성된다.  
이런 숫자는 아무런 의미도 없기 때문에 외우기 힘들다는 단점이 있다. 따라서 naver.com  
같은 문자열로 서버주소를 표현하게 된다. 다만 실제 컴퓨터 통신에서는 naver.com이라는  
문자열 주소를 192.168.0.1 같은 IPv4 주소로 변환해주는 서비스가 필요하다. 이런 서비스를  
DNS 서비스라고 한다.  
  
DNS는 Domain Name System의 약자로 naver.com 같은 문자열 주소를 IP 주소로 해석해주는  
네트워크 서비스를 말한다. DNS 서버에는 도메인 주소와 IP 주소의 쌍(Pair)이 저장된다.  
예를 들어, 
- naver.com      192.168.0.1
- google.com     172.17.0.1
- plusblog.co.kr 10.234.34.12

이런 정보가 DNS 서버에 저장되고 사용자가 웹 브라우저나 프로그램에서 naver.com을 요청하면   
이 테이블에서 naver.com을 찾아 192.168.0.1을 응답해준다.  
사용자의 웹 브라우저나 프로그램은 이 IP 주소를 이용해서 통신하게 된다.  
  
결국 위 테이블에서 하나의 행(Row)를 '레코드(Record)'라고 하면, 저장되는 타입에 따라 A Record와  
CNAME으로 구분할 수 있다.  
  
## A 레코드(A Record)
A 레코드(A Record)는 DNS에 저장되는 정보의 타입으로 도메인 주소와 서버의 IP 주소를 직접 매핑시키는  
방법이다.  위 테이블에 적혀있는 것처럼 첫 번째 컬럼값이 naver.com 같은 도메인 주소고, 두 번째 컬럼  
값이 192.168.0.1 같은 IP 주소인 형태를 말한다.  
  
위에서 말했던 것처럼 사용자가 A 레코드에 해당하는 도메인 주소에 대한 해석을 요청하면 DNS 서버는 IP  
주소를 리턴해준다.  
  
## CNAME
CNAME은 Canonical Name의 약자로 도메인 주소를 또 다른 도메인 주소로 매핑시키는 형태의 DNS 레코드  
타입이다. 

- naver.com              192.168.0.1
- dev.plusblog.co.kr     172.17.0.1
- develop.plusblog.co.kr dev.plusblog.co.kr

위와 같은 정보가 DNS 서버에 저장되어 있다고 하자. 사용자가 dev.plusblog.co.kr 주소를 요청하면  
서버는 172.17.0.1을 응답한다. A 레코드 정보의 해석이다.  
  
만약 develop.plusblog.co.kr을 요청하면 DNS 서버는 dev.plusblog.co.kr을 리턴하고 사용자는 다시  
dev.plusblog.co.kr 정보를 DNS 서버에 요청한다. DNS 서버는 이제 172.17.0.1을 리턴하고  
사용자는 IP 주소를 사용하게 된다.  
  
develop.plugblog.co.kr과 dev.plusblog.co.kr 정보가 매핑되어 있는 행(Row)을 CNAME 타입이라고 한다.  
즉, 도메인 주소에 대한 별명을 붙여주는 정보라고 할 수 있다.  

## A 레코드 VS CNAME
A 레코드 타입과 CNAME 타입의 장단점은 명확하다.  
  
A 레코드의 장점은 한 번의 요청으로 찾아갈 서버의 IP 주소를 한 번에 알 수 있다는 점이다.  
반면 단점은 IP 주소가 자주 바뀌는 환경에서는 조금 번거로울 수 있다는 점이다.  
예를 들어, 172.17.0.2 서버에서 plusblog.co.kr, dev.plusblog.co.kr, travel.plusblog.co.kr등  
여러 개의 서브 도메인들을 처리하고 있다고 하자. 각 서브 도메인들을 A 레코드로만 매핑시켰다면  
172.17.0.2라는 IP 주소가 172.17.0.3이라는 주소로 변경되었다면 모든 A 레코드를 찾아서 변경해야 한다.  
  
CNAME 레코드의 장점은 IP 주소가 자주 변경되는 환경에서 유연하게 대응할 수 있다는 점이다.   
예를 들어 dev.plusblog.co.kr, travel.plusblog.co.kr 도메인 정보를 plusblog.co.kr이라는  
주소로 매핑시키는 CNAME 레코드로 저장하고, plusblog.co.kr이라는 주소를 172.17.0.2라는 IP 주소로  
매핑시키는 A 레코드로 저장해놨다면, 서버의 IP 주소가 바뀌었을 때 plusblog.co.kr의 A 레코드 정보만  
변경시키면 된다.  

CNAME 레코드의 단점은 실제 IP 주소를 얻을 때까지 여러 번 DNS 정보를 요청해야 한다는 점이다.  
DNS 정보를 해석하는 데 여러 번 요청이 필요하다는 점은 경우에 따라서 성능저하를 유발할 수 있다. 


  














































