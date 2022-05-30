##### S3 Access Point 소개(기존에 단일 정책을 여러개로 쪼개서 관리하게 해준다.)     
S3 Access Point는 기존에 단일 정책(Bucket Policy)으로 수행하던 접근제어를   
개별 정책으로 나누어 관리할 수 있게 해주는 서비스이다. 무슨 의미가 있냐라고  
반문할 수 있겠지만, 자세히 생각해보면 그 효용은 생각보다 크다. Bucket Policy를  
통해 관리하게 되면 단일 정책이므로 크기가 커질수록 점점 복잡해지게 된다.  
이해하고 분석하기 어렵고 시간이 걸린다는 이야기이다. 이를 쪼개어 다수의   
정책으로 나눈다면 훨씬 이해하기 쉽고 효율적으로 정책을 설정할 수 있을 것이다.   
  
AWS에서는 이 기능을 "S3 공유 데이터에 접근하는 애플리케이션의 대규모 액세스를   
간단하게 관리하는 서비스"라고 소개하고 있으며 다음과 같은 장점을 내세우고 있다.   
- 각각의 애플리케이션에 대한 권한 커스터마이징이 쉽다.
- 개인 네트워크 내의 데이터 접근을 VPC로 제한할 수 있다.   
- 하나의 Bucket에서 수백개의 Access Point를 쉽게 만들 수 있다.   

# S3 Access Point 데모  
우선 액세스 포인트를 먼저 생성해보자.  
S3 버킷으로 이동 후 "액세스 지점 생성" 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/155849394-14d14e43-dedc-4b03-a217-03d5e5e6d29b.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/155849425-d12f3d0a-ac10-47c0-ada0-0ac248602a68.png" width="50%" height="50%"/>    
다음으로 사용할 액세스 포인트의 이름을 작성하고, 네트워크 액세스 유형을   
선택한다. 유형에 따라 추가적인 차단 옵션을 선택하고 마지막으로 Access Point  
Policy를 작성한다. 이 부분이 기존의 Bucket Policy와 동일하다고 보면 된다.   
<img src="https://user-images.githubusercontent.com/33191974/155875115-b8f96e1b-bc90-4b86-b7cb-f37c73935922.png" width="50%" height="50%"/>      
<img src="https://user-images.githubusercontent.com/33191974/155875682-bf7031d9-494f-406e-9e8b-41383f0150ed.png" width="50%" height="50%"/>   
```
{
    "Version":"2012-10-17",
    "Statement": [
    {
        "Effect": "Allow",
        //Alice 계정에 대한 액세스 지점 정책 설정
        "Principal": {
            "AWS": "arn:aws:iam::123456789012:user/Alice"
        },
        "Action": ["s3:GetObject", "s3:PutObject"],
        //Alice 폴더 아래 모든 객체 대상에 대하여 해당 정책을 실행한다.
        "Resource": "arn:aws:s3:us-west-2:123456789012:accesspoint/my-access-point/object/Alice/*"
    }]
}
```  
여기까지 잘 진행했다면 아래와 같이 액세스 포인트가 잘 만들어져 있는 것을    
확인할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/155875747-1a6432c8-44d1-4f8e-a622-98bb47003727.png" width="50%" height="50%"/>      
이제 만들어진 액세스 포인트를 사용해보자. 액세스 포인트를 사용할 Bucket을   
선택한 후 "액세스 지점" 탭으로 이동한다. 이동하면 방금 만든 액세스 포인트를   
확인할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/155876018-1e446d43-3077-4afc-a2f1-b5f0bf620e3c.png" width="50%" height="50%"/>   
   
실제 보여줄 데모는 여기까지이다. 액세스 포인트가 잘 동작하는 것을 확인할 수  
있다. 액세스 포인트의 생성과 실행을 확인했으니, 이제 앞에서 말씀드린 효율성에  
대해 이야기해보자.  

# 마치며
이런 상황을 가정해보자. 3명의 유저가 모두 다른 권한을 가진 상태로 S3에 접근해야  
한다. 기존 Bucket Policy에서는 하나의 Policy를 만들어 관리를 해야한다. 예를  
다음과 같은 Bucket Policy가 만들어질 수 있다.   
```
{
  "Version": "2012-10-17",
  "Statement": [
  ///////1
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam:${ACCOUNT_ID}:user/A"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::${S3_BUCKET_A}/*"
      "Resource": "arn:aws:s3:::${S3_BUCKET_B}/*"
    },
    ///////2
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::${ACCOUNT_ID}:user/B"
      },
      "Action": {
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListObjectsV2"
      },
      "Resource": "arn:aws:s3:::${S3_BUCKET_B}/*"
      "Resource": "arn:aws:s3:::${S3_BUCKET_C}/*"
      "Condition": {"aws:SourceIp": "192.168.10.10/32"}
    },
    ///////3
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::${ACCOUNT_ID}:user/C"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBuckets"
      },
      "Resource": "arn:aws:s3:::${S3_BUCKET_A}/*",
      "Resource": "arn:aws:s3:::${S3_BUCKET_C}/*"
    ]
}
```
수 많은 권한 중 중첩되는 몇 가지만 예시로 작성해보았다. 만약 이러한 형식으로  
유저 10명이라면? 100명이라면? 점점 더 복잡한 구성이 필요하게 된다. 이 때 액세스  
포인트를 사용하게 된다면 보다 쉬운 구성으로 유연하게 Policy를 관리할 수 있게   
된다.   
  
데모 마지막에 언급했듯이 이 서비스의 의도는 결국 접근 유저별 맞춤형 정책을 지원  
하겠다는 것이다. 물론 하나의 Bucket Policy를 사용하는 경우가 더 효과적인 경우도  
존재하겠지만 여러 옵션을 고려해볼 수 있다는 점에서, 그리고 인프라가 거대해질수록  
필요해질 서비스라는 점에서 사용을 고려해볼만한 서비스라고 생각한다.   
  
이 외에도 CloudFormation과 SDK, CLI 등을 지원하고 있어서 더 편하게 사용   
가능하며, 이 외에도 VPC로만 S3 액세스를 제한할 수 있어 보안적인 부분도 고려해서  
사용 가능하다.   
  
다만 다음과 같은 제약사항이 있다.  
- 계정당 1000개까지만 생성 가능
- 계정에서 소유한 버킷만 생성 가능
- 액세스 포인트 당 하나의 버킷만 매칭 가능
- 액세스 포인트 생성 후에는 VPC 변경이 불가능  

크게 불편할만한 제약사항은 없어 보인다. 또한 이 서비스는 무료로 제공된다.   
하나의 Bucket Policy로 정책을 관리하는 것에 어려움을 느낀다면, 이 서비스를   
사용해 보는 건 어떨까? S3 Access Point 소개 포스팅은 여기서 마무리 하도록 한다.  






















https://www.wisen.co.kr/pages/blog/blog-detail.html?idx=11987






















