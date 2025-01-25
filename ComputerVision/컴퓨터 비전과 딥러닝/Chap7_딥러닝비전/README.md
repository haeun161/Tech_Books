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
