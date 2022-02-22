오늘은 아이펠 Exploration 노드 14번을 공부하고 공부한 내용을 포스팅해보았습니다. 

전체 코드는 깃허브를 참고하시고, 여기서는 중요한 메소드만을 작성하였습니다.

------

###### 출처

- AIFFEL LMS 

  문제시 연락 부탁드립니다. :)


---

목차

- 추천 시스템이란?
- 데이터 탐색과 전처리
- 사용자의 명시적 / 암묵적 평가
- Matrix Factorization ( MF )
- CSR (Compressed Sparse Row) Matrix
- MF 모델 학습하기
- 비슷한 아티스트 찾기 + 유저에게 추천하기



----



# 1. 추천시스템이란?

추천 시스템은 사용자가 선호할 만한 아이템을 추측함으로써 여러 가지 항목 중 사용자에게 적합 한 특정 항목을 선택(information filtering)하여 제공하는 시스템입니다.



- 고전적 추천 시스템

  - 협업 필터링

    - 대규모의 기존 사용자 행동 정보를 분석하여 해당 사용자와 비슷한 성향의 사용자들이 기존에 좋아했던 항목을 추천하는 기술

    - 행렬분해(Matrix Factorization), k-최근접 이웃 알고리즘 (k-Nearest Neighbor algorithm; kNN) 등의 방법이 많이 사용

    - 협업 필터링을 위해서는 반드시 기존 자료를 활용 BUT 이러한 자료들을 사용자에게 직접 요구해야만 하는것은 아님

    - 단점

      - Cold Start

        협업 필터링은 기존의 자료가 필요한바, 기존에 없던 새로운 항목이 추가되는 경우는 추천이 곤란해지는 현상

      - 사용자 수가 많은 경우 효율 적으로 추천할 수 없음

        계산이 몇 시간에서 며칠까지 걸리는 경우가 종종 생김

      - Long tail

        시스템 항목이 많다 하더라도 사용자들은 소수의 인기 있는 항목에만 관심을 보이기 마련이다. 따라서 사용자들의 관심이 적은 다수의 항목은 추천 을 위한 충분한 정보를 제공하지 못하는 경우

  - 콘텐츠 기반 필터링

    - 항목 자체를 분석하여 추천을 구현
    -  항목을 분석한 프로파일(item profile)과 사용자의 선호도를 추출한 프로파일(user profile)을 추출하여 이의 유사성을 계산
    - 아이템 분석 알고리즘이 핵심적
      - 군집분석(Clustering analysis), 인공신경망(Artificial neural network), tf-idf(term frequencyinverse document frequency) 등의 기술이 사용
    - 로 협업 필터링에서 발생하는 콜드 스타트 문제 를 자연스럽게 해결
    - 단점
      - 만 다양한 형식의 항목을 추천하기 어려움

- 모델 기반 협력 필터링

  - 기존 항목 간 유사성을 단순하게 비교하는 것에서 벗어나 자료 안에 내재한 패턴 을 이용하는 기법

    - 자료의 크기를 동적으로 변화시키는 방법

      영 화를 추천하는 경우, ‘해리 포터’ 시리즈 2편을 추천하기 위해서는 ‘해리 포터’ 시리즈 1편, 단 한 편을 좋아했는가가 다른 무엇보다 중요한 요소

    - 추천을 위한 자료의 크기를 변화시키는 방법

    - 잠재(latent) 모델에 기반을 둔 방법

      잠재 모델이란 사용자가 특정 항목을 선호 하는 이유를 알고리즘적으로 알아내는 기법

  - LDA(Latent Dirichlet Allocation), 베이지안 네트워크 (Bayesian Network) 등의 알고리즘이 사용

  

- 출처 : [콘텐츠 추천 알고리즘의 진화](http://www.kocca.kr/insight/vol05/vol05_04.pdf)



# 2. 데이터 탐색과 전처리



- TSV 파일이란?
  -  tsv는 Tab-Separated Values의 약자로서,  구분자가 tab('\t') 문자
- CSV 파일이란?
  - csv는 Comma-Separated Values의 약자로서, 구분자가 comma(',') 문자



- tsv 파일 읽어오기

  ```python
  data = pd.read_csv(fname, sep='\t', names= col_names)      # sep='\t'로 주어야 tsv를 열 수 있습니다.  
  ```



- pandas에서 사용할 컬럼만 남기기

  ```python
  using_cols = ['user_id', 'artist', 'play']
  data = data[using_cols]
  ```

  

- pandas Dataframe에서 유니크한 데이터 개수알아보기

  ```python
  data['user_id'].nunique()
  ```

  

- pandas Dataframe에서 group by 사용하기

  ```python
  user_median = data.groupby('user_id')['play'].median()
  
  ```



- 꼭 암기

  ```python
  # 아티스트 이름은 꼭 데이터셋에 있는 것과 동일하게 맞춰주세요. 
  my_favorite = ['black eyed peas' , 'maroon5' ,'jason mraz' ,'coldplay' ,'beyoncé']
  
  # 'hwi'이라는 user_id가 위 아티스트의 노래를 30회씩 들었다고 가정하겠습니다.
  my_playlist = pd.DataFrame({'user_id': ['hwi']*5, 'artist': my_favorite, 'play':[30]*5})
  
  if not data.isin({'user_id':['zimin']})['user_id'].any():  # user_id에 'zimin'이라는 데이터가 없다면
      data = data.append(my_playlist)                           # 위에 임의로 만든 my_favorite 데이터를 추가해 줍니다. 
  ```

  - inplace , drop, ignore_index 다안됨.
    - https://yganalyst.github.io/data_handling/Pd_2/



# 3. 사용자의 명시적 / 암묵적 평가



# 4. 사용자의 명시적 / 암묵적 평가



# 5. Matrix Factorization ( MF )



# 6. CSR (Compressed Sparse Row) Matrix



# 7. MF 모델 학습하기




# 8. 비슷한 아티스트 찾기 + 유저에게 추천하기