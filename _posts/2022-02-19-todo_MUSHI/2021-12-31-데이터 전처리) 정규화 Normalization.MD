# 정규화

모델을 학습시킬 때 입력 데이터를 통일화 시켜주는 것은 중요합니다. 예를 들어, 데이터의 범위가 0 - 1까지인 데이터와 10000-100000인데이터가 있을 경우, 이런 데이터를 클러스터링 한다고 할 경우, 데이터간의 범위를 잴 때 범위가 큰 후자의 데이터에만 영향을 크게 받을 것입니다.

이렇게 데이터간의 범위가 크게 다를 경우, 전처리 과정에서 데이터를 정규화 합니다. 보통 많이 알려진 방법은 표준화와 Min-Max Scaling이 있습니다.



## 정규화 방법



### Standardization

데이터의 평균을 0, 분산을 1로 변환합니다.
$$
\frac{X-μ}{σ}
$$

```python
# x를 dataframe이라 할때

x_standardization = (x - x.mean())/x.std()
x_standardization
```





### Min-Max Scaling

데이터의 최솟값은 0, 최댓값은 1로 변환
$$
\frac{X-X_{min}}{X_{max}-X_{min}}
$$

```python
# 데이터 x를 min-max scaling 기법으로 정규화합니다. 

x_min_max = (x-x.min())/(x.max()-x.min())
x_min_max
```





### 그밖 : 로그변환

https://www.youtube.com/watch?v=FDCfw-YqWTE&feature=emb_logo



여기 더 공부하자.





----

참고자료 : https://www.youtube.com/channel/UCcIXc5mJsHVYTZR1maL5l9w

------

###### 출처

- AIFFEL LMS 

