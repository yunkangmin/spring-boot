## GROUPING SETS를 사용하여 ROLLUP 대신하기
```SQL
SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM
      ,T1.CUS_ID
      ,COUNT(*) ORD_CNT
      ,SUM(T1.ORD_AMT) ORD_AMT
FROM   T_ORD T1
-- 3월, 4월이고
WHERE  T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
AND    T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
-- 고객아이디가 CUS_OO61, CUS_0062인 고객
AND    T1.CUS_ID IN('CUS_0061', 'CUS_0062')
GROUP BY GROUPING SETS(
          -- GROUP BY 기본 데이터
          (TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1_CUS_ID)
          -- 주문년월별 소계
         ,(TO_CHAR(T1.ORD_DT, 'YYYYMM'))
          -- 고객아이디별 소개
         ,(T1.CUS_ID)
          -- 전체합계
         ,()
       );
```
![image](https://user-images.githubusercontent.com/33191974/146642187-4c16bddb-453c-4aed-8b07-904be70f04cd.png)
