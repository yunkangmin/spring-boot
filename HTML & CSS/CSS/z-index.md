### z-index
position 속성을 이용하면 요소를 겹치게 놓을 수 있다. 이 때 요소들의 수직 위치를 z-index  
속성으로 정한다. 값은 정수이며, 숫자가 클수록 위로 올라가고, 숫자가 작을 수록 아래로 내려간다.  

#### 예제 1
세 개의 div 요소를 겹치게 배치했다. 코드 상 나중에 입력한 것일수록 위로 올라온다.  
```html
<!doctype html>
<html lang="ko">
    <head>
        <meta charset="utf-8">
        <title>CSS</title>
        <style>
            div {
              width: 100px;
              height: 100px;
              position: absolute;
            }
            div.x {
              background-color: #2196f3;
              top: 20px;
              left: 200px;
            }
            div.y {
              background-color: #1976D2;
              top: 50px;
              left: 260px;
            }
            div.z {
              background-color: #0D47A1;
              top: 80px;
              left: 230px;
            }
        </style>
    </head>
    <body>
      <div class="x"></div>
      <div class="y"></div>
      <div class="z"></div>
  </body>
</html>
```
[코드펜](https://codepen.io/pen/)에서 확인하면 아래와 같이 나온다.  
![image](https://user-images.githubusercontent.com/33191974/147535221-b92f52ae-95b7-4d20-8aed-5f55fa9056eb.png)  
z-index 속성을 추가해서 수직 위치를 역순으로 바꿔보자.  
z-index 속성이 없으면 0처럼 취급한다(z-index의 기본값은 auto이다). 
```html
div.x {
  background-color: #2196F3;
  top: 20px;
  left: 200px;
  z-index: 1;
}
div.y {
  background-color: #1976D2;
  top: 50px;
  left: 260px;
}
div.z {
  background-color: #0D47A1;
  top: 80px;
  left: 230px;
  z-index: -1;
}
```
![image](https://user-images.githubusercontent.com/33191974/147535820-fe0795e7-8898-4a52-8664-4e8e4cd6a560.png)    
