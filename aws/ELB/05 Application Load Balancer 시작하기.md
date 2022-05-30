# 정리  
모든 스위치는 말그대로 'Switching', 즉 패킷을 어느쪽으로 보내야 하는 역할을    
해야한다. 문제는 그 어느 쪽으로 보낼 때 어떠한 정보를 보고 판단을 하는 것인    
지에 따라 Layer가 구분된다.    
1. L2 스위치는 MAC 정보(MAC Table)를 보고 스위칭을 하는 것  
2. L3 스위치는 IP 정보(Routing Table)를 보고 스위칭을 하는 것  
3. L4 스위치는 IP + Port(Session or Connection)를 보고 스위칭을 하는 것  
4. L7 스위치는 실제 App 데이터(Content)를 보고 스위칭을 하는 것  
그리고 중요한 것은 상위 레이어 스위치는 하위 레이어 스위치의 기능을 기본적    
으로 다 할 수 있는 것. 다만, 실제 사이트 구성 시에는 자신의 주력 기능만 수행    
하는 것이 일반적이다.    

# Application Load Balancer란
Elastic Load Balancing은 둘 이상의 가용영역에서 EC2 인스턴스, 컨테이너, IP  
주소 등 여러 대상에 걸쳐 수신되는 트래픽을 자동으로 분산한다. 등록된 대상의  
상태를 모니터링하면서 상태가 양호한 대상으로만 트래픽을 라우팅(데이터를   
출발지부터 목적지까지 경로를 찾아가게 하는 것)한다.    

# Application Load Balancer 구성 요소   
로드 밸런서는 클라이언트에 대한 단일 접점 역할을 수행한다. 로드 밸런서는  
여러 가용 영역에서 EC2 인스턴스 같은 여러 대상에 수신 어플리케이션 트래픽을   
분산한다. 이렇게 하면 애플리케이션의 가용성이 향상된다. 로드밸런서에 하나  
이상의 리스너를 추가할 수 있다.    
  
리스너는 구성한 프로토콜 및 포트를 사용하여 클라이언트의 연결 요청을 확인  
한다. 리스너에 대해 정의한 규칙에 따라 로드 밸런서가 등록된 대상으로 요청을   
라우팅하는 방법이 결정된다. 각 규칙은 우선 순위, 하나 이상의 작업, 하나  
이상의 조건으로 구성된다. 규칙에 대한 조건이 충족되면 작업이 수행된다. 각  
리스너에 대한 기본 규칙을 정의해야 하며, 필요에 따라 추가 규칙을 정의할 수  
있다.  
  
각 대상 그룹은 지정한 프로토콜과 포트 번호를 사용하여 EC2 인스턴스 같은 하나  
이상의 등록된 대상으로 요청을 라우팅한다. 여러 대상 그룹에 대상을 등록할 수  
있다. 대상 그룹 기준으로 상태 확인을 구성할 수 있다. 로드 밸런서의 리스너  
규칙에서 지정한 대상 그룹에 등록된 모든 대상에서 상태 검사가 수행된다.     
  
다음 다이어그램은 기본 구성 요소를 보여 준다. 각 리스너에는 기본 규칙이 포  
함되어 있고 하나의 리스너에는 요청을 다른 대상 그룹으로 라우팅하는 다른  
규칙이 포함되어 있다. 하나의 대상은 두 개의 대상 그룹에 등록된다.   
<img src="https://user-images.githubusercontent.com/33191974/159907948-4238e2d1-89ef-496e-83a1-a42f6b62c7b9.png" width="50%" height="50%"/>  

---

# 1. 생성화면 찾아가기
EC2 서비스 좌측 메뉴들에 로드밸런서에서 로드밸런서를 생성할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/159894406-31cef9bd-0994-40ec-9aa4-b65f001d8197.png" width="50%" height="50%"/>    
  
# 2. 로드 밸런서 생성하기   
<img src="https://user-images.githubusercontent.com/33191974/159894735-f6125588-d6a6-4805-bfd3-1c9c907792eb.png" width="50%" height="50%"/>  

# 3. 로드밸런서 설정하기
1. 로드 밸런서 이름(Load Balancer name)에 로드 밸런서의 이름을 입력한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159902010-9dd0c075-a2ea-4afe-a67f-eb2a45b3deb0.png" width="50%" height="50%"/>  
  
2. [Scheme] 및 [IP address type]은 기본값으로 유지한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159902101-4454afa8-5646-42be-b18b-acd91141b6ce.png" width="50%" height="50%"/>   

3. 가용영역(Availability Zones)에서 EC2 인스턴스에 사용한 VPC를 선택한다.  
2개 이상의 가용영역과 영역당 1개의 서브넷을 선택한다. EC2 인스턴스를 시작할   
때 사용한 각 가용 영역에서 가용영역을 선택한 후 해당 가용 영역에 대한 하나의   
퍼블릭 서브넷을 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/159902245-80e0d96b-35e6-4bd7-bada-005a6477ef50.png" width="50%" height="50%"/>  
  <img src="https://user-images.githubusercontent.com/33191974/159902339-8778ed66-09f0-4aa3-b826-c21de60f90a2.png" width="50%" height="50%"/>  
  
4. 보안그룹(Security groups)은 22, 80번 포트를 열어둔 아래 보안 그룹을  
선택한다. 해당 포트와 프로토콜에 대해서만 트래픽을 허용한다는 의미이다.    
  <img src="https://user-images.githubusercontent.com/33191974/160231022-fc13b2cf-0664-4509-b1d3-23cf580e715f.png" width="50%" height="50%"/>     
    
