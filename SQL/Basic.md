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
```sql
WHERE country Like 'Br%'
: country에서 Br로 시작하는 것들을 찾고 싶다. 
% : 어떤 것이 들어가도 상관없다. 
ex) 'Br%' BR 뒤에 어떤 것이 들어 있어도 상관없다 
    '%r%' 앞뒤로 어떤 것이 들어있어도 상관없는데 r이 있는 거 찾고 싶다. 
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
