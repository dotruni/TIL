[(Easy)](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT id
      ,movie
      ,description
      ,rating
FROM Cinema
WHERE (id%2 = 1) AND (description <>"boring")
ORDER BY rating DESC
```

\*description IS NOT "boring" 하면 안된다. IS NOT NULL이 있지만 <>처럼 사용할 수는 없다. 

[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT l.id AS product_id
      ,ROUND(SUM(up)/SUM(un),2)AS average_price
FROM (
      SELECT p.product_id AS id
                        ,start_date
                        ,end_date
                        ,u.purchase_date
                        ,u.units AS un
                        ,p.price
                        ,u.units*p.price AS up
      FROM Prices AS p
      LEFT JOIN UnitsSold AS u ON (u.purchase_date BETWEEN p.start_date AND end_date) AND p.product_id=u.product_id
)l
GROUP BY l.id
```

\*교훈 : 조건절을 잘 걸자 

[1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT p.project_id AS project_id
      ,ROUND(SUM(e.experience_years)/COUNT(e.employee_id),2) AS average_years
FROM Project AS p 
LEFT JOIN Employee AS e ON p.employee_id=e.employee_id
GROUP BY p.project_id
```

[1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT contest_id
      ,ROUND(COUNT(user_id)*100/(SELECT COUNT(user_id) FROM Users AS u),2)AS percentage
FROM Register 
GROUP BY contest_id
ORDER BY percentage DESC , contest_id ASC
```

\-> 다 똑같은 풀이였다.

[1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=top-sql-50)

\* it may have duplicate rows : 중복행이 있을 수 있습니다. 

```
SELECT  query_name
       ,ROUND(SUM(rating/position)/COUNT(1),2) AS quality
       ,ROUND(COUNT(CASE WHEN rating<3 THEN 1 END)*100/COUNT(1),2) AS poor_query_percentage
FROM Queries 
GROUP BY query_name
```

\*average를 아까 푼 문제나 너의 통념을 가지고 풀지 말고 각 문제 풀이 예시를 따르자!

#### **_[(Medium)](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)_**

[1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT SUBSTR(trans_date,1,7) AS month 
       ,country
       ,COUNT(state) AS trans_count
       ,COUNT(CASE WHEN state="approved" THEN 1 END) AS approved_count
       ,SUM(amount) AS trans_total_amount 
       ,SUM(CASE WHEN state="approved" THEN amount ELSE 0 END) AS approved_total_amount 
FROM Transactions
GROUP BY 1,2
```

[1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)

1) 처음 푼것 -> 오류 남 

```
WITH first AS(
SELECT customer_id
     , MIN(order_date) as firstdate
     , customer_pref_delivery_date
FROM Delivery 
GROUP BY customer_id
)
SELECT ROUND( COUNT(CASE WHEN customer_pref_delivery_date=firstdate THEN 1 END)*100
              /COUNT(customer_id) ,2) AS immediate_percentage   
FROM first
```

2) 두밴째  ( Accepted) 

```
SELECT ROUND(AVG(order_date=customer_pref_delivery_date)*100,2)AS immediate_percentage 
FROM Delivery 
WHERE (customer_id,order_date) IN  (SELECT customer_id
                                         , MIN(order_date) as firstdate
                                    FROM Delivery 
                                    GROUP BY customer_id)
```

\* Group by, MIN의 특성을 파악하고 예외 케이스들을 생각하는게 핵심! 

[550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/?envType=study-plan-v2&envId=top-sql-50)

1) 처음 문제의 풀이 ( 오답 ) 

```
WITH a AS(
SELECT COUNT(player_id) AS id 
FROM Activity
WHERE (player_id,event_date) IN (SELECT player_id,DATE_ADD(event_date,INTERVAL 1 DAY) FROM Activity) 
),di AS(
SELECT COUNT(DISTINCT player_id) AS id
FROM Activity
)
SELECT ROUND(a.id/di.id,2) AS fraction
FROM a,di
```

#### _**Sorting and Grouping**_

#### _**(Easy)**_

[2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT teacher_id
      , COUNT(DISTINCT subject_id) AS cnt  
FROM Teacher
GROUP BY teacher_id
```

[1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT activity_date AS day
      ,COUNT(DISTINCT user_id) AS active_users
FROM Activity  
GROUP BY activity_date
HAVING activity_date <='2019-07-27' AND activity_date >DATE_SUB('2019-07-27',INTERVAL 30 DAY)
```

\*헷갈리지 말기, 날짜 DATE와 가깝게 = , INTERVAL으로 주면 둘다 포함 관계 안해야 계산 착오 없음

[596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT class 
FROM Courses
GROUP BY class
HAVING COUNT(student)>=5
```

[1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT user_id
      ,COUNT(follower_id) AS followers_count 
FROM Followers
GROUP BY user_id
ORDER BY user_id
```

\* 조건 하나하나를 빼먹지 말자 

[619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT (CASE WHEN num IS NOT NULL THEN MAX(num) ELSE NULL END)AS num
FROM (SELECT num
      ,COUNT(num) as cnt
      FROM MyNumbers
      GROUP BY num)a
WHERE a.cnt=1
```

다른 사람의 풀이 (IF 를 사용함) 

```
select if(count(*) =1, num, null) as num from number 
group by num 
order by count(*), num desc 
limit 1
```
#### **_[(Medium)](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)_**

[1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/?envType=study-plan-v2&envId=top-sql-50)

1\. 정답 풀이 

```
SELECT customer_id
FROM Customer 
GROUP BY customer_id
HAVING (SELECT COUNT(DISTINCT product_key) FROM Product)= COUNT(DISTINCT product_key)
```

2\. 오답 풀이 

\--> 위 정답 풀이에서 DISTINCT를 뺌

> There is no primary key for this table. It may contain duplicates. customer\_id is not NULL.

\*이 부분을 유심히 보고 중복된 것들을 DISTINCT로 제외해주었어야 함.

[1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)

```
SELECT product_id
      ,year AS first_year
      ,quantity
      ,price
FROM Sales
WHERE (product_id,year)IN (SELECT product_id
                                ,MIN(year) 
                          FROM Sales 
                          GROUP BY product_id)
```

\* 역시.. 복잡하게 풀 필요 없이 WHERE 절로 서브쿼리를 포함해 주면 편하게 결과 값 나옴 여기서 WHERE = ( ) 이렇게 하면 row가 1행 밖에 지정이 안된다는 점 주의! 

\* 항상 unknown column 이 에러 메세지에 뜨면 철자가 틀린 것이 있다는 것..! 내 눈에만 안보이는 오타..! 에러문 근처에 

이렇게 칼럼 끝을 클릭하면 같은 칼럼인 것들이 표시가 된다..! 안 뜨는 것이 있다면 그것이 오타! 

