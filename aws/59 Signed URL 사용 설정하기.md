# Signed URL 사용 설정하기
먼저 Signed URL을 사용하려면 CloudFront 배포에서 Behavior 설정이 필요하다.   
BeHavior 설정은 CloudFront가 어떻게 동작(행동)할지 설정하는 것이다.   
  
`S3와 CloudFront 연동하기`에서 생성한 S3 버킷과 CloudFront 배포를 그대로 사용한다.   
아직 생성하지 않았다면 `S3와 CloudFront 연동하기`를 참조하여 S3 버킷과 CloudFront  
배포를 생성하기 바란다.  
  
CloudFront 배포 목록에서 S3와 연동한 CloudFront 배포를 선택한다.   
Behavior 설정이다.  
- Restric Viewer Access: Yes를 선택하여 Signed URL을 사용한다.  
- Trusted Signed: Signed URL에서 서명(Signature)할 계정을 설정한다.   
   - Self: 현재 접속한 AWS 계정이다. 기본값 그대로 사용한다.   
   - Specify Accounts: 서명할 AWS 계정을 추가할 수 있다. AWS 계정 번호를  
   입력해야 한다. AWS 계정 번호는 AWS 콘솔에서 위쪽 `<내 이름>` -> My Account  
   페이지에서 확인할 수 있다.      

설정이 완료되었으면 Yes, Edit 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156706828-9654d229-4b04-4904-85eb-823242ec181e.png" width="50%" height="50%"/>     
CloudFront 배포 Behavior 목록에서 Trusted Signers가 self로 설정되었다.   
이제 위쪽 CloudFront Distributions를 클릭한다.   
설정이 모든 에지 로케이션에 전파가 완료되면 Status가 Deployed로 바뀐다.   
Domain Name에 CloudFront 배포의 도메인이 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/156707197-94d92404-ed50-4c4d-90f5-9219ca9d9972.png" width="50%" height="50%"/>    
웹 브라우저에서 Signed URL을 설정한 CloudFront의 test.jpg 파일에 접속한다.   
MissingKey라는 에러가 발생하면 test.jpg 파일의 내용이 표시되지 않는다.   
이제 Signed URL에 맞는 파라미터들이 없으면 파일의 내용을 볼 수 없다.     
<img src="https://user-images.githubusercontent.com/33191974/156708250-c986f178-7763-4168-8a22-bbd31d9c925f.png" width="50%" height="50%"/>    












---
# 번외(다른 방법 )

# CloudFront에 Signed URLs 적용하기