5. 포트 80에서 HTTP 트래픽을 수락하는 리스너를 뜻하는 리스너 및 라우팅은  
기본값으로 유지한다. 이 자습서의 경우 HTTPS 리스너를 생성하지 않는다.   
  <img src="https://user-images.githubusercontent.com/33191974/159902593-8451f888-1dd7-4c40-9832-c99780f71b43.png" width="50%" height="50%"/>   
  
6. 기본 작업(Default action)에 대해 아래 대상그룹 구성을 참고하자. 
  <img src="https://user-images.githubusercontent.com/33191974/159905420-07a8b758-9b03-46fc-96b8-c49eff8ef6bb.png" width="50%" height="50%"/>  

7. (선택사항) 태그를 추가하여 로드 밸런서를 분류한다. 태그 키는 각 로드   
밸런서에 대해 고유해야 한다. 허용되는 문자는 문자, 공백, 숫자(UTF-8 형식)  
및 특수문자 `+-=._:/@`이다. 선행 또는 후행 공백을 사용하면 안된다. 태그  
값은 대소문자를 구분한다.   
8. 구성을 검토하고 로드 밸런서 생성(Create load balancer)을 선택한다.  
생성 중에 로드 밸런서에 몇 가지 기본 특성이 적용된다. 로드 밸런서를 생성한  
후 이를 보고 편집할 수 있다.   

# 대상 그룹 구성
라우팅 요청에서 사용되는 대상 그룹을 만든다. 리스너의 기본 규칙은 이 대상  
그룹에 등록된 대상에 대해 요청을 라우팅한다. 로드 밸런서는 해당 대상 그룹에  
대해 정의된 상태 확인 설정을 사용하여 이 대상 그룹의 대상 상태를 확인한다.   
  
#### 대상 그룹을 구성하려면
1. 기본구성(Basic configuration) 아래에서 대상 유형(Target type)을 인스턴  
스로 유지한다.   
  <img src="https://user-images.githubusercontent.com/33191974/159904502-29156306-f9cb-4728-bab9-22be4386cebb.png" width="50%" height="50%"/>  

2. 대상 그룹 이름(Target group name)에 새로운 대상 그룹의 이름을 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/159905022-8ee0b68b-5244-418a-8ea2-17e7cbe804e6.png" width="50%" height="50%"/>  

3. 기본 프로토콜(HTTP) 및 포트(80)를 유지한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159904665-b8508d4f-3844-4bf3-862e-5809ae9c4a56.png" width="50%" height="50%"/>  

4. 사용자의 인스턴스를 포함하는 VPC를 선택한다. 프로토콜 버전을 HTTP1로   
유지한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159904769-2f15f290-e110-4b40-8bc2-36b6e6b192e6.png" width="50%" height="50%"/>  

5. Health checks(상태 확인)에는 기본 설정을 그대로 둔다.  
  <img src="https://user-images.githubusercontent.com/33191974/159904871-9bdf9134-9d0e-4aba-a5a6-bc97b42d230e.png" width="50%" height="50%"/>
  
7. [next]를 선택한다.   
8. 대상 등록(Register Targets) 페이지에서 다음 단계를 완료한다(인스턴스가  
없으면 인스턴스 등록없이 바로 Create target group을 눌러도 됨). 로드 밸런서  
를 생성하기 위한 선택적 단계이다. 그러나 로드 밸런서를 테스트하고 대상으로  
트래픽을 라우팅하고 있는지 확인하려면 대상을 등록해야 한다.  
   1. 사용 가능한 인스턴스(Available instance)에서 인스턴스를 하나 이상  
   선택한다.  
   2. 기본 포트 80을 유지하고 아래에서 보류중인 것으로 포함(Includes as   
   pending below)을 선택한다.  
8. [Create target group]을 선택한다.   


# 로드 밸런서 테스트
로드 밸런서를 생성한 후에는 EC2 인스턴스에 트래픽을 전송하고 있는지 확인할  
수 있다.   
  
#### 로드 밸런서를 테스트하려면  
1. 로드 밸런서가 생성되었다는 통보를 받은 후 [Close]를 선택한다.  
2. 탐색창의 Load Balancing 아래에서 Target Groups를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159908728-9aa66e74-674f-48b2-a93f-e842e7d67b95.png" width="50%" height="50%"/>  
  
3. 새로 생성한 대상 그룹을 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159909005-9c9e11be-a6c5-4655-b63b-adda9b525734.png" width="50%" height="50%"/>   

4. [Target]를 선택하고 인스턴스가 준비되었는지 확인한다. 인스턴스 상태가  
initial인 경우 아직 인스턴스 등록이 진행중이거나 정상으로 간주될 만한 최소   
상태 확인 횟수를 통과하지 못했기 때문일 가능성이 높다. 하나 이상의 인스  
턴스 상태가 healthy여야 로드 밸런서를 테스트할 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/159909463-1760bcc2-abbe-4239-806e-5dbfb425933d.png" width="50%" height="50%"/>    
  
5. 탐색 창의 Load Balancing에서 로드 밸런서를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/159909687-abbf58c4-980c-41ec-b344-07874eb5d669.png" width="50%" height="50%"/>   

6. 새로 생성한 로드 밸런서를 선택한다.  
7. [설명(Description)]을 선택하고, 로드 밸런서의 DNS 이름(예) exampletic  
ket-1297194435.ap-northeast-2.elb.amazonaws.com)을 복사한다. DNS 이름을  
인터넷에 연결된 웹 브라우저의 주소 필드에 붙여넣는다. 모든 것이 잘 작동  
하는 경우 브라우저에 서버 기본 페이지가 표시된다.   
8. (선택 사항) 추가 리스너를 정의하려면 [규칙 추가](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/application/listener-update-rules.html#add-rule) 단원을 참조하자.    































