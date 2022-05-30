## LAG, LEAD 분석함수 사용법
LAG는 자신의 이전 값을, LEAD는 자신의 이후 값을 가져오는 분석함수이다.  
사용법은 아래와 같다.  
- LAG(컬럼명, offset) OVER([PATITION BY ~] ORDER BY ~)
- LEAD(컬럼명, offset) OVER([PARTITION BY ~] ORDER BY ~)
- offset: 현재 로우에서 몇 로우 이전 또는 몇 로우 이후를 뜻한다.  

```sql
SELECT T1.CUS_ID
      ,SUM(T1.ORD_AMT) ORD_AMT
      -- 3월 총주문금액(ORD_AMT)이 많은 순(DESC(내림차순))으로 순위매기기
      ,ROW_NUMBER() OVER(ORDER BY SUM(T1.ORD_AMT) DESC) RNK
      -- 3월 총주문금액이 많은 순서에서 1로우 이전 고객아이디 조회
      ,LAG(T1.CUS_ID, 1) OVER(ORDER BY SUM(T1.ORD_AMT) DESC) LAG_1
      -- 3월 총주문금액이 많은 순서에서 1로우 이후 고객아이디 조회
      ,LEAD(T1.CUS_ID, 1) OVER(ORDER BY SUM(T1.ORD_AMT) DESC) LEAD_1
FROM   T_ORD T1
-- 3월 주문데이터
WHERE  T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND    T1.ORD_DT < TO_DATE('20170401', 'YYYYMMDD')
-- 고객아디가 CUS_0020, CUS_0021, CUS_0022, CUS_0023
AND    T1.CUS_ID IN ('CUS_0020', 'CUS_0021', 'CUS_0022', 'CUS_0023')
-- 그룹핑
GROUP BY T1.CUS_ID;
```
![image](https://user-images.githubusercontent.com/33191974/146642473-8be03309-276c-4aea-ac3c-a519b10725bd.png)  

## 주문년월 별 주문금액에 전월 주문금액 같이 표시하기  
아래 SQL을 활용하여 이전 대비 현재 월의 증가율을 손쉽게 구할 수 있다.  
``SQL
SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM
      ,SUM(T1.ORD_AMT) ORD_AMT
      -- 주문년월 오름차순 순서에서 1로우 이전 주문금액을 같이 조회
      ,LAG(SUM(T1.ORD_AMT), 1) OVER(ORDER BY TO_CHAR(T1.ORD_DT, 'YYYYMM') ASC) BF_YM_ORD_AMT
FROM   T_ORD T1
-- 주문유형이 회사
WHERE  T1.ORD_ST = 'COMP'
-- 월별로 그룹핑
GROUP BY TO_CHAR(T1.ORD_DT, 'YYYYMM');
```
![image](https://user-images.githubusercontent.com/33191974/146642619-bb27abf7-951b-44ab-9256-ae3f6992d65d.png)  


      

  
