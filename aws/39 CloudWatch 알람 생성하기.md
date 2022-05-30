## CloudWatch 알람 생성하기
EC2 인스턴스의 CPU 사용률을 모니터링하고 설정한 측정치에 도달하면  
알림 메일을 전송하도록 알람을 생성해보자.  
아직 EC2 인스턴스를 생성하지 않았다면 EC2 인스턴스를 생성하자.  
먼저 EC2 인스턴스(Example Server)를 1분 당위로 모니터링 가능하도록 세부 모니터링  
설정을 한다.   

> 세부 모니터링 설정은 프리티어에서 사용할 수 없으며 추가 요금을 지불해야 한다.   
> 추가 요금을 지불하고 싶지 않다면 이 부분은 건너뛰어도 되고 기본 모니터링만으로  
> 실습이 가능하다. 단, 알람이 동작하는 것을 확인하려면 5분 이상 기다려야 한다.  

EC2 인스턴스 목록에서 EC2 인스턴스(Example Server)를 선택하고, 마우스 오른쪽 버튼을  
클릭하면 팝업 메뉴가 나온다. Enable Detailed Monitoring을 클릭한다.   
![image](https://user-images.githubusercontent.com/33191974/138238032-5cacc75d-d0d2-4b1f-aa2f-6e0c679c53ae.png)  
Yes, Enable 버튼을 클릭하여 세부 모니터링을 활성화한다.  
![image](https://user-images.githubusercontent.com/33191974/138238423-3ad54271-fbe3-4a4f-8064-e2ae79e7ea0c.png)  
AWS 콘솔로 접속한 뒤 메인 화면에서 CloudWatch를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138238607-a27e8e2a-254c-4417-b6b4-dc6e92e6f62e.png)  
왼쪽 메뉴에서 Alarms를 클릭하고 위쪽 Create Alarm 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138239099-91c984e6-167b-48a2-b898-723cac7c68c1.png)  
알림 생성창이 나온다. EC2 Metrics를 클릭한다.   
![image](https://user-images.githubusercontent.com/33191974/138239283-7d6a2175-910b-41c3-bd0f-079b0679419e.png)  
아래 그림에서 Example Server는 이전에 생성한 EC2 인스턴스이다.  
EC2 인스턴스가 여러 개일 경우 스크롤을 내리면 다른 EC2 인스턴스도 선택할 수 있다.  
CPUUtilization을 선택하고, 아래 그래프 부분에서 모니터링 간격을 1 분으로 변경한다.
![image](https://user-images.githubusercontent.com/33191974/138241975-bd01540a-1d5a-44a5-891a-7f82381dbdd8.png)
![image](https://user-images.githubusercontent.com/33191974/138242093-6e28c75a-802d-4699-ba99-b254d06b7c51.png)
![image](https://user-images.githubusercontent.com/33191974/138242250-8666dd74-874d-425b-a834-6c8f0138fe9b.png)
기간은 1분으로 하고 측정치는 30으로 한다. CPU 사용률(%)이다. >= 30으로 설정한다.  
(CPU 사용률 30% 이상에서 알람동작). 30을 입력하면 CloudWatch 그래프에서 30에 해당하는  
위치에 빨간색으로 선이 표시된다.    
경보를 알릴 데이터 포인트는 N개 데이터 포인트 중 M개가 경보 임계값을 넘을 경우 경보를 지원할 수 있다.  
데이터 포인트는 측정치 집계 기간 동안의 측정치 값이다. 예를 들어 측정치 집계 기간으로 1분을 사용할 경우   
1분당 1개의 데이터 포인트가 생성된다. 측정치의 N개 데이터 포인트 중 M개가 사전 정의된 임계값을 초과할 경우  
경보가 발생한다. 1 / 1은 1개 데이터 포인트 중 1개가 임계값을 초과하면 경보가 발생한다.  
누락된 데이터 처리는 경보를 보내기 위해 평가할 때 누락된 데이터들을 어떻게 처리하는지를  
말하는 듯하다.   
테스트용 알람 설정이기 때문에 무조건 알람이 발생하도록 CPU 사용률 측정치를 낮게 잡았다.  
실제로 Auto Scaling과 연계할 때는 보통 60% 또는 80%로 설정한다.  
![image](https://user-images.githubusercontent.com/33191974/138242860-40125dfb-a28e-4bf1-9c51-123e0b76bb37.png)
![image](https://user-images.githubusercontent.com/33191974/138265491-8db5dbc5-934b-4629-b12f-f19399425204.png)
주제생성 버튼을 누르면 이메일 확인 메일 간다.  
스팸 메일을 방지하기 위해 자신의 이메일이 맞는지 확인하는 절차이다.  
웹 브라우저를 실행하고 입력한 이메일 주소의 메일함으로 이동한다.  
메일함을 보면 AWS Notificaiton - Subscription Confirmation이라는 메일이 도착했을 것이다.   
메일 내용에서 Confirm subscription 링크를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138267383-ae5552be-1255-4aed-8a68-2a0aa877045f.png)  
이메일 확인이 완료되었다는 페이지가 표시된다.  
이제 CloudWatch(실제로는 SNS(Simple Notification Service)가 보내는 알림 이메일을 받을 수 있게 되었다.  
![image](https://user-images.githubusercontent.com/33191974/138267563-e99babb3-5575-4dd8-9acd-ef80fe8ce441.png)  
SNS 서비스로 가면 이메일 확인이 완료되었다고 표시된다.  
![image](https://user-images.githubusercontent.com/33191974/138273059-0c3eb8f8-a2bd-4478-a0cc-051da060b891.png)  
경보이름을 new Cpu Watch로 생성한다.  
![image](https://user-images.githubusercontent.com/33191974/138244539-c63ad850-196d-4a21-ad39-aed1f8ca58fd.png)
![image](https://user-images.githubusercontent.com/33191974/138244663-d7b832c6-98e9-4193-8c13-df4614570d60.png)
CloudWatch 알람 목록에서 새로 생성한 알람을 확인할 수 있다.  
현재 CPU 상태는 생성한지 얼마되지 않아 데이터부족으로 나온다.  
![image](https://user-images.githubusercontent.com/33191974/138273542-40da3b92-6638-469e-abc5-4d1de0172713.png)  
이제 CPU 사용률을 강제로 올려서 알람상태가 되도록 만들어보자.  
이전에 생성한 EC2 인스턴스(Example Server)에 SSH로 접속한 뒤 yes > /dev/null 명령을 입력한다.   
![image](https://user-images.githubusercontent.com/33191974/138273759-1a53839f-f8f7-4429-ab70-35e213d7dbaa.png)  
이 명령을 입력한 뒤 가만히 두고 잠시 기다린다. 1분 정도 시간이 지나면 알람이 경보상태로 바뀐다.  
아래 그래프에서도 CPU 사용률이 올라간 것을 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/138274038-b57773cb-87b1-482c-8a70-1ca81b5c70e4.png)  
웹 브라우저 메일함을 열어보면 ALARM: "new Cpu Watch" in Asis Pacipic(Seoul)라는 알람 이메일이  
도착한 것을 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/33191974/138274333-002c6cbe-668d-49fb-a28f-688776371024.png)



































