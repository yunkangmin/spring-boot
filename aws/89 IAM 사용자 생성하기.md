이제 IAM 사용자를 생성하고 IAM 그룹에 추가해볼것이다. IAM 사용자 목록에서   
<img src="https://user-images.githubusercontent.com/33191974/157667516-bb0bc08f-212a-4783-b64b-8833672b541e.png" width="50%" height="50%"/>     
IAM 사용자를 생성한다. 한 번에 5개까지 생성할 수 있다. Enter User Names의 첫번째   
칸에 exampleuser1을 입력하고 다음 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157667944-6b6ebd62-6fda-41da-a535-f54a249472b5.png" width="50%" height="50%"/>     
아래 부분은 현재는 생략한다.   
<img src="https://user-images.githubusercontent.com/33191974/157668315-da939b36-ac82-459b-a3df-6c6fc829a398.png" width="50%" height="50%"/>    
  
---

IAM 사용자가 생성되었다. Show User Security Credentials를 클릭하면 examplesuer1의   
액세스 키와 시크릿 키가 표시된다. 이 부분을 복사해서 따로 보관하거나 Download    
Credentials 버튼을 클릭하여 액세스 키, 시크릿 키 파일을 다운로드한다.    
  
> #### `<Warning>` 여기서 액세스 키와 시크릿 키를 따로 복사하여 보관하지 않거나  
> Download Credentials 버튼을 클릭하여 파일을 받지 않고 그냥 Close 버튼을 클릭하여   
> 창을 닫아버리면 두 번 다시 시크릿 키를 확인할 수 없게 된다(시크릿 키는 일종의   
> 비밀번호이다). 따라서 생성된 직후 키들을 복사하여 보관하거나 파일을 꼭 받아   
> 두어야 한다. 그냥 닫아버렸거나 키 파일을 분실하였다면 이 액세스 키는 사용할   
> 방법이 없으므로 폐기하고, 새로운 액세스 키를 생성해야 한다.   

IAM 사용자 목록에 IAM 사용자(exampleuser1)가 생성되었다. IAM 사용자(exampleuser1)의    
체크 박스를 선택하고 위쪽 User Actions 버튼을 클릭하면 팝업 메뉴가 나온다.    
Add User to Groups를 클릭한다.     

---

<img src="https://user-images.githubusercontent.com/33191974/157670931-5716256f-eb94-429b-b693-bdc362dde3f3.png" width="50%" height="50%"/>  
다음 화면부터는 Next를 눌러 사용자를 생성한다.    
<img src="https://user-images.githubusercontent.com/33191974/157673678-290bc0b3-0029-426b-b4cd-faa33f48527d.png" width="50%" height="50%"/>     
액세스 키와 시크릿 키가 표시된다. 이 부분을 복사해서 따로 보관하거나 Download      
Credentials 버튼을 클릭하여 액세스 키, 시크릿 키 파일을 다운로드한다.   
  
IAM 그룹에 사용자가 추가되었다. IAM 사용자 목록에서 IAM 사용자(exampleuser1)를    
선택하면 아래 세부 내용에서 이 IAM 사용자가 속한 IAM 그룹이 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/157671614-54318bcb-bd96-46de-8eb8-6f2840dc2686.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/157671887-5a97b333-8934-4689-afba-a996214fddc9.png" width="50%" height="50%"/>   
이제 이 IAM 사용자(examleuser1)가 S3에 접근할 수 있도록 설정해보자. IAM 사용자   
목록에서 IAM 사용자(exampleuser1)를 선택한다. 그리고 Permissions 탭을 클릭하고     
Attach User Policy 버튼을 클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/157672277-7371be56-2c4a-4e52-9ca1-2794bbba26fb.png" width="50%" height="50%"/>     
IAM 사용자의 권한이다. Select Policy Template에는 AWS의 모든 리소스에 대한 접근    
권한을 Full Access, Read Only Access, 기타 Access로 구분하여 준비해 놓았다.     
개수가 상당히 많으므로 스크롤을 내려 Amazon S3 Full Access의 Select 버튼을    
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157672783-386eed70-b93d-4253-bc26-7385e6e18d30.png" width="50%" height="50%"/>      
S3에만 접근할 수 있도록 해주는 정책 파일(Policy Document)이 자동으로 생성된다.   
Apply Policy 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157673046-7206258e-6cde-41f2-a912-a440c1da73aa.png" width="50%" height="50%"/>   
IAM 사용자(exampleuser1)의 세부내용에서 User Policies 부분에 S3 접근권한 정책이   
추가되었다.   
<img src="https://user-images.githubusercontent.com/33191974/157673361-3d817c1e-ab3c-4420-9612-487626646ad1.png" width="50%" height="50%"/>   













































