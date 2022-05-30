## ROW_NUMBER 사용법
```SQL
-- ROW_NUMBER를 사용한 순위구하기
-- ROW_NUMBER를 사용하면 순위가 같게 나오지 않는다.
SELECT T1.ID
      ,T1.AMT
      ,RANK() OVER(ORDER BY T1.AMT DESC) RANK_RES
      ,ROW_NUMBER() OVER(ORDER BY T1.AMT DESC) ROW_NUM_RES
FROM  (
      SELECT 'A' ID, 300 AMT FROM DUAL UNION ALL
      SELECT 'B' ID, 150 AMT FROM DUAL UNION ALL
      SELECT 'C' ID, 150 AMT FROM DUAL UNION ALL
      SELECT 'D' ID, 100 AMT FROM DUAL
      ) T1;
```
![image](https://user-images.githubusercontent.com/33191974/146669487-45c77f9b-e30f-4ec2-a25d-833b717fe9c2.png)  
```SQL
-- 3, 4월 주문에 대해 월별로 주문금액 TOP 3 구하기
SELECT T0.ORD_YM
      ,T0.CUS_ID
      ,T0.ORD_AMT
      ,T0.BY_YM_RANK
FROM   (       -- 주문년월
        SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM
               -- 고객아이디
              ,T1.CUS_ID
               -- 주문금액 합계(그룹핑이 3월이면 3월에 A라는 고객이 주문한 합계액이 한 번 나오고  
               -- 4월이면 4월에 A라는 고객이 주문한 합계액이 한 번 나온다.)
              ,SUM(T1.ORD_AMT) ORD_AMT
               -- 순위를 주문년월별로 그룹핑하고 주문금액 합계가 많은 순으로 정렬하여  
               -- 순위를 매긴다.
              ,ROW_NUMBER() OVER(PARTITION BY TO_CHAR(T1.ORD_DT, 'YYYYMM') 
               ORDER BY SUM(T1.ORD_AMT) DESC) BY_YM_RANK
        FROM   T_ORD T1
        -- 3, 4월
        WHERE  T1.ORD_DT >= TO_DATE('20170301', 'YYYYMMDD')
        AND    T1.ORD_DT < TO_DATE('20170501', 'YYYYMMDD')
        -- 주문년월, 고객아이디 별 그룹핑
        GROUP BY TO_CHAR(T1.ORD_DT, 'YYYYMM')
                ,T1.CUS_ID
        ) T0
        -- 인라인 뷰에서 조회된 데이터에서 다시 1, 2, 3위만 조회한다.
        -- 인라인 뷰안에서 3등까지 조회할 수 없으므로 인라인 뷰를 사용했고 
        -- 메인쿼리 조건절에서 3등까지만 조회한 것이다.
WHERE   T0.BY_YM_RANK <= 3
         -- 주문년월
ORDER BY T0.ORD_YM
         -- 순위
        ,T0.BY_YM_RANK;
```        
![image](https://user-images.githubusercontent.com/33191974/146669921-414a304e-157d-4692-a5e7-675c607144ee.png)  

## 고객별 마지막 주문조회하기
```SQL
SELECT T2.*
FROM   (
        SELECT T1.*
               -- 순위를 매기는데 고객아이디별로 그룹핑하고  
               -- 주문년월이 최근이고 주문아이디가 큰 순서대로 정렬하여 순위를 매긴다.
               -- 서브쿼리에서 그룹핑을 안했기 때문에 조회 결과에는 고객아이디별로  
               -- 그룹핑되서 결과가 나오지 않고 ROW_NUMBER()에서만 고객아이디별로  
               -- 그룹핑하고 주문년월이 최근이고 주문아이디가 큰 순서대로 정렬하여 순위를 매긴다.
               -- 서브쿼리만 조회 시 결과데이터는 A,B..Z라는 고객의 모든 주문데이터가 나오고  
               -- 고객별로 주문년월이 최근이고 주문아이디가 큰 순서대로 순위가 매겨져서 나온다.
              ,ROW_NUMBER()
                  OVER(PARTITION BY T1.CUS_ID ORDER BY T1.ORD_DT DESC
                                                      ,T1.ORD_SEQ DESC) ORD_RNK
        FROM   T_ORD T1
        ) T2
        -- 서브쿼리에서 나온 결과에서 1인 것(마지막 주문만 나온다.)
WHERE   T2.ORD_RNK = 1;
```
![image](https://user-images.githubusercontent.com/33191974/146670342-c2e5cd7b-9c0b-4ace-9a11-2fd04dda44fe.png)
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
              
