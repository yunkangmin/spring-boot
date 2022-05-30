# 정리
1. S3 2개 생성
   1. slkjfd.cf(최종적으로 브라우저에 보여질 버킷, DNS 주소는 http://slkj  
   fd.cf.s3-website.ap-northeast-2.amazonaws.com)  
      - index.html 업로드
   2. www.slkjfd.cf
      - www.slkjfd.cf 버킷주소(예) http://www.slkjfd.cf.s3-website.ap-nort  
      heast-2.amazonaws.com)로 접속시 slkjfd.cf 주소로 리다이렉트  
2. Route 53 A 레코드와 CNAME 레코드 생성
   1. A 레코드
      - slkjfd.cf로 접속시 slkjfd.cf 버킷(예) http://www.slkjfd.cf.s3-website  
     .ap-northeast-2.amazonaws.com)으로 접속되게 설정  
   2. CNAME
      - www.slkjfd.cf로 접속 시 www.slkjfd.cf 버킷(예) http://www.slkjfd.cf.s3-  
      website.ap-northeast-2.amazonaws.com)으로 접속되게 설정
3. 프로세스
   1. www.slkjfd.cf로 접속
      1. CNAME에 의해 www.slkjfd.cf 버킷(예) http://www.slkjfd.cf.s3-web  
      site.ap-northeast-2.amazonaws.com)으로 접속됨   
      2. www.slkjfd.cf 버킷에서 slkjfd.cf로 리다이렉팅  
      3. slkjfd.cf로 접속
      4. A 레코드에 의해 slkjfd.cf 버킷(http://slkjfd.cf.s3-website.ap-north  
      east-2.amazonaws.com)으로 접속  
      5. 최종적으로 slkjfd.cf 버킷에 있는 index.html 파일이 브라우저에  
      표시됨.  
   2. slkjfd.cf로 접속
      1. A 레코드에 의해 slkjfd.cf 버킷(http://slkjfd.cf.s3-website.ap-north  
      east-2.amazonaws.com)으로 접속     
      2. 최종적으로 slkjfd.cf 버킷에 있는 index.html 파일이 브라우저에  
      표시됨.    

- A 레코드 별칭과 CNAME 레코드의 비교  
별칭 레코드는 CNAME 레코드와 비슷하지만, 다음과 같은 중요한 차이점이   
몇 가지 있다.   
   - 쿼리를 리디렉션할 수 있는 리소스
      - 별칭 레코드
      별칭 레코드는 쿼리를 다음과 같이 선택한 **AWS 리소스**로만 리디  
      렉션할 수 있다.
         - Amazon S3 버킷  
         - CloudFront 배포
         -  동일한 Route 53 호스팅 영역의 다른 레코드  
         -  등등
      예를 들어 acme.example.com이라는 이름의 Amazon S3 버킷으로 쿼리를  
      리디렉션하는 acme.example.com이라는 별칭 레코드를 생성할 수 있다.  
      example.com 호스팅 영역의 zenith.example.com 레코드로 쿼리를  
      리디렉션하는 acme.example.com 별칭 레코드를 생성할 수도 있다.   
      - CNAME 레코드   
      CNAME 레코드는 DNS 쿼리를 DNS 레코드로 리디렉션할 수 있다.  
      예를 들어 acme.example.com에서 zenith.example.com 또는 acme.   
      example.org로 쿼리를 리디렉션하는 CNAME 레코드를 생성할 수 있다.   
      쿼리를 리디렉션할 도메인의 DNS 서비스로 Route 53을 사용할 필요가   
      없다.  
  
- A레코드에서 별칭은 도메인 주소가 아닌 AWS 리소스를 설정한 것이고  
CNAME에서는 도메인 주소를 설정했다.   

---

S3는 정적 웹사이트 호스팅 기능이 있다. 정적 웹 사이트 호스팅 기능을 사용하는   
S3 버킷을 Route 53을 통해 도메인과 연결할 수 있다. Route 53과 연동하기   
위해서는 S3 버킷을 두 개 생성해야 한다. S3 메인 페이지로 이동하고, 위쪽   
Create Bucket 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159209255-0a454838-c39e-4113-9290-ba40145f3d82.png" width="50%" height="50%"/>    
  
