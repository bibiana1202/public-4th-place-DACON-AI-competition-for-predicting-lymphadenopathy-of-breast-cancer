# [public 4등/0.8409] 유방암의 임파선 전이 예측 AI 경진대회 

<img width="763" alt="스크린샷 2022-12-06 오후 5 00 39" src="https://user-images.githubusercontent.com/105691874/205854231-db02a12e-e96a-4bd5-baf2-983945ebed31.png">


- 유방암의 임파선 전이 예측 AI 경진대회  : <a href="https://www.notion.so/wew1202/AI-6368467724394e8a956701057aa0e37a">![Notion](https://img.shields.io/badge/Notion-%23000000.svg?style=for-the-badge&logo=notion&logoColor=white)

- 데이콘 - 유방암의 임파선 전이 예측 AI 경진대회 : <a href="https://dacon.io/competitions/official/236011/leaderboard">![dacon](https://img.shields.io/badge/-dacon-blue)  
    
---
## 🔬 프로젝트 개요
* ### 대회 소개
  - 데이콘 경진대회 및 프로젝트 설명
  - 주제 : 유방암 병리 슬라이드 영상과 임상항목을 통한 유방암의 임파선 전이 여부 예측
  - 프로젝트 설명 : 유방암 병리 슬라이드 영상(image)와 임상 항목(Tabular)를 조합하여 유방암의 임파선 전이 여부 이진 분류
  - 유의사항 : 유방암 병리 슬라이드 영상을 모델 학습과 추론에 필수로 사용해야 하며, 임상 항목 데이터만 활용하는 경우 수상 제외

* ### 개발 환경

  - Google Colab, AWS, VSCode,Phycharm,Anaconda
  
---
## 🔬 프로젝트 수행 절차 및 방법
* ### 개발 과정
    * 데이터 전처리
        * Tabular 데이터 : 결측치 제거
        * Image 데이터 : 이미지 노이즈 제거, 이미지 사이즈 조절, 
    * 모델 선정 및 분석
        * Multi-modal
        * Tabular model
        * Image model
    * 예측치 출력 방식
        * Ensemble model : hard voting, soft voting
---
## 🔬 프로젝트 수행 결과
* ### 데이터 분석
    * #### 대회 제공 데이터
      <img width="300" alt="스크린샷 2022-12-06 오후 2 55 27" src="https://user-images.githubusercontent.com/105691874/205830106-a9d6eb9b-e5bd-4810-af7f-8f813c04dbb3.png">
        
        * Train dataset 1000, Test dataset 250
    
    * #### Image Data 분석
      <img width="500" alt="스크린샷 2022-12-06 오후 3 00 22" src="https://user-images.githubusercontent.com/105691874/205831855-e1ea26f8-aff9-4fe8-8b3f-7b543f1c8602.png">
        
        * 병리 슬라이드 이미지 파일 (png)
        * 1500 ~ 7300px 크기의 고해상도 이미지
        * 각 데이터마다 종횡비가 상이
        * 노이즈 데이터 존재

    * #### Tabular Data 분석
        <img width="700" alt="스크린샷 2022-12-06 오후 3 02 30" src="https://user-images.githubusercontent.com/105691874/205832630-6847b290-9f71-47bf-b984-fbfcb153aaa4.png">

        * 환자 정보 테이블 파일(csv)
        * 나이, 진단명, 암의 개수 등 학습에 필요한 23가지의 항목 존재
        * 많은 양의 결측치 발견

    
* ### 데이터 전처리
    
    * #### Image Data 필터링
      <img width="500" alt="스크린샷 2022-12-06 오후 3 03 20" src="https://user-images.githubusercontent.com/105691874/205832931-fdb597db-0f9a-4563-9cea-33d4887eb14c.png">

    * #### Image Data 증강 : <a href="https://docs.google.com/spreadsheets/d/1TYMVaDAlyu5X-v9ZCodokFLaaDzuRrNjLoaMPMOnTSE/edit#gid=0">![EXCEL](https://img.shields.io/badge/-EXCEL-green)  

        * 원본 이미지 갯수 1000장
            M/D	| flip | rotate | zoomin | equalization | CLAHE | centercrop |증강 갯수| score |
            --------------|-------|-------|-------|-------|-------|-------|-------|-------|
            category A | --- | --- | --- | --- | --- | --- | 0 | 0.7067 |
            category B-1 | Horizontalflip | shiftscalerotate | --- | --- | --- | --- | 1000 | 0.6829459108 |
            categroy C-1 | Horizontalflip | shiftscalerotate | --- | eaualization | --- | --- | 1000 | 0.6933083453 |
            categroy D-1 | Horizontalflip | shiftscalerotate | zoomin(10%) | eaualization | CLAHE | centercrop | 1000 | 0.7490981241 |
            categroy D-2 | Horizontalflip | shiftscalerotate | zoomin(10%) | eaualization | CLAHE | centercrop | 2000 | 0.7681871552 |
            categroy D-3 | Horizontalflip | shiftscalerotate | zoomin(10%) | eaualization | CLAHE | centercrop | 3000 | 0.6846697012 |
            categroy E-2 | Horizontalflip | shiftscalerotate | zoomin(20%) | --- | CLAHE | gamma | 2000 | 0.7535557 |
            Padding_512_resize | --- | --- | --- | --- | --- |  | 0 | 0.6738 |
            Gamma | --- | --- | --- | --- | --- | --- | 1000 | 0.6456 |
            Zoom | --- | --- | --- | --- | --- | --- | 1000 | 0.6555 |

    * #### Tabular Data
        * Tabular 항목 : 범주형 항목 17개 , 수치형 항목 6개
        * 결측치 존재 : 범주형 결측치 항목 13개 , 수치형 결측치 항목 5개
            <img width="800" alt="스크린샷 2022-12-06 오후 4 01 53" src="https://user-images.githubusercontent.com/105691874/205843373-3b2316c1-5153-41f8-9748-5766fac2f02e.png">
        * utlier(이상치) 존재  : PR_Allred_score(범주형)  : 0~8 범주를 벗어나는 데이터 존재

    
    
* ### 모델 선정
    * #### 선별 데이터
        * Image Data    
            * MIL : 원본 해상도 유지
                <img width="615" alt="스크린샷 2022-12-06 오후 4 07 32" src="https://user-images.githubusercontent.com/105691874/205844366-5795ba6b-6643-48aa-bcc2-948aadeea8f5.png">
                * 패치 개수 조절 (200개) => 성능 저조
                * 모든 패치 사용 => 자원부족 , 학습불가
            * Resize : 1024,512,256,224 (동일모델)
                * 1024 Resnext50 F1-score 0.7575
                * 512 Resnext50 F1-score 0.7716
                * 256 Resnext50 F1-score 0.6812
                * 224 Coatnet_1_rw_224 F1-score 0.7215
        * Tabular Data
            * 결측치 채우는 방식
            * 종속변수와 독립변수 간의 상관관계에 따른 Feature Selection
    
    * #### Single Tabular Model
        <img width="500" alt="스크린샷 2022-12-06 오후 4 10 17" src="https://user-images.githubusercontent.com/105691874/205844845-da44484e-f8ee-413c-ace1-27300781c708.png">
        
        * Model Selection
            * AutoML
            * Bayesian optimization
        * Feature selection(Correlation)
            * 카이제곱 상관성(범주형)
            * 피어슨 상관계수(수치형)
        * Tabular Classifier 
            * Gradient Boosting
            * Cat Boosting Classifier


    * #### Single Image Model
        <img width="500" alt="스크린샷 2022-12-06 오후 4 15 53" src="https://user-images.githubusercontent.com/105691874/205845837-85e0bb71-de8d-4489-8bb1-78fd548e83f2.png">

       * 사용 라이브러리
            * MMclassfication
            * Timm
    
       * Image Classifier
            * ResNext50_32x4d
            * Res2Net-50
            * Mobilenet_v2
            * Densenet-169
            * CoatNet-1
            * EfficientNet-b0


    * #### Multi-Modal Model
        <img width="1000" alt="스크린샷 2022-12-06 오후 4 23 23" src="https://user-images.githubusercontent.com/105691874/205847184-6dc939da-8c07-4ede-a3be-b522d04ef72b.png">

        * Tabular Feature Extractor
            * Sequential model(MLP)
            * TabNet Encoder
        * Image Feature Extractor
            * ResNext
            * EfficientNet
            * SqueezeNet
            * VGG
            * AlexNet
            * CoatNet
            * ResNet
            * DenseNet
    
    
    
* ### 모델 평가 및 개선 : <a href="https://docs.google.com/spreadsheets/d/127NGPFhW_sOfHffROYjIONv1kPIGLyZN74Zu64_nzDk/edit#gid=2061805860">![EXCEL](https://img.shields.io/badge/-EXCEL-green)  
    
    * #### Single Tabular Model
    
        Model|F1- Score
        -------|-------|
        XGBoost|0.8000000000000002|
        CatBoost|0.8113207547169812|
        AdaBoost|0.8000000000000002|
        Ridge|0.7735849056603773|

    * #### Single Image Model : <a href="https://docs.google.com/spreadsheets/d/1fLho9xu-qGD4FQPmGHMIS650to86aCSxDNjJkuFz044/edit#gid=0">![EXCEL](https://img.shields.io/badge/-EXCEL-green)  
    
    
        Model|F1- Score
        -------|-------|
        resnext50|0.7067|
        cspresnext50|0.7612396694|
        ghostnet_100|0.75613487|
        mobilenetv3_rw|0.748808652|
        ssl_resnext50_32x4d|0.748734329|
        rexnet_150|0.7414935362|
        tinynet_b|0.7414418|
        ecaresnext50t_32x4d|0.7109136911|
        edgenext_base|0.70936788|
        efficientnetv2_rw_m|0.697245416|

    * #### Multi-Modal Model : <a href="https://docs.google.com/spreadsheets/d/1O9QuRCk46nilIeahO8paoYhJG1HvMMePanEKc9aAru0/edit#gid=0">![EXCEL](https://img.shields.io/badge/-EXCEL-green)  
    
    
        Model|F1- Score
        -------|-------|
        resnext50|0.7994|
        densenet169|0.7768|
        resnet18_40|0.7546|
        vgg16|0.7502|
    
    * #### Ensemble : <a href="https://docs.google.com/spreadsheets/d/1tWmCUIzbLK0ckttNemSRQAWZSfiE-5oJLLXwG8iRwac/edit#gid=0">![EXCEL](https://img.shields.io/badge/-EXCEL-green)  

        * high score 4 (0.8409710181)
            * [1] high score 2(0.83)+ MMC_resnext + (M_0.7535557)resnext50_E2_20e_binary + AdaBoost + Ridgeclassfier
            * [2] #20 + CatBoost + [1]
    
        * high score 2 (0.8326277762)
            * [1] Ensemble(AdaBoost + Catboost + XGBoost + MMC_resnext  + 앙20#)
            * [2] Ensemble(tabular_GradientBoost + AdaBoost + [1])
            * [3] Ensemble (앙17# + MMC_resnext + [2])
            
        * 앙상블 17번 (0.8157543391) = 앙5# +앙6# + 앙13# + 앙14# + 앙15# + 앙16#
            * 앙상블 5번 (0.7977893511) = pred#9(0.7994386703) + tabular_3_0.816793893129771(0.8167) + pred#15(0.7067)
            * 앙상블 6번 (0.7986071899) = pred#7(0.7865034694) + pred#9(0.7994386703) + pred#12(0.77071428)
            * 앙상블 13번 (0.7902708482) = pred#7(0.7865034694) + pred#9(0.7994386703) + submission_catboost_train(0.8128)
            * 앙상블 14번 (0.8067434067) = pred#39(0.7472) + pred#12(0.77071428) + pred#9(0.7994386703) + 	앙5#	0.790270842 + tabular_XGBoost_submission(0.8205) + pred#15(0.7067)
            * 앙상블 15번 (0.8108412585) = pred#5(0.7716) + 앙14#(0.8067434067) +pred#15(0.7067)
            * 앙상블 16번 (0.77) = vgg(0.75027) + pred#4(0.7575) + efficientnet_b230(0.736)	+ pred#5(0.7716) + resnet18_40(0.754)
          
        * 앙상블 20번(0.8157543391) = pred#39(0.7472) + pred#12(0.77071428)	+ 앙5#(0.7977893511) + pred#9	(0.7994386703) + 앙6# (0.7986071899)
            
---
## 🔬 자체 의견 평가
 - Medical trend인 object detection 을 공부하기 위하여 선택한 프로젝트
 - 학습 data안에서 train & valid로 나누지 않고 Group K-Fold를 사용했던 이유?
    - 적은 data set에 대하여 정확도를 향상시킬 수 있다.
    - ∵ training/validation/test 세개의 집단으로 분류하는 것보다 training & test로만 분류시 학습할 data가 더 많게되어 underfitting등 성능이 미달되는 model로 학습되지 않도록 함. 또한 1개의 이미지에 다중 label이므로 예측의 정확도를 확실히 평가하기위해 train set & valid에 포함된 image가 겹치지 않도록 하기위하여 k-fold중에서도 group k-fold를 사용하였다. 하지만 학습 시간이 꽤 오래걸려 시간상 k = 1 로 세팅해 놓았음. (즉, kfold를 하지 않고 train:valid = 8:2로 데이터셋을 별도로 나눠 학습한 것과 같음.) 
    - 10epoch으로 학습시 k=1로만 본것과 k=5로 하여 성능을 비교한 결과 public score가 0.014에서 0.025로 향상됨을 확인할 수 있었다. 그러므로 데이터를 augmenation한 B와 C도 제대로 k = 5로 세팅해서 학습했다면 더 좋은 성능을 보였을 듯 하다.

k|EPOCH|Score|EPOCH|Score
--|-------|-------|-------|-------|
1|10e|0.014|20e|0.016
5|10e|0.025|20e|0.024
    
-  ### Zoom in augmentation시 10%로 한 이유?
    - 10%보다 더 zoom in을 했을경우 이미지의 가장자리에 위치하던 병변들이 잘리는 경우들이 있어서 이를 막기위해 10%정도만 zoom in을 하였다. normalization 후 다시 size(512x512) 재정의시 정수화함에따라 같은 값을 갖게되는 경우가 있었다.

    - 병변이 너무도 작아 bbox의 y_max와 y_min이 별 차이가 나지 않았기 때문. 이런 데이터로인해 학습시 오류가 발생하여 해당 데이터(10개 미만)는 삭제하기로 함.

* ### Model selection

    - 1 stage model
        - YOLOX: 1 stage에서 유명하고 속도가 빠르기때문에 사용함.
        - EfficientDet (one-stage detector paradigm 기반으로 구성됨): 사람들이 주로 사용하는 YOLO v5보다 average precision이 좋기때문에 선택.
    - 2 stage model
        -  Faster R-CNN: 이전 수업에서 사용했던 model이 1 stage라서 2 stage 공부 겸 여전히 현역으로 쓰이고 있는 기초적인 모델이라서 선택하였음.

* ### Augumentation
    - 학습하는 data 양이 15,000장인줄 알았지만 data를 분석한 결과 정상인을 제외한 환자의 data는 4,394장이었다.
    - 질병을 학습해야하는 model이기때문에 환자의 data만 갖고 학습을 시켜야하는데 data의 양이 너무 적어 양을 늘리기 위하여 여러 augmentation을 적용해보았다.
    - Augmentation에 따른 성능 평가를 비교해 보기위해 augmentation을 안한 A그룹과 기본적인 augmentation을 한 B그룹, 마지막으로 기본적인 augmentation외 여러 다양한 기법까지 적용한 C그룹으로 나누었다.
그 결과 3개의 model 모두 augmentation을 하면 할수록 성능이 향상됨을 확인하였다.
    - Data내에서 모든 label이 비슷한 양으로 존재하지않고 특정 label위주로 존재하고있다. 즉, data imbalance가 심한상황.
    - Data imbalance 문제를 해결하기 위해 너무 많은 양을 갖고있는 특정 label(0,3,11,13)은 down sampling하고 적은 양을 갖는 label(1,12)엔 여러가지 augmentation으로 up sampling하는 작업을 하였다.
    - 1,12에는 Rotation, Flip, Zoomin, Cutmix, CLAHE, Equalization 을 적용하고
    - 0,3,11,13은 약 3,000개로 down sampling하여 label간의 극단적인 차이가 어느정도 해소된 D그룹을 만들었다. 가장 성능이 좋았고 학습속도가 빠른 YOLOX로 D그룹을 학습한 결과 A그룹에 비해 성능이 향상됨 을 확인할 수 있었고 양이 적은데도 불구하고 기본 3가지 augmentation을 한 B그룹보다 성능이 좋았다.
    - 하지만 기본 augmentation외에 추가적인 augmentation을 했던 C그룹보다는 성능이 덜 나왔다. (이는 data의 양이 6배나 차이가 나기때문에 나온 결과)
    - C그룹에서 훨씬 성능이 좋았던것을 통해 data imbalance를 해결한것보다는 data의 양이 충분히 있는것이 성능향상에 더 많은 효과가 있음을 유추할 수 있었고
    - imbalance와 data의 양을 동시에 해결한다면 이보다 훨씬 더 좋은 성능을 낼 수 있지 않을까 싶다.

---
## Ref
https://www.kaggle.com/code/dschettler8845/visual-in-depth-eda-vinbigdata-competition-data
https://www.kaggle.com/code/yerramvarun/pytorch-fasterrcnn-with-group-kfold-14-class
https://www.kaggle.com/code/pestipeti/vinbigdata-fasterrcnn-pytorch-inference/notebook
https://www.kaggle.com/code/pestipeti/vinbigdata-fasterrcnn-pytorch-train/notebook

