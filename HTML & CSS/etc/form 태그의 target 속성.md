form 태그의 target 속성은 submit 후 결과를 어디에 뿌려줄지를 결정한다.  
`_black`, `_self`, `_top`, `_parent`, name 이렇게 5가지 값이 올 수  
있다.  
  
- `_self`: 현재 창에 결과를 보여준다.(기본값)  
- `_black`: 브라우저 새탭에 결과를 보여준다.   
- `_top`: 브라우저 최상단 창에 결과를 보여준다.(상위 창에 없으면 = `_self  
`)  
- `_parent`: 부모창에 결과를 보여준다.(부모창 없으면 = `_self`)  
- `name`: name 속성을 가지고 있는 엘리먼트와 값이 같은 엘리먼트에 결과를  
보여준다.  
  
예제)  
html   
```html  
<iframe name="myFrame"></iframe>
<form name="form" action="test.jsp" method="POST" target="popupName"></form>
```
  
javascript  
```javascript
window.open("", "popupName", "width=100, height=100");
document.form.submit();  
```
위처럼 form의 target 값으로 "popupName"을 주면 post로 날린 요청의 결과를   
윈도우 팝업창에서 볼 수도 있고, "myFrame"으로 주면 iframe에다 결과를 뿌려  
주게 된다.   
(참고로 동적으로 만든 form은 꼭 appendChild로 DOM 트리에 붙여줘야 submit  
()을 할 수 있다)  


































