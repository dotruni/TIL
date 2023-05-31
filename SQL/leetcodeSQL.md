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
