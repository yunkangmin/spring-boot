AWS의 Certificate Manager로 생성한 인증서는 무료이다.   
CloudFront 배포 설정에서 인증서를 설정해야 하는데 없다면 아래와 같이 한다.  
<img src="https://user-images.githubusercontent.com/33191974/159256203-73b093a5-4452-4ce5-8ccd-bcce0342b9aa.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/159256258-b3336fa1-5007-4caa-80a4-b1381ecb9315.png" width="50%" height="50%"/>   
아래와 같이 `*.aaa.com` 식으로 등록하면 `*`로 인해 하위 도메인들이 모두 사용할  
수 있는 인증서가 된다(www.aaa.com, admin.aaa.com 등등이 하위 도메인이다).  
다만, aaa.com에선 사용할 수 없는 인증서이다보니 2차 도메인(aaa.com)과 3차  
도메인(`*.aaa.com`) 모두에서 사용하려면 아래와 같이 도메인 이름을 추가하면 된다.  
<img src="https://user-images.githubusercontent.com/33191974/159256398-01605bad-b37c-462c-8cca-ecb09484ec74.png" width="50%" height="50%"/>  
  
인증서 요청을 생성하면 검증 보류 상태가 된다.  
이는 DNS 검증이 진행되지 않았기 때문이다. 아래와 같이 Route 53에서 레코드   
생성 버튼을 클릭한 뒤 레코드 생성 버튼을 누른다.    
<img src="https://user-images.githubusercontent.com/33191974/159262535-1a36812a-4591-4333-8e9f-80003ab99493.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/159262660-577ae9c3-27fb-412d-b6f8-258f55728d68.png" width="50%" height="50%"/>   
  
Route 53에 가보면 CNAME이 생성된 것을 확인할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/159263027-7ef02893-ce74-4b4c-9636-d79eb48ff84d.png" width="50%" height="50%"/>  
  
최대 30분정도 기다리면 발급 완료를 확인할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/159263237-e1ded5e2-fbc3-43c0-b65d-76f4b747ce0d.png" width="50%" height="50%"/>   




































