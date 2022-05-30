EMP 테이블을 확인해보면 해당 사원의 관리자가 있다.  
그 관리자가 최종 관리자일 수도 있지만 그 관리자도 더 상위 관리자가 있을 수 있다.  
하지만 결국 마지막 최종 관리자가 있을 것이고 그 최종 관리자는 더 이상 자신의 관리자는  
없기에 관리자 컬럼은 NULL일 것이다.  
```sql
  SELECT *
    FROM EMP
ORDER BY MGT DESC;
```
![image](https://user-images.githubusercontent.com/33191974/146645303-f2a78468-3b1a-474b-8737-6dc5fdfa7cea.png)  
### START WITH
그래서 시작을 최종(최고) 관리자부터 해야 하기에  
START WITH에서 관리자 컬럼이 NULL인 부분을 조건식으로 넣게 된다.  
EMP에서 관리자 컬럼 MGR을 보면 ENAME = 'KING'이 NULL로 되어 있다.  
최종 관리자라는 의미다.  

### CONNECT BY 
CONNECT BY는 연결 고리를 가지고 목록을 가져온다.  
먼저 START WITH에서 조건에 맞는 최상위 행을 가져온다.  
이제 최상위 행 하나를 갖게 되었다.  
다음으로 최상위 행을 관리자로서 갖는 다음 계층 데이터를 가져와야 한다.  
그럼 현재 찾아 온 최상위 관리자의 EMPID를 추출해서 다음 행들을 구해야 한다.  
그 최상위 EMPID를 MGR로 갖는 행들을 찾아야 한다. 
```SQL
CONNECT BY PRIOR EMPID = MGR
```
연결하는 방식은 미리 구한 앞 행의 EMPID와 구해야 할 MGR이 같은 행들을 구한다.  
그럼 이제 START WITH에서 구한 최상위 행과 그 행의 EMPID를 MGR로 갖는 행들을 갖게 되었다.  
이제 다음 작업은 최상위 행으로 구한 두번째 계층의 행들을 가지고 그 계층들의 EMPID를 MGR로  
갖는 다음 계층의 행들을 구한다. 
```SQL
CONNECT BY PRIOR EMPID = MGR
```
연결 방식은 바로 전에(PRIOR)의 구한 계층의 EMPID를 MGR로 갖는 데이터를 구한다.  
  
만약 PRIOR을 반대로 설정하면 어떻게 될까? 
```SQL
CONNECT BY EMPID = PRIOR MGR
```
연결 방식은 바로 전에(PRIOR)의 구한 계층의 MGR를 EMPID로 갖는 데이터를 구한다.  
START WITH로 먼저 최상위 행을 구했는데 그 다음 구할 계층이 최상위 행의 MGR(최상위이니 
NULL이다.)을 EMPID로 갖는 값은 없다.



