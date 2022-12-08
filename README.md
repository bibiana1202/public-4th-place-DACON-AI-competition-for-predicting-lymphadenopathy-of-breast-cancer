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
            * 앙상블 14번 (0.8067434067) = pred#39(0.7472) + pred#12(0.77071428) + pred#9(0.7994386703) + 	앙5#(0.790270842) + tabular_XGBoost_submission(0.8205) + pred#15(0.7067)
            * 앙상블 15번 (0.8108412585) = pred#5(0.7716) + 앙14#(0.8067434067) +pred#15(0.7067)
            * 앙상블 16번 (0.77) = vgg(0.75027) + pred#4(0.7575) + efficientnet_b230(0.736)	+ pred#5(0.7716) + resnet18_40(0.754)
          
        * 앙상블 20번(0.8157543391) = pred#39(0.7472) + pred#12(0.77071428)	+ 앙5#(0.7977893511) + pred#9(0.7994386703) + 앙6#(0.7986071899)
            
---
## 🔬 자체 의견 평가
 
    
    
    
    
    

---
## Ref
https://www.kaggle.com/code/dschettler8845/visual-in-depth-eda-vinbigdata-competition-data
https://www.kaggle.com/code/yerramvarun/pytorch-fasterrcnn-with-group-kfold-14-class
https://www.kaggle.com/code/pestipeti/vinbigdata-fasterrcnn-pytorch-inference/notebook
https://www.kaggle.com/code/pestipeti/vinbigdata-fasterrcnn-pytorch-train/notebook

