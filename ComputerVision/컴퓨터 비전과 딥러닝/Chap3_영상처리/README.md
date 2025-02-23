# Chap3 영상처리
- **영상처리**: 특정 목적 달성을 위해 원래 영상을 개선된 영상으로 변환하는 작업 ex) 화질 개선
- 컴퓨터 비전에서 영상처리를 전처리 단계에서 활용

## 3.1 디지털 영상 기초

### 3.1.1 영상 획득과 표현
- 핀홀카메라 모델: 영상 획득 과정
  - 물체에서 반사된 빛은 카메라 구멍을 통해 들어가 영상 평면(image plane- 눈의 망막, 필름, CCD센서)에 맺힘.
  - ![image](https://github.com/user-attachments/assets/b09bb25a-fd1e-4e48-8bda-c2c92bee3590)
- 디지털변환하는 과정에서 샘플링과 양자화 수행
  - **샘플링**: 2차원 영상 공간을 가로 N, 세로 M 구간으로 나눔
    - 화소pixel: 형성된 한 점
    - (M X N)을 영상의 크기 또는 해상도resolution이라고 지칭
  - **양자화**: pixel의 명암을 L개의 구간으로 나눔(보통 1byte로 표현할 수 있도록 L=256)
  - ![image](https://github.com/user-attachments/assets/c88e8128-01d7-4db2-9ff2-441adc6a57e3)
- 디지털 영상의 좌표
  - ![image](https://github.com/user-attachments/assets/eb980cdc-8bba-4852-982c-57f33b34fca5)

### 3.1.2 다양한 종류의 영상
- **명암 영상grayscale image)**
  - 2차원 구조의 배열러 표현, 채널 하나
- **RGB**
  - 3개 채널로 구성된 컬러 영상
- **컬러 동영상**
  - 4차원 구조의 배열
  - 인간의 눈이 식별할 수 있는 가시광선에서 획득
- **다분광multi-spectral 영상**
  - 적외선과 자외선 영역까지 확장
  - 3~10개 가량의 채널
- **초분광hyper-spectral 영상**
  - 다분광보다 조밀하게 획득한 수백~수천개의 채널
- 다분광과 초분광 영상
  - 컬러 영상과 마찬가지로 3차원 구조의 배열로 표현. 단치 축의 크기가 3이상으로 확장될 뿐.
  - 농업, 군사, 우주, 기상 등의 응용에 유용하게 활용됨
  - 컬러 영상에서 식별 불가한 농작물의 병을 다분광이나 초분광 영상에서는 식별 가능
  - MR영상과 CT영상도 같은 구조
- **레인지 영상/깊이 영상**
  - 물체까지 거리를 측정한 영상
  - 거리(depth, range)
  - 빛은 날씨나 조명 변화에 매우 민감하지만 거리는 둔감하여 해당 영상은 변화에 강건robust
  - 안전이 중요한 자율주행차에서도 깊이 영상 사용
- **RGB-D 영상**
  - 다른 방식의 깊이 센서로 라이다lidar가 존재
  - lidar: 어떤 조건을 만족하는 점의 거리만 측정 => 완벽한 격자 구조 x
    - 따라서, 획득한 거리 데이터를 **점 구름point cloud**영상으로 표현
    - 점 좌표 x,y와 거리 d (x,y,d) 형식으로 저장
- ![image](https://github.com/user-attachments/assets/2b51cd8c-6e0a-499e-98db-ddef38c2214c)

### 3.1.3 컬러 모델
- **RGB 컬러 모델**
  - ![image](https://github.com/user-attachments/assets/917aef7f-6de8-4e41-945f-9f9b9b9e8cc8)
  - 빛이 약해지면 R,G,B값 모두 작아짐.
  - 익은 과일 검출하는데에는 방해 요인이 될 수 있음
- **HSV 컬러 모델**
  - H는 색상, S는 채도, V는 명암
  - 익은 과일 검출에 유리

### 3.1.4 RGB 채널별로 디스플레이
- ![image](https://github.com/user-attachments/assets/658eb113-0b49-41e9-82af-af2542c817c3)

## 3.2 이진 영상
- 이진 영상 binary image: 화소가 0(흑) 또는 1(백)인 영상
- 컴퓨터 비전에서는 (에지를 검출)에지만 1로 표현 / (물체 검출) 물체는 1, 배경은 0으로 표현하는 이진 영상 활용

### 3.2.1 이진화
- 명암 영상을 이진화 하면, 임계값 T보다 큰 화소는 1, 그렇지 않는 화소는 0으로 변환
  - ![image](https://github.com/user-attachments/assets/2f8708ca-1c34-499d-9760-8cd6d3af1473)
- **임계값 T를 설정**
  - T가 너무 낮거나 높으면 대부분의 pixel이 물체 또는 배경에 쏠리는 문제 발생
  - 보통 히스토그램의 **계곡 근처를 임계값으로** 설정해 쏠림 현상 누그러뜨림
    - 히스토그램: 0,1,..,L-1의 명암 단계를 각각에 대해 화소의 발생 빈도를 나타내는 1차원 배열
    - ![image](https://github.com/user-attachments/assets/91f7c5be-9129-4505-935c-1671528007b8)
    - L=0~8, 4가 계곡이므로 T=4
- **계곡 근처를 임계값으로 설정시 한계점**
  - ![image](https://github.com/user-attachments/assets/d5794f90-2194-442a-97a6-e0fc8c4f139d)
  - 다음과 같이 큰 계곡만 해도 3개이다. 이처럼 계곡이 여러개인 경우 임곗값 정하는 것이 어렵다
  - 해결방안: 오츄 알고리즘
    
### 3.2.2 오츄 알고리즘 Otsu
- 모든 명암값에 대해 목적 함수 J를 계산하고, J가 최소인 명암값을 최적값 t로 결정, 결정한 t가 임계값
  - ![image](https://github.com/user-attachments/assets/e00cbbcc-43e5-4821-895b-f9631fb66cc0)
  - J는 이진화시 0이 되는 분산과 1이되는 분산의 가중치 합을 사용
    - 분산이 작을수록 0인 화소 집합과 1인 화소집합이 균일하기에 좋은 이진 영상이 된다는 발상
    - ![image](https://github.com/user-attachments/assets/ac760ff1-7280-4429-9b58-0545632d85af)
      - n은 t로 이진화 된 영상에서 0인 화소와 1인 화소의 개수
      - v는 0과1인 화소의 분산
  - ![image](https://github.com/user-attachments/assets/367da1f3-70a7-4a14-865e-232db9f832a4)
- 이와 같이, 모든 t의 후보를 다 검사하는 방법은 Exhaustive Search Alogorithm이라고 함
- 실제 컴퓨터비전이 푸는 문제는 해의 공간이 수만~수억 또는 무한대로 Exhaustive Search Alogorithm는 비현실적
  - 대안으로, 물체의 외곽선을 찾는 스네이크 알고리즘은 명암 변화와 곡선의 매끄러운 정도가 최대가 되는 최적해를 Greedy Algorithm로 찾음
  - 신경망 학습시에는 back-propagation 알고리즘 사용해 최적해 빨리 찾음

### 3.2.3 연결 요소 Connected Component
- **연결 요소**: 이진 영상에서 1의 값을 가진 연결된 화소의 집합
- ![image](https://github.com/user-attachments/assets/58b7c045-2c43-45b7-87d5-bd94a9edc5ee)
- ![image](https://github.com/user-attachments/assets/99efd004-4c08-4de6-8140-799cadfd21b6)

### 3.2.4 모폴로지
- **목적**: 영상을 변환하는 과정에서 하나의 물체가 여러 영역으로 분리되거나 다른 물체가 한 영역으로 붙는 경우 발생
  - 위의 부작용을 누그러뜨리기 위해 모폴로지 연상 사용
- 종류: 이진 영상 위한 모폴로지, 명암 영상 위한 모폴로지
- 모폴로지는 구주 요소를 이용해 영역의 모양을 조작
  - ![image](https://github.com/user-attachments/assets/6ccdee01-248a-4d55-9287-2393acdfbcc2)

- 모폴로지 기본 연산은 **팽창 dilation, 침식 erosion**, **열림. 닫힘**
  - **팽창**: 구조 요소의 중심을 1인 화소에 씌운 다음, 구조 요소에 대항하는 모든 화소를 1로 변형
    - 작은 홈을 메우거나 끊어진 영역을 하나로 연결하는 효과
  - **침식**: 구조 요소의 중심을 1인 화소에 씌운 다음, 구조 요소에 해당하는 모든 화소가 1인 경우 1 유지, 그렇지 않으면 0으로 변형
    - 경계에 솟은 돌출 부분을 깎는 효과
  - 침식을 수행한 영상에 팽창 적용하면 대략 원래 크기 유지
  - 침식 결과에 팽창 적용하는 연산 = 열림opening
  - 팽창 결과에 침식 적용한 연산 = 닫힘closing
  - ![image](https://github.com/user-attachments/assets/b6627c14-6e56-40d5-9a75-125fa1a8d02f)
 
## 3.3 점 연산
- 화소 입장에서 영상 처리 연산이랑? 화소가 새로운 값을 받는 과정
- 새로운 값을 어디에서 받느냐에 따라 **점 연산, 영역 연산, 기하 연산**으로 구분
  - ![image](https://github.com/user-attachments/assets/86e2b4f5-b1fd-4b6e-8ab6-269656173574)

### 3.3.1 명암 조절
- 영상을 밝거나 어둡게 조정
- ![image](https://github.com/user-attachments/assets/d926cef2-8bb9-42fc-864d-dde24ed1596c)
  - 1번째 식: pixel 가질 수 있는 최댓값 L-1 넘지 않게 min 사용
  - 2번째 식: 원래 영상에서 양수 a만큼을 빼서 어둡게 만들고, max를 취해 음수 방지
  - 3번째 식: L-1에서 원래 명암값을 빼서 반전시킴
  - (곱하고 더하는 선형 연산만으로 구성됨)
- **감마 보정Gamma Correction**
  - 인간의 눈은 빛의 밝기 변화에 비선형적으로 반응
    - 예를 들어, 명암 10->20과 120->130은 같은 양이지만 인간이 느끼는 밝아지는 정도가 다르다
  - ![image](https://github.com/user-attachments/assets/42db3ecb-8610-4527-93ec-cc95d9b4178e)
    - 곱하는 f: [0,L-1]를 L-1로 나눠 [0,1] 범위로 정규화한 영상
    - 감마: 사용자가 조종하는 값, (1은 원래 영상 유지, 1보다 작으면 밝아지고, 1보다 크면 어두워짐)
      - ![image](https://github.com/user-attachments/assets/0e788cca-af1f-4feb-9660-cfe5ec3b3575)

### 3.3.2 히스토그램 평활화 histogram equalization
- 히스토그램 평활화: 히스토그램이 평평하게 되도록 영상 조작해 **영상의 명암 대비를 높이는 기법**
- ![image](https://github.com/user-attachments/assets/f586f455-9105-4684-aaac-a75b28ee62db)
  - h(.): 모든 칸을 더하면 1.0이 되는 정규화 히스토 그램
  - h(..): i번 칸은 0~i번 칸을 더한 값을 가진 누적 정규화 히스토그램
  - l: 원래 명암 값
  - l': 평활화로 얻은 새로운 명암
  - ![image](https://github.com/user-attachments/assets/4c65cbb4-bb4c-42bf-ae9b-75ea7df81ca9)
  - ![image](https://github.com/user-attachments/assets/80b08af1-bd2e-429b-bb33-874d1543051b)
  - ![image](https://github.com/user-attachments/assets/2c48071e-f471-40de-8385-a3e4da6ecd26)
- ![image](https://github.com/user-attachments/assets/b1e31fa8-03a9-4b43-a5a9-6e9fe6ec2ef3)


## 3.4 영역 연산
- 영역 연산: 아웃 화소 같이 고려해 새로운 값 결정
- 주로 컨볼루션 통해 이루어짐

### 3.4.1 컨볼루션

### 3.4.1 컨볼루션 


## 3.5 기하 연산
## 3.6 OpenCV의 시간 효율
