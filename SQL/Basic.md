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
