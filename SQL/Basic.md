#### '데이터리안_ 데이터 분석을 위한 기초 SQL 강의' 를 듣고 저만의 방식으로 기록한 내용입니다. 

### 비교 연산자, 논리연산자 
```sql
SELECT * FROM Customers
WHERE Country = 'Germany'
비교 연산자, 특정 컬럼이 특정 값을 가지는 데이터만 불러오기 위해서 사용
=, <> (다르다),>=,<=,<,>

WHERE CustomerName < "B" 
B 이전, 그러니까 A로 고객 이름이 시작하는 고객들 데이터만 불러옴 

WHERE CustomerName < "B" AND/OR Country= 'Germany' 
AND/ OR 파이썬과 같은 조건 
```
### LIKE
패턴, 비슷한 문자를 찾아줘라 
```sql
WHERE country Like 'Br%'
: country에서 Br로 시작하는 것들을 찾고 싶다. 
% : 어떤 것이 들어가도 상관없다. (와일드카드 라고도 함) 
ex) 'Br%' BR 뒤에 어떤 것이 들어 있어도 상관없다 
    '%r%' 앞뒤로 어떤 것이 들어있어도 상관없는데 r이 있는 거 찾고 싶다. 
    
찾고자 하는게 확실할 때, 
ex) Like 'Brazil' 하는 것보다 = 을 쓰는게 속도가 빠르다

WHERE country LIKE 'B_____' : _ 5개 > Brazil 만 나온다. 
_:몇개의 문자, 한글자 와일드 카드
'백_이_여_타'

\ (escape 문자 활용)
WHERE discount LIKE '50\%'
--mysql 기준 

```
### IN, BETWEEN, IS NULL
``` sql
WHERE country IN ('Germany','France')
= WHERE country='Germany' 0R country='France' 
BUT 국가가 많아지면 구문이 너무 길어지기 때문에 IN을 씀  
WHERE CustomerID BETWEEN 3 AND 5

WHERE CustomerID IS NULL 
결측치 있으면 보여줘~ 
--NULL, NAN 
```

#### 문제 풀이 
```sql
SELECT CITY 
FROM STATION 
WHERE CITY LIKE 'a%'
OR CITY LIKE'e%'
OR CITY LIKE'i%'
OR CITY LIKE'o%'
OR CITY LIKE'u%'

-> WHERE CITY LIKE 'a%' OR 'e%'OR ~ 이게 되는줄 알았다.
하지만 안된다, 왜? : 문법이 LIKE를 다써줘야 하는 것 같아.. 보충하기 

SELECT DISTINCT (중복 없이 뽑아주기) 

Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. 
Your result cannot contain duplicates.

SELECT DISTINCT CITY 
FROM STATION 
WHERE CITY NOT LIKE 'a%'
AND CITY NOT LIKE'e%'
AND CITY NOT LIKE'i%'
AND CITY NOT LIKE'o%'
AND CITY NOT LIKE'u%'
AND CITY NOT LIKE '%a'
AND CITY NOT LIKE'%e'
AND CITY NOT LIKE'%i'
AND CITY NOT LIKE'%o'
AND CITY NOT LIKE'%u'

-> 구글링해볼까 하다가 파이썬이랑 비슷한 논리겠지, NOT 써볼까? 했는데 맞았다! 유휴!   
```
### ORDER BY 
```sql

-- 데이터 정렬하기 
SELECT * 
FROM Customers
WHERE
ORDER BY customerid DESC

: 원래 오름차순이 디폴트, DESC 해주면 내림차순 (91>90>>>1) 
ASC 는 오름차순, 근데 어차피 이게 디폴트 
순서는 SELECT -> FROM -> WHERE -> ORDER BY 

- ORDER BY 명령 자체는 DB의 저장된 데이터를 건드리는 게 아니고, SELECT문으로 수행해서 
데이터를 가져와서 보여줄 때 그 순서만 변경해주는 것이다. 

SELECT * 
FROM Products
WHERE price >=20
ORDER BY price DESC
-- 가격을 기준으로 / 내림차순 정렬해달라

DESC : Descending (내림차순)의 약자, 내리는것? 
미끄럼틀을 생각하면 위에서부터 슈육~ 그러니까 큰 것부터 오겠죠? 

가장 비싼 것 하나 보고싶다. LIMIT
SELECT * 
FROM Products
ORDER BY price DESC
LIMIT 1

가장 싼 것 하나 보고싶다 
SELECT * 
FROM Products
ORDER BY price ASC
LIMIT 1
```

#### 문제풀이
```sql

Query the Name of any student in STUDENTS who scored higher than 75 Marks. Order your output by the last three 
characters of each name. If two or more students both have names ending in the same last three characters 
(i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

My answer 
SELECT NAME FROM STUDENTS WHERE Marks > 75 ORDER BY RIGHT(NAME, 3), ID ASC;
```

### MySQL 문자열 자르기 
```sql
LEFT(컬럼명 또는 문자열, 문자열의 길이) : 왼쪽에서(이거를,멸글자) 가져와
예 : SELECT LEFT ('20160327',4)
-> 2016 (네,,제가 16학번입니다 :))

RIGHT (컬럼명 또는 문자열, 문자열의 길이): 오른쪽에서(이거를,멸글자) 가져와
예 : SELECT RIGHT ('20160327',4)
-> 0327 (넵,,제가 사회과 27학번..)

SUBSTRING (컬럼명 또는 문자열, 시작 위치, 길이)
=SUBSTR()
예 : SUBSTR('20160327',7,2)
-> 27
예 : SUBSTR('20160327',5)
->0327

한줄 주석 처리 : --안녕
여러 줄 주석 처리: 
/*
안
녕
*/

근데 ; Error 는..? 해커랭크탓..
```

#### 문제풀이
![image](https://user-images.githubusercontent.com/89775352/156361619-7a436ada-606b-4515-87e4-0b2390e16686.png)
decimal places -> 소수점 자리 
```sql
SELECT ROUND(LONG_W,4)
FROM STATION
WHERE LAT_N < 137.2345
ORDER BY LAT_N DESC 
LIMIT 1;

-> 0이 계속 포함되는 문제 질문 글 올림 

```
### MySQL 소수점처리
```sql
CEIL() 올림
예: SELECT CEIL (5.5)
-> 6
CEIL 이 천장이잖아 

FLOOR() 내림
예 : SELECT FLOOR(5.5)
-> 5
FLOOR은 바닥이고 

ROUND() 반올림
예: ROUND (5.5313132,4)
->5.5313
ROUND 는 파이썬이랑 똑같네
```

SQL 쉽네! 중급으로 고고씽




