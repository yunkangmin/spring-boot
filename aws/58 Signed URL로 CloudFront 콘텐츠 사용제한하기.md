Signed URL은 CloudFront로 배포되는 파일의 사용을 제한하는 기능이다. Signed URL은  
특정 날짜가 지나면 파일을 받지 못하게 하고 싶을 때, 특정 날짜와 이후에 파일을   
받게 하고 싶을 때, 특정 IP에서만 파일을 받을 수 있도록 할 때 사용한다.   
  
Signed URL은 크게 2가지가 있다.  
- Canned Policy를 사용한 Signed URL: 파일 1개의 사용을 제한한다. 또한 특정 날짜가   
지나면 파일을 받지 못하게 하는 기능만 사용할 수 있다. 정책(Policy)의 내용이  
URL에 포함되어 있지 않아 URL의 길이가 짧다. 
- Custom Policy를 사용한 Signed URL: 파일 여러 개의 사용을 제한한다. 특정 날짜가   
지나면 파일을 받지 못하게 하는 기능, 특정 날짜 이후에 파일을 받을 수 있도록 하는   
기능, 특정 IP 혹은 IP 대역에서만 파일을 받을 수 있도록 하는 기능을 사용할 수 있다.   
정책(Policy)의 내용이 URL에 포함되어 있어 URL의 길이가 길다.   
  
다음은 Canned Policy를 사용한 Signed URL의 구조이다.   
```
http://d39hltbb812jpd.cloudfront.net/index.html?Expires=1399721576
&Signature=MgYk5FP4wQHHrdSwgcLjrlpo4DoDX-4CbwmtUR3eRRwdyj-OLTZDfBpl
GlkMBTd5o8CAuvkAf3G5I5ip5tE4qK--zIFa-ufZsJqpdWsk9tr2nRFO8BBa3IYLRyUg
yh0PeMlPx1sIAlJDiVSiCDF7SL4GAeSDKccoVayDbxAbknUhcV6vHbRBAJPqkw2H0aozE
BtIUQEiIEX79pCoRfP6gFQA4yXf-GJxk3Lbwl0TgdWkSWramF1vAB06eoqDqOrq0koRAZ
DbTRiYloxm9a0DXI~hzQv7cdOrKYVTHpVGzqWueCLpDT~F9vIly6sOHyeynuT-POgAbBZ~
fZC3CfTqSw__&Key-Pair-Id=APKAJERVTR4A7EO47UYA
```
- http://d39hltbb812jpd.cloudfront.net: CloudFront 배포 URL이다. 
- index.html: CloudFront로 배포되는 파일이다. 여기서는 index.html 파일을 예로  
들었다. JPG, PNG 등의 그림 파일과 ZIP등의 압축 파일을 비롯한 모든 파일에 적용  
가능하다.  
- Expires: 이 파일의 사용이 만료되는 날짜와 시간이다. 이 날짜와 시간 규칙은   
UTC(Coordinated Universal Time, 협정 세계 표준시)를 사용한다. 이 UTC는 1970년  
1월 1일 자정을 0으로 시작하여 특정 시간까지 밀리초(1000분의 1초)로 계산한 것이다.   
이 값은 사람이 직접 알아내기는 힘들며 프로그래밍 언어에서 간단하게 얻을 수 있다.   
- Signature: 파일 사용 제한을 정의하는 정책(Policy Statement) 파일의 내용을   
CloudFront 개인 키(Private Key)로 서명한 해시값이다. 이 값은 BASE64로 인코딩   
되어 있으며 `+`는 `-`로, `=`는 `_`로 `/`는 `~`로 바뀌어져 있다.  
- Key-Pair-Id: CloudFront 전용 키 쌍(Key Pair)의 액세스 키(Access Key)이다.   
이 액세스 키는 AWS 계정의 액세스 키와는 별개이다.   

다음은 Canned Policy 파일의 내용이다. 이 파일에 정의한 대로 파일의 사용을   
제한한다. 그리고 이 정책 파일의 내용을 CloudFront 개인키로 서명한 해시 값이   
위에서 말한 Signature 값이다.   
```
{
  "Statement": [{
    "Resource": "http://d39hltbb812jpd.cloudfront.net/index.html",
    "condition": {
      "DateLessThan" {
        "AWS:EpochTime": 1399721576
      }
    }
  }]
}
```
- Resource: 파일의 경로이다. 꼭 http://, https://, http*://로 시작해야 한다.   
쿼리 문자열(Query String)도 붙을 수 있다(예: hello.jpg?size=large&license=yse),  
쿼리 문자열이 없을 경우 맨 뒤 ?는 삭제해야 한다. 또한, Expires, Signature,   
Key-Pair-Id는 쿼리 문자열에 사용할 수 없다.   
- Condition: Signed URL이 동작할 조건을 지정한다. Canned URL에서는 DateLess  
Than만 사용할 수 있다.  
  - DateLessThan : 특정 날짜와 시간 이전에만 파일을 사용할 수 있도록 하는 조건  
  이다. AWS: EpochTime으로 설정하면 같은 UTC형식이다. Signed URL에서 Expires  
  파라미터의 값과 이곳에 설정한 값이 동일해야 Signed URL이 동작한다.   
  이 조건은 필수이다.   
  
> #### CloudFront 키 쌍과 AWS 키 쌍  
> CloudFront는 Signed URL을 위해 전용 키 쌍(Key Pair)과 액세스 키(Access Key)가  
> 필요하다. Signed URL을 생성할 때 개인 키를 사용하는데 이 개인 키는 서비스  
> 환경이나 애플리케이션의 구현 방식에 따라 외부에 노출될 수 있다. 따라서 AWS  
> 키 쌍과 분리된 별도의 키와 액세스 키를 사용하는 것이다.    