Route 53과 연동할 첫 번째 S3 버킷을 생성한다.  
- Bucket Name: Route 53과 연동할 때는 버킷 이름을 도메인 이름으로 설정해야 한다.   
여기서 www 서브 도메인을 제외한 루트 도메인을 입력한다. 필자는 slkjfd.cf를 사용한다.  
각자 구입한 도메인 이름을 입력한다.   
- Region: 리전을 설정한다. 서울리전을 선택한다.  

설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159209484-d93f8d9b-846a-40e2-8add-61484de2d8e7.png" width="50%" height="50%"/>   
  
이후부터는 [여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/46%20S3%20%EC%A0%95%EC%A0%81%20%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8%20%ED%98%B8%EC%8A%A4%ED%8C%85%20%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0.md)를 참고하여 버킷을 생성하자.  
위의 글에서 버킷에 파일업로드에서는 아래 파일을 사용하자. 메모장이나 기타  
텍스트 편집기를 열고 다음과 같이 작성한 뒤 index.html로 저장한다.     
    
index.html   
```
<html>
<head>
  <title>Example S3</title>
</head>
<body>
  <p>Hello S3 - Route 53</p>
</body>
</html>
```
  
Route 53과 연동할 두 번째 S3 버킷을 생성하자.  
- Bukcet Name: www 서브 도메인에 연결할 S3 버킷을 생성한다. 필자는 slkjfd.cf  
를 사용한다. 각자 구입한 도메인 이름 앞에 wwww.를 붙여서 입력한다.  
- Region: 리전을 설정한다. 서울리전을 선택한다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159211504-dea42acf-062e-4daf-a5e9-ecff44cf91b2.png" width="50%" height="50%"/>   
  
S3 버킷 목록에서 www가 붙은 S3 버킷을 선택하고 위쪽의 Properties 버튼을  
클릭한다. 그리고 Static Website Hosting 탭을 클릭한 뒤 Redirect all request  
to에 여러분들이 구입한 도메인 이름을 입력한다(www.제외).  

앞에서 도메인 이름으로 된 S3 버킷을 생성하고, www.가 붙은 S3 버킷을 생성  
했다면 Redirect all request to에 도메인 이름이 이미 설정되어 있을 것이다.  
설정이 완료되었다면 Save 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159212090-c672f308-9b9a-4564-a3bc-f329cd4cf437.png" width="50%" height="50%"/>  
  
www.가 붙은 S3 버킷으로 접속하는 모든 트래픽은 루트 도메인으로 이동하도록    
설정했다. 필자는 www.slkjfd.cf을 입력하면 slkjfd.cf로 이동하게 설정했다.    
www.가 붙은 S3 버킷에는 데이터를 올리지 않아도 된다.   
  
Route 53 메인 페이지로 이동한다. 도메인을 선택하고 위쪽 Go to Record Sets  
버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159212540-93b66c2f-7c30-44d5-9ce4-8cbf05ccbca5.png" width="50%" height="50%"/>    
도메인의 레코드 목록이 표시된다. 위쪽 Create Record Set 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159212600-616b3a0b-d0af-4892-9f47-8a34ca1cc7e2.png" width="50%" height="50%"/>    
  
루트 도메인 A 레코드를 생성한다.  
- Name: 루트 도메인 A 레코드를 생성할 것이므로 아무것도 입력하지 않는다.   
- Type: 레코드 종류를 설정한다. 기본값 그대로 A - IPv4 address를 선택한다.   
- Alias: Yes를 선택하여 IP 주소 대신 AWS 리소스를 설정한다.   
- Alias Target: AWS 리소스의 주소를 설정한다. Alias Target 입력 부분을  
클릭하면 사용할 수 있는 AWS 리소스(S3, ELB, CloudFront)의 목록이 표시된다.   
S3 Website Endpoint의 `<도메인>`(s3-website-ap-northeast-2)를 선택한다.   
- Alias Hosted Zone ID: Alias Target을 선택하면 자동으로 설정된다.   
- Routing Policy: 라우팅 정책을 설정한다. 기본값 그대로 Simple을 선택한다.  
   - Simiple: 아무런 부가 기능없이 IP 주소만 알려준다.   
   - Weighted: Weighted Round Robin 기능을 사용한다.   
   - Latency: Latency Based Routing 기능을 사용한다.  
   - Failover: DNS Failover 기능을 사용한다.  
- Evaluate Target Health: 서버 동작 상태 체크(Health Check)를 사용할지   
설정한다. 기본값 그대로 사용한다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.  

