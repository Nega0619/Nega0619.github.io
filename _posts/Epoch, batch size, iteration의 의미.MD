# Epoch

전체 데이터 셋에 대하여 foward pass/ backward pass 학습을  진행 



전체 데이터가 순전파와 역전파를 통해 신경망을 한 번 통과했습니다.



# Batch Size

batch size는 Single batch이 구성하고 있는 데이터 샘플의 크기를 의미합니다. Single batch는 mini-batch라고 보통 표현됩니다.



전체 데이터셋을 batch size만큼의 수로 여러개 나눕니다.



전체 데이터 셋을 나누는 이유는, 트레이닝 데이터를 통째로 신경망에 넣으면 메모리 초과가 날 수 도 있기 때문입니다.



**batch size만큼 feed foward 학습과 backprop 학습이 둘 다 진행됩니다.**



# Iteration

전체 데이터는 batch size수 만큼 데이터가 나뉘어져 있고, ㅑiteration수만큼 batch를 학습하면 한 epoch를 학습하게 되는 것입니다.



즉, 1-epoch를 마치는데 필요한 mini batch의 갯수



# 총정리

5000개의 데이터가 존재하고 epoch = 100, batch_size = 250이라고 가정합니다.



전체 데이터는 batch_size 수인 250만큼 나뉘고 1 Epoch당 250개 씩 데이터를 받아 20 iteration 학습합니다.



그리고 전체 데이터는 100번 학습됩니다.

