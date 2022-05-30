# 정리
- CloudFront 배포를 Route 53을 통해 도메인과 연결 
1. S3 버킷 생성
2. CloudFront 배포 생성
   1. CNAME 설정
      1. cf.sljkfd.cf로 도메인 설정
      2. SSL 인증서 설정(cf.slkjfd.cf가 안전하다는 것)
3. Route 53에서 A레코드 생성
   1. cf.slkjfd.cf로 레코드 이름 설정
   2. 트래픽 라우팅 대상에 위에서 생성한 CloudFront 설정
4. cf.slkjfd.cf 접속 확인

- 왜 CloudFront에서는 CloudFront 배포에서 따로 CNAME을 설정할까?  
CloudFront에서 CNAME이라고 하는 대체 도메인 이름을 사용하면   
파일 URL에 CloudFront에서 배포에 배정하는 도메인 이름 대신에 고유의  
도메인 이름(예: www.example.com)이 사용된다. 
  
- 호스팅 영역: 레코드의 컨테이너(집합)이며, 레코드에는 특정 도메인(예:  
example.com)과 그 하위 도메인(acme.example.com, zenith.example..com)  
의 트래픽을 라우팅(경로를 찾아가는 과정)하는 방식에 대한 정보가 포함된  
다. 호스팅 영역의 이름과 도메인의 이름은 동일하다. 
  
---

CloudFront 배포(Distribution)를 Route 53을 통해 도메인과 연결할 수 있다.   
'전 세계에 콘텐츠를 배포하는 CDN 서비스인 CloudFront'를 참조하여 CloudFront  
배포를 먼저 생성하기 바란다. Route 53와 연동할 CloudFront 배포를 선택하고   
위쪽 Distribution Settings 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159254786-8719830b-dd99-4d47-90bc-a718d64fea25.png" width="50%" height="50%"/>  
  
CloudFront 배포 설정에서 General 탭에서 Edit 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159255226-232b5e5a-4820-4be3-a831-e99a155b1d0d.png" width="50%" height="50%"/>  
  
CloudFront 배포 설정에서 Alternate Domain Names 부분에 연결하고자 하는 도메인을   
입력한다. 필자는 cf.가 붙은 서브 도메인을 입력했다. 설정 내용이 많기 때문에   
스크롤을 쭉 내려 Yes, Edit 버튼을 클릭한다.  
인증서가 없다면 [여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/Route%2053/etc/AWS%EC%9D%98%20Certificate%20Manager%EB%A1%9C%20SSL%20%EC%9D%B8%EC%A6%9D%EC%84%9C%20%EB%B0%9C%EA%B8%89%EB%B0%9B%EA%B8%B0.md)를 참고하여 생성하자.   
<img src="https://user-images.githubusercontent.com/33191974/159255414-20eb3c01-3273-4fc5-a259-89b5900837d2.png" width="50%" height="50%"/>  
  
> #### 루트 도메인과 www 서브 도메인  
> CloudFront 배포를 루트 도메인과 www 서브 도메인에 모두 연결하려면 도메인   
> 두 가지 모두 입력한다.   
> 예) example.com  
> www.example.com  

CloudFront 배포 설정의 Alternate Domain Names 부분이 변경되었다. CloudFront   
Distributions를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159263814-4372e711-1f19-4ff0-ad38-2e54701eb53c.png" width="50%" height="50%"/>   
  
방금 설정을 변경한 CloudFront 배포의 Status를 보면 InProgress로 표시되며   
인디케이터가 회전하는 것을 확인할 수 있다. 설정이 모든 에지 로케이션에   
전파되기까지 약 15 ~ 20분이 소요된다.  
  
Route 53 메인 페이지로 이동한다. 도메인을 선택하고 위쪽 Go to Record Sets  
버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159264217-b0720a8e-266e-4de1-a85f-05f36fc2d220.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/159264255-81e0a8af-aba8-4c3b-a790-19348276afe5.png" width="50%" height="50%"/>    
cf 서브 도메인에 대한 A 레코드를 생성한다.   
- Name: cf 서브 도메인에 대한 A 레코드를 생성할 것이므로 cf를 입력한다.   
- Type: 레코드 종류를 설정한다. 기본값 그대로 A - IPv4 address를 선택한다.   
- Alias: Yes를 선택하여 IP 주소 대신 AWS 리소스를 설정한다.   
- Alias Target: AWS 리소스의 주소를 설정한다. Alias Target 입력 부분을   
클릭하면 사용할 수 있는 AWS 리소스(S3, ELB, CloudFront)의 목록이 표시된다.  
CloudFront Distributions의 `cf.<도메인>` (xxxxxxxx.cloudfront.net)을 선택한다.   
- Alias Hosted Zone ID: Alias Target을 선택하면 자동으로 설정된다.  
- Routing Policy: 라우팅 정책을 설정한다. 기본값 그대로 Simple을 선택한다.  
- Evaluate Target Health: CloudFront는 이 기능을 사용할 수 없으므로 기본값   
그대로 사용한다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159264780-a4869387-052e-415a-b17c-5cf11aa9d69e.png" width="50%" height="50%"/>      

도메인의 레코드 목록에 A 레코드가 추가되었다.   
<img src="https://user-images.githubusercontent.com/33191974/159265390-1424b917-95ae-402f-9ac8-85f2fd642505.png" width="50%" height="50%"/>   

웹 브라우저에서 A 레코드로 설정한 도메인에 접속한다. CloudFront 배포의   
웹 페이지가 표시된다.  
<img src="https://user-images.githubusercontent.com/33191974/159265621-1613749d-7912-4c47-811a-65b9cee7fbc8.png" width="50%" height="50%"/>   
  
> #### 루트 도메인과 www 서브 도메인  
> CloudFront 배포를 루트 도메인과 www 서브 도메인으로 연결하려면 A 레코드를   
> Alias로 설정해 루트 도메인과 www 서브 도메인을 추가하면 된다.   


                                                                                                                                        





























