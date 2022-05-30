이제 CloudSearch 검색 도메인에 AWS에서 제공하는 IMDB 영화정보 예제 데이터를  
올려보자. CloudSearch 검색 도메인(exampledomain)의 메인화면에서 위쪽 Upload   
Documents 버튼을 클릭한다. 버튼이 비활성화되어 있다면 검색 인스턴스가 아직   
생성되지 않아서 그렇다. 1분 정도 기다린 후 Refresh 버튼을 클릭하면 Upload  
Documents 버튼이 활성화된다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158731768-5692cf5d-7d3b-4abc-8433-b2123d1ef711.png" width="50%" height="50%"/>  
  
CloudSearch 검색 도메인에 데이터를 올린다. Predefined data를 선택하고    
Continue 버튼을 클릭한다.   
- File(s) on my local disk: 현재 컴퓨터에 저장된 파일을 올린다.  
- Object(s) from Amazon S3: S3 버킷에 저장된 데이터를 올린다.  
- Item(s) from Amazon DynamoDB: DynamoDB에 저장된 데이터를 올린다.   
- Predefined data: AWS에서 제공하는 예제 데이터를 올린다. 현재는 IMDB  
movies (demo)만 선택할 수 있다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158731920-7597e2bf-ffdc-4c3e-b81f-65aca58eef95.png" width="50%" height="50%"/>   
   
CloudSearch 검색 도메인에 올릴 데이터의 항목을 확인한다. Download the gener  
ated document batch 링크를 클릭하면 생성된 문서 배치를 다운로드할 수 있다. 
Upload Documents 버튼을 클릭한다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158732024-889e2b71-bead-4e0d-8cc6-35f857a90d89.png" width="50%" height="50%"/>    
  
CloudSearch에 데이터(문서 5,000개) 업로드가 완료되었다. Finish 버튼을   
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158732120-352c21f5-02ec-4ae7-a14f-f941d18aabf3.png" width="50%" height="50%"/>     
   
CloudSearch 검색 도메인(exampledomain)의 세부 내용에서 색인 생성이 완료되어   
검색 가능한 문서의 개수가 표시된다. 약 1분 정도 기다리면 색인 생성이 완전히   
끝난다.    

<img src="https://user-images.githubusercontent.com/33191974/158732234-4304a58b-4949-42f2-b069-53248ea26580.png" width="50%" height="50%"/>   
  
이제 CloudSearch 검색 도메인에서 검색을 할 수 있다.  




























