# git HEAD 정리
- HEAD는 현재 내가 바라보고 있는 commit을 가리킨다.   
- `git checkout HEAD`는 HEAD가 현재 브랜치의 가장 최신 커밋을 가리키게 한다.    

---

# git HEAD, HEAD 포인터란?
git을 처음사용하면, head는 현재 바라보고 있는 커밋을 가리키는 건지, 현재 브랜치  
이름을 가리키는 건지 헷갈리는 경우가 있을 것이다.   
  
HEAD는 현재 체크아웃된 브랜치의 가장 최신커밋을 가리킨다.   
따라서 branch를 변경하게 되면, 변경한 branch의 가장 최신 commit을 가리키게   
된다. 이렇게 될 경우, 보통 HEAD는 브랜치 이름을 가리키도록 표시된다.   
(커밋 옆에 브랜치 이름은, 해당 브랜치의 가장 최상단 커밋에 표시된다.)    
<img src="https://user-images.githubusercontent.com/33191974/155531634-5c1bc3d0-3a1c-4948-98f8-1c0733983d35.png" width="50%" height="50%"/>  
예시를 통해 간단히 살펴보자.  

> main 브랜치에서 "상품정보" 커밋을 하고, elice 브랜치를 생성하고, elice    
> 브랜치로 checkout했다. 이후 "Add m2.js" 커밋을 한 상태이다.  
>  
> 현재 elice 브랜치에 있지만, main 브랜치의 커밋이 보이는 이유는, main 브랜치   
> 에서 "상품정보" 커밋 이후 갈라져 생성되었기 때문이다.  
> 브랜치가 갈라진 이후에는 각 브랜치가 독립적이므로, 다른 브랜치에서 작업한   
> 커밋이 보이지 않는게 당연하다.   

> 따라서 HEAD -> elice의 뜻은, 현재 체크아웃되어있는 브랜치는 elice이고, 현재  
> HEAD가 "Add m2.js" 커밋을 바라보고 있다라는 의미이다.  

#### HEAD는 현재 내가 바라보고 있는 commit을 가리킨다. 
라고 생각하면 된다. 다만, HEAD가 현재 브랜치의 가장 최상단 커밋을 바라보고   
있다면 HEAD -> 브랜치명으로 표시될 뿐이다.    
HEAD -> elice와 같은 것을 보고, HEAD는 브랜치를 가리키는 것 아닌가?라고 착각  
할만한 요소가 분명히 있는 것 같다.   
따라서 브랜치명과 분리된(?) 모양의 HEAD를 보자.   
아래는 elice 브랜치에서, "상품정보" 커밋으로 checkout한 경우이다.  
<img src="https://user-images.githubusercontent.com/33191974/155533245-754ca71a-19fa-42fe-be03-d035d14bb207.png" width="50%" height="50%"/>   
HEAD는 현재 과거 커밋을 바라보고 있으므로, "Add m2.js" 커밋이 보이지가 않는다.  
`git log --all`을 통해, 전체 커밋을 보면 아래와 같다.   
<img src="https://user-images.githubusercontent.com/33191974/155533981-aa4e1bbf-93b5-4df7-8831-a4331e924ef8.png" width="50%" height="50%"/>     
위에서 보이는 것처럼 HEAD는 현재 보고있는 커밋을 가리킬뿐, 현재 유저가 어느  
브랜치에 checkout 했는지에 대한 정보는 없다.  







----



HEAD는 현재 체크아웃된 커밋을 가리킨다. checkout 명령어는 커밋(해시값)에 사용해도   
되고 브랜치에 사용해도 된다. 브랜치에 사용하면 HEAD와 브랜치 모두 최신 커밋을   
가리킨다. 보통 HEAD는 브랜치 안에 숨어 있다. `git checkout C2(커밋해시값)`과    
같이 HEAD로 checkout하면 브랜치 아래 숨어있던 HEAD가 보인다.   
HEAD를 분리한다는 것은 HEAD를 브랜치 대신 커밋에 붙이는 것을 의미한다.   


#### checkout 명령을 사용하기기 전
```
HEAD -> master -> C2
```
#### 명령을 사용하면  
```
HEAD -> C2
```

# git reset
현재 브랜치에서 HEAD만 다른 커밋으로 옮기기

# git restore
워킹트리나 스테이징 되돌리기


























