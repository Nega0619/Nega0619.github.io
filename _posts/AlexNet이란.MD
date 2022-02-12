오늘은 AlexNet에 대해서 정리해보겠습니다.

참고한 자료는 다음과 같습니다.

- chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/viewer.html?pdfurl=https%3A%2F%2Fproceedings.neurips.cc%2Fpaper%2F2012%2Ffile%2Fc399862d3b9d6b76c8436e924a68c45b-Paper.pdf&clen=1418820
- https://bskyvision.com/421

----------



# 1. AlexNet이란?

2012년 Image Classification Task에서 Geoffrey Hinton 교수님이 이끄는 토론토 대학의 SuperVision팀이 오류율 16%로 1등을 달성합니다. 작년의 우승자는 오류율 26%, 제작년은 28%였으니 엄청난 차이를 보여주었습니다. 

superVision팀이 사용한 모델이 AlexNet입니다.



![img](../images/pages/99FEB93C5C80B5192E)



## AlexNet의 구조

기본 구조는 [LeNet-5](https://bskyvision.com/418)와 크게 다르지 않습니다. 2개의  GPU로 병렬연산을 수행하기 위해 병렬 구조로 설계되었다는 점이 가장 큰 변화입니다.



총 8개의 레이어로 이루어져 있으며, 5개는 Conv 레이어, 3개는 FC 레이어로 구성되어 있습니다. (맨 왼쪽은 input data입니다.)

2, 4, 5번째 컨볼루션 레이어들은 전 단계의 feature map과 연결되어 있지만, 3번째 레이어에서는 병렬처리로 나뉜 두 단계의 feature맵과 모두 연결되어 있는 것이 특징입니다. 



### input data

227 * 227 * 3 의 이미지가 들어옵니다. (그림에선 224라고 되어있는데, 잘못된 것입니다.)



### 첫번째 Conv 층

96개의 11\*11\*3 사이즈의 필터로 영상을 처리합니다.

stride = 4

zero-padding 사용 X

-> 55\*55\*96 feature map 생성



Activation func : Relu



Relu 활성화 함수를 지나고 나면, 3\*3 overlapping max pooling이 stride 2값으로 시행됩니다.

그 결과 27\*27\*96 fearture map을 가지게 됩니다.



local response normalization이 시행됩니다. 이는 수렴 속도를 가속화 해준다고 합니다.

또한, 특성맵의 차원을 변화시키지 않아 27\*27\*96의 feature map이 유지된다고 합니다.

