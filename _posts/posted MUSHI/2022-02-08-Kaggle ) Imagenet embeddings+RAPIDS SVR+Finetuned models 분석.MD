오늘은 새로 가입한 kaggle 스터디에서 발표준비를 하면서 공부한 내용을 정리해 보았습니다.

주제는 kaggle의 Pawpularity Contest이며,  링크는 다음과 같습니다.

- kaggle의 주제 사이트

[PetFinder.my - Pawpularity Contest]: https://www.kaggle.com/c/petfinder-pawpularity-score

- 분석할 코드 링크

[Imagenet embeddings+RAPIDS SVR+Finetuned models]: https://www.kaggle.com/titericz/imagenet-embeddings-rapids-svr-finetuned-models/data



< 목차 >





# 1. Pawpularity Data



## sample_submission.csv

[id, pawpularity] 로 구성



## test.csv

train에서 pawpularity가 없는 데이터입니다.

id값 빼고는 전부 0, 1 이진데이터입니다.

| id            | The photos unique Pet Profile ID corresponding to the photo's file name. |
| ------------- | ------------------------------------------------------------ |
| Subject Focus | Pet stands out against uncluttered background, not too close / far. |
| Eyes          | Both eyes are facing front or near-front, with at least 1 eye / pupil decently clear. |
| Face          | Decently clear face, facing front or near-front.             |
| Near          | Single pet taking up significant portion of photo (roughly over 50% of photo width or height). |
| Action        | Pet in the middle of an action (e.g., jumping).              |
| Accessory     | Accompanying physical or digital accessory / prop (i.e. toy, digital sticker), excluding collar and leash. |
| Group         | More than 1 pet in the photo.                                |
| Collage       | Digitally-retouched photo (i.e. with digital photo frame, combination of multiple photos). |
| Human         | Human in the photo.                                          |
| Occlusion     | Specific undesirable objects blocking part of the pet (i.e. human, cage or fence). Note that not all blocking objects are |
| Info          | Custom-added text or labels (i.e. pet name, description).    |
| Blur          | Noticeably out of focus or noisy, especially for the pet’s eyes and face. For Blur entries, “Eyes” column is always set to 0. |



## train.csv

id값, pawpularity 빼고는 전부 0, 1 이진데이터입니다.

pawpulairty는 1-100까지의 int형 data입니다.

| id            | The photos unique Pet Profile ID corresponding to the photo's file name. |
| ------------- | ------------------------------------------------------------ |
| Subject Focus | Pet stands out against uncluttered background, not too close / far. |
| Eyes          | Both eyes are facing front or near-front, with at least 1 eye / pupil decently clear. |
| Face          | Decently clear face, facing front or near-front.             |
| Near          | Single pet taking up significant portion of photo (roughly over 50% of photo width or height). |
| Action        | Pet in the middle of an action (e.g., jumping).              |
| Accessory     | Accompanying physical or digital accessory / prop (i.e. toy, digital sticker), excluding collar and leash. |
| Group         | More than 1 pet in the photo.                                |
| Collage       | Digitally-retouched photo (i.e. with digital photo frame, combination of multiple photos). |
| Human         | Human in the photo.                                          |
| Occlusion     | Specific undesirable objects blocking part of the pet (i.e. human, cage or fence). Note that not all blocking objects are |
| Info          | Custom-added text or labels (i.e. pet name, description).    |
| Blur          | Noticeably out of focus or noisy, especially for the pet’s eyes and face. For Blur entries, “Eyes” column is always set to 0. |
| Pawpularity   | The Pawpularity Score is derived from each pet profile's page view statistics at the listing pages, using an algorithm |



# 2. OpenAI CLIP

