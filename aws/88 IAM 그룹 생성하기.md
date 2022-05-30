IAM 그룹은 이름 그대로 IAM 사용자들을 모아 놓은 것이다. IAM 그룹에 접근제어 및   
권한 설정을 할 수 있으며 IAM 그룹에 설정된 내용은 IAM 그룹 안에 포함된 모든  
사용자들에게 적용된다.   
  
EC2 인스턴스만 제어할 수 있는 IAM 그룹을 생성해보자. AWS 콘솔로 접속한 뒤 메인   
화면에서 Deployment & Management의 IAM을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157663778-ddfb7e60-0456-484f-9ac7-1c321bddcda4.png" width="50%" height="50%"/>   
  
IAM 그룹 목록(Details -> Groups)에서 위쪽 Create New Group 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157664265-02db7625-fc3b-4f0c-a3dd-884adf898407.png" width="50%" height="50%"/>   
  
IAM 그룹 이름을 설정한다. Group Name에 EC2Admin을 입력한다.  
<img src="https://user-images.githubusercontent.com/33191974/157664526-b8717476-4ffb-4342-a390-0adbf92c40d3.png" width="50%" height="50%"/>    
IAM 그룹에 권한을 설정한다. Select Policy Template에는 AWS의 모든 리소스에 대한  
접근 권한을 Full Access, Read Only Access, 기타 Access로 구분하여 준비해 놓았다.   
개수가 상당히 많으므로 스크롤을 내려 Amazon EC2 Full Access의 Select 버튼을   
클릭한다.   
  
AWS Policy Generator나 직접 정책(Custom Policy)을 작성하여 설정할 수도 있다.  
<img src="https://user-images.githubusercontent.com/33191974/157666740-190e98c9-fcfd-4348-8a93-1674fcff1c49.png" width="50%" height="50%"/>     
    
EC2에만 접근할 수 있도록 해주는 정책 파일(Policy Document)이 자동으로 생성된다.    
<img src="https://user-images.githubusercontent.com/33191974/157666474-8eadb939-db7a-4341-9a6c-d0100cdb0110.png" width="50%" height="50%"/>     
지금까지 설정한 내용이 이상이 없는지 확인한다. 이상이 없으면 Create Group 버튼을   
클릭한다.  
  
IAM 그룹 목록에 IAM 그룹(EC2Admin)이 생성되었다. 이 IAM 그룹 안에 속한 IAM 사용자는  
EC2 인스턴스만 제어할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/157667176-888871af-d488-4154-a684-616e6652b057.png" width="50%" height="50%"/>   



   
   





























