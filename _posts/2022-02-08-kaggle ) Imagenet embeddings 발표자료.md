오늘은 새로 가입한 kaggle 스터디에서 발표준비를 하면서 공부한 내용을 정리해 보았습니다.

주제는 kaggle의 Pawpularity Contest이며,  링크는 다음과 같습니다.



- kaggle의 주제 사이트

[PetFinder.my - Pawpularity Contest]: https://www.kaggle.com/c/petfinder-pawpularity-score

- 분석할 코드 링크

[Imagenet embeddings+RAPIDS SVR+Finetuned models]: https://www.kaggle.com/titericz/imagenet-embeddings-rapids-svr-finetuned-models/data



< 목차 > 

- 사전지식
  - 이미지 임베딩
  - Rapids SVR
  - Finetuned models
  - StatifiedKFold
- 코드 분석



### 0. 소개(라 하고 약치기라고 이해한다)



- 아팠음!
- 결론 -> 다못했음!
- 어차피 다들 코드 한번 본다고 바로 못쓰자나?!(당당) 전 그래요!ㅋㅋㅋㅋ



이번 발표의 목표

- 후에 kaggle이나 익스노드를 진행할 때, 이런 아이디어가 있더라 하는 아이디어만이라도 줍줍해가시면 좋겠습니다 :>
- 가끔 보이는 파이썬 기법들 줍줍해가시면 좋겠습니다 map이라던지 lambda라던지.
- 코드 이해는 못하셔도 됩니다! 저도 못했거든요^^,,,



