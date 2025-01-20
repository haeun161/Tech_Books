# Chap5: 지역 특징
- 풀어야할 가장 중요한 문제: 대응점 문제(Correspondence Problem)
- **대응점 문제(Correspondence Problem):**
  - 이웃한 영상에 나타난 같은 물체의 같은 곳을 쌍으로 묶어주는 일
  - 두 영상의 특징점(Feature Point)을 추출하고 매칭을 통해 특징점을 찾는 일
  - ![image](https://github.com/user-attachments/assets/ab090560-32ed-42c4-8764-e1162b70551c)
  - = 위와 같이 다른 시간의 영상에서 같은 물체 인지하는 것
- 활용 예시: "휴대폰의 파노라마 영상 기능", 물체 인식, 물체 추적, 스테레오 비전, 카메라 캘리브레이션 등 응용 문제에 적용 가능
- 대응점 문제(Correspondence Problem)의 대안으로 지역특징(local features) 연구에 집중하여 성능 개선 이룸

- ## 발상
- **기존**: 에지를 연결한 경계선에서 corner를 찾아 특징적으로 사용하는 아이디어 사용
- **아이디어**: 좁은 지역을 보고 특징점 여부를 판정하는 방식 = **지역 특징**
- 지역 특징은 다양한 종류(위치, 스케일, 방향, 특징 기술자)로 표현
- 위치와 스케일은 Dectection 단계에서 알아내고, 방향과 특징 기술자(feature descriptor)는 Description 단계에서 알아낸다

### 지역 특징의 조건
- **반복성repeatability** 같은 물체가 서로 다른 두 영상에 나타났을 때 특징점이 두 번째 영상에서도 같은 위치에서 높은 확률로 검출되어야 한다
- **불변성invariance** 물체에 이동, 회전, 스케일, 조명 변환이 일어나도 특징 기술자의 값은 비 
슷해야 한다. 불변성을 만족해야 다양한 변환이 일어난 상황에서도 매칭에 성공할 수 있기 
때문
- **분별력discriminative power** 물체의 다른 곳에서 추출된 특징과 두드러지게 달라야 한다. 그렇 
지 않다면 물체의 다른 곳에서 추출된 특징과 매칭될 위험이 있다. 
- **지역성locality** 작은 영역을 중심으로 특징 벡터를 추출해야 물체에 가림occlusion이 발생해도 
매칭이 안정적으로 동작한다 
- **적당한 양** 물체를 추적하려면 몇 개의 대응점만 있으면 된다.
  - 대응점은 오류를 내포할 가능성이 있기 때문에 특징점이 더 많으면 더 정확하게 추적할 수 있다.
  - 단, 특징점이 너무 많으면 계산 시간이 과다해진다. 
- **계산 효율** 초당 몇 프레임 이상을 처리해야 하는 실시간 조건이 필수
- 단, trade-off 존재하므로 응용에 대해 깉은 이해 바탕으로 조절하기!

## 5.2 이동과 회전 불변한 지역 특징

### 5.2.1 모라벡 알고리즘
- ![image](https://github.com/user-attachments/assets/5482ea9c-ce78-4694-b551-d32fbe8e1149)
- 모라벡은 다음과 같은 특징점(a,b,c) 중 a는 여러 방향으로 색상 변화가 있어 찾기 쉽고, c는 어느 방향으로도 밝기 변화가 미세하여 찾기 어렵다고 설명
- **찾기 쉬운 정도를 나타내는 수식**(제곱차의 합):
- ![image](https://github.com/user-attachments/assets/1aa8d36f-ad4f-417d-bb5e-b28563f58594)
- ![image](https://github.com/user-attachments/assets/155b1b18-1859-4405-9c1d-794a6fbbeefb)
- **지역 특징으로 좋은 정도** 는 상하좌우에 있는 화소의 최소값을 특징 가능성 값 C로 취함
  - a는 C=2, B=0, C=0

### 해리스 특징점
- 모라벡 알고리즘의 상하좌우 이웃만 보고 점수를 매기는 방식은 현실적이지 않다는 한계점 존재
- 가중치 개념 추가
- **가중치 제곱차의 합**
  - ![image](https://github.com/user-attachments/assets/a7a1a16d-69d4-4b40-8cd7-0e7bd41b8b48)
- 테일러 확장을 통해 해당 식을 확장하면: **2차 모멘트 행렬**
  - ![image](https://github.com/user-attachments/assets/49b281ba-ea8c-488d-a030-9e35f715e181)
  - ![image](https://github.com/user-attachments/assets/4edb16d3-e1ed-4cd4-8dd0-66cf9af3a89a)
  - 이는 특정 화소 주의의 영상 구조를 표현하고 있어, A만 분석하면 지역특징 여부 판단 가능
  - 정수 뿐만 아니라 실수도 계산 가능
- 해리스는 A의 고유값을 보고 지역 특징으로 좋은 정도를 측정하는 기법 제안
  - ![image](https://github.com/user-attachments/assets/11d35548-55e2-4d08-a164-3e9bab4dbfbe)
    - 고유값이 모두 큰 경우 지역 특징으로 훌륭하다
  - ![image](https://github.com/user-attachments/assets/b4233836-7b00-4168-979d-1e3828f61075)
    - 하지만 위의 식은 고유값을 계산하는데 시간 상당하므로 피하는 방법이 좋음
  - ![image](https://github.com/user-attachments/assets/f4a0ddee-a02c-42a5-9219-d90b6252ed6a)

### 해리스 특징점의 분석
- 검출된 특징점들은 모퉁이뿐만 아니라 blob도 잘 검출한다
- 해리스는 자신의 알고리즘이 찾은 특징은 모퉁이라 하지만, 추후 연구자들은 이를 feature point 또는 interest point라고 지칭
- 해리스 특징점은 이동과 회전에 equivariant하며, 회전에 불변이다.
- 스케일 변환에서 물체의 크기에 따라 마스크의 크기를 적절하게 조정해야 한다

## 5.3 스케일 불변한 지역 특징
- **스케일 공간scale space** 이론에서는 스케일을 모르는 상황에 대응하기 위해 3단계 전략을 사용
- 대표적인 알고리즘: SIFT

### 핵심 알고리즘
- ![image](https://github.com/user-attachments/assets/22d928eb-7de6-4182-954c-a22a6b8b7b55)
  - **1행**: 다중 스케일 영상은 가까이부터 멀리까지 본 장면으로 표현
    - **방법1**: 거리가 멀어지면 세부 내용이 점점 흐려지는 현상은 모방
      - 표준편차를 키우면서 가우시안 필터로 입력 영상을 스무딩하여 흐려지는 현상을 시뮬
      - ![image](https://github.com/user-attachments/assets/f4ba9ebc-8167-4bba-8f24-b00cb5271037)
    - **방법2**: 거리가 멀어짐에 따라 물체의 크기가 작아지는 현상 모방
      - 영상의 크기를 반씩 줄인 영상을 쌓은 피라미드 영상으로 현상을 시뮬
      - ![image](https://github.com/user-attachments/assets/f75a0b8c-25ca-4241-a6b1-ad289b8f3aed)
    - 방법1,2로 만들어진 다중 스케일링 영상
      - ![image](https://github.com/user-attachments/assets/ae9497e1-f293-4e89-a043-24a15486c823)
   
  - **2행**: 스케일 공간의 미분은 Laplacian을 주로 사용
    - ![image](https://github.com/user-attachments/assets/bcc568ec-be72-4b20-ad49-c68fbebb0888)
    - Laplacian은 스케일 공간에서 극점 찾기 유리
    - 표준편차가 클수록 d_yy, d_xx가 작아지는 문제로 주로 정규 라플라시안 사용
      - ![image](https://github.com/user-attachments/assets/abba1aa1-652e-4570-b846-ecb03081c376)
  - **3행**: 극점 검출 - 주로 비최대 억제 사용
  
## 5.4 SIFT
- 스케일 고려한 기본 알고리즘의 변형인 SIFT(Scale-Invariant Feature Transform)
- SIFT는 가장 성공적이고 지금까지 널리 사용됨
- SIFT는 3단계를 거쳐 특징점을 검출하며, 각 단계는 최적의 검출 정확도를 달상하고 계산 시간을 최대한 줄이는 연상 사용

### 검출
- **단계1: 다중 스케일 영상 구축**
- 기존과 같이, 가우시안 스무딩과 파라미드 방법을 결함하여 다중 스케일링 영상을 구성한다
- ![image](https://github.com/user-attachments/assets/cc25f373-0564-4707-9497-0c80712a359d)
  - 옥타브octave: 원래 영상에 가우시안 스무딩 적용해 만든 영상
  - 영상을 반으로 줄이는 일을 반복하여 옥타브1,2,3,4,...를 추가

- **단계2: 더중 스케일 영상에 미분 적용**
- 정규 Lapacian 사용
- Lapacian은 큰 필터로 컨볼루션을 수행하므로 시간이 많이 걸린다
- SIFT는 정규 Lapacian와 매우 유사하다고 증명된 DOG(Difference of Gaussian) 사용하여 계산시간 획기적으로 중임

- **단계3: 극점 검출**
- 