> #### Zone Apex 지원   
> 루트 도메인을 IP 주소가 아닌 S3 Endpoint와 같은 URL로 연결하는 것이 Zone  
> Apex(최상위 도메인, TLD) 지원 기능이다.   
  
<img src="https://user-images.githubusercontent.com/33191974/159214987-2c115e2d-f4e3-41da-8704-92d748d1c233.png" width="50%" height="50%"/>    
  
도메인의 레코드 목록에 A 레코드가 추가되었다. 다시 Create Record Set 버튼을   
클릭하여 www 서브 도메인에 대한 CNAME 레코드를 생성하자.   
<img src="https://user-images.githubusercontent.com/33191974/159213940-482e0e8e-ee26-4826-b608-2d6557f99907.png" width="50%" height="50%"/>   
  
www 서브 도메인에 대한 CNAME 레코드를 생성하자.  
- Name: www 서브 도메인을 생성할 것이므로 www를 입력한다.   
- Type: 레코드 종류를 설정한다. CNAME - Canonical name을 선택한다. CNAME은    
IP 주소 대신 도메인을 연결하는 기능이다.     
- Alias: A 레코드만 사용할 수 있는 기능이므로 기본값 그대로 No를 선택한다.  
- TTL: Time To Live의 약자이며 A 레코드가 갱신되는 주기를 설정한다. 초 단위로   
설정한다. 이후 CNAME 레코드의 도메인을 바꾸면 TTL에 설정한 시간이 지나야  
적용된다.  
- Value: 연결할 도메인을 입력한다. www.가 붙은 S3 버킷의 Endpoint를 입력한다.   
(http://제외, Endpoint는 www.가 붙은 S3 버킷의 Properties에서 Static Web   
site Hosting 탭에 나와있다).  
- Alis Target: AWS 리소스의 주소를 설정한다. Alias Target 입력 부분을 클릭  
하면 사용할 수 있는 AWS 리소스(S3, ELB, CloudFront)의 목록이 표시된다.   
S3 Website Endpoint의 `<도메인>`(s3-website-ap-northeast-2)를 선택한다. 
- Alias Hosted Zone ID: Alias Target을 선택하면 자동으로 설정된다.  
- Routing Policy: 라우팅 정책을 설정한다. 기본값 그대로 Simple을 선택한다.   
   - Simple: 아무런 부가 기능없이 IP 주소만 알려준다.   
   - Weighted: Weighted Round Robin 기능을 사용한다.   
   - Latency: Latency Based Routing 기능을 사용한다.  
   - Failover: DNS Failover 기능을 사용한다.   

설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159215814-ba312476-ff65-483b-a266-24ab60fca868.png" width="50%" height="50%"/>   
  
도메인의 레코드 목록에 CNAME 레코드가 추가되었다.  
<img src="https://user-images.githubusercontent.com/33191974/159215907-c1f78753-70e4-42bf-89b2-afec23a49a40.png" width="50%" height="50%"/>  
  
웹 브라우저에서 A 레코드로 설정한 도메인(slkjfd.cf)과 CNAME으로 생성한   
www 서브 도메인(www.slkjfd.cf)에 접속한다.   

<img src="https://user-images.githubusercontent.com/33191974/159216150-042b4075-47ea-4ee3-86af-2b2e7e3f6f28.png" width="50%" height="50%"/>   

  
Route 53을 사용하기 전에 루트 도메인과 www 서브 도메인의 A 레코드 TTL을   
길게(예: 4시간) 설정했다면 앞에서 설정한 내용이 바로 적용되지 않을 수 있다.  
TTL에 설정한 시간이 지난 후에 다시 확인해보자.   















  
  
---

S3 버킷 목록에서 방금 생성한 S3 버킷을 선택하고, 위쪽의 Properties 버튼을   
클릭한다. 그리고 Permissions 탭을 클릭한 뒤 Add Bucket policy 버튼을   
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159209641-637efaa7-1c3c-46ff-9b57-e242b43a3c2e.png" width="50%" height="50%"/>  
  
버킷 정책 편집기 화면이 나온다. 이 곳에 아래 JSON 텍스트를 입력한다.   
인터넷에 버킷의 모든 파일을 공개하는 정책은 AWS Policy Generator에서 생성할   
수 있다. AWS Policy Generator에 관련된 것은 'S3 버킷 권한 관리하기'를    
참조하자.   
```
{
  "Id": "Policy1647841128676",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1647841126556",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::slkjfd.cf/*",
      "Principal": "*"
    }
  ]
}
```
