CLIP(Contrastive Language-Image Pre-Training)은 다양한 (이미지, 텍스트) 쌍으로 훈련된 신경망입니다. GPT-2 및 3의 제로샷 기능과 유사하게 작업에 대한 직접 최적화 없이 주어진 이미지에서 가장 관련성 높은 텍스트 스니펫을 예측하도록 자연어로 지시할 수 있습니다. 우리는 CLIP이 원본 ResNet50의 성능과 일치함을 발견했습니다. ImageNet에서 원래 128만 개의 레이블이 지정된 예제를 사용하지 않고 "제로샷"을 수행하여 컴퓨터 비전의 몇 가지 주요 문제를 극복했습니다.

참고 링크 : https://github.com/openai/CLIP



# Image Embedding



### 임베딩 Embedding이란?

고차원 벡터를 저차원 공간으로 변환하는 것입니다. 이상적으로, 임베딩은 임베딩 공간에서 의미적으로 비슷한 입력 사항들을 가깝게 배치함으로써 입력에 포함된 의미 중 일부를 포착합니다. 

- https://cloud.google.com/architecture/overview-extracting-and-serving-feature-embeddings-for-machine-learning?hl=ko



### 이미지 임베딩

텍스트 시스템과 달리, 이미지 처리 시스템은 개별적인 원시 픽셀 강도를 지닌 이미지를 나타내는 풍부하고 고차원적인 데이터세트로 작동합니다. 하지만 원래의 밀도가 높은 형태의 이미지는 일부 작업에 그리 유용하지 않을 수 있습니다. 예를 들어 잡지 표지 이미지를 보고 비슷한 잡지를 찾거나 참조 사진과 비슷한 사진을 찾아야 한다고 가정해보세요. 입력 사진의 원시 픽셀(2,048 ✕ 2,048)을 다른 사진과 비교하여 비슷한지 여부를 찾는 것은 효율적이거나 효과적이지 않습니다. 하지만 이미지의 저차원적 특성 벡터(임베딩)를 추출하면 이미지에 포함된 내용이 무엇인지를 나타내는 일정한 지표를 얻고, 더 효과적으로 비교할 수 있습니다.

