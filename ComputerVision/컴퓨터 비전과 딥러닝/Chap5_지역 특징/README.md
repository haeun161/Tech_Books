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
  - ![image](https://github.com/user-attachments/assets/84f21847-ba70-49df-9221-57feed5c8827)
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

- **단계2: 다중 스케일 영상에 미분 적용**
  - 정규 Lapacian 사용
  - Lapacian은 큰 필터로 컨볼루션을 수행하므로 시간이 많이 걸린다
  - SIFT는 정규 Lapacian와 매우 유사하다고 증명된 DOG(Difference of Gaussian) 사용하여 계산시간 획기적으로 중임
  - 

- **단계3: 극점 검출**
  - 기존과 같이, 비최대 억제를 적용하여 극점을 찾고 특징점 취하지만, 2차원 공간이 3차원으로 확장된 차이 존재
  - ![image](https://github.com/user-attachments/assets/6ade6057-a35b-4039-854c-477b42d938db)
  - i번째 영상의 이웃화소 8개, i-1과 i+1번째 영상을 조사하여, X 화소가 26개 이웃 화소보다 크면 극점으로 인정하고, 특징점으로 취한다
  - X를 키포인트(Keypoint)라고 함
    - (y,x,o,i)로 표현
    - 검출된 특징점을 나타내는 o, DOG 번호를 나타내는 i, 옥타브에서 화소 위치를 나타내는 y와 x를 모아 (y,x,o,i)로 표현
    -  [o,i는 스케일 정보 나타냄]
    -  [y,x는 미세 조정한 위치를 나타내는 실수]
  - 이 값들을 테일러 확장으로 미세조정하여 최종 확정 수행

### SIFT 기술자(Descriptor)
- 앞에서는 위치, 스케일 정보 알아냈지만, 물체 매칭하기에는 부족 
=> Descriptor 추출하는 단계 거침
- Descriptor기술자는 특징 벡터에 해당
  
#### 알고리즘
1. **키포인트 주변 영역 추출**
   - 각 키포인트를 중심으로, 해당 스케일에서 16×16 크기의 **지역 패치(Local Patch)**를 추출합니다.
     - 이 패치의 크기는 스케일 공간에서 검출된 키포인트의 스케일에 따라 조정됩니다.
     - 예: 키포인트가 큰 스케일에 있다면, 물리적으로 더 넓은 영역을 선택.

2. **지역 패치를 하위 블록으로 분할**
   - 16×16 패치를 4×4 블록으로 나눕니다.
   - 총 16개의 하위 블록(Sub-region)이 생성됩니다.
   - 각 하위 블록은 4×4=16개의 픽셀을 포함합니다.

3. **각 하위 블록에서 Gradient 계산**
- 각 하위 블록 내에서, 픽셀들의 기울기(Gradient Magnitude)와 방향(Gradient Orientation)을 계산합니다. 여기서 \( L(x, y) \)와 \( L(x+1, y), L(x, y+1) \)은 X 및 Y 방향의 밝기 변화(미분).
  1. 기울기의 크기 \( m(x, y) \):
     \[
     m(x, y) = \sqrt{(L_x(x, y))^2 + (L_y(x, y))^2}
     \]
  2. 기울기의 방향 \( \theta(x, y) \):
     \[
     \theta(x, y) = \tan^{-1}\left(\frac{L_y(x, y)}{L_x(x, y)}\right)
     \]

4. **360도 방향을 양자화**
   - 기울기 방향 \( \theta(x, y) \)는 0~360° 사이의 값을 가집니다.
  - 이를 8개의 방향 히스토그램 \( b(bin) \)으로 양자화(Quantize)합니다.
    - 각 빈의 범위: 360°/8=45°.
      - 예: 0°~45°는 첫 번째 빈, 45°~90°는 두 번째 빈.

5. **히스토그램 생성**
   - 하위 블록별로 기울기 크기 \( m(x, y) \)를 양자화된 방향에 따라 히스토그램에 투입합니다.
     - 예: 픽셀의 방향이 30°라면 첫 번째 빈에 기울기 크기를 추가.
   - **선형 보간(Linear Interpolation)**을 사용해, 픽셀이 두 방향 사이에 걸쳐 있다면 두 빈에 가중치를 나누어 추가.
  - ![image](https://github.com/user-attachments/assets/996192b6-76de-4279-8747-1ee30c5291f9)

## 5.5 매칭
- 매칭은 물체 인식, 물체 추적, 스테레오, 카메라 캘리브레이션 등 다양한 문제에서 핵심 역할을 수행

### 5.5.1 매칭 전략
- **문제의 의해**
  - 영상에서 추출한 기술자 집합 A={a1,a2,...a_n}와 B={b1,b2,..,b_m}가 주어졌을 때 같은 물체의 같은 곳에 해당하는 a_j와 b_i의 쌍을 모두 찾는 문제
  - 잡음이 심할 경우, 물체 추적하기 어려움
- 가장 쉽게 생각할 수 있는 알고리즘: A와 B를 조합한 mn개의 쌍 각각에 대해 거리를 계산하고, 거리가 임계값보다 작은 쌍을 모두 취하는 것 (주로 유클리디안 거리 사용)
  - 한계점1: 물체의 같은 곳이 아닌데 매칭되는 쌍(FT), 같은데 매칭 실패하는 경우(FN)가 자주 발생
  - 한계점2: m과 n이 커서 매칭에 많은 시간 소요
    
- **매칭 전략**
  - 3가지 전략
  - **(1)고정 임곗값 방법**: 두 기술자 a와 b 거리가 임계값보다 작으면 매칭되었다고 간주
    - ![image](https://github.com/user-attachments/assets/d34bbfe0-0ee8-427f-a9bd-a146cf9f5662)
    - 임계값 T를 정하는 일이 매우 중요
    - T 너무 작으면, 진짜 매칭 쌍이 조건을 통과하지 못해 FN이 많이 발생
    - T 너무 크면, FP이 많이 발생
  - **(2)최근접 이웃nearest neighbor**: a는 B에서 거리가 가장 b를 찾고, a와 b가 식을 만족시키면 매칭 쌍으로 취한다
    - ![image](https://github.com/user-attachments/assets/00b3639d-c313-417d-ba90-80466ece11e4)
  - **(3) 최근접 이웃거리 비율**: a는 B에서 가장 가까운 b_j와 두번째로 가까운 b_k를 찾아 아래 식을 만족하면 매칭 쌍이 된다
    -  ![image](https://github.com/user-attachments/assets/8ccda5ea-2703-496d-b4cd-0f0921d9c8b3)
    -  많은 눈문이 최근접 이웃 거리 비율 전략이 가장 좋은 성능을 보인다는 실험 결과를 보고
    -  SIFT도 이 전략 사용
  - ![image](https://github.com/user-attachments/assets/d80fb28f-9de7-4902-a7a4-e8bf269d043f)

### 5.5.2 매칭 성능 측정
- **컴퓨티 비전 측정 참조문헌**
  - 컴퓨터비전의 성능 측정을 폭넓게 이해하려면 [Christensen2002,Scharstein2021]
  - [Scharstein2021] 컴퓨터 비전의 성능 측정에 대한 논문 여러 편을 실은 IJCV의 2021년 4월 특집호를 소개
    - 영상 데이터셋, 성능 측정 기준, 벤치마킹 결과 등을 상세하게 다룸
      
- **정밀도와 재현율**

- **ROC 곡선과 AUC**
  - [기존 한계점] 고정 임계값이나 최근접 이웃 거리 비율 방법은 임계값 T에 따라
    - T 작게 하면, 거짓긍정률(False Positive Rate)은 작아진다
    - T 크게 하면, 거짓 긍정률이 커지는데, 참긍적률도 따라 커진다
    - ![image](https://github.com/user-attachments/assets/bbba5723-24fc-4e82-bff4-4e3e444bd168)
  - [**ROC: Reciever Operating Characteristic**] ![image](https://github.com/user-attachments/assets/6f435b47-9fb3-4ff3-8ae9-c28d431a9f25)
    - C2 성능이 가장 뛰어나다
  - [**AUC: Area Under Curve**]: 아래쪽 면접을 나타냄

- **빠른 매칭**
  - 속도 성능 지표
  - 빠르게 탐색하는 알고리즘 ex) 이진 탐색 트리, 해싱, kd트리, 위치 의존 해싱, FLANN과 FAISS
  - **k d트리**
    - ![image](https://github.com/user-attachments/assets/d917af53-26bc-41cc-aaaa-ff9ef8655693)

## 5.6 호모그래피 추정
- outlier 걸러내는 과정 + 매칭 쌍을 이용해 물체 위치 찾는 과정 추가 필요 => 호모그래피
- **투영 변환:** 3차원 점이 2차원 평면으로 변환되는 기하 관계
- ![image](https://github.com/user-attachments/assets/84eba3d3-536f-4a43-93cb-320c1d997afa)
  - 서로 다른 방향에서 획독한 영상으로 평면 P에 있는 p1,p2가 각각 투영된다 => 투영 변환
  - 호모그래피(평면 호모그래피): 두 평면 사이의 투영 변환 관계를 나타내는 행렬로, 한 이미지에서 다른 이미지로 점 대응 가능
  - 제한된 상황에서 이루어지는 투영 변환 = **평면 호모그래피**(호모그래피)

