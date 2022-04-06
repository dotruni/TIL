- 아래 내용은 백문이불여일타_데이터리안_고급반을 수강하면서 정리한 내용입니다. 

### WINDOW 함수
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
