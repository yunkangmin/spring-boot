앞에서 생성한 ELB 로드 밸런서를 도메인에 연결한다(도메인을 구입하지 않았다면   
이 부분은 넘어가도 된다). 모바일 게임을 출시할 때는 AWS 리소스에 도메인을   
연결하는 것을 권장한다. AWS에 장애가 발생했거나 실수로 CloudFront 배포, ELB  
로드 밸런서를 삭제했을 때에 대비할 수 있다. 도메인을 연결하면 게임을 다시   
배포하지 않고 도메인 설정만 바꾸면 간단히 해결된다.  
  
게임 서버용 ELB 로드 밸런서에 회사의 서브 도메인(예: examplegame.slkjfd.cf  
)을 연결한다. A - IPv4 address를 선택하고 Alias Target 부분을 클릭하면  
연결할 수 있는 AWS 리소스가 표시된다. 여기서 방금 생성한 ELB 로드 밸런서를  
선택한다.   
  
게임 서버용 ELB 로드 밸런서에 도메인 연결   
<img src="https://user-images.githubusercontent.com/33191974/160239070-e0e35dbe-d79c-4dcc-885b-e98f718af920.png" width="50%" height="50%"/>    
  
게임 리소스(이미지, 사운드, 게임 데이터) 업데이트용 CloudFront 배포를  
생성했다면 CloudFront 배포에도 도메인을 연결한다.   






