## S3 버킷 CORS 설정하기   
해당 CORS 에러를 해결하고자 S3 버킷의 Permissions 탭의 Cross-origin resource   
sharing(CORS) 설정을 다음과 같이 작성해주었다(버킷 권한탭).    
<img src="https://user-images.githubusercontent.com/33191974/156573590-c9f39424-b6be-46aa-bfb0-388b42092d76.png" width="50%" height="50%"/>    
```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "x-amz-server-side-encryption",
            "x-amz-request-id",
            "x-amz-id-2"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

[참고](https://velog.io/@leehaeun0/AWS-CloudFront%EC%97%90-Signed-URLs-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)


## CloudFront를 통해 Origin 설정
그러나 이렇게 하면 CORS 에러가 발생한다. 이 에러는 크롬 브라우저에서만 발생하는  
버그였고, 다른 브라우저에서는 정상적으로 get 요청이 되었다.   
  
위 에러를 해결하고자 검색해보니 나와 같은 에러를 겪고 있는 [블로그 글](https://velog.io/@kimsehwan96/S3-CORS-%ED%97%A4%EB%8D%94-%EA%B4%80%EB%A0%A8-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-html2canvas-lottie)이 있었고   
정리해보면 다음과 같다.  
1. `access-control-allow-origin`이 헤더에 없어서 발생하는 CORS에러니 헤더에   
access-control-allow-origin`이 어떻게든 들어가도록 해야한다.  
2. 요청을 넣을 때 request header에 Origin을 넣으면 response header에 `access-
control-allow-origin'을 넣어 내려보내준다.  
3. 그러나 Origin은 forbidden header name(금지된 헤더명, 프로그래밍 방식으로  
수정할 수 없다)이라서 프로그래밍 방식으로 Origin을 설정할 수 없다.  
4. CloudFront에서는 request header에 Origin을 설정할 수 있다.   
5. S3를 바라보는 CloudFront를 생성하고 request header Origin을 설정하니   
response header에 `access-control-allow-origin`이 담겨서 내려왔고 CORS 에러가   해결되었다.   

S3에 CloudFront를 붙인 구조는 다음과 같다. 클라이언트가 S3로 바로 접근하는 것이  
아닌 CloudFront을 통해서 S3로 접근하는 것이다.  

> 클라이언트 -> CloudFront -> S3

CloudFront를 사용하면 다운로드 에러를 해결할 뿐만이 아니라, 캐쉬된 데이터인   
CloudFront로 접근하는게 S3보다 훨씬 속도가 빠르기 때문에 이미지 url을 Cloud  
Front로 변경했다.   
  
CloudFront의 request header에 Origin을 설정하는 방법은 다음과 같다.   
  
AWS에서 `CloudFront > Distributions > CloudFront id 선택 > behavior > edit`   
이 경로를 통해 접근하면 CloudFront의 request를 설정할 수 있다.   
  
참고로 이 request는 CloudFront가 S3로 보내는 request이다. CloudFront는    
중간다리 역할이라서 원본에게 요청을 보내는 request 세팅도 할 수 있고,    
CloudFront에 접근하는 쪽(주로 클라이언트)에 응답을 하는 Response 세팅도 할   
수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/156695888-ef8bb662-f31f-4c1e-9d11-6ea9057270a5.png" width="50%" height="50%"/>     
aws에서 아주 친절하게도 자주 사용되는 origin request 정책들을 제공해준다.   
View policy를 통해 aws에서 제공한 CORS-S3Origin 정책을 보면 다음과 같이  
생겼다. 보다싶이 headers에 origin을 설정해주고 있다.  
<img src="https://user-images.githubusercontent.com/33191974/156696470-524cb1e7-38e7-4079-9bf8-a0c71a07bc2c.png" width="50%" height="50%"/>   
이제 이렇게 하면 아래와 같이 response header에 `access-control-allow-origin`이  
들어갔고 request header에 `origin`이 들어갔다. Status Code도 200으로 get  
요청에 성공했다.   
이제 CloudFront를 통해서 S3에 있는 이미지를 가져올 수 있게 되었고 다운로드도   
정상적으로 된다(다른 서버의 HTML 코드 상에서 호출시).    

## S3 접근권한에 CloudFront 등록
위 CORS error 해결과는 상관없이 CloudFront를 S3와 연결할 때에 짚고 넘어갈   
부분이 하나있다.   
  
`CloudFront > Distributions > CloudFront id 선택 > origin > edit`에서 bucket  
access 접근 설정을 할 수 있다. S3를 선택한 후, 버킷정책에서 Yes, update the   
bucket policy를 선택하고 저장하면 S3의 버킷 정책이 업데이트된다.   
<img src="https://user-images.githubusercontent.com/33191974/156698522-9dd0e15e-8ff9-4d88-bf70-9078f56ffa07.png" width="50%" height="50%"/>   

> #### S3 버킷 액세스  
> OAI를 사용하지 않는 경우 S3 버킷에서 퍼블릭 액세스를 허용해야 한다.   
> S3 버킷에서 퍼블릭 액세스를 허용하지 않으려는 경우 OAI를 사용할 수 있다.   
> OAI를 사용하면 CloudFront에서 인증된 요청을 S3 버킷으로 보낸다. 즉,  
> CloudFront가 버킷에서 객체를 가져올 수 있도록 허용하면서 S3 버킷에 대한   
> 퍼블릭 액세스를 차단할 수 있다.  
> OAI를 사용하려면 OAI에 대한 액세스를 허용하도록 S3 버킷 정책을 업데이트해야   
> 한다. 예, OAI 사용을 선택하는 경우 CloudFront에서 버킷 정책을 업데이트  
> 하도록 예, 버킷 정책 업데이트를 함게 선택할 수 있다. 이렇게 하지 않으면   
> 정책을 직접 업데이트해야 한다.

업데이트된 S3 버킷 정책은 `S3 > Bucket > Permissions` 탭에서 확인해볼 수 있다.   
  
Principal을 `*`all로 설정해서 아무 경로로나 다 S3에 접근 가능하도록 열어두지  
말고 보안을 생각해서 CloudFront을 통해서만 S3에 접근할 수 있도록 하는 것을   
추천한다.   

# 아직 남은 일 Signed URLs
이제 CloudFront를 통해서 이미지를 가져올 수 있게 되었지만, 추가적으로 해줘야   
할 작업이 아직남아 있다.   
회사에서 사용하는 이미지들은 보안이 필요하기 때문에 Signed URLs을 통해 url  
쿼리문자열에 인증정보를 추가하는 작업이 필요하다.
  
# CloudFront에 Signed URLs 적용하기  
CloudFront에서 Signed URLs를 사용하는 방법을 간단히 요약하자면 다음과 같다.  

- Signed URLs 인증 절차에 사용할 `공개키`와 `개인키` RSA key pair를 만든다.   
- `공개키`를 AWS CloudFront keygroups에 등록하고 해당 CloudFront에 접근제한을  
설정하고 keygroups을 연결한다.  
- 애플리케이션에서 `개인키`를 사용하여 인증정보가 쿼리에 담겨있는 Signed URLs을  
생성한다.  
- 해당 Signed URLs로 접근하면 url 쿼리에 등록된 개인키가 CloudFront에 등록된   
공개키와 복호화를 성공하여 콘텐츠에 접근이 가능하다.   

CloudFront에서 Signed URLs를 사용하려면 '루트 사용자가 aws에서 key pair를  
생성하도록 하는 방법'과 '키 그룹을 생성하는 방법'이 있는데 aws에선 키 그룹을   
생성하는 방법을 권장한다. 이 글에서도 키 그룹방식으로 진행할 것이다. 
  
# 키 생성
S3에 컨텐츠를 업로드하고 CloudFront를 통해 배포할 때 private하게 제한을 걸고   
싶은 경우라면 AWS계정에서 제공하는 CloudFront 키페어를 발급받아 사용할 수   
있다. 이 키페어는 EC2를 생성할 때 발급할 수 있는 인스턴스 접속용도의 키페어와는   
다르다. 이 키페어는 오직 루트계정에서만 발급 및 관리가 가능하다.   
AWS 콘솔 우측 상단의 계정이름을 클릭하면 펼쳐지는 메뉴 중 하단의 My Security   
Credentials를 클릭한다.  
그러면 아래의 화면과 같은 페이지가 열린다. Create New Key Pair를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156560529-a7edae84-5e64-4313-a91e-b56c6623b99d.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/156562751-e40f8343-402c-4ec8-a344-1dbfad51e4bc.png" width="50%" height="50%"/>  
비밀키와 공개키를 다운받자.  
<img src="https://user-images.githubusercontent.com/33191974/156562927-f2fdd89e-6a84-4804-8ddb-981924e3baef.png" width="50%" height="50%"/>    
CloudFront 용도의 키페어는 여러 개를 생성할 수 있으며 Make Inactive를 눌러  
비활성화 시킬 수 있고 Delete를 눌러 삭제도 가능하다. 키 방식은 언젠가는 유출이   
될 수 있으니 주기적으로 교체를 권장한다.   
이렇게 키페어를 생성해서 잘 보관해두면 CloudFront를 통해 Private하게 컨텐츠를  
배포하기 위한 Signed URL, Signed Cookie에 사용할 키 페어 준비는 완료된 것이다.  

# AWS CloudFront에 공개키 연결
## CloudFront public key 등록   
AWS의 CloudFront > Public Key > Create public key에서 공개키를 등록할 수 있다.   
이름을 지정하고 키를 입력하는 곳에 퍼블릭 키를 등록한다.  
<img src="https://user-images.githubusercontent.com/33191974/156564262-091464a0-d870-4432-b1b5-93688febb88c.png" width="50%" height="50%"/>    
퍼블릭 키 생성 버튼을 누르면 공개키 등록이 완료되고 등록된 키를 목록페이지에서   
확인해 볼 수 있다.   

## CloudFront Key groups 등록  
그 다음 CloudFront > Key groups > Create key group 경로를 통해 키그룹 생성   
페이지로 이동한다.   
Public Keys에서 방금 만든 공개키를 선택하고 키 그룹 생성을 완료한다.  
<img src="https://user-images.githubusercontent.com/33191974/156564751-c303edca-85ea-476f-8347-c705b0fc5ae1.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/156564869-b43dcf7f-5581-4b4d-82c2-734a73acd114.png" width="50%" height="50%"/>  

## 해당 CloudFront id에 key groups 연결  
CloudFront > Distributions > CloudFront id 선택 > behavior > edit으로  
이동 후, Restrict viewer access(뷰어 액세스 제한)을 yes로 선택하고 방금   
만든 키 그룹을 등록한다.    
<img src="https://user-images.githubusercontent.com/33191974/156576101-15ac6b91-1980-4d54-847d-97d85d2f111d.png" width="50%" height="50%"/>    
이제 CloudFront에 접근 제한 설정이 완료되었다. 이전에 CloudFront에 접근했던   
url로 접근하면 다음과 같은 화면이 나오면서 더이상 접근이 안된다. Signed URLs를  
통해서만 접근이 가능하다.   
<img src="https://user-images.githubusercontent.com/33191974/156701269-ddba798b-cbe9-4ca5-b9fe-407c3fb29055.png" width="50%" height="50%"/>    

# Signed URLs 생성
이제 `Signed URLs`를 코드상에서 생성해주면 된다.   
[AWS 공식문서에서 제공하는 언어별 코드 예제](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/PrivateCFSignatureCodeAndExamples.html)를 참고하면 된다.     
  
우리 회사에선 파이썬과 django-storages 라이브러리를 사용하는 중인데 django-  
storages 최신 버전에서는 setting.py에 다음과 같이 설정하면 Signed URLs를   
자동으로 생성해준다.   
```
AWS_CLOUDFRONT_KEY = os.environ.get('AWS_CLOUDFRONT_KEY', None).encode('ascii')
AWS_CLOUDFRONT_KEY_ID = os.environ.get('AWS_CLOUDFRONT_KEY_ID', None)
```
AWS_CLOUDFRONT_KEY에는 생성했었던 개인키 파일을 등록해주면 된다.   
AWS_CLOUDFRONT_KEY_ID에는 AWS `CloudFront > Public key`에서 등록했었던  
key의 id를 등록해주면 된다.   
만약 django-storages가 최신버전이 아니라서 위 기능이 지원되지 않는다면   
[링크](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/cloudfront.html#generate-a-signed-url-for-amazon-cloudfront)  
를 참고해서 Signed URLs를 직접 생성하면 된다.   
위 방법을 통해 생성된 Signed URLs 주소로 들어가면 접근 제한된 CloudFront에    
접근이 가능하다.




















 

















