Signed URL을 생성하려면 먼저 CloudFront 전용 키 쌍과 액세스 키를 생성해야 한다.   
이 키쌍의 개인 키를 사용하여 정책을 서명하게 된다. 정책을 서명하는 이유는 정책이  
변조되었는지 검증하기 위해서이다. 만약 Signed URL에 설정된 정책 내용과 서명   
데이터(Signature)가 맞지 않으면 Signed URL은 동작하지 않는다.   
  
AWS 콘솔에서 위쪽 자신의 이름 부분을 클릭하면 메뉴가 나온다. 여기서 Security   
Credentials를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156708845-de3f991f-37d3-4536-91a5-f3cbb79a96c9.png" width="50%" height="50%"/>    
Security Credentials 경고창이 표시된다. Continue to Security Credentials 버튼을   
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156709171-5989fb8d-cf61-4b6f-a17b-56e1abab249e.png" width="50%" height="50%"/>    
버튼 클릭 즉시 CloudFront 키 쌍이 생성된다. Download Private Key File,   
Download Public Key File 버튼을 각각 클릭하여 개인 키와 공개 키 파일을 다운  
로드한다.   

> Download Private Key File, Download Public Key File 버튼을 클릭하여 파일을  
> 받지 않고 그냥 Close 버튼을 클릭하여 창을 닫아버릴 경우, 두 번 다시 개인 키와  
> 공개 키 파일을 받을 수 없게 된다. 따라서 생성된 직후 파일들을 꼭 받아두어야  
> 한다. 그냥 닫아버렸을 경우, 혹은 키 파일들(개인키, 공개키)을 분실하였을 경우  
> 이 CloudFront 액세스 키는 사용할 방버이 없으므로 폐기해야 하고, 새로 액세스  
> 키를 생성해야 한다.   

생성된 키 목록이 표시된다. Actions의 Make inactive는 액세스 키를 사용하지   
못하도록 비활성화 하는 것이고, 다시 사용하기 위해 활성화(Active)할 수 있다.   
Delete는 액세스 키를 완전히 사용할 수 없도록 삭제한다.   
<img src="https://user-images.githubusercontent.com/33191974/156709669-0e3661fa-1198-4a09-9b64-6ab538a8b181.png" width="50%" height="50%"/>     
다운로드한 개인키와 공개키 파일의 이름은 다음과 같은 형식이다.  
- 개인키 : pk-<액세스키 ID>.pem (pk-APKAJERVTR4A7EO47UYA.pem)  
- 공개키 : rsa-<액세스키ID>.pem (rsa-APKAJERVTR4A7EO47UYA.pem)  
이제 Signed URL을 생성할 때 액세스 키와 개인키 준비가 끝났다.  
































