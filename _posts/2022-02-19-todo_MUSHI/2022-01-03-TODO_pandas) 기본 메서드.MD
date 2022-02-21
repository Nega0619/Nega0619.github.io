todo : 

- pandas.head() 메서드 검색해야함
- pandas.drop(blah, axis??) axis 뭔지 검색 ㄱ
- pandas.any(blah, axis??) axis 뭔지 검색 ㄱ
- 

------



cf. pandas 객체는 DataFrame 형식입니다.

Dataframe은 0번지 인덱스부터 시작합니다.



# 1. 판다스 csv파일 읽어오기



```python
import os

# csv파일 경로 읽어오기
csv_file_path = os.getenv('HOME')+'/aiffel/data_preprocess/data/trade.csv'
# 판다스 객체생성
trade = pd.read_csv(csv_file_path)

trade.head()
```

todo : pandas.head() 메서드 검색해야함



# 2. csv파일에서 필요없는 컬럼 삭제하기

```python
# 위 코드의 연장선
trade.drop('삭제할 컬럼', axis=1)

```



# 3. csv파일에서 결측치 확인하기



## 3-1. null값 확인하기

null 데이터가 들어간거 출력



`trade(pandas객체).isnull()` 

- csv파일에서 null로 처리된 값이 있다 = true
- csv파일에서 유효한 값이 있다 = false



## 3-2. csv행에서 null값 존재 여부 확인하기

해당 행에서 null이 존재하는지 확인



`trade(pandas객체).any(axis=1)`

- 행에 null값 존재 : true
- 행에 null값 없음 : false



# 4. 결측치 제거하기

`trade(pandas객체).dropna(how='all(모든행을지운다.)', subset=[특정 컬럼 리스트], inplace=True)`

- how
  - 'all' : subset에서 선택한 컬럼 전부가 결측치인 행을 삭제
  - 'any' : subset에서 선택한 컬럼 일부에 결측치가 존재하면 삭제
- subset
  - 컬럼 선택하는 옵션
- inplace
  - True : DataFrame 파일(pandas객체)을 직접적으로 수정한다.
  - False :  DataFrame 파일(pandas객체)을 수정 X



# 5. 원하는 행의 값 불러오기

`trade(DataFrame 객체).loc[행 라벨, 열 라벨]`

- `trade.loc[[행라벨1, 행라벨2]]` : 행 리스트를 이용하여 행여러개를 출력 가능
- `trade.loc[행라벨]` : 행 하나만 출력할 때
- `trade.loc[[행라벨]]` : 행 하나만 출력할 때
  - 두개 출력방법이 다른데 왜다름-_-????????



# 6. 원하는 행의 열 값 수정하기

`trade(DataFrame객체).loc[행값, '컬럼이름(열)']=원하는 대입식`

- NaN 값 주고싶다면 null아닌 None 대입



# 7. 중복 데이터 



## 7-1. 중복 데이터 확인하기

`trade(DataFrame객체).duplicated()` 



## 7-2 중복 데이터 삭제하기

`trade(DataFrame객체).drop_duplicates`













------

###### 출처

- AIFFEL LMS 

