## float의 성격
- block의 성격을 무시한다.
- 형제 구조로 이루어져 있다.
- 해당 요소들의 높이 값이 맞아야 미디어쿼리 적용 시 제대로 동작한다. 

## float 속성값
- float : left 왼쪽으로 배치
- float : right 오른쪽으로 배치
- float : none 좌우 어느 쪽으로도 배치하지 않는다. 

![image](https://user-images.githubusercontent.com/33191974/146766042-54e18232-d067-4db6-91a5-dd544014ac3b.png)   
#red {  
  float: left;  
}  
![image](https://user-images.githubusercontent.com/33191974/146766151-97e6078b-a562-4fa1-8ecc-9c426dd49c29.png)  
![image](https://user-images.githubusercontent.com/33191974/146766233-92af3279-1a2c-41f8-83ad-631b8ffdd29d.png)  
#green {  
  float: left;  
}     
![image](https://user-images.githubusercontent.com/33191974/146766315-8f73e93e-1d36-40ed-bf5a-52bdae471364.png)  
전체 다 float: left;  
![image](https://user-images.githubusercontent.com/33191974/146766404-41101c1d-c50f-43fb-95c4-fe805517084b.png)  
#blue {  
  float: left;  
  clear: left;  
}  
![image](https://user-images.githubusercontent.com/33191974/146766542-309974d5-c1ba-4750-acff-002751a4b198.png)  
#yellow {  
  float: left;  
  clear: left;  
}  
![image](https://user-images.githubusercontent.com/33191974/146767020-3557ef1e-2ff9-46c9-b918-c6bfc896cdf8.png)    

## clear 속성 : float 속성 해제하기
clear: both는 취소하다라는 개념으로 float: left/right와 짝꿍 개념이다.  
**float 속성을 적용하면 그 이후에 오는 다른 요소들까지 똑같은 속성이 전달되어  
둘러싼 형태가 되거나 부유된 영역 아래(under)로 들어가게 된다.**  
float 속성을 더 이상 사용하지 않고 그 전으로 되돌리고 싶다면 clear: both를 사용하면 된다.  






