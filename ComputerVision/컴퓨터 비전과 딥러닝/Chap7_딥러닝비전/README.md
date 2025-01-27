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
    - - 1980년대에는 데이터셋이 작고, 컴퓨터가 느리고, 학습 알고리즘 미숙한 탓에 다층 퍼셉트론 학습이 제대로 되지 않음 => 은닉층 1,2개 쌓은 shallow neural network 사용
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
    - 모든 샘플을 담은 행렬 X를 설계 행렬(design matrix)라고 지칭
  - **바이어스 노드 추가 해야하는 이유?** 바이어스 없으면 직선이 x2=-x1와 같으므로 항상 원점을 지나므로 데이터를 제대로 분류할 수 없다
  - 퍼셉트론은 특징 공간을 두 부분 공간으로 나누는 분류기이다
  - ![image](https://github.com/user-attachments/assets/343d162d-4e8c-4ca6-aecf-0f76b136c1ef)

## 7.5 깊은 다층 퍼셉트론
- 퍼셉트론은 단순한 OR 데이터셋 문제뿐만 아니라 수백 차원 샘플이 수만개인 분류 문제도 풀 수 있다
- **한계점**: 선형분류기이기 때문에 선형 분리 가능(linearly separable) 데이터셋에서만 100% 정확률로 분류하고 선형 분리 불가능한 상황에서는 정확도 낮음
  - ![image](https://github.com/user-attachments/assets/2b197b45-d2b2-4e90-b017-a8266c357116)
  - XOR 문제조차 풀지 못함(최대 75%의 정확도)
- **해결안**: 은닉층을 추가한 다층 퍼셉트론 등장(루멜하트가 제시)

#### 다층 퍼셉트론: MLP(Multi-Layer Perceptron)
- **발상: XOR 문제**
  - 퍼셉트론을 2개 사용하면 특징 공간을 3개 영역으로 분할 가능
    - ![image](https://github.com/user-attachments/assets/43b4a37d-bfa4-48fd-8228-1d442c46449d)
  - 병렬 병합하면 특징 공간 x=(x1,x2)를 새로운 특징 공간인 z=(Z1,z2)로 변환 가능
    - ![image](https://github.com/user-attachments/assets/c62de5ab-acf5-4272-9d9e-84a8dfa67ca4)
    - ![image](https://github.com/user-attachments/assets/639fdb74-d0de-449a-bd06-ad1228c25f89)
    - ![image](https://github.com/user-attachments/assets/bd518c3f-b6ec-432b-b5ae-39bf3ddb2ad1)

- **다층 퍼셉트론의 일반화된 구조**
  - 입력층과 출력층 사이에 은닉층(hidden layer)가 추가됨
  - 은닉층이 형성하는 새로운 특징 공간을 은닉 공간(hidden space) 또는 latent space라고 한다
  - ![image](https://github.com/user-attachments/assets/c0da92df-b45e-4aec-b4c2-3ceb710bfb9e)
  - 이웃한 두 층에 있는 노드의 모든 쌍에 에지가 있으면 FC:Fully-Connected 구조라고 한다
  - 입력층과 은닉층을 연결하는 가중치를 U1, 은닉층과 출력층을 연결하는 가중치 행렬을 U2로 표현
    - U1_ji : 은닝층 j번째 노드 & 입력층의 i번째 노드 연결하는 가중치
    - U2_kj: 출력층 K번째 노드 & 은닉층 j번째 노드 연결하는 에지의 가중치
    - ![image](https://github.com/user-attachments/assets/a48325d1-dbe5-4eac-bf54-33c09fbde1c0)
  - 노드의 개수와 같이 사용자가 설정해줘야하는 매겨변수를 hyperparameter라고 함

- **전방 계산(forward computation)**
  - 특징 벡터 x가 입력층으로 들어와 은닉층, 출력층 거치면서 순차적으로 연산 수행하는 과정
  - ![image](https://github.com/user-attachments/assets/209b3fbd-abfd-45e4-bf6f-802fcfee6b0e)
  - ![image](https://github.com/user-attachments/assets/97eb788a-8a78-4ae9-92a6-36bea4aa63bb)
  - ![image](https://github.com/user-attachments/assets/384332be-57ca-4c51-8f22-93125919539b)
  - 정리:
    - ![image](https://github.com/user-attachments/assets/87d293ca-414e-47cf-b04d-7f207eafe0d9)
    - ![image](https://github.com/user-attachments/assets/fc6c22b9-6bab-45d2-803b-3a6cbb36c9f0)

- **신경망 출력을 부류 정보로 해석**
  - 전방계산을 통해 o를 구하고, 식을 이용하여 부류 정보를 알아내는 과정을 예측(prediction) 또는 추론(inference)라고 한다
  - ![image](https://github.com/user-attachments/assets/1eeb43b8-4203-41af-ba1a-2b13af4efc62)
  - 출력층은 보통 활성함수로 softmax를 사용한다
    - softmax: 모든 요소 더하면 1이 되는 특성이 있어 확률로 해석할 수 있는 것이 장점

- **활성 함수**
  - ![image](https://github.com/user-attachments/assets/906c2a42-42d0-4abb-b001-bb828ef37f88)
  - ![image](https://github.com/user-attachments/assets/93b62a97-2e24-4c2b-b00b-f5dd43579b05)
    - Sigmoid 중, logistic은 범위가 [0,1]이고 hyperbolic tangent는 범위가 [-1,1]
    - 딥러닝은 주로 ReLU(Rectified Linear Unit) 또는 ReLU 변종 사용
      - ReLu는 양수 구간이면 미분값이 무조건 1이기 떄문에 그레디언트 소멸을 크게 누그러뜨림
      - 그레디언트 소멸: 가중치 갱신하는 과정에서 미분값을 계속 곱하는데 활성함수의 미분값이 작은 경우, 급격하게 0에 가까워져서 가중치 갱신이 일어나지 않는 현상이 발생 => Vanishing Gradient
    - 출력층으로는 주로 softmax 활성함수 사용
      - 확률로 간주할 수 있어서 신경망의 최종 출력으로 적합
     
## 7.6 학습 알고리즘
- ![image](https://github.com/user-attachments/assets/b6afcd24-7370-43cc-837e-52cf6a4a730d)
- ![image](https://github.com/user-attachments/assets/a5ed3d3c-f9c7-4667-bf6e-b76f4cdc6525)

#### 경사하강법
- **경사하강법GD**: 미분값을 보고 손함수가 낮으지는 방향을 찾아 이동하는 일을 반복하여 최저점에 도달하는 알고리즘
- ![image](https://github.com/user-attachments/assets/4fefce30-230b-47bd-9606-99e0ab895b1c)
  - 최적해(여기서는 u=5)에 점점 가까워지도록 반복 수행
- **스토캐스틱 경사하강법**
  - 1.가중치가 여럿인 식으로 확장 2. 데이터셋 전체를 사용하지 않고 랜덤하게 선택된 하나의 샘플 또는 batch만을 사용
  - 알고리즘: ![image](https://github.com/user-attachments/assets/583eb312-e663-4dc5-bb56-8106ed3112f4)![image](https://github.com/user-attachments/assets/586f8c0f-b19b-4778-85d7-e91665ae6f76)
- ![image](https://github.com/user-attachments/assets/668f8d8d-0e89-473a-8e66-c7e7d3116710)

## 7.7 다층 퍼셉트론 구현하기
- 하이퍼파라미터 다루기: optimizer, learning_rate, beta_1, beta_@, epsilon, amgsfrad 등
- Adam & SGD 학습 비교: SGD < Adam
  - ![image](https://github.com/user-attachments/assets/a48cdb4b-31a9-42f6-980d-106c876bb34c)
- 과잉적합 확인
  - ![image](https://github.com/user-attachments/assets/da9d8c13-e478-48f9-8586-4b21d5d506f1)
  - 과잉적합: 훈련 집합에 과다하게 학습되어 일반화 능력을 상실하는 현상

-**Tensorflow 하이퍼파라미터 설정 체험**: Tensorflow PlayGround
  - https://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=4,2&seed=0.61598&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false
  - ![image](https://github.com/user-attachments/assets/1a1bb468-b62e-4321-bb60-998d903778a4)


## 7.8 (비전 에이전트) 우편번호 인식기기
- ![image](https://github.com/user-attachments/assets/67d4e8ab-baa3-4d3f-ae67-07060694c151)
- e는 reset, s는 show, r는 recognition하는 함수
