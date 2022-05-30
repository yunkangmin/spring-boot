구글 FCM API의 사용 등록을 하고 AWS에서 제공하는 예제 애플리케이션 소스로   
푸시 알림을 받아보자. 먼저 구글 GCM API를 사용하려면 구글 계정(Gmail)이  
필요하다. 구글 계정이 없다면 구글 계정을 생성하자. 구글 계정 생성 방법은 따로  
설명하지 않는다.  

# FIREBASE CLOUD MESSAGING(FCM) 설정하기
https://console.firebase.google.com/ 페이지로 이동한다.  
<img src="https://user-images.githubusercontent.com/33191974/158812150-ffb3c90a-4699-4c6f-b129-260730609233.png" width="50%" height="50%"/>  
  
Create a project를 선택하자. 프로젝트 이름을 넣고 계속을 눌러 다음단계로  
이동한다.  
<img src="https://user-images.githubusercontent.com/33191974/158812386-701c7a46-9d46-4abf-bfea-202f2adccaea.png" width="50%" height="50%"/>   
  
본 프로젝트는 FCM을 push message 용도로만 사용할 예정이기 때문에 Emable  
Google Analytics for this project를 disable하고 다음 단계로 이동한다.  
<img src="https://user-images.githubusercontent.com/33191974/158813799-6d7e8988-8c32-4ef0-a63a-0825e9b08977.png" width="50%" height="50%"/>  
  
project가 생성되면 아래와 같은 화면이 보인다.  왼쪽 상단에 설정 아이콘인  
톱니바퀴 모양의 아이콘을 선택하고 Project settings를 선택한다.   
<img src="https://user-images.githubusercontent.com/33191974/158814025-26fce58c-6f3f-4ac9-b2d2-b30e6bb357ba.png" width="50%" height="50%"/>   
  
General 탭에 아래부분에 Your apps가 있다. 안드로이드 모양의 아이콘을   
선택한다.   
<img src="https://user-images.githubusercontent.com/33191974/158814193-6062c491-e271-41f0-a210-a73eede8c70a.png" width="50%" height="50%"/>    
  
Package name(com.example.socialandroidapp)과 nickname(aws-android-work  
shop)을 각각 아래와 같이 입력한다. Register app 버튼을 눌러 다음 단계로   
이동한다.   
<img src="https://user-images.githubusercontent.com/33191974/158814434-b6b112dc-79b2-4d87-8036-6e12692aff4d.png" width="50%" height="50%"/>   
  
화면에 보이는 Download 버튼을 통해 json 파일을 그림을 참고하여 해당 경로에  
download한다.   
<img src="https://user-images.githubusercontent.com/33191974/158814643-4a06d037-21a7-4fa1-8ffe-5671972bac85.png" width="50%" height="50%"/>   
  
이후 모두 default로 선택한 후 완료한다.   
Settings로 돌아와서 두번째 탭인 Cloud Messaging을 선택한다.   
Server key에 해당하는 값을 복사한다.   
<img src="https://user-images.githubusercontent.com/33191974/158815109-c5e2124d-e713-4123-8673-cbb133174eb4.png" width="50%" height="50%"/>  

---

아래는 현재는 해당 안됨.  
# 구글 FCM API 사용 등록하기  
웹 브라우저에서 구글 개발자 콘솔에 접속한 뒤 Create Project 버튼을 클릭한다.   
이미 만들어 놓은 프로젝트가 있다면 해당 프로젝트를 사용한다.   
  
https://console.developers.google.com   
  
구글 API 프로젝트를 생성한다.  
<img src="https://user-images.githubusercontent.com/33191974/158795889-49f6a1ab-53df-47e5-9597-6d49bea04481.png" width="50%" height="50%"/>   
- PROJECT NAME: 프로젝트의 이름이다. ExamplePush를 입력한다.  
- PROJECT ID: 프로젝트 ID이다. 기본값 그대로 사용한다.    

설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158796227-bc5a85b3-c10b-4d61-9d84-f57f0fc7ec75.png" width="50%" height="50%"/>  
  
프로젝트 생성이 완료되면 자동으로 ExamplePush 프로젝트 페이지로 이동한다.  
위쪽에 프로젝트 번호가 표시된다. 이 프로젝트 번호는 모바일 장치의 Registration  
ID를 생성할 때 필요하므로 위치를 꼭 기억해두자.   
<img src="https://user-images.githubusercontent.com/33191974/158796604-9302dbfe-08cc-4346-a0ff-9c0a6f66b5a4.png" width="50%" height="50%"/>   
  
APIs & auth -> APIs를 클릭하고 Google Cloud Messaging for Android의 OFF  
버튼을 클릭하여 ON으로 만든다.   
<img src="https://user-images.githubusercontent.com/33191974/158798423-9452fd5a-678e-43d0-8e0e-34506caad414.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158798569-109ad71f-2557-46a5-b57e-f993bf9da398.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158798725-3f5827e6-cee2-477d-a2a7-726ccaf28866.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158799099-9e3628ec-c3ce-4f30-84ea-342642120909.png" width="50%" height="50%"/>     
구글 API 서버 키 생성이 완료되었다.   
