핵심은 대규모 이미지 데이터 세트(예: [ImageNet](https://arxiv.org/abs/1409.4842))에서 [Inception](https://arxiv.org/abs/1512.03385), [ResNet(Deep Residual Learning)](https://ai.googleblog.com/2017/11/automl-for-large-scale-image.html) 또는 [NASNet(Network Architecture Search)](http://image-net.org/)과 같은 이미지 분류 모델을 학습시키는 것입니다. 그런 다음 마지막 [softmax](https://wikipedia.org/wiki/Softmax_function) 분류 기준이 *없는* 모델을 사용하여 입력 테스트에 따라 특징 벡터를 추출합니다. 이 유형의 특징 벡터로 검색 작업 또는 유사성 일치 작업에서 이미지를 효과적으로 표현할 수 있습니다. 특징 벡터는 다른 특성과 함께 ML 작업용 추가 입력 특성(벡터)으로 작동할 수 있습니다. 예를 들어 의류를 쇼핑 중인 고객에게 패션 아이템을 추천하는 시스템에서는 색상, 치수, 가격, 유형, 하위유형을 비롯한 개별 항목을 기술하는 속성이 사용될 수 있습니다. 패션 아이템 이미지에서 추출된 특성과 함께 이러한 모든 특성을 추천 모델에서 사용할 수 있습니다.



# RAPID SVR





# StratifiedKFold

- k-fold : 학습데이터셋과 검증 데이터 셋을 나누어서 진행하는 것
- startifiedkfold : 불균형한 dataset일 경우 사용하는 kfold 방법
  - k개의 fold를 분할한 이후에도 전체 훈련 데이터의 class 비율과 각 fold가 가지고 있는 class 비율을 맞춰줍니다.
  - ![img](../images/pages/img.png)

- 코드 : https://guru.tistory.com/35

- 이론 : https://steadiness-193.tistory.com/287



# 코드 분석

```python
train = pd.read_csv('../input/petfinderdata/train-folds-1.csv')
test = pd.read_csv('../input/petfinder-pawpularity-score/test.csv')
sub = pd.read_csv('../input/petfinder-pawpularity-score/sample_submission.csv')

train['path'] = train['Id'].map(lambda x: '../input/petfinder-pawpularity-score/train/'+x+'.jpg')
test['path'] = test['Id'].map(lambda x: '../input/petfinder-pawpularity-score/test/'+x+'.jpg')

#print(test)
# If its Public LB run, then augment Testset to chack batch size memory consumption.
if test.shape[0]<10:
    test = pd.concat([
        test, test, test, test, test, 
    ])
    test = test.reset_index(drop=True)
    
# print(test)    

print(train.shape, test.shape, sub.shape)

print(train.head(50))    

```

## train과 test 데이터에 path 컬럼 추가.

path+id.jpg 값을 넣어줌.



## 기존 test 데이터의 수를 늘려줌.

if 블록의 역할은 기존의 test데이터 수를 늘려주는 것입니다.
8 + 8 + 8 + 8 + 8로, 그대로 test데이터를 연장시켜주었습니다.
(근데 같은 데이터값이고 index colum만 달라지는데 굳이..? 왜..?)

## `test.reset_index(drop = True)` 

drop매개변수를 사용하면 DataFrame에서 인덱스를 완전히 삭제할지 여부를 지정할 수 있습니다 . drop = True하면 reset_index가 인덱스를 DataFrame의 열에 다시 삽입하는 대신 삭제합니다. 를 설정 drop = True하면 현재 인덱스가 완전히 삭제되고 숫자 인덱스가 이를 대체합니다.

- https://www.sharpsightlabs.com/blog/pandas-reset-index/



```python
# print(train.head(50))    

train['bins'] = (train['Pawpularity']//5).round()
# print(train['Pawpularity'])
# print(train['Pawpularity'].value_counts())
# print(train.head(50))    
print(train['bins'].value_counts())



train['fold0'] = -1
skf = StratifiedKFold(n_splits = 20, shuffle=True, random_state = 1)
for i, (_, test_index) in enumerate(skf.split(train.index, train['bins'])):
    train.iloc[test_index, -1] = i

print('test_index', test_index)    
print('test_index len', len(test_index))
# print(train.head(50))    
    
train['fold0'] = train['fold0'].astype('int')
gc.collect()

train.groupby(['fold0'])['Pawpularity'].agg(['mean','std','count'])
```

## bins 열

pawpularity의 정답라벨을 20개로 맞춰주기 위함 (1-100 까지의 정답라벨 존재, //5 하면 20개로 나뉘어짐)

## StratifiedKFold

- 불균형한 dataset일 경우 사용하는 kfold 방법

- k개의 fold를 분할한 이후에도 전체 훈련 데이터의 class 비율과 각 fold가 가지고 있는 class 비율을 맞춰줍니다.

## enumerate문

나도 모르겠음.

## astype

train['fold0'] 컬럼을 int형으로 변환

## groupby

`train.groupby(['fold0'])['Pawpularity'].agg(['mean','std','count'])`

fold마다 pawpularity의 mean(평균), std(표준편차), count(개수)값을 나타내줍니다.



```python
modelpath = { m.split('/')[-1].split('.')[0] :m for m in glob('../input/pytorch-pretrained-0/*.pt')+glob('../input/pytorch-pretrained-1/*.pt')+glob('../input/pytorch-pretrained-2/*.pt')+glob('../input/pytorch-pretrained-3/*.pt')}
modelpath

```

## 예시

-  str = ../input/pytorch-pretrained-0/resnetv2_101x1_bitm.pt 일때, 

  str.split('/') 의 결과

  > ../input/pytorch-pretrained-0/resnetv2_101x1_bitm.pt

  str.split('/')[-1] 의 결과

  > ['..', 'input', 'pytorch-pretrained-0', 'resnetv2_101x1_bitm.pt']

  str.split('/')[-1].split('.')의 결과

  > ['resnetv2_101x1_bitm', 'pt']



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



## timm.create_model(arch, pretrained=False).to('cuda')

- timm.create_model

  > `timm` is a deep-learning library created by [Ross Wightman](https://twitter.com/wightmanr) and is a collection of SOTA computer vision models, layers, utilities, optimizers, schedulers, data-loaders, augmentations and also training/validating scripts with ability to reproduce ImageNet training results.

  https://fastai.github.io/timmdocs/

  pretrained=false인 상태로 names의 객체들을 불러와 모델을 생성해줍니다. gpu를 사용할 수 있도록 `.to('cuda')`명령어를 사용해 주었습니다.

  그밖의 gpu를 사용할 수있는 명령어는 다음과 같습니다.

  - x = torch.tensor([1., 2.], device="cuda")  

  - x = torch.tensor([1., 2.]).cuda()  

  - https://y-rok.github.io/pytorch/2020/10/03/pytorch-gpu.html

    

## load_state_dict(torch.load(modelpath[arch]))

- load_state_dict는 가중치를 불러오는 메소드입니다.
- 새로운 모델 인스턴스를 생성한 후에 가중치를 불러올 경우, pretrained =False를 주고, load_state_dict메소드를 이용해 가중치 값을불러오게 됩니다.

- https://tutorials.pytorch.kr/beginner/basics/saveloadrun_tutorial.html



## model.eval()

- 추론(inference)을 하기 전에 `model.eval()` 메소드를 호출하여 드롭아웃(dropout)과 배치 정규화(batch normalization)를 평가 모드(evaluation mode)로 설정해야 합니다. 그렇지 않으면 일관성 없는 추론 결과가 생성됩니다.

- `.eval()` 함수는 evaluation 과정에서 사용하지 않아야 하는 layer들을 알아서 off 시키도록 하는 함수이며

  evaluation/validation 과정에선 보통 `model.eval()`과 `torch.no_grad()`를 함께 사용합니다.

- 사용 방법 관련 url : https://tutorials.pytorch.kr/beginner/basics/saveloadrun_tutorial.html
- eval 이 무엇인지 : https://stackoverflow.com/questions/60018578/what-does-model-eval-do-in-pytorch/60018731#60018731
- eval 이 무엇인지 : https://bluehorn07.github.io/2021/02/27/model-eval-and-train.html



## DataLoader(train_dataset, batch_size=BS, num_workers= 2, shuffle=False)

- dataloader는 기본적으로 단일 프로세스 데이터 로드를 사용합니다. num_workers 에 양의 정수를 설정하게되면, 지정된 수의 로더 작업자 프로세스로 멀티 프로세스 데이터 로드가 켜집니다.

- https://tutorials.pytorch.kr/beginner/basics/data_tutorial.html



## with torch.no_grad():

- 자원을 획득하고 사용 후 반납해야 하는 경우에 주로 사용합니다.

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=wideeyed&logNo=221653260516

- ![image-20220208130816814](../images/pages/image-20220208130816814.png)







- [네이버 링크](www.naver.com)
- [네이버링크](www.naver.com)
- `[]` 기
- ****1**** 2 `*` 별표그대로






```python
class PawpularDataset_HFLIP:
    def __init__(self, images, base_path='../input/petfinder-pawpularity-score/train/', modelcfg=None, doflip=False ):
        
        self.images = images.copy()
        self.base_path = base_path
        self.transform = modelcfg
        self.doflip=doflip
        
    def __len__(self):
        return len(self.images)
    
    def __getitem__(self, item):
        img = Image.open(self.base_path + self.images[item] + '.jpg').convert('RGB')
        
        if self.doflip==True:
            img = img.transpose(PIL.Image.FLIP_LEFT_RIGHT)
            width, height = img.size
            img = img.crop((0.0*width, 0.02*height, 0.98*width, 0.98 * height))  
        
        img = self.transform(img)
        return img


for arch in names_hflip_crop:
    starttime = time.time()

    archname = arch.split('_hflip_')[0]
    if arch == 'tf_efficientnet_l2_ns_512':
        archname = 'tf_efficientnet_l2_ns'
    model = timm.create_model(archname, pretrained=False).to('cuda')
    model.load_state_dict(torch.load(modelpath[archname]))
    model.eval()
    
     # Get model default transforms
    transf = resolve_data_config({}, model=model)
    sz = int(arch.split('_')[-1])
    transf['input_size'] = (3, sz, sz)
    transf['crop_pct'] = 1.0        
    transf = create_transform(**transf)

    doflip = True if arch.split('_')[-2] == 'hflip' else False
    train_dataset = PawpularDataset_HFLIP(
        images = test.Id.values,
        base_path='../input/petfinder-pawpularity-score/test/',
        modelcfg = transf,
        doflip = doflip,
    )

    BS = 10 if archname in ['tf_efficientnet_l2_ns'] else 16
    train_dataloader = DataLoader(train_dataset, batch_size=BS, num_workers= 2, shuffle=False)

    with torch.no_grad():
        res = [model(img.to('cuda')).cpu().numpy() for img in train_dataloader]
    res = np.concatenate(res, 0)
    EMB_TEST[arch] = res

    print( arch, 'imge size:', sz, 'Hflip:', doflip, ',Done in:', int(time.time() - starttime), 's' )
    
    del model, res
    torch.cuda.empty_cache() # PyTorch thing to clean RAM
    gc.collect()

print(time.time() )    
len(EMB_TEST), EMB_TEST.keys()
```

## img.transpose(PIL.Image.FLIP_LEFT_RIGHT)

이미지를 가로방향으로 뒤집기



## arch.split()

![image-20220208140254326](../images/pages/image-20220208140254326.png)

```python
archname = arch.split('_hflip_')[0]
    if arch == 'tf_efficientnet_l2_ns_512':
        archname = 'tf_efficientnet_l2_ns'
```

archname에 모델이름만 들어가도록 처리



##     sz = int(arch.split('_')[-1])











































# 새로 알게된 함수

- round()

  반올림 함수입니다.

  import math를 하면 사용할 수 있으며, 숫자데이터.round()형식으로 사용합니다.

- pandas.astype('타입')

  컬럼의 데이터 타입을 변경



# 궁금한 점

- Load Train and Test

  ```python
  # If its Public LB run, then augment Testset to chack batch size memory consumption.
  if test.shape[0]<10:
      test = pd.concat([
          test, test, test, test, test, 
      ])
      test = test.reset_index(drop=True)
  ```

  기본 제공 데이터에서 test 데이터 수가 8개가 있습니다. 코드를 실행하면 test 데이터 수가 10개보다 작기 때문에 test 한세트를 5번 연장 시켜 총 40개의 test셋으로 늘려주었습니다. 하지만 이렇게 해봤자 같은 test데이터이기 때문에 각각 같은 결과가 나올 것 같은데 굳이 왜 이렇게 해주었는지 모르겠습니다.



- StratifiedKFold

  ```python
  train['bins'] = (train['Pawpularity']//5).round()
  
  train['fold0'] = -1
  skf = StratifiedKFold(n_splits = 20, shuffle=True, random_state = 1)
  for i, (_, test_index) in enumerate(skf.split(train.index, train['bins'])):
      train.iloc[test_index, -1] = i
  
  train['fold0'] = train['fold0'].astype('int')
  gc.collect()
  
  train.groupby(['fold0'])['Pawpularity'].agg(['mean','std','count'])
  ```

  enumerate부분 잘 몰겠다. 어떤 효과인지 ㅎ_ㅎ































