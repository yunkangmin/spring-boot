IAM은 앞서 설명한 주요 기능들 이외에도 다양한 기능을 제공한다. 이 부분은   
간략한 설명으로 대신한다.   
- Identity Providers: SAML Provider를 생성한다.  
- Password Policy: IAM 사용자의 비밀번호 정책을 설정한다. 대소문자, 숫자,   
특수문자 포함 여부를 설정할 수 있고, IAM 사용자가 자기 자신의 비밀번호를   
변경하는 것을 허용할 지 여부를 설정할 수 있다.  
- MFA 설정: IAM 사용자가 로그인할 때 비밀번호와 OTP(One-Time-Password)를  
동시에 사용하도록 설정하여 보안성을 높인다. 이 MFA 설정은 AWS 계정(root)에도   
설정할 수 있다(AWS 콘솔의 Security Credentials 페이지). 추가할 수 있는 MFA  
장치(Multi-Factor Authentication Device)는 두 가지가 있다.   
   - 가상 MFA 장치: 스마트폰 애플리케이션 형태의 OTP 생성기이다. Google Authen  
   tication(Android, iPhone, Blackberry), AWS Virtual MFA(Android), Authenti  
   cation(Windows Phone)등이 있다.   
   - 하드웨어 MFA 장치: 하드웨어 형태의 MFA 장치이다. Gemalto에서 판매하고   
   있으며 가격은 12.00USD이다.   
   
   





















