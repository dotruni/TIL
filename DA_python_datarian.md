### 데이터리안 판다스 강의로 요즘 안쓰는 파이썬 데이터 분석을 복습해 보자 
```python
import pandas as pd 
air_quality = pd.read_csv('https://raw.githubusercontent.com/pandas-dev/pandas/main/doc/data/air_quality_no2.csv')
air_quality.head(2)
```
- 기존 컬럼에 숫자 곱하기 
```python
air_quality['london_mg_per_cubic'] = air_quality['station_london']*1.882

b = []

for i in a : 
   b.append(i*2)
```
- 컬럼 이름 재정의하기 -> .rename
```python
air_quality.renamed = air_quality.rename(columns={'station_paris':'FR04014'}) 
```

- 컬럼 소문자, 대문자로 만들기 -> ( columns = str.lower / upper ) 
```python
air_quality.renamed_lower = air_quality.rename(columns=str.lower)
air_quality.renamed_lower
# lower, upper
```
- 특정 컬럼 삭제하기 -> df.drop(['',''], axis=1)
```python
df = air_quality.drop(['station_paris','station_antwerp'], axis=1)
df
```
- 특정 로우 삭제하기 
```python
df = air_quality.drop([0,1]) # axis=0 default 값
df.shape
# 인덱스값
```
- 중복 데이터 삭제하기 -> df.drop_duplicates( ) 
```python
df = air_quality.drop_duplicates()
print(air_quality.shape)
print(df.shape)
```
![image](https://user-images.githubusercontent.com/89775352/185288455-4752e729-1177-4c4e-a879-7a97d5fe787f.png)

- element-wise 함수에 적용하기, apply 
```python
s = pd.Series([20, 21, 12], index = ['London', 'New York', 'Helsinki'])

# apply 쓰기
def square(x):
  return x**2
  
square(6)  
s.apply(square)

# element wise
s.apply(lambda x:x**2)

```
- 집계 함수 
```python
mean = air_quality.mean().mean()

def level(x):
  if x >= mean:
    result = 'high'
  elif x < mean : 
    result = 'low'
  else:
    result = x
  return result   
  
air_quality['station_paris_level'] = air_quality['station_paris'].apply(level)
air_quality.head(2)  

# 적용 
air_quality['station_paris'].apply(lambda x: 'high' if x >= mean else 'low' )

```
- 그룹 별 집계

```sql
SELECT Sex, AVG(Age)
FROM titanix
GROUP BY Sex
```
- 아래와 같다
```python
t = pd.read_csv('https://raw.githubusercontent.com/pandas-dev/pandas/master/doc/data/titanic.csv')

t.groupby('Sex')['Age'].mean()
# Sex 라는 컬럼을 기준으로 age의 평균을 구하는 것 
```
