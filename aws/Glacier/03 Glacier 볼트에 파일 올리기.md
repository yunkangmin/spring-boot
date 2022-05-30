Glacier는 아직 AWS 콘솔에서 파일을 업로드할 수 없다. 그래서 무료 Glacier 클라이언트인 FastGlacier를  
이용해서 파일을 업로드하고, 반출 요청을 하고, 다운로드해본다.  
  
> #### Mac OS X용 Glacier 클라이언트   
> FastGlacier는 Window용 프로그램이다. 구글에 Glacier client mac으로 검색하면 다양한 제품들이 나오니   
> 각자 제품을 선택하기 바란다. 전용 클라이언트뿐만 아니라 요즘 나오는 FTP 클라이언트들은 S3와 Glacier  
> 까지 지원한다(CrossFTP).   

http://fastglacier.com에서 FastGlacier를 다운로드한다.   
<img src="https://user-images.githubusercontent.com/33191974/158053723-2074e160-998d-40a8-8d86-7f50f136f866.png" width="50%" height="50%"/>   
FastGlacier를 설치한다. 설치 방법은 특별한 것이 없으므로 따로 설명하지 않는다. FastGlacier를 실행하면   
계정 추가 화면이 나온다. 여기서 액세스 키와 시크릿 키를 입력한다. 액세스 키와 시크릿 키를 생성하는  
방법은 'API와 툴 사용을 위한 액세스 키 생성하기'를 참조하기 바란다. 
- Account Name: FastGlacier 내에서 계정을 구분할 수 있는 이름을 입력한다.   
- Access Key ID: AWS 계정의 액세스 키를 입력한다. IAM 사용자의 액세스 키도 사용할 수 있다. 단, IAM  
사용자에게 Glacier 접근권한을 설정해야 한다(rootkey.csv 파일에 있음).     
- Secret Access Key: AWS 계정이나 IAM 사용자의 시크릿 키를 입력한다.    

설정이 완료되었으면 Add new account 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158054294-1155d6ac-8b40-46a2-8400-5b5bdeb752eb.png" width="50%" height="50%"/>    
FastGlacier 왼쪽 부분에 리전과 Glacierr 볼트의 목록이 표시된다. 방금 생성한 Glacier 볼트(Example Vault)를  
선택한다. 그리고 Upload 버튼을 클릭하고, 팝업 메뉴에서 Upload file(s)를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158054397-2d5a811e-befc-421f-9386-c15d7f2fa01f.png" width="50%" height="50%"/>   
Glacier 볼트에 업로드할 파일을 선택한다.   


Glacier 볼트에 파일 업로드가 완료되었다. 방금 올린 파일이라도 다시 파일을 받으려면 반출 요청을 해야한다.   
<img src="https://user-images.githubusercontent.com/33191974/158054449-c6eaf8ee-cb6f-49bb-8a57-9574c6da1b5c.png" width="50%" height="50%"/>   
약 하루 정도 지난 뒤 Glacier 볼트 목록을 보면 Glacier 볼트(ExampleVault)의 용량과 아카이브(파일) 개수가   
표시된다.   




  



























