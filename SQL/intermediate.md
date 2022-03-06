#### 데이터리안_백문이불여일타_SQL 중급 강의를 듣고 저만의 방식으로 기록한 내용입니다. 
프로그래머스 SQL 고득점 키트도 뿌셔뿌셔! 

### 집계함수 
Data 예시 (w3schools 의 Products 데이터셋) 

![image](https://user-images.githubusercontent.com/89775352/156735240-d7bc8d1d-d475-4496-bddd-be9324b890d3.png)

```sql
-- COUNT --
SELECT COUNT(*)--Product에 있는 행들, 데이터 레코드의 개수들을 세준다.
FROM Products

SELECT COUNT(Price) -- Price 열에 결측치가 없으면 위에 COUNT(*)과 같은 값을 반환, 결측치 있으면 결측치를 제외한 값을 반환  
FROM Products

SELECT DISTINCT SupplierID
FROM Products
--Supplier ID들을 중복된 값들 빼고 다 나열한다. ex) 1,2,3,~~29

SELECT COUNT (DISTINCT SupplierID)
FROM Products
--Supplier ID들을 중복된 값들 빼고 센다. ex) 29
--'그러니까 이 제품들 공급자가 총 몇명이야?'

-- SUM --
SELECT SUM (Price) __제품 가격의 합계 
FROM Products

-- AVG --
SELECT AVG (Price) -- 제품 가격의 평균 (주의: Null 값은 없는 것으로 치는 것, 나누는 몫에 안넣어줌) 
FROM Products

SELECT SUM (Price)/COUNT(*) -- 제품 가격의 평균 (Null 값을 0으로 치는 것, 나누는 몫에 넣어줌) 
FROM Products

-- MIN/MAX --
SELECT MIN (Price)
FROM Products

SELECT MAX (Price)
FROM Products

-- 다함께 쓸 수 있다 --
SELECT COUNT(price),SUM(price),AVG(Price),MIN(Price),MAX(Price)
FROM Products
```

### GROUP BY & HAVING
```sql
SELECT SupplierID, AVG(Price)
FROM Products
GROUP BY SupplierID 
-- SELECT하고 GROUP BY 해주는 [기준 컬럼] 무조건 적고, [보고싶은 집계함수] 적는다.

-- 이런식으로 나옴 
SupplierID	        AVG(Price)
1	                15.666666666666666
2               	20.35
3	                31.666666666666668

-- 여러가지 기준으로도 쓸 수 있음 (ORDER BY 처럼) --
SELECT SupplierID, Categoryid, AVG(Price)
FROM Products
GROUP BY SupplierID, Categoryid  

-- 개인의 취향이지만, 이렇게 띄어쓰면 기분도 좋고 다시 보기도 좋음 
-- 파이썬 처럼 띄어쓰기가 크리티컬한 것은 아니지만 협업할 때 나만 알아 볼 수 있게 하면 어려움 
SELECT SupplierID, 
       Categoryid, 
       AVG(Price)
FROM Products
GROUP BY SupplierID, Categoryid  

-- Mysql 은 숫자도 지원한다.
SELECT SupplierID, 
       Categoryid, 
       AVG(Price)
FROM Products
GROUP BY 1,2 -- supplierid, categoryid 로 그룹바이해달라 라는 말
-- 하지만 대부분의 경우 숫자는 안쓰는게 명확하게 나타내기에 좋다. 

-- ORDER BY도 같이 쓸 수 있다.
-- 예를 들어 평균 가격이 가장 낮은/높은 순으로 보고 싶다 할때. 
SELECT SupplierID, 
       Categoryid, 
       AVG(Price)
FROM Products
GROUP BY SupplierID, Categoryid  
ORDER BY AVG(Price)

-- WHERE 조건절은 같이 못쓴다. 
SELECT SupplierID, 
       Categoryid, 
       AVG(Price)
FROM Products
WHERE AVG(Price) >= 100 _이렇게 하면 에러뜬다(해보니까)
GROUP BY SupplierID, Categoryid  
-- 전체에서 100불 이상인 테이블 모으고 그룹 바이 하려고 해진다..그렇다!!
-- 그러면 어떻게..? 신에겐 아직 HAVING 이 있습니다!!

--두둥
-- HAVING --
SELECT SupplierID, 
       Categoryid, 
       AVG(Price)
FROM Products
GROUP BY SupplierID, Categoryid  
HAVING AVG(Price) >=100 -- 정상 작동 
-- 그룹 바이 다음 HAVING 이 와서 순서가 맞다. 

-- Alias 활용하기 (파이선 import numpy as np랑 비슷한듯..?)
SELECT SupplierID, 
       Categoryid, 
       AVG(Price) AS avg_price
FROM Products
GROUP BY SupplierID, Categoryid  
HAVING avg_price >=100

```

####  헷갈렸던 문제풀이
```sql

1. We define an employee's total earnings to be their monthly salary x months worked, and the maximum total earnings 
to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings 
for all employees as well as the total number of employees who have maximum total earnings. Then print these values as  space-separated integers.

SELECT salary * months AS earnings 
    , COUNT(*)
FROM EMPLOYEE
GROUP BY earnings
ORDER BY earnings DESC
LIMIT 1 

```
### CASE
```sql

SELECT CASE
		WHEN categoryid=1 AND Sipplierid=1 THEN '음료'
        	WHEN categoryid=2 THEN '조미료'
        	ELSE '기타'
       END AS categoryname
FROM Products


SELECT CASE
	     WHEN CategoryID=1 THEN '음료'
            WHEN CategoryID=2 THEN '소스'
            ELSE '이외'
	END AS new_category,AVG(price)            
	,AVG(price)
FROM PRODUCTS 
GROUP BY new_category

new_category	AVG(price)
소스	       23.0625
음료	       37.979166666666664
이외	       28.11716981132075

-- 파이썬 처럼 A=B=C 이렇게 불가능, AND로 묶어줘야한다.
-- 파이썬 처럼 (IF) WHEN 뒤에는 아닌거 나옴 그러니까 쓴 것 처럼 정삼각형 아닌 조건들 
-- 쓸 필요 없이 자동으로 정삼각형은 아니고->가 되니까 생략할 수 있음. 
-- 그래서 CASE문 쓸 때는 WHEN 순서 중요

1. Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. 
Output one of the following statements for each record in the table:

Equilateral: It's a triangle with  sides of equal length.
Isosceles: It's a triangle with  sides of equal length.
Scalene: It's a triangle with  sides of differing lengths.
Not A Triangle: The given values of A, B, and C don't form a triangle.


SELECT CASE
            WHEN A=B AND B=C AND A=C THEN 'Equilateral'
            WHEN A+B <= C OR A+C <=B OR B+C<=A THEN 'Not A Triangle'
            WHEN A=B OR B=C OR A=C THEN 'Isosceles'
            ELSE 'Scalene' 
        END
FROM TRIANGLES    

-- 데이터 피보팅 데이터를 옆으로 넓게 보는 것 

SELECT AVG(CASE WHEN categoryid=1 THEN price  ELSE NULL END) AS category1_price -- 알리아스 위치 이해 
	 , AVG(CASE WHEN categoryid=2 THEN price  ELSE NULL END) AS category2_price
     , AVG(CASE WHEN categoryid=3 THEN price  ELSE NULL END) AS category3_price
FROM Products

OUTPUT: 
category1_price 	category2_price	 category3_price
37.979166666666664	23.0625	           25.16

```
