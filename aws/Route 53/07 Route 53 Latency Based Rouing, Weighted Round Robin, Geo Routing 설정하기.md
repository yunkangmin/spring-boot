# 정리
예를 들어 미국 서부(오레곤) 리전 및 아시아 태평양(싱가포르) 리전에  
ELB 로드 밸런서가 있다고 가정하자. 각 로드 밸런서에 대해 지연 시간  
레코드를 생성했다. 다음은 런던에 있는 사요아가 브라우저에 도메인  
이름을 입력했을 때 발생하는 현상이다.   
1. DNS가 Route 53 네임서버로 쿼리를 라우팅한다.  
2. Route 53은 런던과 싱가포르 리전 간, 그리고 런던과 오레곤 리전 간의   
지연 시간에 대한 데이터를 참조한다.  
3. 런던 리전과 오레곤 리전 간의 지연 시간이 더 짧다면, Route 53은   
오레곤 로드 밸런서의 IP 주소로 쿼리에 응답한다. 런던 리전과 싱가포르   
리전 간의 지연 시간이 더 짧다면, Route 53은 싱가포르 로드 밸런서의  
IP 주소로 응답한다.   

따라서 지연 시간 기반 레코드 생성 시 EC2 인스턴스가 속한 리전을 선택  
하는 것이 좋다.  

---

Latency Based Routing, Weighted Round Robin, Geo Routing 기능을 사용하는  
방법은 어렵지 않다. 반복되는 내용을 모두 설명하기에는 분량이 많아지므로   
간단하게 설명한다.  
  
[Latency Based Routing](https://github.com/yunkangmin/spring-boot/blob/main/aws/Route%2053/01%20AWS%20%EB%A6%AC%EC%86%8C%EC%8A%A4%EC%99%80%20%EC%97%B0%EB%8F%99%20%EA%B0%80%EB%8A%A5%ED%95%9C%20DNS%20%EC%84%9C%EB%B9%84%EC%8A%A4%20Route%2053.md)을 설정하는 방법은 다음과 같다.   
- Name: 사용하고 싶은 서브 도메인을 설정한다(루트 도메인도 설정할 수 있다).  
- Type: A - IPv4 address를 선택한다.  
- Alias: EC2 인스턴스라면 No를 선택하고, ELB라면 Yes를 선택한다.  
   - Alias Target: Alias를 Yes로 선택했다면 목록에서 ELB 주소를 선택한다.   
- Value: EC2 인스턴스의 공인 IP 주소를 입력한다. ELB는 이 부분을 생략한다.   
- Routing Policy: Latency를 선택한다.   
- Region: Value에 입력된 IP 주소, Alias Target에서 선택한 ELB가 속한 리전이   
자동으로 선택된다.   
- Set ID: 같은 도메인의 Latency Based Routing A 레코드끼리 구분할 수 있도록   
ID를 설정한다(예) Tokyo Data Center, California Data Center).  
  
동일한 도메인(서브, 루트 도메인)으로 IP 주소(리전)을 다르게 설정해 Latency   
Based Routing A 레코드를 계속 생성한다.   
  
<img src="https://user-images.githubusercontent.com/33191974/159422582-d35fc5b4-5204-4276-91b6-20707ab02a2e.png" width="50%" height="50%"/>  
  
Weighted Round Robin을 설정하는 방법은 다음과 같다.  
- Name: 사용하고 싶은 서브 도메인을 설정한다(루트 도메인도 설정할 수 있다).    
- Type: A -IPv4 address를 선택한다. Weighted Round Robin은 CNAME 레코드를   
사용할 수 없다.   
- Alias: EC2 인스턴스라면 No를 선택하고, ELB라면 Yes를 선택한다.   
   - Alias Target Alias를 Yes로 선택했다면 목록에서 ELB 주소를 선택한다.  
- Value: EC2 인스턴스의 공인 IP 주소를 입력한다.  
- Routing Policy: Weighted를 선택한다.  
- Weight: 가중치를 설정한다. 0부터 255까지 설정할 수 있다. 각 레코드별  
가중치 계산은 (설정한 가중치)/(전체 가중치 합계)이다.  
- Set ID: 같은 도메인의 Weighted Round Robin A 레코드끼리 구분할 수  
있도록 ID를 설정한다(예) Tokyo Data Center, California Data Center).  
  
동일한 도메인(서브, 루트 도메인)으로 가중치(Weight) 값을 다르게 설정해  
Weighted Round Robin A 레코드를 계속 생성한다.  
<img src="https://user-images.githubusercontent.com/33191974/159424065-831bc2ec-6db1-4b87-8342-a570b4d33c86.png" width="50%" height="50%"/>   

Geo Routing을 설정하는 방법은 다음과 같다.  
- Name: 사용하고 싶은 서브 도메인을 설정한다(루트 도메인도 설정할 수  
있다).    
- Type: A -IPv4 address를 선택한다.   
- Alias: EC2 인스턴스라면 No를 선택하고, ELB라면 Yes를 선택한다.   
   - Alias Target: Alias를 Yes로 선택했다면 목록에서 ELB 주소를 선택한다.   
- Value: EC2 인스턴스의 공인 IP 주소를 입력한다. ELB는 이 부분을 생략한다.  
- Routing Policy: Geolocation을 선택한다.  
- Location: 지역 설정이며 국가별로 선택할 수 있다.  
- Sublocation: 미국은 주(State)를 선택할 수 있다.  
- Set ID: 같은 도메인 Geo Routing A 레코드끼리 구분할 수 있도록 ID를   
설정한다(예: Tokyo Data Center, California Data Center).  
  
동일한 도메인(서브, 루트 도메인)으로 지역을 다르게 설정해 Geo Routing  
A 레코드를 계속 생성한다.   
<img src="https://user-images.githubusercontent.com/33191974/159427656-7881b56c-04e6-4a35-846e-c7ecad259880.png" width="50%" height="50%"/>   




  