다음은 Custom Policy를 사용한 Signed URL의 구조이다.   
```
http://d39hltbb812jpd.cloudfront.net/index.html?Policy=eyJTdGF0ZW1lbnQiOlt
7IlJlc291cmNlIjoiaHR0cDovLyoiLCJDb25kaXRpb24iOnsiSXBBZGRyZXNzIjp7IkFXUzpT
b3VyY2VJcCI6IjE4My45OC4zOC4yNDQifSwiRGF0ZUdyZWF0ZXJUaGFuIjp7IkFXUzpFcG9jaF
RpbWUiOjEzOTg2ODg0ODZ9LCJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTM5OTcyM
TU3Nn19fV19&Signature=C5DhUjOWy88Nbnb5s9XZiZtvR1GdP3jYYRzbnRQGtsbqi6wj8v2wd
ASOJv5GZ2dycTwS5Ld1~3h4JWfUo1afj5PBhqzQYlz~JxwK~IoimjiknBiurKU2H273064e4bkbr
~VhLIDMTkIwNgE75UxiZCevtTttCVls7oPMAVOeBU656zu8dAfoTc1cSAP7BMTRzZn9xCqN9lIc9
BTMGxH~Qu49yN4zMDJaQac0PNR60VWpDQQhtg1bPRgskN4XU7uM2DuzuHgSIua85HuzYEXJLOLnj
EXQMdxQo-uK-zTk2QG72SmQ1PCHd8vs7y4Cm7RyRlo3JNtUT~VMUJTy04kduA__&Key-Pair-Id=
APKAJERVTR4A7EO47UYA
```
- http://d39hltbb812jpd.cloudfront.net: CloudFront 배포 URL이다.   
- index.html: CloudFront로 배포되는 파일이다. 여기서는 index.html 파일을 예로   
들었다. JPG, FNG 등의 그림 파일과 ZIP 등의 압축 파일을 비롯한 모든 파일에 적용  
가능하다.   
- Policy: 파일 사용 제한을 정의하는 정책(Policy Statement) 파일의 내용을  
BASE64로 인코딩한 값이다. `+`는 `-`로, `=`는 `_`로 `/`는 `~`로 바뀌어져 있다.   
- Signature: 파일 사용 제한을 정의하는 정책(Policy Statement) 파일의 내용을   
CloudFront 개인 키(Private Key)로 서명한 해시값이다. 이 값은 BASE64로 인코딩   
되어 있으며 `+`는 `-`로, `=`는 `_`로 `/`는 `~`로 바뀌어져 있다.   
- Key-Pair-Id: CloudFront 전용 키 쌍(Key Pair)의 액세스 키(Access Key)이다.   
이 액세스 키는 AWS 계정의 액세스 키와는 별개이다.   
  
다음은 Custom Policy 파일의 내용이다. 이 파일에 정의한 대로 파일의 사용을   
제한한다. 그리고 이 정책 파일의 내용을 CloudFront 개인 키로 서명한 해시 값이   
위에서 말한 Signature 값이다.   
```
{
  "Statement": [{
    "Resource": "http://*",
    "Condition": {
      "IpAddress": { "AWS:SourceIp": 192.0.2.10/32" },
      "DateGreaterThan": { "AWS:EpochTime": 1298688486 },
      "DateLessThan" : { "AWS:EpochTime": 1399721576 }
    }
  }]
}
```
- Resource: 파일의 경로이다. 꼭 `http://`, `https://`, `http*://`로 시작해야  
한다. `http://*`처럼 모든 파일을 지정할 수도 있고, `http://*.pdf`처럼 확장자를   
지정할 수도 있다. 또한, 쿼리 문자열(Query String)도 붙을 수 있다(예: hello.jpg?  
size-large&license=yes). 쿼리 문자열이 없을 경우 맨 뒤 ?는 삭제해야 한다.   
또한, Expires, Signature, Key-Pair-Id는 쿼리 문자열에 사용할 수 없다.   
- Condition: Signed URL이 동작할 조건을 지정한다. **Custom URL에서는 DateLess   
Than, DateGreaterThan, IpAddress를 사용할 수 있다.**     
   - DateLessThan: 특정 날짜와 시간 이전에만 파일을 사용할 수 있도록 하는   
   조건이다. `AWS:EpochTime`으로 설정하며 값은 UTC 형식이다. Signed URL에서   
   Expires 파라미터의 값과 이 곳에 설정한 값이 동일해야 Signed URL이 동작한다.  
   이 조건은 필수이다.   
   - DateGreaterThan: 특정 날짜와 시간 이 후에 파일을 사용할 수 있도록 하는   
   조건이다. `AWS: EpochTime`으로 설정하며 값은 UTC형식이다.   
   - IpAddress: 특정 IP주소에서 파일을 사용할 수 있도록 하는 조건이다. `AWS:  
   SourceIp`로 설정하며 공인 IP를 지정해야 한다. CIDR 형식으로 IP 대역을   
   지정할 수도 있다.  
   
---

# Signed URLs
회사에서 사용하는 이미지들은 보안이 필요하기 때문에 아무나 접근할 수 없도록   
이미지 url에도 인증 절차가 필요하다.   
이럴 때 Signed URLs을 사용하면 된다.   

> Signed URL이란 어떠한 요청을 수행하는데 필요한 제한된 권한과 시간을 제공하는  
> URL이다.   

Signed URL에는 쿼리스트링에 인증정보, 만료 날짜 및 시간같은 추가 정보가 포함  
되므로 콘텐츠에 대한 액세스를 보다 세부적으로 제어할 수 있다.   



























