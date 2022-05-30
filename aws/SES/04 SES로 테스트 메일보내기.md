프로덕션 액세스 권한을 얻었으므로 이제 SES를 이용하여 외부로 메일을  
보낸다. 왼쪽 메뉴에서 이메일 목록(Verified Senders -> Email Address)을  
클릭한다. 그리고 메일을 보낼 이메일을 선택하고 위쪽 Send a Test Email  
버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158934578-56aade42-b29b-47e9-b778-0c3f59559231.png" width="50%" height="50%"/>  
  
이메일 주소로 테스트 메일을 보낸다.  
- Email Format: 이메일 포멧이다. 기본값 그대로 사용한다.   
   - Formatted: HTML 포맷으로 메일을 보낸다.  
   - Raw: 일반 텍스트로 메일을 보낸다.  
- Subject: 메일의 제목이다. 여기서는 Hello를 입력한다.  
- Body: 메일의 내용이다. 여기서는 Hello SES를 입력한다.  

설정이 완료되었으면 Send Test Email 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158936414-8cb9261e-a4fb-4b81-961a-4eb5473cdf4b.png" width="50%" height="50%"/>  
  
이메일 주소(hvs123@naver.com)의 메일함에 방금 보낸 메일이 도착했다. (DKIM을   
사용하지 않으면 메일을 보낸 실제 도메인이(예) us-west-2.amazonses.com) 표시  
된다). 참고로 포털 사이트 도메인의 이메일 주소는 DKIM을 사용할 수 없다.  
<img src="https://user-images.githubusercontent.com/33191974/158936597-816fbfb4-b359-49a0-a19a-04b71aa98bea.png" width="50%" height="50%"/>  























