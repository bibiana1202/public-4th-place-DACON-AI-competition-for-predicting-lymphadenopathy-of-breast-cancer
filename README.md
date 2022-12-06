# [public 4등/0.8409] 유방암의 임파선 전이 예측 AI 경진대회 

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
<img width="600" alt="스크린샷 2022-12-06 오후 2 49 54" src="https://user-images.githubusercontent.com/105691874/205828484-18e6fb12-1c06-4da2-90dc-08deccede781.png">
  
---
## 🔬 프로젝트 수행 결과
* ### 데이터 분석
    * #### 대회 제공 데이터
      <img width="300" alt="스크린샷 2022-12-06 오후 2 55 27" src="https://user-images.githubusercontent.com/105691874/205830106-a9d6eb9b-e5bd-4810-af7f-8f813c04dbb3.png">
        
        * Train dataset 1000, Test dataset 250
    
    * #### Image Data 분석
      <img width="300" alt="스크린샷 2022-12-06 오후 3 00 22" src="https://user-images.githubusercontent.com/105691874/205831855-e1ea26f8-aff9-4fe8-8b3f-7b543f1c8602.png">
        
        * 병리 슬라이드 이미지 파일 (png)
        * 1500 ~ 7300px 크기의 고해상도 이미지
        * 각 데이터마다 종횡비가 상이
        * 노이즈 데이터 존재

    * #### Tabular Data 분석
        <img width="300" alt="스크린샷 2022-12-06 오후 3 02 30" src="https://user-images.githubusercontent.com/105691874/205832630-6847b290-9f71-47bf-b984-fbfcb153aaa4.png">

        * 환자 정보 테이블 파일(csv)
        * 나이, 진단명, 암의 개수 등 학습에 필요한 23가지의 항목 존재
        * 많은 양의 결측치 발견

    
* ### 데이터 전처리
    
    * #### Image Data 필터링
      <img width="1035" alt="스크린샷 2022-12-06 오후 3 03 20" src="https://user-images.githubusercontent.com/105691874/205832931-fdb597db-0f9a-4563-9cea-33d4887eb14c.png">

    * #### Image Data 증강
        * 원본 이미지 갯수 1000장

M/D	| flip | rotate | zoomin | equalization | CLAHE | centercrop |증강 이미지 갯수| score |
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
        
        
        
* ### 모델 선정
* ### 모델 평가 및 개선

    
    
    
    
    
    
M/D	| 512 A | 512 B | 512 C | 1024A | 1024B | 1024C |
--------------|-------|-------|-------|-------|-------|-------|
Faster R-CNN | 0.013/0.016 | 0.135/0.124 | --- | 0.131/0.110 | 0.137/0.127 | 0.123/0.154 |
Yolov5 | 0.179/0.149 | 0.114/0.095  | 0.119/0.090 | 진행중 | 진행중 | 진행중 |
RetinaNet | 0.041/0.043 | --- | --- | 0.135/0.114 | 0.126/0.132 | 0.142/0.121 |
Yolof | 0.039/0.041 | --- | --- | 0.142/0.106 | 0.135/0.122 | 0.126/0.108 |
Yolox | 0.145/0.112 | 0.108/0.077 | --- | 0.141/0.118 | 0.127/0.137 | 0.179/0.156 |
CenterNet | 0.038/0.039 | --- | --- | 0.069/0.065 | --- | 0.083/0.077 |

* ### 앙상블 성능 비교(kaggle score)
  - 2-pred score
    
n | WBF | Faster R-CNN | Yolov5 | RetinaNet | Yolof | Yolox | SCORE |
--------------|-------|-------|-------|-------|-------|-------|-------|
#1 | Faster,Retina | 1024A(0.131/0.110) | - | 1024A(0.135/0.114) | - | - | 0.179/0.151 |
#2 | Faster,yolov5 | 1024A(0.131/0.110) | 512A KFLOD(0.179/0.149) | - | - | - | 0.210/0.196 |
#3 | Retina,yolov5 | - | 512A KFLOD(0.179/0.149) | 1024A(0.135/0.114) | - | - | 0.212/0.204 |
#4 | Faster,yolov5,Retina | 1024A(0.131/0.110) | 512A KFLOD (0.179/0.149) | 1024A (0.135/0.114) | - | - | 0.216/0.209 |
#5 | yolov5,yolovf | - | 512A KFLOD (0.179/0.149) | - | 1024A (0.142/0.106) | - | 0.218/0.205 |
#6 | Faster,yolov5,Retina,yolovf| 1024A (0.131/0.110) | 512A KFLOD (0.179/0.149) | 1024A (0.135/0.114) | 1024A  (0.142/0.106) | - | 0.229/0.206 |
#7 | Faster,Retina,yolof,yolox| 1024A (0.131/0.110) | - | 1024A (0.135/0.114),1024C(0.142/0.121) | 1024A (0.142/0.106),1024B(0.135/0.122) | 1024C(0.179/0.156) | 0.215/0.165 |
#8 | Faster,Retina,yolof,yolox| 1024A (0.131/0.110) | - | 1024A (0.135/0.114),1024C(0.142/0.121) | 1024A (0.142/0.106),1024B (0.135/0.122) | 1024A(0.141/0.118),1024C(0.179/0.156) | 0.222/0.170 |    

    
* ### 예측 이미지

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

