현재 master, exp라는 branch가 있을 때,   
exp를 master에 병합할 경우 master 브랜치로 이동하고 exp를 병합한다.   
```
$ git checkout master
$ git merge exp
```
<img src="https://user-images.githubusercontent.com/33191974/155521933-9acde918-3909-4eda-b050-4d543b0efce4.png " width="50%" height="50%"/>     
commit 5는 원래 마스터가 가지고 있는 내용. 3, 4가 exp 꺼였는데 master에 병합된   
내용.   
  
master를 exp로 병합할 경우   
```
$ git checkout exp
$ git merge master
```
<img src="https://user-images.githubusercontent.com/33191974/155522211-dfeb204f-a0ab-4fdc-be3c-42ad6a774db5.png" width="50%" height="50%"/>  

master를 exp에 병합하면 exp와 master의 내용이 똑같아 지고 같은 위치(head)에  
존재한다.   
그렇다면 master와 동일한 내용을 갖고 있는 exp를 삭제해보자.   
```
$ git checkout master
$ git branch -d exp
```
<img src="https://user-images.githubusercontent.com/33191974/155527615-6a8ec32e-1edc-4838-bb84-20c7fd6e8b32.png" width="50%" height="50%"/>    
이제는 master branch만 존재하는 것을 확인할 수 있다.   

# fast forward와 merge commit 차이점   
fast forward의 예  
<img src="https://user-images.githubusercontent.com/33191974/155527793-693ddb5a-4b39-40fb-8ade-e8638261117d.png" width="50%" height="50%"/>   
위의 그림을 해석해보자면 master는 C0, C1, C2 순으로 커밋했다.  
이후 급한 일을 처리하기 위해 hotfix 브랜치를 만들고, C4커밋, iss53 브랜치를 만들고  
C3 커밋을 했다.   
  
그 이후에 master는 새로운 커밋을 하지 않은 상태이다.     
<img src="https://user-images.githubusercontent.com/33191974/155528133-5ae36623-9092-4e44-b29d-f7ee9bd4b0d7.png" width="50%" height="50%"/>  
이 때, C2 커밋 상태에 있던 master에 hotfix를 병합하려고 할 경우 fast forward가 일어난다.   
서로 다른 상태를 병합하는 것이 아니고 master를 hotfix 위치로 이동만 해도 되는   
상태이기 때문에 별도의 머지를 위한 커밋이 발생하지 않는다.   
   
# 이번에는 merge commit이 될 경우를 살펴보자.   
<img src="https://user-images.githubusercontent.com/33191974/155528450-602e8f05-288d-4ace-a8a5-0fcc4235188a.png" width="50%" height="50%"/>  
iss53이 커밋 C5까지 된 상태이고, C4에는 hotfix branch가 삭제되고 master만  
존재하는 상태이다.     
<img src="https://user-images.githubusercontent.com/33191974/155528583-86edf894-bfda-4c6f-b4fd-561a3f0e8c40.png" width="50%" height="50%"/>   
iss53을 master에 병합해보려고 한다. 아예 다른 결과물을 갖고 있는 master와 iss53은 merge되어 C6이라는 commit을   
만들게 된다.     
이렇게 병합할 때 새로운 commit을 생성할 경우를 merge commit이라고 한다.     

# 정리
merge commit은 서로 다른 상태의 브랜치를 병합해서 새로운 commit을 만드는 경우이고   
fast forward는 동일 내용이 포함되는 브랜치일 경우 브랜치 이동만으로 병합해서   
따로 commit을 생성하지 않는 경우이다.  













































