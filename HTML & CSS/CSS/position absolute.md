# position absolute - 자유로운 엘리먼트 배치
부모 엘리먼트에 구애받지 않고 엘리먼트를 자유롭게 배치시킬수 있는 position absolute에 대해서  
알아보자.  

## 엘리먼트 배치관련 CSS 속성  
CSS의 position 속성은 엘리먼트가 브라우저 화면에 어떻게 배치되는가를 결정한다.  
**기본값은 static이며 relative나 absolute, fixed 등으로 변경이 가능하다.** 이번 포스팅에서  
중점적으로 다룰 position 속성값은 absolute이다. 
  
position 속성을 static이 아닌 다른 값으로 설정했을 때 함께 사용하는 포지셔닝관련 CSS 속성이 있다.  
대표적으로 top, left, bottom, right 등을 들 수 있는데, 웹페이지 상에서 엘리먼트의   
[오프셋(offset)](https://hyo-ue4study.tistory.com/56)(기준에서 얼마나 떨어져있는지)을  
지정하기 위해 사용한다. 예를 들어, top은 위에서 얼마나, left는 좌측에서 얼마나, bottom은 아래에서  
얼마나, right는 우측에서 얼마나 떨어져야 하는지를 결정한다.  

## position absolute의 특징
position 속성이 absolute로 설정되어 있는 엘리먼트는 웹페이지 상에 배치될 때 다음과 같은 특징을  
갖는다.  
- 부모 엘리먼트 내부에 속박되지 않고 독립된 배치 문맥(positioning context)을 가지게 된다.  
마치 포토샵같은 그래픽 툴에서 새로운 레이어를 추가하는 효과에 비슷하다고 생각하면 된다.  
- 따라서, 엘리먼트를 기본적으로 브라우저 화면(viewport) 상에서 어디든지 원하는 위치에 자유롭게  
배치시킬 수 있으며, 심지어 부모 엘리먼트 위에 겹쳐서 배치할 수도 있다.  
- 단, 상위 엘리먼트 중에 position 속성이 relative인 엘리먼트가 있다면, 그 중 가장 가까운 엘리먼트의  
내부에서만 엘리먼트를 자유롭게 배치할 수 있다. 즉, 전체 화면이 아닌 해당 상위 엘리먼트를 기준으로  
offset 속성(top, left, bottom, right)이 적용된다.  


## position absolute의 적용
어떤 엘리먼트의 position 속성을 absolute로 적용하였을 때, 위에서 언급한 특징이 어떻게 발현되는지  
이해하는 것이 매우 중요하다.   
  
예를 들어, 다음과 같이 두 개의 자식을 갖는 부모 엘리먼트가 있다고 가정을 해보자.  
각 엘리먼트를 보기 편하도록 parent와 child 클래스에 간단한 스타일을 적용하였다.   
```html
<style>

</style>
<div class="parent">
  Parent
  <div class="child">Child #1</div>
  <div class="child">Child #2</div>
</div>
```
아직까지는 position 속성을 건들지 않았기 때문에, 부모와 자식 엘리먼트 간의 일반적인 배치 흐름을 가지게  
된다.   
![image](https://user-images.githubusercontent.com/33191974/147537777-7b250ec3-3269-4813-adbd-5f64e0c65f5a.png)   

그 다음, position 속성을 absolute로 변경하기 위해서 abs라는 클래스에 대한 스타일을 추가로 정의한다.     
```html
.abs {
  position: absolute;
}
```
그리고 두번째 자식 엘리먼트에 이 abs 클래스를 적용하는 순간,
```html
<div class="child abs">Child #2"</div>
```
부모 엘리먼트는 이 자식 엘리먼트를 테두리 밖으로 밀어내며 마치 없는 자식 취급을 하게 된다.     
![image](https://user-images.githubusercontent.com/33191974/147538033-61a6e96f-5230-4eb1-95d5-ba613ad5d065.png)  
그런데 여기서 밀려난 자식 엘리먼트는 왜 원래 자리에 멀뚱히 남아 있을까?  
그 이유는 offset 속성을 명시하지 않으면, 기본값인 auto가 적용되는데, 그러면 원래 position 속성이  
static이었을 때의 위치와 일치하도록 offset 속성값이 자동 지정되기 때문이다(아래 css참고).  
```html
.abs {
  position: absolute;
  top: auto;
  left: auto;
  bottom: auto;
  right: auto;
}
```
top과 left 속성값을 각각 0과 10px로 수동 지정해주면   
```html
.abs {
  position: absolute;
  top: 0;
  left: 10px;
}
```
![image](https://user-images.githubusercontent.com/33191974/147538529-3ba595c2-8b84-4f60-b491-137c8c02c0ea.png)  
이 상태에서 인라인 스타일을 통해 부모 엘리먼트의 position 속성을 relative로 변경해보자.  
```html
<div class="parent" style="position: relative">
  <!-- 생략 -->
</div>
```
그러면 positioning context가 전체 화면(viewport)에서 부모 엘리먼트로 변경되어, top과 left 속성이  
적용됨을 알 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/147538703-fd829a01-48e0-43f7-882f-7801d4a2a3d8.png)    














































    
