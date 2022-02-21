Aiffel Exploration 2에서 배운 내용 중, Decision Tree와 Random Tree에 대해서 작성해보려 합니다.



# 1. Decision Tree



# 2. Random forest

Decision Tree의 단점을 극복하기 위해 여러개 모아 놓은 것이 Random Forest입니다.

이러한 기법을 앙상블 Ensemble 기법이라고 합니다. 데이터 사이언스에서 앙상블이란, 의견을 통합하거나 여러가지 결과를 합치는 방식을 의미힙니다. 



![img](https://1.bp.blogspot.com/-Ax59WK4DE8w/YK6o9bt_9jI/AAAAAAAAEQA/9KbBf9cdL6kOFkJnU39aUn4m8ydThPenwCLcBGAsYHQ/s0/Random%2BForest%2B03.gif) 출처 : [텐서플로우 허브](https://blog.tensorflow.org/2021/05/introducing-tensorflow-decision-forests.html)



위 그림은 Random Forest를 나타냅니다. 여기서 랜덤은 의사 결정트리를 만드는 데 있어 사용되는 feature값을 무작위적으로 선정합니다. 조금 더 풀어서 설명하자면 다음과 같습니다.

1.  30개의 feature가 존재한다면, 30개의 feature에 대한 의사결정 트리를 만듭니다. 
2. 그 중 무작위적으로 일부만 선택하여 하나의 의사결정 트리를 생성합니다. 
3. 이를 여러번 반복합니다.
4. 여러 결정 트리들이 내린 예측값 중 가장 많이 나온 값을 최종 예측값으로 지정합니다.

이렇게 의견을 통합하거나, 여러 가지 결과를 합치는 방식을 앙상블이라고 합니다. 정리하자면, Random forest는 하나의 거대한 결정 트리를 만드는 것이 아니라, 여러 개의 작은 결정 트리를 만드는 것이며 앙상블 기법을이용하여 예측값을 냅니다.



Random foreset는 상위 모델들이 예측하는 편향된 결과보다 다양한 모델들의 결과를 반영하여 더 다양한 데이터에 대한 의사결정을 내리게 됩니다.



해당 코드는 다음 링크에 설명되어 있습니다.















------

###### 출처

- AIFFEL LMS  Exploration 2

  문제시 연락부탁드립니다 :)



----

###### 질문

- 다음 사진에서 8번이 이해안가요

  ![image-20220112113531153](../../../../Typora Images/image-20220112113531153.png)

