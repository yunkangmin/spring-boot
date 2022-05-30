# 정리  
핑(영어: Ping)은 IP 네트워크를 통해 특정한 호스트가 도달할 수 있는지의  
여부를 테스트하는 데 쓰이는 컴퓨터 네트워크 도구 중 하나이다.   
핑 명령의 기본적인 작동 원리는 네트워크 상태를 확인하려는 대상(target)   
컴퓨터를 향해 일정 크기의 패킷(packet, 네트워크 최소 전송단위)을 보낸 후(  
ICMP echo request) 대상 컴퓨터가 이에 응답하는 메시지(ICMP echo reply)를  
보내면 이를 수신, 분석하여 대상 컴퓨터가 작동하는지, 또는 대상 컴퓨터까지  
도달하는 네트워크 상태가 어떠한지 파악할 수 있다.  
   
핑 명령은 TCP/IP 프로토콜 중 ICMP(Internet Control Message Protocol)을   
통해 동작하므로, 이 프로토콜을 지원하지 않는 기기를 대상으로 ping을 수행  
할 수 없다. 또한 보안의 이유로 ICMP 사용을 차단하는 기기 역시 ping 요청에  
대응하지 않는다.  
  
샘플 핑 테스트   
아래는 `www.example.com`이라는 대상 호스트로 5번의 핑을 보냈을 때의 출력물  
이다.  
```
$ ping -c 5 www.example.com  
PING www.example.com (93.184.216.119): 56 data bytes
64 bytes from 93.184.216.119: icmp_seq=0 ttl=56 time=11.632 ms
64 bytes from 93.184.216.119: icmp_seq=1 ttl=56 time=11.726 ms
64 bytes from 93.184.216.119: icmp_seq=2 ttl=56 time=10.683 ms
64 bytes from 93.184.216.119: icmp_seq=3 ttl=56 time=9.674 ms
64 bytes from 93.184.216.119: icmp_seq=4 ttl=56 time=11.127 ms

--- www.example.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 9.674/10.968/11.726/0.748 ms
```
이 유틸리티는 특정 횟수의 핑을 마친 뒤 결과물을 요약하여 보여준다.  
가장 짧은 왕복 시간은 9.674 밀리초였으며, 평균은 10.968 밀리초, 최대값은   
11.726 밀리초였다.  

---

**A 레코드(Address Record)는 DNS 서버에서 IP 주소를 알려주도록 설정하는 기능이다.**     
EC2 인스턴스(Example Server)의 IP 주소를 A레코드로 생성해보자. 아직 EC2 인스턴스   
를 생성하지 않았다면 'EC2 인스턴스 생성하기'를 참조하여 EC2 인스턴스를 생성한다.   
그리고 '고정 IP를 제공하는 Elastic IP'를 참조하여 EC2 인스턴스에 Elastic IP를  
연결한다.   
  
먼저 EC2 인스턴스의 공인 IP 주소를 확인한다. EC2 인스턴스 목록에서 EC2 인스턴스(  
ExampleServer)를 선택한다. 아래 세부 내용에서 Public IP, Elastic IP 부분이   
EC2 인스턴스의 공인 IP 주소이다.  
<img src="https://user-images.githubusercontent.com/33191974/159204025-66d0d325-ae13-49d2-a4a9-20f89035b760.png" width="50%" height="50%"/>     
  
다시 Route 53으로 돌아온다. Route 53 Hosted Zone 목록에서 도메인을 선택하고   
위쪽 Go to Record Sets 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159204337-cb63b538-8cac-48d5-ad85-e5c09aae6dc4.png" width="50%" height="50%"/>     
도메인의 레코드 목록이 표시된다. 위쪽 Create Record Set 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159204421-a7e606f9-6a9a-4b13-a338-a60ba0768cda.png" width="50%" height="50%"/>   
  
A 레코드를 생성한다.  
- Name: 생성할 서브 도메인 이름이다. 예를 들어 ec2를 입력하면 ec2.example.com  
에 대해 A 레코드를 생성한다(서브 도메인). 아무것도 입력하지 않으면 example.com   
에 대해 A 레코드를 생성한다. ec2를 입력한다.   
- Type: 레코드 종류이다. 기본값 그대로 A - IPv4 address를 선택한다.  
- Alias: A 레코드만 사용할 수 있는 기능이다. IP 주소 대신 AWS 리소스인 S3,   
CloudFront, ELB를 설정할 수 있다. 기본값 그대로 사용한다.   
- TTL: Time To Live의 약자이며, A 레코드가 갱신되는 주기이다. 초 단위로   
설정한다. 이 후 A 레코드의 IP 주소를 바꾸면 TTL에 설정한 시간이 지나야    
적용된다. 기본값 그대로 사용한다.  
- Value: 도메인 네임을 쿼리했을 때 알려줄 IP 주소이다. EC2 인스턴스의 공인   
IP 주소를 입력한다.  
- Routing Policy: 라우팅 정책이다. 기본값 그대로 Simple을 선택한다.   
   - Simple: 아무런 부가 기능없이 IP 주소만 알려준다.  
   - Weighted: Weighted Round Robin 기능을 사용한다.  
   - Latency: Latency Based Routing 기능으르 사용한다.  
   - Failover: DNS Failover 기능을 사용한다.   

설정이 완료되었으면 Create 버튼을 클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/159205288-8a9a8b85-88ff-41fd-a343-0431ab4dc609.png" width="50%" height="50%"/>     
도메인의 레코드 목록에 A 레코드가 생성되었다.  
<img src="https://user-images.githubusercontent.com/33191974/159205350-69817a04-82b8-450a-ab3a-662d73e28124.png" width="50%" height="50%"/>     
웹 브라우저에서 방금 A 레코드로 생성한 도메인에 접속한다. EC2 인스턴스에서   
웹 서버를 실행했다면 웹 페이지가 표시될 것이다(EC2 인스턴스에 웹 서버를   
실행하는 방법은 'EC2와 CloudFront 연동하기'를 참조하자).    
  
A 레코드로 생성한 도메인에 접속  
<img src="https://user-images.githubusercontent.com/33191974/159205835-467df4ff-9143-4db1-9ae4-3fe4ce400ff4.png" width="50%" height="50%"/>   
  
웹 서버를 실행하지 않았다면 ping 명령을 이용해서 A 레코드가 정상적으로 설정   
되었는지 확인할 수 있다. Windows의 명령 프롬프트(cmd.exe)나 Linux 터미널에서   
다음과 같이 입력한다. EC2 인스턴스의 공인 IP 주소가 출력되면 A 레코드가   
정상적으로 생성된 것이다.  

```
C:\Users\yun>ping ec2.slkjfd.cf
Ping ec2.slkjfd.cf [13.124.66.144] 32바이트 데이터 사용:
```