[PetFinder.my](https://petfinder.my/) 는 180,000마리 이상의 동물과 54,000마리가 행복하게 입양된 말레이시아 최고의 동물 복지 플랫폼입니다. PetFinder는 동물 애호가, 미디어, 기업 및 글로벌 조직과 긴밀하게 협력하여 동물 복지를 개선합니다.

현재 PetFinder.my는 기본 [귀여움 측정기](https://petfinder.my/cutenessmeter) 를 사용하여 애완 동물 사진의 순위를 매깁니다. 수천 개의 애완 동물 프로필의 성능과 비교하여 사진 구성 및 기타 요소를 분석합니다. 이 기본 도구는 유용하지만 아직 실험 단계에 있으며 알고리즘을 개선할 수 있습니다.



![image-20220211190235834](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220211190235834.png)

![image-20220211190315705](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220211190315705.png)



# 1. 이미지 임베딩이란? image Embedding



### 임베딩 Embedding이란?

머신러닝의 핵심은 데이터에서 패턴을 찾는 것입니다. 머신러닝은 데이터의 특징으로부터 데이터의 패턴을 찾아 유사 데이터를 뽑아내거나 분류합니다. 하지만, **데이터는 우리가 적절한 특징을 찾도록 구조화되지 않는 경우가 많습니다.** 이러한 경우 구조화되있지 않은 데이터의 특징을 나타내는 벡터가 필요한데 이러한 벡터를 ***임베딩 벡터*** 라고 부릅니다.

**고차원의 정보를 저차원으로 변환하면서 필요한 정보만 임베딩 벡터에 보존하는 것을 임베딩**이라고 합니다. 임베딩 과정을 통해 컴퓨터는 데이터에 대한 저차원 임베딩 벡터를 생산하고 이를 통해 새로운 데이터에 대한 예측을 진행하게 됩니다.



다음 예를 통해 쉽게 이해하실 수 있습니다.

![image-20220209112930636](../images/pages/image-20220209112930636.png)

위와 같이 범주형 데이터가 있다고 할때, ID에는 아무 의미가 없기 때문에 이 데이터를 학습하기 위해선 다른 전처리가 필요합니다. 보통 일반적인 방법은 one-hot-encoding입니다.

![image-20220209113257816](../images/pages/image-20220209113257816.png)

위의 변환된 벡터 값을 이용하여 참치김치찌개와 다른 메뉴 사이의 거리를 계산하면 다음과 같습니다.

![image-20220209113310297](../images/pages/image-20220209113310297.png)

김치찌개 종류가 참치초밥과 된장찌개와는 다를텐데 전부 거리가 같습니다.

이런 경우, 메뉴 추천시스템에서 어제 참치김치찌개를 먹어도 비슷한 생고기 김치찌개를 추천해주게 됩니다.



임베딩을 적용하면 다음과 같습니다. 변환되는 벡터의 차원은 3차원으로 정하고 각각 찌개 강함 정도, 고기 선호도, 건강한 정도를 나타낸다고 합시다. 

![image-20220209113511472](../images/pages/image-20220209113511472.png)

참치김치찌개와 다른 메뉴 사이의 거리를 다시 계산해보면 다음과 같습니다.

![image-20220209113605829](../images/pages/image-20220209113605829.png)

복합적인 모든 특성이 통합되어있는 고차원 데이터를 각각의 특성요소를 떼서 저차원 벡터를 만드는 과정을 임베딩이라고 합니다.



- https://simonezz.tistory.com/43
- https://velog.io/@dongho5041/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EC%9D%B8%EA%B3%B5%EC%8B%A0%EA%B2%BD%EB%A7%9D%EC%9D%98-Embedding%EC%9D%B4%EB%9E%80
- 임베딩 추천 글 : https://blog.linewalks.com/archives/6408



### 이미지 임베딩

잡지 표지 이미지를 보고 비슷한 잡지를 찾는다고 가정할 때, pixel-by-pixel로 유사성을 측정하는 것은 의미가 없습니다. 이런 경우, 임베딩을 추출하면 semantic information을 통해 효과적으로 측정할 수 있다. 



# 2. Transfer learning

많은 사람들이 ML 모델들을 이용해서 고차원이거나 복잡하거나 구조화되지 않은 데이터를 임베딩으로 인코딩하고자한다.

 

텍스트나 이미지, 그 외 데이터들을 인코딩하기 위해 **미리 트레이닝된 네트워크**를 이용하여 임베딩 시키는 것을 ***transfer learning***이라 한다. 



그러므로, **Transfer Learning**을 통해 지식을 재사용할 수 있을 뿐만 아니라, 트레이닝 시키는 시간을 대폭 줄일 수 있다.



임베딩은 모델 다음방법으로 적용가능합니다. **임베딩을 인풋 feature vectors로 사용**하는 것이다. Base input featrues를 미리 트레이닝된 임베딩에 쌓아서 인풋 feature vector를 형성할 수 있다. 키포인트는 이러한 임베딩이 트레이닝이 가능하지 않다는 것이다. 즉, 모델을 트레이닝시킬 때 튜닝되지 않는다.



![image-20220209115041484](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220209115041484.png)

- https://simonezz.tistory.com/44



# 3. RAPIDs SVR

래피즈(Rapids)는 엔비디아에서 개발하고 관리하는 오픈 소스 GPU 가속 데이터 과학 및 머신러닝 라이브러리입니다. Panda, Scikit-learn, numpy 등과 같은 기존의 많은 CPU 도구와 호환되도록 설계되었습니다. 많은 데이터 과학 및 기계 학습 작업을 대규모 가속화할 수 있으며, 종종 100X 또는 그 이상의 비율로 가속화할 수 있는 툴입니다. Rapids는 여전히 개발 중에 있다고 합니다.

https://rapids.ai/



# 4. Finetuned models

- fine tuning이란?

  \- **기존에 학습되어져 있는 모델을 기반으로 아키텍쳐를 새로운 목적(나의 이미지 데이터에 맞게)변형하고 이미 학습된 모델 Weights로 부터 학습을 업데이트하는 방법**을 말합니다.

  

- 예시

  고양이와 개 분류기를 만드는데 다른 데이터로 학습된 모델(VGG16, ResNet 등) 을 가져다 쓰는 경우

  

  VGG16 모델의 경우 1000 개의 카테고리를 학습시켰기 때문에 고양이와 개, 2개의 카테고리만 필요한 우리 문제를 해결하는데 모든 레이어를 그대로 쓸 수는 없습니다.

  

  따라서 가장 쉽게 이용하려면 내 데이터를 해당 모델로 예측(predict)하여 보틀넥 피쳐만 뽑아내고, 이를 이용하여 풀리 커넥티드 레이어(Fully-connected layer) 만 학습시켜서 사용하는 방법을 취하면 됩니다.

  - 보틀넥 피처 : 가장 마지막 CNN 블록을 의미(모델에서 가장 추상화된 피처)하며, Fully connected layer 직전의 CNN블록의 결과를 의미한다.

  





출처: https://eehoeskrap.tistory.com/186 [Enough is not enough]

# 5. Stratifiedkfold

- k-fold : 학습데이터셋과 검증 데이터 셋을 나누어서 진행하는 것

- startifiedkfold : 불균형한 dataset일 경우 사용하는 kfold 방법

  - k개의 fold를 분할한 이후에도 전체 훈련 데이터의 class 비율과 각 fold가 가지고 있는 class 비율을 맞춰줍니다.

  - 파라미터

    n_splits : Fold의 개수 k 값 (정수형, 기본 : 5)
    
    shuffle : 데이터를 쪼갤 때 섞을지 유무 (True/False, 기본 : False)
    
    random_state : 난수 설정
    
    https://8888-wpkwtqrc21bbba6iczeu3g152.e.prod.connect.ainize.ai/notebooks/aiffel/Untitled.ipynb
    
    ```python
    features = iris.data
    labels = iris.target
    dt_clf = DecisionTreeClassifier(random_state=11)
    
    iris_df = pd.DataFrame(data=iris.data, columns = iris.feature_names)
    iris_df['label']=iris.target
    
    skf = StratifiedKFold(n_splits=5)
    n_iter = 0

    for train_index, val_index in skf.split(iris_df, iris_df['label']):
        print(train_index)
        print('\n\n\n')
        print(val_index, '\n\n\n')
        n_iter += 1
        
        label_train = iris_df['label'].iloc[train_index]
        label_val = iris_df['label'].iloc[val_index]
        
        X_train, X_val = features[train_index], features[val_index]
        Y_train, Y_val = labels[train_index], labels[val_index]
        dt_clf.fit(X_train, Y_train)
        
        print('###### cross validation ##### : {}'.format(n_iter))
        print('교차 검증 정확도 : {}'.format(accuracy_score(Y_val, dt_clf.predict(X_val))))
        print('학습 레이블 데이터 분포 \n', label_train.value_counts())
        print('검증 레이블 데이터 분포 \n', label_val.value_counts())
        print('\n\n\n')
    ```
    
    

- 코드 : https://guru.tistory.com/35

- 코드 : https://jinnyjinny.github.io/deep%20learning/2020/04/02/Kfold/

- 이론 : https://steadiness-193.tistory.com/287



# 5. 코드 분석

- OpenAI CLIP

![CLIP.png](../images/pages/CLIP.png)

CLIP(Contrastive Language-Image Pre-Training)은 다양한 (이미지, 텍스트) 쌍으로 훈련된 신경망입니다. (이미지, 물체 분류) 데이터 대신 (이미지, 텍스트)의 데이터를 사용하는데, 수작업 labeling 없이 웹 크롤링을 통해 자동으로 이미지와 그에 연관된 자연어 텍스트를 추출하여 4억개의 (이미지-텍스트) 쌍을 가진 거대 데이터셋을 구축하였습니다.

작업에 대한 직접 최적화 없이 주어진 이미지에서 가장 관련성 높은 텍스트 스니펫을 예측하도록 할 수 있습니다. CLIP의 성능은 ResNet50정도로 뛰어난 편에 속합니다.

CLIP은 ImageNet의 대용량 데이터셋으로 미리 학습시킨 네트워크를 가져와 약간의 추가적인 학습만 수행하여도 원하는 task에서 훌륭한 성능을 낼 수 있어, 여러 분야에서 활발히 활용되고 있습니다.

CLIP 제작자 설명  : https://github.com/openai/CLIP

- https://inforience.net/2021/02/09/clip_visual-model_pre_training/



```
train['path'] = train['Id'].map(lambda x: '../input/petfinder-pawpularity-score/train/'+x+'.jpg')
```

- train과 test 데이터에 path 컬럼 추가.

  path+id.jpg 값을 넣어줌.



```
if test.shape[0]<10:
    test = pd.concat([
        test, test, test, test, test, 
    ])
    test = test.reset_index(drop=True)
```

- 기존 test 데이터의 수를 늘려줌.

  if 블록의 역할은 기존의 test데이터 수를 늘려주는 것입니다.
  8 + 8 + 8 + 8 + 8로, 그대로 test데이터를 연장시켜주었습니다.
  (근데 같은 데이터값이고 index colum만 달라지는데 굳이..? 왜..?)



```
train['bins'] = (train['Pawpularity']//5).round()

train['fold0'] = -1
skf = StratifiedKFold(n_splits = 20, shuffle=True, random_state = 1)
for i, (_, test_index) in enumerate(skf.split(train.index, train['bins'])):
    train.iloc[test_index, -1] = i

train['fold0'] = train['fold0'].astype('int')
gc.collect()

train.groupby(['fold0'])['Pawpularity'].agg(['mean','std','count'])
```

- bins 열

  stratified K fold 라벨을 20개로 맞춰주기도 하고, stratified K fold에서 pawpularity의 라벨링 비율을 맞춰주기 위해 사용 (1-100 까지의 정답라벨 존재, //5 하면 20개로 나뉘어짐)

  

- StratifiedKFold
  - 불균형한 dataset일 경우 사용하는 kfold 방법
  - k개의 fold를 분할한 이후에도 전체 훈련 데이터의 class 비율과 각 fold가 가지고 있는 class 비율을 맞춰줍니다.

  

- enumerate문

  나도 모르겠음.

  

- astype

  train['fold0'] 컬럼을 int형으로 변환

  

- groupby

  `train.groupby(['fold0'])['Pawpularity'].agg(['mean','std','count'])`

  fold마다 pawpularity의 mean(평균), std(표준편차), count(개수)값을 나타내줍니다.

  fold마다 제대로 stratified k fold 잘 되었는지 확인하기 위해 쓴듯

  ![image-20220209202030083](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220209202030083.png)



## timm library

PyTorch Image Models(timm)는 이미지 모델, 레이어, 유틸리티, 옵티마이저, 스케줄러, 데이터 로더/증강 및 참조 교육/검증 스크립트의 모음으로, ImageNet 교육 결과를 재현할 수 있는 다양한 SOTA 모델을 통합하는 것을 목표로 합니다.

![image-20220209011128949](../images/pages/image-20220209011128949.png)

https://github.com/rwightman/pytorch-image-models#introduction



> 보다시피 timm 라이브러리에서 사용할 수 있는 사전 훈련된 모델 아키텍처가 575개 있습니다.¶
>
> 
>
> 솔루션의 첫 번째 부분은 기본적으로 해당 모델의 마지막 계층에서 특징을 추출하고 추출된 특징에 대해 SVR을 실행하는 것입니다
>
> 
>
> timm의 모델들은 대부분 Imagenet에서 1000개의 클래스를 사용하여 훈련되기 때문에 출력 모양은 모델마다 1000개입니다.
> 모든 575 모델에서 기능을 추출하는 것은 특히 제출 시간이 9시간이라는 점을 고려할 때 말도 안되고 상상도 할 수 없는 일이다. 따라서 RMSE 측면에서 성능이 좋은 모델의 하위 집합을 찾아보았습니다.
>
> 
>
> 이를 위해 RMSE 힐 클라이밍 알고리즘에 따라 전진 모델 선택 알고리즘을 사용했다. 한 모델부터 시작하여 RMSE 성능 향상을 중지할 때까지 모델을 계속 추가합니다.
>
> 이제 Imagenet 사전 훈련된 모델 기능 추출을 시작하겠습니다.
> 이 솔루션에 사용된 전진 모델 선택 알고리즘에 의해 발견된 사전 교육된 모델은 위에 나열되어 있습니다.



```python
modelpath = { m.split('/')[-1].split('.')[0] :m for m in glob('../input/pytorch-pretrained-0/*.pt')+glob('../input/pytorch-pretrained-1/*.pt')+glob('../input/pytorch-pretrained-2/*.pt')+glob('../input/pytorch-pretrained-3/*.pt')}
modelpath
```

 str = ../input/pytorch-pretrained-0/resnetv2_101x1_bitm.pt 일때, 

str.split('/') 의 결과

> ['..', 'input', 'pytorch-pretrained-0', 'resnetv2_101x1_bitm.pt']

str.split('/')[-1] 의 결과

>  'resnetv2_101x1_bitm.pt'

str.split('/')[-1].split('.')의 결과

> ['resnetv2_101x1_bitm', 'pt']



< 참고 >

![image-20220209011713971](../images/pages/image-20220209011713971.png)





```python
# EMB TEST가 뭘까
EMB_TEST = {}
for arch in names:
    starttime = time.time()

    model = timm.create_model(arch, pretrained=False).to('cuda')
    model.load_state_dict(torch.load(modelpath[arch]))
    model.eval()

    train_dataset = PawpularDataset(
        images = test.Id.values,
        base_path='../input/petfinder-pawpularity-score/test/',
        modelcfg = resolve_data_config({}, model=model),
        aug = 0,
    )
    BS = 10 if arch in ['tf_efficientnet_l2_ns'] else 16
    train_dataloader = DataLoader(train_dataset, batch_size=BS, num_workers= 2, shuffle=False)
    
    with torch.no_grad():
        res = [model(img.to('cuda')).cpu().numpy() for img in train_dataloader]
    res = np.concatenate(res, 0)
    EMB_TEST[arch] = res
    
    print( arch, ', Done in:', int(time.time() - starttime), 's' )
    
    del model, res
    torch.cuda.empty_cache() # PyTorch thing to clean RAM
    gc.collect()

print(time.time() )    
len(EMB_TEST), EMB_TEST.keys()
```

- timm.create_model(arch, pretrained=False).to('cuda')
  - timm.create_model

    > `timm` is a deep-learning library created by [Ross Wightman](https://twitter.com/wightmanr) and is a collection of SOTA computer vision models, layers, utilities, optimizers, schedulers, data-loaders, augmentations and also training/validating scripts with ability to reproduce ImageNet training results.

    https://fastai.github.io/timmdocs/

    pretrained=false인 상태로 names의 객체들을 불러와 모델을 생성해줍니다. gpu를 사용할 수 있도록 `.to('cuda')`명령어를 사용해 주었습니다.

    그밖의 gpu를 사용할 수있는 명령어는 다음과 같습니다.

    - x = torch.tensor([1., 2.], device="cuda")  

    - x = torch.tensor([1., 2.]).cuda()  

    - https://y-rok.github.io/pytorch/2020/10/03/pytorch-gpu.html

      

- load_state_dict(torch.load(modelpath[arch]))
  - load_state_dict는 가중치를 불러오는 메소드입니다.
  - 새로운 모델 인스턴스를 생성한 후에 가중치를 불러올 경우, pretrained =False를 주고, load_state_dict메소드를 이용해 가중치 값을불러오게 됩니다.

  - https://tutorials.pytorch.kr/beginner/basics/saveloadrun_tutorial.html

  

- model.eval()
  - 추론(inference)을 하기 전에 `model.eval()` 메소드를 호출하여 드롭아웃(dropout)과 배치 정규화(batch normalization)를 평가 모드(evaluation mode)로 설정해야 합니다. 그렇지 않으면 일관성 없는 추론 결과가 생성됩니다.

  - `.eval()` 함수는 evaluation 과정에서 사용하지 않아야 하는 layer들을 알아서 off 시키도록 하는 함수이며

    evaluation/validation 과정에선 보통 `model.eval()`과 `torch.no_grad()`를 함께 사용합니다.

  - 사용 방법 관련 url : https://tutorials.pytorch.kr/beginner/basics/saveloadrun_tutorial.html

  - eval 이 무엇인지 : https://stackoverflow.com/questions/60018578/what-does-model-eval-do-in-pytorch/60018731#60018731

  - eval 이 무엇인지 : https://bluehorn07.github.io/2021/02/27/model-eval-and-train.html

  

- DataLoader(train_dataset, batch_size=BS, num_workers= 2, shuffle=False)
  - dataloader는 기본적으로 단일 프로세스 데이터 로드를 사용합니다. num_workers 에 양의 정수를 설정하게되면, 지정된 수의 로더 작업자 프로세스로 멀티 프로세스 데이터 로드가 켜집니다.

  - https://tutorials.pytorch.kr/beginner/basics/data_tutorial.html



- with torch.no_grad():
  - 자원을 획득하고 사용 후 반납해야 하는 경우에 주로 사용합니다.
  - 프로세스 : 현재 실행되고 있는 프로그램
    - 자원 : 기억장치 Ram이나 프로세서 디스크 등의 하드웨어 장치나 메시지, 파일등의 소프트웨어 요소들
  - https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=wideeyed&logNo=221653260516



Now its time to fit a GPU accelerated SVR using cuml[¶](https://www.kaggle.com/titericz/imagenet-embeddings-rapids-svr-finetuned-models/notebook#Now-its-time-to-fit-a-GPU-accelerated-SVR-using-cuml)


```python
for fold in range(train[kfoldcol].max()+1):
        ind_train = train[kfoldcol] != fold
        ind_valid = train[kfoldcol] == fold

```

- 이해못함 ^_^/



```python
model = SVR(C=16.0, kernel='rbf', degree=3, max_iter=4000, output_type='numpy')
        model.fit(TRAIN[ind_train], train.Pawpularity[ind_train].clip(1, 85)  )

        ypredtrain_[ind_valid] = np.clip(model.predict(TRAIN[ind_valid]), 1 , 100)
        ypredtest_ += np.clip(model.predict(TEST), 1, 100)
```

- SVR

  Rapids SVR을 사용하는 방법은 쉽네요. cuml.svm 패키지에서 SVR을 import 해서 사용해주었습니다.

  

- np.clip(array, min, max)

  array내의 element에 대하여 min값보다 작은 값들을 min으로 바꿔주고, max보다 큰 값을 max로 만들어줍니다.



> 첫째, 각 아키텍처에 대해 하나의 SVR을 독립적으로 장착합니다.



```python
for col in names:
    
    TRAIN = EMB_TRAIN[col].copy()
    TEST = EMB_TEST[col].copy()

    scaler = StandardScaler()
    scaler.fit( np.vstack((TRAIN, TEST)) )
    TRAIN = scaler.transform(TRAIN)
    TEST = scaler.transform(TEST)
    
    ypredtrain, ypredtest = fit_gpu_svr(TRAIN, TEST, 'fold0')
    print(rmse(train.Pawpularity,ypredtrain), col)    
```

- StandartScaler()

  <img src="D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220209213749442.png" alt="image-20220209213749442" style="zoom: 67%;" /> ->![image-20220209213757080](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220209213757080.png)

  - 평균 0 , 분산 1로 조정합니다

  - 보통의 싸이킷런 함수들과 비슷하게 fit, transform, fit_transform 다 지원합니다.

  - https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=demian7607&logNo=222009975984



- np.vstack
  - 두 배열을 위에서 아래로 붙이기
  - ![image-20220211200744146](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimage-20220211200744146.png)
  - np.hstack
    - 두 배열을 왼쪽에서 오른쪽으로 붙이기
  - https://rfriend.tistory.com/352
- StandardScaler.transform()
  - 정규화 / 표준화 Standardization, z = (𝑥-𝜇)/𝜎





> 위에서 볼 수 있듯이, 개별 아키텍처에서 추출된 기능의 SVR RMSE 범위는 17.56 ~ 18.52입니다.
>
> 하지만 SVR에 맞추기 전에 아키텍처 기능을 나란히 쌓으면 어떻게 될까요?
>
> 일부 기능 연결 및 표준화
>
> 모든 K 폴드를 사용하여 SVR A를 장착합니다.
> RMSE를 약간 증가시킨 85에서 목표물을 클립하는 것을 발견했어요.



> 또한 1.032의 승수를 사용하면 CV와 LB가 모두 향상된다는 것을 알게 되었습니다. 이는 SRV가 RMSE가 아닌 평균 제곱 오차를 최적화하기 때문일 수 있다.



> 이제 두 번째 feature 부분 집합을 사용하여 SVR B를 장착합니다. 더 많은 하위 집합을 적합시키는 아이디어는 후방 모델 앙상블에 다양성을 추가하고 형상 수를 너무 많이 증가시키는 차원성의 저주를 피하기 위한 것이다.



> 이제 딥 러닝 미세 조정된 이미지 모델을 사용하여 추론을 실행합니다.



```python
from torch.utils.data import Dataset, DataLoader
import albumentations as A

device = torch.device('cuda')
class Config:
    model_name = "swin_large_patch4_window7_224"
    base_dir = "../input/petfinder-pawpularity-score"
    data_dir = base_dir
    model_dir = "exp"
    output_dir = model_dir
    img_test_dir = os.path.join(data_dir, "test")
    model_path = "swin_large_patch4_window7_224"
    im_size =  384
    batch_size = 16


class PetDataset(Dataset):
    def __init__(self, image_filepaths, targets, transform=None):
        self.image_filepaths = image_filepaths
        self.targets = targets
        self.transform = transform
    
    def __len__(self):
        return len(self.image_filepaths)

    def __getitem__(self, idx):
        image_filepath = self.image_filepaths[idx]
        with open(image_filepath, 'rb') as f:
            image = Image.open(f)
            image_rgb = image.convert('RGB')
        image = np.array(image_rgb)

        if self.transform is not None:
            image = self.transform(image = image)["image"]
        
        image = image / 255
        image = np.transpose(image, (2, 0, 1)).astype(np.float32)
        target = self.targets[idx]

        image = torch.tensor(image, dtype = torch.float)
        target = torch.tensor(target, dtype = torch.float)
        return image, target    


def get_inference_fixed_transforms(mode=0, dim = 224):
    if mode == 0: # do not original aspects, colors and angles
        return A.Compose([
                A.SmallestMaxSize(max_size=dim, p=1.0),
                A.CenterCrop(height=dim, width=dim, p=1.0),
            ], p=1.0)
    elif mode == 1:
        return A.Compose([
                A.SmallestMaxSize(max_size=dim+16, p=1.0),
                A.CenterCrop(height=dim, width=dim, p=1.0),
                A.HorizontalFlip(p = 1.0)
            ], p=1.0)


class PetNet(nn.Module):
    def __init__(
        self,
        model_name = Config.model_path,
        out_features = 1,
        inp_channels = 3,
        pretrained = False,
    ):
        super().__init__()
        self.model = timm.create_model(model_name, pretrained=False, in_chans=3, num_classes = 1)
    
    def forward(self, image):
        output = self.model(image)
        return output    


def tta_fn(filepaths, model, ttas=[0, 1]):
    print('Image Size:', Config.im_size)
    model.eval()
    tta_preds = []
    for tta_mode in ttas:#range(Config.tta_times):
        print(f'tta mode:{tta_mode}')
        test_dataset = PetDataset(
          image_filepaths = filepaths,
          targets = np.zeros(len(filepaths)),
          transform = get_inference_fixed_transforms(tta_mode, dim = Config.im_size )
        )
        test_loader = DataLoader(
          test_dataset,
          batch_size = Config.batch_size,
          shuffle = False,
          num_workers = 2,
          pin_memory = True
        )
        #stream = tqdm(test_loader)
        tta_pred = []
        for images, target in test_loader:#enumerate(stream, start = 1):
            images = images.to(device, non_blocking = True).float()
            target = target.to(device, non_blocking = True).float().view(-1, 1)
            with torch.no_grad():
                output = model(images)

            pred = (torch.sigmoid(output).detach().cpu().numpy() * 100).ravel().tolist()
            tta_pred.extend(pred)
        tta_preds.append(np.array(tta_pred))
    
    fold_preds = tta_preds[0]
    for n in range(1, len(tta_preds)):
        fold_preds += tta_preds[n]
    fold_preds /= len(tta_preds)
        
    del test_loader, test_dataset
    gc.collect()
    torch.cuda.empty_cache()
    return fold_preds    
```





> cuML SVR A, B, C 및 이미지 모델 앙상블의 OOF 예측을 사용하여 전체 RMSE를 최적화합니다.

## OOF out of fold

- 머신러닝 모델의 성능을 평가하는 방법

- OOF 방식은 실무보다는 Kaggle, Dacon과 같은 예측 알고리즘 대회에서 자주 사용되는 방식이며 fold를 사용

- 두가지 방식

  - Stacking
    -  5 Fold 기반으로 개별 모델이 5회 번갈아 가면 4/5 학습 데이터로 학습하고, 1/5 학습 데이터로 예측하여 별도의 학습 데이터를 만듭니다. 그리고 이렇게 만들어진 학습 데이터를 다시 메타 모델이(아마도 Model 6) 학습하여 최종 예측 하는 방식입니다. 
  - 개별 예측값 평균하는 방법
    - K-Fold로 학습을 수행 한 뒤 예측을 테스트 데이터에 K번 만큼 수행한 뒤 개별 예측값을 평균하여 최종 예측
    - ![img](D:%5CGithub%5CtistoryPostings%5Cartificial%20Intelligence%5Cimages%5Cpages%5Cimg.png)

- https://techblog-history-younghunjo1.tistory.com/142

- 위 그림의 좌측은 4개의 Fold로 교차검증하는 K=4 일때의 K-fold 교차검증 방법이다. 이렇게 총 4번의 교차검증으로 (동일한 하이퍼파라미터를 사용한) 예측 알고리즘으로 각 검증 데이터로 평가한 Model 1~4를 만들어낸다.(이 때, Model 1,2,3,4에서 각각 최적화된 파라미터 값들은 다를 것이다. 왜냐하면 각 모델이 학습한 데이터가 서로 다르기 때문이다.)

- 그리고 난 후, **Model 1~4를 동일한 테스트 데이터에 대해 예측**하도록 하여 각 Model 별 테스트 데이터에 대한 예측값을 계산한다. 이렇게 되면 위 그림에서 노란색 박스의 Model 1~4 예측값이 나오게 된다. 

   

  마지막으로 할 일은 **이 4개의 예측값들의 평균값을 취하여 테스트 데이터에 대한 최종 예측값을 계산**한다. 