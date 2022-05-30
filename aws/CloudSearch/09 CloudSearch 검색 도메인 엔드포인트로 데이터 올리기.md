**AWS CLI를 사용하여 검색 도메인의 엔드포인트로 데이터를 올려보자.**      
AWS CLI를 설치하고 설정하는 방법은 '[AWS CLI 설치하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/CLI/CLI%20%EC%84%A4%EC%B9%98%EB%B0%A9%EB%B2%95(Linux).md)'를 참조하자.    
  
다음 명령을 입력하여 data.json 파일을 검색 도메인에 올린다.   
  
- cloudsearchdomain: CloudSearch 검색 도메인용 명령이다.  
- --endpoint-url: 여러분들이 생성한 검색 도메인의 엔드포인트 주소를 입력한다.  
doc-<검색 도메인 이름>-<검색 도메인 ID>-ap-northeast-2.cloudsearch.amazon.com  
형식이다. 
- upload-documents: 문서를 올리는 명령어이다.  
- --content-type: HTTP 헤더를 설정한다. JSON 형태의 파일이므로 application/  
json으로 설정한다.  
- --documents: 올릴 파일명을 설정한다. 여기서는 data.json을 입력한다.   

```
aws cloudsearchdomain --endpoint-url http://doc-exampledomain2-6o4kmjqgddx
4cdhz7jbpsuclbe.ap-northeast-2.cloudsearch.amazonaws.com upload-documents 
--content-type "application/json" --documents data.json
```

검색 도메인(exampledomain2)에서 Refresh 버튼을 클릭한다. 잠시 기다리면   
Searchable Documents가 2로 표시된다. data.json에 포함된 문서 2개가 정상적으로   
올라갔다.  
<img src="https://user-images.githubusercontent.com/33191974/158755413-0a973db4-060a-489f-a38f-1ff35b65b4a8.png" width="50%" height="50%"/>   
  
검색 도메인(exampledomain2)에서 Run a Test Search를 클릭한다. 그리고 Search   
부분에 홍길동을 입력하고 Go 버튼을 클릭한다. 이제 홍길동의 문서가 검색된다.   
<img src="https://user-images.githubusercontent.com/33191974/158756433-f8dbef02-4808-4fa3-9f89-17853b1c873a.png" width="50%" height="50%"/>  

























