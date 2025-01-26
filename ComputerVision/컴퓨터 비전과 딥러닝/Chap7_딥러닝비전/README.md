# Chap7 딥러닝 비전
- 여러 성공적인 알고리즘에도 불구하고 여전히 해결하지 못하는 어려운 문제 아주 많다
- 대표적으로 1. 의미 분할(Semantic Segmentation) 2. 영상 설명(image captioning)하는 문제
  - 의미 분할(Semantic Segmentation): 물체를 구성하는 화소를 모아 영역으로 분할하고 물체 부류 알아내야 함
  - 영상 설명(image captioning): 영상의 핵심 내용을 파악 후, 10개 가량의 단어로 이루어진 설명 문장 생성
  - ![image](https://github.com/user-attachments/assets/37fa5e7e-c38c-47c6-8cee-789e09a80a89)


## 7.1 방법론의 대전환
- rule-based에서 data-centric 딥러닝으로 발전
- 컴퓨터 비전 방법론은 크게 1. Rule-based 2.machine learning으로 나뉨
- ![image](https://github.com/user-attachments/assets/4ec9333e-6b0d-4feb-a7c8-273e2d3b71f2)

#### 기존 rule-based의 한계
- 사람들이 만든 이런 특징과 매칭 알고리즘을 수작업(handcrafted) 특징과 수작억 알고리즘이라 지칭
  - 사람의 합리적인 사고를 통해 데이터를 관찰하고 정교한 알고리즘을 개발하여 개선됨
  - 수작업 개발의 결과는 대부분 rule로 표현 ex) 지역특징이 가능성 나타내는 식, SIFT 스케일 공간

    
## 7.2 기계학습 기초
- y =f(x)
- 기계학습의 4단계: 1.데이터 수집 2. 모델 선택 3. 학습 4. 예측
- 데이터 수집
  - 모델의 입력을 feature vector, 출력을 GroundTruth 또는 label이라고 지칭
- 학습: 수집한 데이터로 방정식을 풀어 함수를 알아내는 일
  - (최소 오류로 맞히는 최적의 가중치 값을 알아내는 작업)
  - 학습 시, 가중치 w값이 얼마나 좋은지를 측정하기 위해 loss function 사용. ex) MSE
- 예측: 학습된 모델로 특정 x에 대해 y를 계산하는 일
  - 추론inference라고도 부름. 
  - W를 가진 모델에 학습에 사용되지 않던 새로운 특징 벡터를 입락하고 출력을 구하는 과정.
  - 이를 통해 모델의 성능 평가
  - k-fold cross-validation을 통해 데이터셋 k개 부분 집합으로 분할하고 성능 실험을 K번하여 평균을 취하는 방식
    - ![image](https://github.com/user-attachments/assets/36de7828-408c-4935-9a68-14a9b2bfe0f1)

- 컴퓨터 비전 문제 종류:  Classification(분류), 검출, 분할, 추적
  - ![image](https://github.com/user-attachments/assets/276cb354-6043-4c2e-8d98-b8c782835d02)
    
## 7.3 딥러닝 소프트웨어 맛보기
- **sklearn**: 파이썬이 제공하는 기계학습 라이브러리
  - 비신경망 모델, 얕은 신경망까지만 지원
- 딥러닝을 구현하는데 가장 널리 쓰이는 도구는 **Tensorflow**, **Pytorch**
- **Tensorflow**
  - 2015년 Google에서 공개
- **Pytorch**
  - 2015년 Facebook에서 공개

## 7.4 인공 신경망의 태동
### 역사
  - 1946년: 초당 3000회 가량 덧샘 가능한 세계 최초의 전자식 컴퓨터 ENIAC 탄생
  - 1943년: 맥컬록과 피츠가 뉴런의 정보 처리를 모방한 수학 모델 발표 (최초 인공 신경망)
  - 1949년: 헤브가 학습 알고리즘 발표
  - 1958년: 로젠블랫이 퍼셉트론 최초로 구현
    - 이후, 위드로와 호프는 퍼셉트론을 개선한 아달리과 마달린 발표
  - 1960년: 신경망이 인공지능을 완성해줄 것처럼 과정되어 매스컴의 주목 받음
  - 1969년: 민스키와 페퍼트는 Perceptrons라는 저서를 통해 퍼셉트론이 선형분류기에 불과하고 XOR 문제 못푼다고 밝힘
  - 신경망 연구 크게 퇴조
  - 1986년: 루멜하트와 동료들이 퍼셉트론에 은닉층 추가한 다층 퍼셉트론과 퍼셉트론 학습할 수 있는 역전파 알고리즘 저술
  - 1990년대: SVM이라는 비신경망 모델 등장하며 다층 퍼셉트론 능가하는 형국
  - 2000년대: 은닉층 개수를 대폭 늘린 딥러닝 등장하며 다시 기계학습의 주류 기술이 됨

### 퍼셉트론
- ![image](https://github.com/user-attachments/assets/27297346-9db0-48ce-87e2-e06468d2c427)
  - 입력층 - d차원의 특징 벡터를 받기 위한 d개의 노드와 1개의 바이어스 노드(x0=1)
  - 출력층
  - 입력 노드와 출력 노드 연결하는 에지 = 가중치
  - tao = 활성 함수(o=0은 decision boundary)
  - 활성 함수 적용 전의 s 값 = logit로짓
-  OR 문제 풀기 적합
  - ![image](https://github.com/user-attachments/assets/33b3adbe-0a85-4757-905a-04bb0187c152)
  - ![image](https://github.com/user-attachments/assets/2e939538-766c-4b2d-bd4f-cfb5425108e7)
    - 모든 샘플을 담은 행렬 X를 설계 행렬(design matrix)라고 지
  - **바이어스 노드 추가 해야하는 이유?** 바이어스 없으면 직선이 x2=-x1와 같으므로 항상 원점을 지나므로 데이터를 제대로 분류할 수 없다
  - 퍼셉트론은 특징 공간을 두 부분 공간으로 나누는 분류기이다
  - ![image](https://github.com/user-attachments/assets/343d162d-4e8c-4ca6-aecf-0f76b136c1ef)

## 7.5 깊은 다층 퍼셉트론
- 퍼셉트론은 단순한 OR 데이터셋 문제뿐만 아니라 수백 차원 샘플이 수만개인 분류 문제도 풀 수 있다
- **한계점**: 선형분류기이기 때문에 선형 분리 가능(linearly separable) 데이터셋에서만 100% 정확률로 분류하고 선형 분리 불강능한 상황에서는 정확도 낮음
  - ![image](https://github.com/user-attachments/assets/2b197b45-d2b2-4e90-b017-a8266c357116)
  - XOR 문제조차 풀지 못함(최대 75%의 정확도)
- **해결안**: 은닉층을 추가한 다층 퍼셉트론 등장(루멜하트가 제시)

#### 다층 퍼셉트론
- **발상: XOR 문제**
  - ![image](https://github.com/user-attachments/assets/43b4a37d-bfa4-48fd-8228-1d442c46449d)
