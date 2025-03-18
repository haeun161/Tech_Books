# Chap13: 생성 비전
- 미약하게 GMM이나 HMM, 볼츠만 머신 등을 활용한 생성 모델이 연구되어 왔으나, GAN 이후 획기적인 진전이 이루어짐

## 13.1 생성 모델 기초

### 생성 모델이란?
- x가 발생할 확률 분포 p(x)를 알면, 새로운 샘플을 생성할 수 있다.
- 특징 벡터가 2차원윈 경우 생성 모델
  - ![image](https://github.com/user-attachments/assets/c8948474-3860-4dc2-8974-f5668334d765)
  - ![image](https://github.com/user-attachments/assets/9059167c-8473-40c5-891b-25bf8af442ad)
  - 다음과 같은 확률 분포를 막대 그래프로 표현한다. p(x)를 샘플링하는 방식은 네가지 경우에 구간을 배정하고 난수를 생성하여, 난수가 속한 값을 취하는 것
- 위의 예시는 단순하지만, 확률분포 p(x)를 추정하면 p(x)가 생성 모델로 작용한다는 원리를 설명한다.
- 데이터 크기가 클수록 Pmodel은 Pdata에 가까워 진다.
-  특징 개수가 적고 특징이 몇개의 이산값만 가져 쉽게 확률 분포를 계산하여 배열로 표현할 수 있는 경우, tractable이라 말한다.
-  하지만, 실제 데이터 차원은 매우 크며, 특징은 아주 많은 값이 가능하다
  - MNIST만 해도 백터 x차원은 784차원이며, 특징이 가질 수 있는 값이 [0,255] 사이의 정수로, 이를 확률 분포로 표현한 경우 256^784개의 요소가 필요하여 intractable하다
- 생성 모델 연구 : Pmodel이 Pdata에 가깝도록 모델링하는 기법을 찾는 일

### 가우시안 혼합 모델
- **데이터셋이 가우시안 분포로 가정하고 확률 분포 추정의 예시**
  - ![image](https://github.com/user-attachments/assets/bd5ab5e2-e7c8-48f1-bdf2-37a62710d36e)
    - 공분산 행렬은 두 변수사이의 상관 관계를 표

- **MNIST를 가우시안 모델링하고 샘플 생성하기**
  - ![image](https://github.com/user-attachments/assets/0ef6169c-87ee-4f1d-a86c-a24e4141ef1a)
    - 위의 예시는 부류를 0으로 제한했지만, 제대로 된 생성 모델은 다양한 부류 다룰 수있어야 함
  - 사람 얼굴 생성하는 경우, 성별, 인종, 머리모양, 수염, 안경, 장신구 등 변화 수용할 수 있어야 함.
  - 여러개 부류 모델링하려면 하나의 가우시안으로는 불가능
    - 같은 부류라도 획읜 두께나 기울기 등 변하기 떄문에 하나의 가우시안을 한계 존재
      
- **가우시안 혼합 모델(GMM)**
  - K개 부류를 찾아 각각을 가우시안으로 표현하는 가우시안 혼합 모델 존재
  - k는 사람이 직접 설정해야하는 하이퍼 매개변수
  - k=3인 경우 2차원 예시
    - ![image](https://github.com/user-attachments/assets/9afe9cea-0b32-46c9-bdb0-cdd076928cc7)

  - **mnist를 GMM으로 모델링하고 샘플 생성하기**
    - ![image](https://github.com/user-attachments/assets/29e286a8-5e30-4cc2-aa8d-5bf877b0bae2)
    - 가우시안 하나로 모델링하는 것보다 나은 품질을 보이지만, 가짜와 진짜를 쉽게 구별 가능..
  - **한계점**: 가우시안과 달리 평균을 중심으로 대칭 모양을 형성하지 않는 데이터들이 다수, 가우시안 혼합 모델의 가정은 실제 데이터를 너무 단순화한다
 
### 최대 우도
- 생성형 모델 문제 정의: 최대 우도라는 최적화 문제로 정의 가능
  - 데이터 X를 발생할 가능성이 가장 높은 매개변수 theta를 추정하는 방식으로 동작
  - ![image](https://github.com/user-attachments/assets/5ca2cb09-b0e9-4d81-80fe-8106ccb447db)
  - P(X)를 likelihood라고 부르고, 이 식을 푸는 학습 알고리즘을 최대 우도법이라 부른다 = 최대 우도법은 가장 좋은 매개변수 theta를 찾는다
  - ![image](https://github.com/user-attachments/assets/b8e59efe-3772-42ef-ba5d-d574ee95fbdc)
    - 보라색이 실제 데이터, theta1이 theta 2보다 데이터셋 X를 잘 표현한다
  

### 고전 생성 모델
- 은닉 마르코프 모델과 제한 볼츠만 머신

## 13.2 오토인코더를 이용한 생성모델
- 오토인코더: 입력 영상을 그대로 출력으로 내놓는 컨볼루션 신경망
- 인코드는 특징 맵을 점점 작게 하여 latent space으로 축소하고, 디코더는 latent space를 키워 원래 영상을 복원한다
- latent space의 특징 맵은 영상을 복원할 수 있을 정도로 핵심 정보를 거의 다 가지고 있음
- ![image](https://github.com/user-attachments/assets/8980cb0f-8087-4a06-95c2-13990f967845)
- ![image](https://github.com/user-attachments/assets/c9c4021a-025e-46d2-af53-27eadd183f0d)

- **MNIST를 오토인코도로 모델링하고 샘플 생성하기**
  - latent space의 한 점을 랜덤 발생시킨 다음, 디코드를 통과시켜 새로운 샘플 영상 생성
  - 인코더
    - ![image](https://github.com/user-attachments/assets/d0a73d1b-0125-4713-90dc-dc729ba068f1)
  - 디코더
    - ![image](https://github.com/user-attachments/assets/21088642-02a6-4761-b6ee-f4eda6535fe0)
  - ![image](https://github.com/user-attachments/assets/bc3c4425-11bd-41eb-bf75-103fda556ebc)
  - ![image](https://github.com/user-attachments/assets/2491a6db-2fdb-4102-a533-bf011930b65d)
  - ![image](https://github.com/user-attachments/assets/2df96570-88ac-43cb-a994-11436bf4dc92)

### 변이 추론
- Dalle나 Imagen같은 뛰어난 언어-비전 생성 모델은 **변이 오토인코더 또는 확산 모델**을 이용
- 변이 오토인코더와 확산 모델은 복잡한 확률 분포를 추정하는데, 이 과정을 위해 variational inference 활용
- VAE: latent space z를 도입
  - latent space에서 정의된 확률 p(z)에 따라 z를 샘플링하고 조건부 확률 p(x|z)에 따라 x를 샘플링하여 데이터셋 얻었다고 가정
  - variational inference = 역뱡향의 p(z|x)를 구하는 일
  - ![image](https://github.com/user-attachments/assets/bc6a089f-d5c9-4e03-9a6d-fb9d498b277c)
- variational inference
  - 확률 분포의 집합 Q를 가정한 후, Q에 속하는 확률 분포 중에서 참 확률 p(z}x)에 가장 가까운 확률 분포 q를 최적화하는 방법 사용
  - 확률 분포 a와 b가 얼마나 다른지 측정해주는 쿨백-라이블러 발산 사용
  - ![image](https://github.com/user-attachments/assets/0cc79835-2172-4f64-b1bc-6cb202a84531)
 
### 변이 오토인코더
- 오토인코드를 생성 모델로 간주하지 않는 이유? : latent space z가 어떤든 상관 없이 손실 함수를 최소화하는데만, 즉 영상 복원에만 신경쓰면 latent space가 좋은 구조를 형성하지 못할 수 있다. 이 경우, 잘못된 점을 디코더에 입력하면 의미 없는 영상이 생성된다
- VAE: latent space가 가우시안 확률 분포를 이루도록 규제하여 생성 모델로 발돋음.
- ![image](https://github.com/user-attachments/assets/3752915c-1810-4197-836a-4efff3b9863c)
- **MNIST를 VAE로 모델링하고 샘플 생성**
  - ![image](https://github.com/user-attachments/assets/d4692733-3047-4096-9c51-80c59ba6bda6)
  - ![image](https://github.com/user-attachments/assets/3f668b7f-229a-4fb4-8364-09d7e311f29b)
- ![image](https://github.com/user-attachments/assets/409160df-1dbe-4518-ba45-dc1aa3a5fd1e)
  - 

### 이산변이 오토인코더: discrete VAE (dVAE)
- ![image](https://github.com/user-attachments/assets/ac8b1ce6-6c9c-42eb-ab0a-146c888bd0bd)

## 13.3 생성 적대 신경망
- GAN
- 코드: PG613~616
- ![image](https://github.com/user-attachments/assets/ec61d99e-eebc-4916-aad3-e2a50f52dd5d)

## 13.4 확산 모델

## 13.5 생성 모델의 평가
- 주요 요소
  - 충실성: 주어진 데이터셋을 잘 모방하여 비슷하지만 같지 않은 샘플을 생성
  - 다양성: 데이터셋을 이루는 모든  영상을 빼놓지 않고 골고루 반영한다
- 자동 평가: IS: Inception Score, FID: Frechet Inception Distance, KID: Kernel Inception Distance 주로 사용

## 13.6 멀티 모달 생성 모델: 언어와 비전의 결합
- Dalle
  - ![image](https://github.com/user-attachments/assets/a24288fc-82d5-48c2-bca5-4cf76fc6acb3)

