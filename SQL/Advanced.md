## Subquery 
### from 절 서브쿼리
- 각 주의 평균 발생 COUNT 
```sql 
SELECT dailly_stat.week
    , AVG(daily_stats.incidents_daily)
FROM( SELECT week
            ,date
            ,COUNT(incident_id) AS incidents_daily
      FROM crimes
      GROUP BY week,date
      )daily_stats
GROUP BY daily_stats.week       

주의: 결측치가 있으면 평균에 오류가 날수가 있다. 
```
### WHERE 절 서브쿼리 
```sql
SELECT * 
FROM crimes
WHERE date=(SELECT MIN(date) FROM crimes)
```
- 서브쿼리의 결과물이 1개여야만 함.

```sql
SELECT*
FROM crimes
WHERE date IN (SELECT date FROM crimes ORDER BY date DESC LIMIT 5)
```
- 서브쿼리의 결과물이 1개이상 0 

#### 문제 풀이
```sql 

## DML (Data Manipulation Language)

### Insert
- 테이블 전체에 데이터 추가하는 방법
```sql
INSERT INTO 테이블명 VALUES(VALUE_LIST)
EX) INSERT INTO Salary VALUES('1','A','250','2020-03-31')
```
- 특정 컬럼에만 값을 넣어 주는 방법 
```sql
INSERT INTO 테이블명(COLUMN_LIST) VALUES(VALUE_LIST)
EX) INSERT INTO Salary(ID,Salary) VALUES('2','550')
```
### UPDATE (수정)
- 컬럼 전체에 데이터 업데이트 
```sql
UPDATE 테이블명 SET 컬럼 = 값
EX) UPDATE Salary SET Salary= Salary+100
```
= : 비교 연산자로 사용 했었는데, SET = : 여기서는 대입 연산자 

- 지정 행의 값 갱신하기 
```sql
UPDATE 테이블명 SET 컬럼 = 값 WHERE 조건식 (행)
EX) UPDATE Salary SET Salary= Salary+100 WHERE Id=2 
```

### DELETE 
- UPDATE 와 비슷
- 테이블 전체 데이터 삭제하는 방법 
```sql
DELETE FROM 테이블명
```
- WHERE 조건절에 일치하는 모든 행 삭제 
```sql
DELETE FROM 테이블명 WHERE 조건식
```

## WINDOW 함수
```sql
함수(컬럼) OVER (PATITION BY 컬럼 ORDER BY 컬럼) 
```
- PATITION BY: GROUP BY 와 비슷하다 
- 함수 : SUM,AVG,COUNT ...    
- PATITION BY 컬럼 / ORDER BY 컬럼 : 둘다 있을 수도 있고, 둘다 없을 수도 있고, 하나만 있을 수도 있다.             
                
#### 집계 함수 
```sql
MAX(Salary) OVER (PARTITION BY Departmentid) AS MaxSalary
```
GROUP BY를 쓰면 {1: 90000, 2:80000} 이런식으로 테이블을 새로 만들지만,

  윈도우 함수를 쓰면 원래 테이블 옆에 MaxSalary row가 추가된다. 
```sql
SUM(kg) OVER (ORDER BY Line) AS CumSum
``` 
 누적합이 연산된다. (Line 컬럼 순서로 해서)
 - 윈도우 함수를 사용하지 않고 누적합을 구해봐라 라는 면접 문제가 나올수도 있다. (MySQL 구버전 경우)
```sql
SUM(kg) OVER (ORDER BY Line PARTITION BY Id) AS CumSum
``` 
 누적합이 ID 별로 연산된다. 
- 다른 집계 함수 EX) AVG,COUNT,MIN 등도 위와 같이 하면 된다. 

누적합 | 윈도우 함수 이외의 방법
```sql
-- JOIN 활용 
SELECT el.Id
      ,el.Name
      ,el.kg
      ,el.Line
      ,SUM(E3.kg) AS CUMSUM 
FROM Elevator e1 
INNER JOIN Elevator e2
        on e1.ID=e2.Id
        and el.Line >= e2.Line
GROUP BY 1,2,3,4

--  SELECT절 서브쿼리 활용 
SELECT el.id
      ,el.Name
      ,el.kg
      ,el.Line
      ,(SELECT SUM(e2.kg)
          FROM Elevator e2
        WHERE e1.id=e2.id
          and e1.Line>=e2.Line) AS CumSum
FROM Elevator e1
```
      

```        
#### 순위 정하기 
- ROW_NUMBER(), RANK(), DENSE_RANK()
- 셋 다 () 안에 아무런 인자도 들어가지 않음.

ROW_NUMBER: 어떻게든 순위 정해줌, 중복 순위 X / EX) 1>2>3>4

RANK(): 중복 순위 O / EX) 1>1>3>4>4>5

DENSE_RANK(): 중복 순위 O, 빠지는 숫자 없음 / EX) 1>1>2>3>3>3

#### 데이터 위치 바꾸기 
- LEAD() : 앞으로 당기고 , LAG() : 뒤로 밀고 

```sql
LAG(컬럼) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)
LAG(컬럼,칸수) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)
LAG(컬럼,칸수,Default) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)

LEAD(컬럼) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)
LEAD(컬럼,칸수) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)
LEAD(컬럼,칸수,Default) OVER (PARTITION BY 컬럼 ORDER BY 칼럼)
``` 

### 문제 풀이
리트코드/ 180. Consecutive Numbers
```sql
SELECT DISTINCT l.Num AS con
FROM(
    SELECT Num
          ,LEAD(NUM,1) OVER (ORDER BY id) AS next
          ,LEAD(NUM,2) OVER (ORDER BY id) AS afternext    
    FROM  Logs      
)l
WHERE l.Num=l.next=l.afternext  
```
- 아래 내용은 백문이불여일타_데이터리안_고급반을 수강하면서 정리한 내용입니다. 
