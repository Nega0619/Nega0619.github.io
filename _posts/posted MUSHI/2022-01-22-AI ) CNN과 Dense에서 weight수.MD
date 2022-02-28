이번 포스팅에서는 keras 에서 Dense와 Conv2D메소드의 매개변수 Arguments를 살펴보고, 이에 따른 parameter의 개수는 어떻게 되는지 살펴보도록 하겠습니다. 아래 모델을 이용하여 parameter 개수를 셀 것입니다.

![image-20220227204519768](../assets/img/posts/image-20220227204519768.png)



# tf.keras.layers.Conv2D



### 1. Conv2D 층의 매개변수

```python
tf.keras.layers.Conv2D(
    filters, kernel_size, strides=(1, 1), padding='valid',
    data_format=None, dilation_rate=(1, 1), groups=1, activation=None,
    use_bias=True, kernel_initializer='glorot_uniform',
    bias_initializer='zeros', kernel_regularizer=None,
    bias_regularizer=None, activity_regularizer=None, kernel_constraint=None,
    bias_constraint=None, **kwargs
)
```

| 인수                   |                                                              |
| :--------------------- | ------------------------------------------------------------ |
| `filters`              | 정수, 출력 공간의 차원(즉, 컨볼루션의 출력 필터 수).         |
| `kernel_size`          | 2D 컨볼루션 창의 높이와 너비를 지정하는 정수 또는 2개의 정수로 구성된 튜플/목록입니다. 모든 공간 차원에 대해 동일한 값을 지정하는 단일 정수일 수 있습니다. |
| `strides`              | 높이와 너비를 따라 컨볼루션의 보폭을 지정하는 정수 또는 2개의 정수로 구성된 튜플/목록입니다. 모든 공간 차원에 대해 동일한 값을 지정하는 단일 정수일 수 있습니다. 보폭 값 != 1을 지정하는 것은 값 != 1을 지정하는 것과 호환되지 않습니다. `dilation_rate` |
| `padding`              | `"valid"`또는 `"same"`(대소문자 구분 안 함) 중 하나입니다 . `"valid"`패딩 없음을 의미합니다. `"same"`결과적으로 입력의 왼쪽/오른쪽 또는 위/아래에 균일하게 0이 채워집니다. `padding="same"`및 `strides=1`인 경우 출력은 입력과 동일한 크기를 갖습니다 . |
| `data_format`          | ( 기본값 ) 또는 . 입력에서 차원의 순서입니다. 모양이 있는 입력에 해당하고 모양이 있는 입력 에 해당합니다 . 기본적으로 Keras 구성 파일의 . 설정하지 않으면 입니다 . `channels_last``channels_first``channels_last``(batch_size, height, width, channels)``channels_first``(batch_size, channels,height, width)``image_data_format``~/.keras/keras.json``channels_last` |
| `dilation_rate`        | 확장된 컨볼루션에 사용할 확장률을 지정하는 정수 또는 튜플/2개의 정수 목록입니다. 모든 공간 차원에 대해 동일한 값을 지정하는 단일 정수일 수 있습니다. 현재 != 1 값을 지정하는 것은 stride 값 != 1을 지정하는 것과 호환되지 않습니다. `dilation_rate` |
| `groups`               | 입력이 채널 축을 따라 분할되는 그룹 수를 지정하는 양의 정수입니다. 각 그룹은 필터로 개별적으로 컨볼루션됩니다. 출력은 채널 축을 따라 모든 결과를 연결한 것입니다. 입력 채널이며 둘 다 로 나눌 수 있어야 합니다 . `filters / groups``groups``filters``groups` |
| `activation`           | 사용할 활성화 기능입니다. 아무 것도 지정하지 않으면 활성화가 적용되지 않습니다( 참조 [`keras.activations`](https://www.tensorflow.org/api_docs/python/tf/keras/activations)). |
| `use_bias`             | 부울, 레이어에서 바이어스 벡터를 사용하는지 여부.            |
| `kernel_initializer`   | `kernel`가중치 행렬 의 이니셜라이저 ( 참조 [`keras.initializers`](https://www.tensorflow.org/api_docs/python/tf/keras/initializers)). 기본값은 'glorot_uniform'입니다. |
| `bias_initializer`     | 바이어스 벡터에 대한 이니셜라이저( 참조 [`keras.initializers`](https://www.tensorflow.org/api_docs/python/tf/keras/initializers)). 기본값은 '0'입니다. |
| `kernel_regularizer`   | `kernel`가중치 행렬 에 적용된 정규화 함수 ( 참조 [`keras.regularizers`](https://www.tensorflow.org/api_docs/python/tf/keras/regularizers)). |
| `bias_regularizer`     | 편향 벡터에 적용된 정규화 함수( 참조 [`keras.regularizers`](https://www.tensorflow.org/api_docs/python/tf/keras/regularizers)). |
| `activity_regularizer` | 레이어의 출력에 적용된 정규화 기능("활성화")( 참조 [`keras.regularizers`](https://www.tensorflow.org/api_docs/python/tf/keras/regularizers)). |
| `kernel_constraint`    | 커널 행렬에 적용되는 제약 조건 함수( 참조 [`keras.constraints`](https://www.tensorflow.org/api_docs/python/tf/keras/constraints)). |
| `bias_constraint`      | 바이어스 벡터에 적용되는 제약 조건 함수입니다( 참조 [`keras.constraints`](https://www.tensorflow.org/api_docs/python/tf/keras/constraints)). |



### 2. Conv2D 층의 parameter 수

W = 컨볼루션 층의 weight 수

B = 컨볼루션 층의 bias 수

N = 커널의 개수

C = input img의 채널 개수

P = 컨볼루션 층의 parameter 개수


$$
W = K^2 * C * N \\
B = N \\
P = W + B
$$




Example ) 

`model.add(keras.layers.Conv2D(16, (3,3), activation='relu', input_shape=(28,28,3)))`

weight 수 = 커널 사이즈 * 채널 수 * 출력 차원 수  = (3*3) * 3 * 16

bias 수 = 출력 차원 수 = 16

총 parameter 수 = weight 수 + bias 수 = 448



`model.add(keras.layers.Conv2D(32, (3,3), activation='relu'))`

weight 수 = 커널 사이즈 * 채널 수 * 출력 차원 수  = (3*3) * 16 * 32

bias 수 = 출력 차원 수 = 32

총 parameter 수 = weight 수 + bias 수 = 4640





# tf.keras.layers.Dense



### 1. Dense 층의 매개변수

```python
tf.keras.layers.Dense(
    units, activation=None, use_bias=True,
    kernel_initializer='glorot_uniform',
    bias_initializer='zeros', kernel_regularizer=None,
    bias_regularizer=None, activity_regularizer=None, kernel_constraint=None,
    bias_constraint=None, **kwargs
)
```

| 인수                   |                                                              |
| :--------------------- | ------------------------------------------------------------ |
| `units`                | 양의 정수, 출력 공간의 차원입니다.                           |
| `activation`           | 사용할 활성화 기능입니다. 아무 것도 지정하지 않으면 활성화가 적용되지 않습니다(예: "선형" 활성화: ). `a(x) = x` |
| `use_bias`             | 부울, 레이어에서 바이어스 벡터를 사용하는지 여부.            |
| `kernel_initializer`   | `kernel`가중치 행렬 의 초기화 프로그램입니다 .               |
| `bias_initializer`     | 바이어스 벡터에 대한 이니셜라이저입니다.                     |
| `kernel_regularizer`   | `kernel`가중치 행렬 에 적용된 정규화 함수 .                  |
| `bias_regularizer`     | 편향 벡터에 적용된 정규화 함수.                              |
| `activity_regularizer` | 레이어의 출력에 적용된 레귤러라이저 기능("활성화").          |
| `kernel_constraint`    | `kernel`가중치 행렬 에 적용되는 제약 조건 함수 입니다.       |
| `bias_constraint`      | 바이어스 벡터에 적용되는 제약 조건 함수입니다.               |



### 2. Dense 층의 parameter수

W = Dense 층의 weight 수

B = Dense 층의 bias 수

N = 노드의 개수

I = input data 개수

P = 컨볼루션 층의 parameter 개수
$$
W = I * N\\
B = N\\
P = W + B
$$
Example ) 

`model.add(keras.layers.Dense(32, activation='relu'))`

weight 수 = input data 수 * 노드의 개수 = 800 * 32

bias 수 = 노드의 개수 = 32

총 parameter 수 = weight 수 + bias 수 = 25632





---------------

###### 출처

https://stackoverflow.com/questions/50212330/dense-layer-weights-shape

https://seongkyun.github.io/study/2019/01/25/num_of_parameters/

https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense

https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv2D