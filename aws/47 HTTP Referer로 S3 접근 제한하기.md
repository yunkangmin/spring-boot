S3에 올려진 그림 파일을 원하는 도메인에서만 보여줄 수 있도록 설정해보자.  
S3은 데이터 전송량에 따라 요금을 책정하기 때문에 원하지 않는 도메인에서의 링크를  
막는다면 그만큼 비용을 절감할 수 있다.  
  
HTTP Referer는 HTTP 헤더값으로서 웹 브라우저에서 생성하는 데이터이다.  
예를 들면 http://example.com이라는 웹사이트에서 http://hello.com의 링크를 클릭하거나  
<img> 태그로 그림 파일을 보여줄 때, http://hello.com으로 보내는 HTTP 헤더의 Referer 값은  
http://example.com이 된다. 따라서 링크를 어디서 클릭했느냐, 그림 파일을 어디서 보여줬느냐를  
알 수 있다. S3에서는 이 Referer 값을 판단해서 파일을 보여줄지 말지 결정할 수 있다.  
  
이번에는 버킷을 2개 사용한다. 버킷 하나에 웹사이트를 구축하고 HTTP Referer를 설정한다면  
정작 웹사이트의 HTML 파일을 볼 때는 HTTP Referer 설정에 막혀버리기 때문에 의미가 없다.  
앞에서 생성했던 버킷을 그림 파일 저장 전용 버킷([42 S3 버킷 생성하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/42%20S3%20%EB%B2%84%ED%82%B7%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.md),
[43 S3 버킷에 파일 올리기/받기](https://github.com/yunkangmin/spring-boot/blob/main/aws/43%20S3%20%EB%B2%84%ED%82%B7%EC%97%90%20%ED%8C%8C%EC%9D%BC%20%EC%98%AC%EB%A6%AC%EA%B8%B0%20and%20%EB%B0%9B%EA%B8%B0.md),
[44 S3 객체 권한 관리하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/44%20S3%20%EA%B0%9D%EC%B2%B4%20%EA%B6%8C%ED%95%9C%20%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0.md),
[45 S3 버킷 권한 관리하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/44%20S3%20%EA%B0%9D%EC%B2%B4%20%EA%B6%8C%ED%95%9C%20%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0.md) 참고)으로 사용하고 HTTP Referer 설정을 할 것이며  
정적 웹 사이트 호스팅을 설정했던 버킷([46 S3 정적 웹사이트 호스팅 사용하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/46%20S3%20%EC%A0%95%EC%A0%81%20%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8%20%ED%98%B8%EC%8A%A4%ED%8C%85%20%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0.md) 참고)에서 그림 파일을 링크해본다.  
  
그림 파일 저장 전용 버킷의 권한을 AWS Policy Generator에서 아래와 같이 설정한다.  
맨 아래 condition에서 value 주소(정적 웹 호스팅 주소) 맨 뒤에 "/*"을 붙여준다.  
![image](https://user-images.githubusercontent.com/33191974/147563240-786cc5ee-a4a6-4cfa-8440-11aa01d38724.png)  
- Effect : 지정한 도메인만 허용할 것이므로 허용(Allow)이다.
- Principal : 정책을 적용하라 대상이다. 인터넷 전체에 공개할 것이므로 *이다.
- Action : 파일을 보여주는(다운로드) 상황이므로 s3:GetObject이다.  
- Condition : 조건절이다. 이 곳에 설정한 조건에 맞으면 허용(Allow) 또는 거부(Deny)한다.  
- StringLike : 조건절 안에 사용하는 조건문이다. 뜻은 문자열을 포함하고 있을 때이다.  
- aws:Refer : Referer값을 지정한다. 보통 도메인을 설정하며 맨 뒤에 /*를 붙여주어, 도메인 이하  
모든 경로에 대해 허용한다. /hello.html 처럼 특정 파일의 경로를 지정할 수도 있다. 여러 도메인을  
지정하려면 ,(콤마)로 구분하면 된다.  

Resource와 aws:Referer 주소 뒤에 /*을 붙여준다.  
```
{
  "Id": "Policy1640692506418",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1640692498755",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::exampleimage112323/*",
      "Condition": {
        "StringLike": {
          "aws:Referer": "http://example12334.s3-website.ap-northeast-2.amazonaws.com/*"
        }
      },
      "Principal": "*"
    }
  ]
}
```
이미지 파일을 저장한 Bucket은 위의 정책에 따라서, aws:Refer에 선언된 도메인에서만 접근가능하게 된다.  
다음으로 퍼블릭 액세스 설정을 해준다.   
Bucket Policy를 기반으로 접근을 제어하기 위해 세번째 항목을 ON으로 설정한다.  
![image](https://user-images.githubusercontent.com/33191974/147564689-bbb653e7-e3ab-4203-a39c-7d731c35a4ed.png)  

## 테스트  
먼저 위의 aws:Referer에 등록되지 않은 도메인에서 이미지 저장 Bucket에 업로드 되어있는 이미지 파일을  
열어보자.  
![image](https://user-images.githubusercontent.com/33191974/147564937-27ccf682-f8cc-4278-b650-937c298174a0.png)  
위와 같이 AccessDenied 에러와 함께 사진이 열리지 않는다. 이는 방금전 Object URL을 클릭했던 도메인  
즉, HTTP Refer가 Bucket Policy에 등록한 Referer와 맞지 않기 때문이다.  

### 정적 웹 호스팅 S3에 html 작성하기
Refer가 등록된 Bucket의 이미지 파일을 불러오기 위한 에제 파일을 작성해보자.  
```html
<html>
<head>
  <title>S3 Example Webstie hosting</title>
</head>
<body>
  <p>Hello S3</p>
  <img src="https://exampleimage112323.s3.ap-northeast-2.amazonaws.com/test.jpg" width="320" height="240">
</body>
</html>
```
작성한 파일을 index.html로 저장후 정적 웹호스팅을 설정한 S3 Bucket에 해당 html 파일을 업로드해준다.  
![image](https://user-images.githubusercontent.com/33191974/147566088-3cb87b7f-d9ce-4f46-b07c-b2fd551aa625.png)    
아래와 같이 정적 웹 호스팅 주소를 클릭하면 이미지가 정상적으로 나오는 것을 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/147566135-7d5313d5-af62-4b04-abd7-f6457050a391.png)  
이미지 파일 저장 Bucket Policy에 등록된 HTTP-Referer 정책에 위의 정적 웹호스팅 S3의 URL이 등록되어  
있기 때문에 아래와 같이 이미지가 정상적으로 출력될 수 있다.  

### 정리
이미지 파일 저장 Bucket에 특정 도메인만 접근할 수 있도로고 제한하려면  
AWS Policy Generator를 사용해 HTTP-Referer 접근 제한자를 설정하면 된다.  






































  
