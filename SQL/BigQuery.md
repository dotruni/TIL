## 쿼리 필수 요소

-   구글 빅쿼리 완벽 가이드를 참고하여 작성하였습니다.
-   p.68~ 배열과 구조체 기초

### UNNEST / ARRAY란?

`SELECT * FROM UNNEST(['Seattle WA', 'New York', 'Singapore']) AS city`

-   NEST: 둥지, NESTED SQL 중첩된 쿼리
-   UNNEST: 평면화 시키는 것 , 둥지 해체!

이런 상황에 배열을 평면화(Flatten)해서 배열에 있는 값을 펴줘야 함  
배열을 펴줄 때 사용하는 것은 UNNEST로, Nest한 데이터를 UNNEST하게 만드는 것  
UNNEST 연산자는 ARRAY를 입력으로 받고 ARRAY의 각 요소에 대한 행이 한 개씩 포함된 테이블을 return함  
[출처](https://zzsza.github.io/gcp/2020/04/12/bigquery-unnest-array-struct/)

```
SELECT
      city,SPLIT(city,' ')AS parts
      FROM (
         SELECT * FROM UNNEST([
           'Seattle WA', 'New York', 'Singapore']) AS city
      )
```

-   ARRAY로 만들고 \[\] - 대괄호 , UNNEST로 평면화 해준 다음에 SPLIT으로 쪼개기

### ARRAY\_AGG

### STRUCT (구조체)

```
SELECT
 [
   STRUCT('male' AS gender , [11111,22222] AS numtrips)
   ,STRUCT('female' AS gender,[3333,4444] AS numtrips)
 ] AS bike
```

-   컬럼 이름과 struct를 지정해주지 않으면 익명 컬럼 처리됨으로 유지보수, 재활용을 위해 꼭 이름을 지정하도록 하자.
-   UNNEST: 배열의 요소를 행으로 반환하는 함수로, 결과 배열을 풀면 (UNNEST하면) 각 배열의 각 항목에 해당하는 행을 가져올 수 있다.
-   배열의 일부만 풀 수도 있다.
    
    ```
    SELECT numtrips FROM UNNEST(
    [
     STRUCT('male' AS gender , [11111,22222] AS numtrips)
     ,STRUCT('female' AS gender,[3333,4444] AS numtrips)
    ] )AS bike
    ```
    
-   다음 쿼리는 numtrips 컬럼의 값만 얻는다JOIN
-   CROSS JOIN (상호 조인) 한 쪽 테이블의 모든 행들과 다른 테이블의 모든 행을 조인시키는 기능을 한다.
-   SQL 그렇게 많이 했는데 왜 CROSS JOIN 모르지..? 했는데
    
    ```
    SELECT *
    FROM A,B 
    ```
    
-   쉼표로 테이블 합치는 것과 같은 역할을 하고 이게 더 쉬워서 쉼표 크로스 조인이라고도 한대. 난 이렇게 썼으니....
    
    # 3장 데이터 타입, 함수, 연산자
    
    SAFE 접두사
-   `SELECT SAFE.LOG(10,-3), LOG(10,3)`
-   음수일 때 등 오류를 발생시키지 않고 NULL을 반환한다.
-   ROUND, SUBSTR등 스칼라 함수에만 사용할 수 있다.

## 불(BOOL) 다루기

### COALESCE, IFNULL

-   COALESCE : null이 아닌 첫 번째 표현식의 값을 반환합니다. (COALESCE 뜻: 합체하다)
    
    ```
    COALESCE (a,b) = IFNULL(a,b)
    ```
    
-   a가 NULL이면 b를 반환

### CAST, SAFE\_CAST (타입 변환, 타입 강제)

```
SELECT SAFE_CAST(hours_worked AS INT64) : 에러 안나오고 Null 값이 나오게 할 수 있음 
SELECT CAST(hours_worked AS INT64) 
```

-   명시적 타입 변환
-   암시적 타입 변환 : 빅쿼리는 INT64 -> FLOAT64,NUMERIC OR NUMERIC-> FLOAT64 로만 지원

### COUNTIF (불리언 변환을 피하기 위해)

SUM, AVG 등은 불리언 값과 작동하지 않는다.

```
SELECT SUM(is_vowel) AS num_vowels FROM A
```

집계를 수행하려면 불리언 값 -> INT64로 변환해야 한다.

```
SELECT SUM(CAST(is_vowel AS INT64)) AS num_vowels FROM A
```

하지만 타입 변환은 피하는 게 좋다.

-   WHY: (메모리 사용량이 늘어나고 성능에 영향을 미칠 수 있음으로 추정)

-   의문이 있을 때 computer science 적인, 성능과 관련한 이슈라는 판단이 들면 바로 짚고 넘어가기\\

이 예제에서는 불리언 값에 IF문을 사용하는 것이 더 깔끔하다.

```
SELECT SUM(IF(is_vowel,1,0)) AS num_vowels FROM A
```

-   IF(조건문, 참일 때 값, 거짓일 때 값)

이것보다 나은 것은 COUNTIF

`SELECT COUNTIF(is_vowel) AS num_vowels FROM A`

-   연산을 두 번 해야 해서..?
-   문자열 함수
-   LENGTH(), LOWER(), UPPER()
-   STRPOS(value1,value2) : value1 내부의 value2에서 첫 번째 어커런스의 1로 시작하는 색인(위치)을 반환합니다. value2를 찾을 수 없으면 0을 반환합니다.
-   SUBSTR(value, position \[, length\]) [출처](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions?hl=ko#substr)
-   SUBSTR(컬럼, 시작 위치, 자르는 길이)

```
WITH items AS (SELECT 'apple' as item UNION ALL SELECT 'banana' as item 
               UNION ALL SELECT 'orange' as item)
               
               
SELECT
SUBSTR(item, 2, 2) as example
FROM items;
```

+---------+

| example |  
+---------+  
| pp |  
| an |  
| ra |  
+---------+

```
- CONCAT : 문자열 합치기 SELECT CONCAT('','','')
- TRIM() : 양쪽의 공백 제거 
- TRIM(' ','*' ) : 양쪽의 *이 제거
```

#### **LEAD  / LAG**

항상 너무 헷갈린다... 어떤 게 앞에 걸 가져오는 거지..? 맨날 까먹는 나 자신...

이럴 때는 스키마를 연결한 스토리 텔링!   
  
 - LEAD : 리드하고 있는 앞의 행을 가져오는 거지! 말 그대로 너나 리더 널 내 것으로 만들겠어!   
  
\- LAG: 이건 영어 뜻이 지연! : 지연되어있는 뒤에 걸 끌어올리는 것!  너머 뒤쳐지는 너 일루와

[##_Image|kage@cIoC1f/btrKUc149ro/2DUE8fKHTTu9PJI1IuGD0K/img.png|CDM|1.3|{"originWidth":663,"originHeight":261,"style":"alignCenter"}_##]

```
LEAD/ LAG(칼럼에 넣을 값) OVER( PARTIION BY A ORDER BY X)
```

-   순서가 테이블에 따라 다를 수 있어 주의해야 함

### TIMESTAMP\_TRUNC

```
TIMESTAMP_TRUNC(timestamp_expression, date_time_part[, time_zone])
```

예시 

```
SELECT TIMESTAMP_TRUNC(created_at,MONTH,'Asia/Seoul')month_k
```

Month자리에 DAY, WEEK, YEAR 등등 기입하면 각 Timestamp에서 원하는 단위 추출 가능 

(달의 경우 각 달의 1일 추출됨)

BUT

# timestamp를 seoul로 하다보니 utc 시간이 전날로 나옴 

\- 기존 Mysql과 다름 

#### 구글 데이터 스튜디오 , 빅쿼리 연결 데이터 리뷰 

\- 데이터 분석가들 사이에 너무 느려서 구글이 일단 데이터 스튜디오를 버리고 Looker로 넘어간다는 이야기를 들었음

\- 사용해 본 결과 지금은 데이터가 그렇게 많지 않아 아직 그렇게 불편하지는 않음. 

#### References

\- 책, 구글 빅쿼리 완벽 가이드, 빌리아파 락쉬마난 저 

\- [https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp\_functions?hl=ko#timestamp\_trunc](https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions?hl=ko#timestamp_trunc) 

 [타임스탬프 함수  |  BigQuery  |  Google Cloud

의견 보내기 타임스탬프 함수 BigQuery는 여러 TIMESTAMP 함수를 지원합니다. 중요: 이러한 함수를 사용하려면 먼저 타임스탬프가 저장 및 표시되는 형식 간의 차이점과 이러한 형식 간 변환에 시간

cloud.google.com](https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions?hl=ko#timestamp_trunc)

\- 각종 경험
