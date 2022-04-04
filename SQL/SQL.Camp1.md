```sql
SELECT day, ROUND(SUM(total_bill),2)
FROM tips
GROUP BY day

-- window: control + enter / control+ / 
-- 쿼리 결과 저장 가능 
SELECT day, ROUND(SUM(CASE WHEN time='Lunch' then total_bill ELSE NULL END),2) AS lunch
,ROUND(SUM(CASE WHEN time='Dinner' then total_bill ELSE NULL END),2) AS Dinner
FROM tips
GROUP BY day

-- IF /CASE WHEN 차이
IF(조건, 조건이 참, 조건이 거짓)
CASE WHEN 조건 1 THEN 조건 1이 참
    WHEN WHRJS 2 THEN 조건 2RK CKA
    ELSE 그 외 
END 
```

### FROM절 서브쿼리 

```sql
SELECT *
FROM(
  SELECT day
      , SUM(total_bill) sales
  FROM tips
  GROUP BY day
  ) AS daily
  
-- daily라는 테이블로 부르겠다. 

SELECT ROUND(AVG(sales),2) daily_sales_avg
FROM(
  SELECT day
      , SUM(total_bill) sales
  FROM tips
  GROUP BY day
  ) AS daily
  
-- 테이블 안에 테이블을 넣은 것 
-- AS는 생략 가능하지만 Alias 꼭 적어줘야 함 
