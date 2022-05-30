## CloudWatch 커스텀 측정치 사용하기
CloutWatch에서는 기본적으로 제공하는 측정치 이외에도 사용자가 측정한 값을  
사용할 수도 있다. 이 것을 커스텀 측정치(Custom Metric)라 한다.  
커스텀 측정치는 서버 애플리케이션, 로그 파일, 언어 레벨에서 측정치를 생성하고,  
이 값들을 모니터링하거나 CloudWatch 액션을 제어하고 싶을 때 사용한다.   
  
이번 예제는 Amazon Linux에서 실행하는 것을 권장한다.  
Amazon Linux는 AWS 명령행 인터페이스(AWS CLI)가 미리 설치되어 있기 때문에   
매우 편리하다. Windows, Ubuntu, Linux 또는 기타 Linux에는 기본적으로 AWS CLI가  
기본적으로 설치되어 있지 않다. 따라서 AWS CLI를 따로 설치해야 한다.  
설치방법은 <http://aws.amazon.com/ko/cli>에 나와 있다. SSH로 EC2 인스턴스(Example Server)에 접속한  
뒤 aws configure 명령을 입력하여 액세스 키와 시크릿 키를 설정한다.  
  
액세스 키와 시크릿 키를 생성하지 않았다면 'API와 툴 사용을 위한 액세스 키 생성하기'를 참조하여  
액세스 키와 시크릿 키를 생성한다.  
- AWS Access Key ID [None] : 액세스 키를 입력한다. 
- AWS Secret Access Key [None] : 시크릿 키를 입력한다. 
- Default Region name [None] : Tokyo 리전을 뜻하는 ap-northeast-1을 입력한다.
- Default output format [None] : 비워둔다(json, text table을 사용할 수 있다.)
![image](https://user-images.githubusercontent.com/33191974/138406040-c91bb40a-513b-4ff4-a82e-787bc14cffc7.png)  

이제 AWS CLI를 사용할 수 있게 되었다. 터미널에 다음과 같이 입력하면 aws cloudwatch 명령으로  
커스텀 측정치를 생성할 수 있다.  
- put-metric-data: 측정치를 보내겠다는 명령이다.
- --namespace : 측정치 대분류다. 이 분류 안에 측정치들이 포함된다.
- --metric-name : 측정치 이름이다.
- --value : 측정치 값이다. 숫자로만 입력해야 하며 소수점을 사용할 수 있다. 
![image](https://user-images.githubusercontent.com/33191974/138406621-91b41b27-4b2f-4f5f-b11d-02a779761654.png)

CloudWatch 페이지의 왼쪽을 보면 Custom Metrics... 콤보 박스가 생긴 것을 볼 수 있다.  
방금 생성한 namespace인 Hello를 선택하면 측정치 목록이 출력된다.  
그리고 World 측정치를 선택하면 그래프에 측정치 10이 표시된 것을 확인할 수 있다.  

![image](https://user-images.githubusercontent.com/33191974/138408388-cefa3ae7-ba9a-4200-8e31-74b6588e4e96.png)
예제에서는 이 명령을 손으로 직접 입력했지만 실무에서 활용하는 방법은 다음과 같다.  
- Linux의 cron에 등록하여 5분 혹은 1분 마다 aws cloudwatch 명령 실행
- Windows의 작업 스케줄러에 등록하여 5분, 혹은 1분마다 aws cloudwatch 명령 실행
- Node.js에서 child_process_exec를 사용하여 aws cloudwatch 명령 실행 또는 AWS JavaScript SDK 활용  
- 기타 어떠한 방법이든 aws cloudwatch 명령을 실행하면 된다.  

이제 Custom Metric을 이용하여 이메일 알림(Notification), 자동 횡적 확장등의 액션을 사용할 수 있다.  



















































