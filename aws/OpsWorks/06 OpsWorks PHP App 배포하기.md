OpsWorks를 사용하면 인스턴스가 1개든 1,000개이든 한 번에 배포할 수 있어 매우   
편리하다. OpsWorks App 목록에서 App(examplephp)이 deploy를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158541389-072251a7-286d-48c8-8d0c-0489e4c282dd.png" width="50%" height="50%"/>  
  
OpsWorks PHP App을 배포한다.   
- App: App 이름이 표시된다.   
- Command: 실행할 명령이다. 기본값 그대로 사용한다.   
- Comment: 특별히 기록할 것이 있다면 입력한다.   
- Custom Chef JSON: Chef 레시피에 넘겨줄 속성(Attribute) 값이다. 기본값 그대로   
사용한다.  
- Instance: App을 배포할 인스턴스를 선택할 수 있다. 기본값 그대로 사용한다.  

Deploy 버튼을 클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/158541654-3d8de40b-665a-4ff3-9c68-0cf60bd2c6de.png" width="50%" height="50%"/>   
OpsWorks PHP App이 배포중이다. 잠시 기다리면 Status가 successful로 바뀐다.  
아래 인스턴스 목록에서 인스턴스(php-app1)를 클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/158542192-26104d68-70d8-45cf-af09-099abfc73d4a.png" width="50%" height="50%"/>  
OpsWorks 인스턴스(php-app1)의 세부 내용이 표시된다. Public DNS 링크를   
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158542385-0ebc22ff-f52d-44a1-b3db-b7ef47c59540.png" width="50%" height="50%"/>   
웹 브라우저의 새 창이 열리고 앞에서 작성한 PHP App의 내용이 표시된다.   
  
<img src="https://user-images.githubusercontent.com/33191974/158544840-9fb12b9c-ab5a-4131-a16f-fe8b0ca5cd16.png" width="50%" height="50%"/>  























