## ROLLUP을 대신하는 방법
### 1.UNION ALL  
UNION ALL을 사용할 때는 컬럼 수를 맞춰주자.
```SQL
SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM
      ,T1.CUS_ID
      -- 주문년월, 아이디별 합계가 나온다.
      ,SUM(T1.ORD_AMT) ORD_AMT
FROM   T_ORD T1
WHERE  T1.CUS_ID IN ('CUS_0001', 'CUS_0002')
-- 3, 4월
AND    T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND    T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
-- 주문년월, 아이디별 그룹핑
GROUP BY TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1.CUS_ID
UNION ALL
SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM
      -- Total 옆에 값을 표시하기 위해 임의의 글자를 출력하고 
      -- 맨 위 컬럼명은 CUS_ID으로 나옴
      ,'Total' CUS_ID
      -- 월별 합계가 나온다.
      ,SUM(T1.ORD_AMT) ORD_AMT
FROM   T_ORD T1    
WHERE  T1.CUS_ID IN ('CUS_0001', 'CUS_0002')
AND    T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND    T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
-- 주문년월별 그룹핑
GROUP BY TO_CHAR(T1.ORD_DT, 'YYYYMM')
UNION ALL
        -- Total이라는 글자 출력하고 맨 위 컬럼명은 ORD_YM으로 나옴
SELECT  'Total' ORD_YM
        -- Total이라는 글자 출력하고 맨 위 컬럼명은 CUS_ID으로 나옴
       ,'Total' CUS_ID
       ,SUM(T1.ORD_AMT) ORD_AMT
FROM    T_ORD T1
WHERE   T1.CUS_ID IN ('CUS_0001', 'CUS_0002')
AND     T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND     T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
-- UNION ALL 후에 정렬한다.
ORDER BY 1, 2
```
![image](https://user-images.githubusercontent.com/33191974/146644537-c677808c-646e-4159-86c7-61d34e66d86e.png)

UNION ALL을 사용한 방법은 T_ORD를 세 번 접근하고 있다.  
성능에서 손해를 볼 수 밖에 없다.  

### 2. 카테시안 조인
카테시안 조인은 A테이블 데이터 수 X 테이블 데이터 수만큼 나온다.
```SQL
       -- 아래 GROUP BY에 적은 내용을 그대로 적어줌.
SELECT CASE WHEN T2.RNO = 1 THEN TO_CHAR(T1.ORD_DT, 'YYYYMM')
            WHEN T2.RNO = 2 THEN TO_CHAR(T1.ORD_DT, 'YYYYMM')
            WHEN T2.RNO = 3 THEN 'Total' END ORD_YM
      ,CASE WHEN T2.RNO = 1 THEN T1.CUS_ID
            WHEN T2.RNO = 2 THEN 'Total'
            WHEN T2.RNO = 3 THEN 'Total' END CUS_ID
       -- 1번 데이터 나올 때 합계
       -- 2번 데이터 나올 때 합계
       -- 3번 데이터 나올 때 합계
      ,SUM(T1.ORD_AMT) ORD_AMT
FROM   T_ORD T1
      ,( -- 1, 2, 3이 나온다. X 3
         -- 즉 T_ORD 테이블 모든 데이터 옆에 1번이 붙어서 나오고, 
         -- 그 밑에 T_ORD 테이블 모든 데이터 옆에 2번이 옆에 붙어서 나오고, 
         -- 그 밑에 T_ORD 테이블 모든 데이터 옆에 3번이 옆에 붙어서 나온다. 
         SELECT ROWNUM RNO FROM DUAL CONNECT BY ROWNUM <= 3
        ) T2
WHERE   T1.CUS_ID IN('CUS_0001', 'CUS_0002')
AND     T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND     T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
              -- T2.RNO가 1이면 주문년월, 고객아이디 별로 그룹핑
              -- T2.RNO가 2이면 주문년월, 'Total' 별로 그룹핑
              -- T2.RNO가 3이면 'Total', 'Total' 별로 그룹핑
GROUP BY CASE WHEN T2.RNO = 1 THEN TO_CHAR(T1.ORD_DT, 'YYYYMM')
              WHEN T2.RNO = 2 THEN TO_CHAR(T1.ORD_DT, 'YYYYMM')
              WHEN T2.RNO = 3 THEN 'Total' END
        ,CASE WHEN T2.RNO = 1 THEN T1.CUS_ID
              WHEN T2.RNO = 2 THEN 'Total'
              WHEN T2.RNO = 3 THEN 'Total' END
-- ORD_YM, CUS_ID 별로 정렬           
ORDER BY 1, 2;
```            
![image](https://user-images.githubusercontent.com/33191974/146646407-d87d8234-bd92-4029-8a7a-c45db77d00ca.png)  
RNO 2와 3은 각각 주문날짜별 소계와 전체 합계를 구하기 위해서 존재한다.  






























      
