# Chap4: 에지와 영역

## Preview
- **에지(Edge)**: 물체 경계에 있는 점
-  Edge를 완벽하게 검출해 물체의 경계를 폐곡선으로 따낼 수 있다면 Segmentation 문제 저절로 풀린다
-  반대로, Region Segmentation 알고리즘이 완벽하여 물체를 독립된 영역으로 분할하면 Detection 문제가 풀린다

## 4.1 에지 검츨
- 에지 검출 알고리즘 <-- 물체 내부는 명암이 서서히 변하고 물체 경계는 명암이 급격히 변화하는 특성 활용

### 4.1.1 영상의 미분
![image](https://github.com/user-attachments/assets/230799ae-118c-49e0-9fa3-02c2923255d5)
- 디지털 영상을 미분하는 식으로, 영상에서 필터 u로 컨볼루션할 때 사용
- 디지털 영상에서 x의 최소 변화량은 1이므로 delta(x) = 1로 설정
![image](https://github.com/user-attachments/assets/bc061feb-ec49-481c-8fa8-7857a2abb8b0)
- 명암 변화가 없는 곳은 0 , 명암 변화가 급격한 부분은 3
- 하지만, 현실에서는 명암 변화가 계단 모양 x 

### 4.1.2 에지 연산자
- 현실에서는 명암이 몇 화소에 걸쳐 변하는 Ramp Edge가 발생
![image](https://github.com/user-attachments/assets/c62a3adb-6254-485e-9d47-a75630e3260a)
- **2차 미분한 영상 필터**: ![image](https://github.com/user-attachments/assets/9397ee8e-25ae-4847-84f3-33c99e6cc560)
![image](https://github.com/user-attachments/assets/352a1dd6-e630-43b5-bd58-c8984ef22a1b)

- <b> 영교차(Zero Crossing) </b> : 왼쪽과 오른쪽에 부호가 다른 반응이 나타나고 자신은 0을 갖는 위치
- **에지 검출**은 1차 미분에서 봉우리를 찾거나 2차미분에서 영교차를 찾는 일

- **1차 미분에 기반한 에지 연산자**
  - (-1,1)은 너무 작고 대칭x --> 실제로는 (-1,0,1)로 확장하여 사용
  - (-1,0,1)을 2차원으로 확장하여, x와y 방향의 두 연산자 사용
    ![image](https://github.com/user-attachments/assets/227b4509-689a-458e-bafc-2082ec94faa4)
  - 3x3 크기로 확장하면 잡음을 흡수하여 더 좋은 성능 보임. 이는 가장 널리 쓰이는 에지 연산자
    ![image](https://github.com/user-attachments/assets/bcbba331-024d-4649-a688-a35a959be115)
    - 소벨 연산자는 보다 가까운 상하좌우 화소에 가중치 2를 줌
    - **에지 강도**: 에지일 가능성
    - **에지 방향**: 에지의 진행 방향(그레디언트 방향을 90도 회전한 방향)
      ![image](https://github.com/user-attachments/assets/baa0a697-9be0-4dd1-8f89-eaca29c17362)
    - **소벨 연산자 적용 사례**
    ![image](https://github.com/user-attachments/assets/e841e859-c139-4426-8579-b09c3bf1c5c9)
    ![image](https://github.com/user-attachments/assets/ffa9423b-ba8b-40b1-9e0b-c38c670bbed8)
      - x방향 연산자는 수직 방향의 에지가 선명
      - Y방향 연산자는 수평 방향의 에지가 선명
      - 명암 변화가 큰 곳에 더욱 선명한 에지가 나타남

## 4.2 캐니 에지
- **1986, 캐니, 캐니 알고리즘**
  - 가장 체계적인 검출 이론이라고 인정받은 알고리즘 제안
  - 최소 오류율, 위치 정확도, 한 두께라는 기준에 따라 목적 함수를 정의하고 에지 검출을 최적화 문제로 품
  - Edge Dectection에서 가우시안에 1차 미분 적용한 연산자가 최적이라는 사실을 수학적으로 증명
  - **한 두께 에지**: edge를 얇게 검출하는 것이 목표
    - 한 두께 에지를 출력하기 위해 Non-Maximum Suppression을 적용
  - **Non-Maximum Suppression**: 에지 방향에 수직인 두 이웃 화소의 에지 강도가 자신보다 작으면 에지로 살아남고, 그렇지 않으면 에지 아닌 화소로 바뀐다 => 두 이웃 화소와 비교해 자신이 최대가 아니면 억제
    - non-maximim supression는 지역 최대가 아닌 점을 억제하고 local macimum만 남기는 전략 
    ![image](https://github.com/user-attachments/assets/8a8cc7bc-a005-4714-a982-3eb0146103c8)
  - False Positive을 줄이기 위해 2개의 이력 임계값 T_low, T_high 이용한 Edge Detection 추가 적용
    - 에지 강도가 T_high 이상인 에지 화소에서 시작(에지일 가능성 높은 곳에서 추적 시작)
    - 시작 화소가 정해지면, 이후 추적은 T_low 임계값을 넘는 에지를 대상으로 진행
    - => 추적 이력이 있는 이웃을 가진 화소는 에지 강도가 낮더라도 실제 에지로 인정하는 전략
    -  T_high를 T_low 보다 2~3배로 설정하는 것 권고
- ![image](https://github.com/user-attachments/assets/4c391571-a77c-4e68-b80e-d087e1482b63)
  - 임계값을 5로 설정하면, 노란색으로 칠한 점 2개만 최종 선택됨
- Edge Dectection 연구는 캐니 이후에 뚜렷한 개선 x

## 4.3 직선 검출
- 앞선 개념에서 검출한 에지 맵에서는 에지 화소는 1, 에지 아닌 화소는 0으로 연결 관계가 암시적으로 나타나 있을 뿐 명시적으로 표현 x
- 화소를 연결하여 경계선으로 변환하고 경계선을 직선으로 변환하면 이후 단계인 물체 표현이나 인식에 무척 유리

### 4.3.1 경계선(Contour) 찾기
![image](https://github.com/user-attachments/assets/53a82d1e-edfd-4d2e-a5e4-508acd7c8e29)
- 경계선을 3개 찾아 연속된 점의 리스트로 표현하는 사례
  ![image](https://github.com/user-attachments/assets/9ce4fad3-3687-45b6-8328-c665736b7266)

### 4.3.2 허프 변환
-<b>이웃한 에지 연결하여 경계선 검출 한계</b>: 같은 물체를 구성하는 에지가 자잘하게 끊겨 나타나는 경우 문제 발생 
- ![image](https://github.com/user-attachments/assets/2b888afe-5473-473c-91be-3a819195378a)
- <b>허프 변환 적용</b>하면 끊긴 에지 모아 선분 또는 원 등을 검출 가능
- ![image](https://github.com/user-attachments/assets/b1d50a11-9d63-4596-83a5-562e6cde76c9)

- 허프변환은 직선의 방적식은 알려주는데, 직선의 양끝 점은 알지x
- 양 끝점을 알아내려면 non-maximum suppression 과정에서 극점을 형성한 화소를 찾아 가장 먼 곳에 있는 두 화소를 계산하는 추가적 과정 필요

<활용>

### 4.3.3 RANSAC(Random Sample Consensus)
- ![image](https://github.com/user-attachments/assets/eabe140f-ca4a-45fc-abdc-2c999c6e9e09)
- ![image](https://github.com/user-attachments/assets/2852ba1d-094f-47a9-943e-7fc1981ec213)


## 4.4 영역 분할(Region Segmentation)
- **Region Segmentation**: 물체가 점유한 영역을 구분하는 작업 [다루는 내용 v]
- **Sementic Segmentation**: 의미 있는 단위로 분할하는 방식

### 4.4.1 배경이 단순한 영상의 영역 분할
- **이진화 알고리즘**을 확장해 영역 분할에 사용 가능
  - 예를 들어, 오츄(otsu) 이진화를 여러 임곗값을 사용하도록 확장한 알고리즘으로 영상을 분할 가능
- **군집화 알고리즘**을 영역 분할에 사용 가능
  - (r,g,b) 3개 값으로 표현된 화소를 샘플로 보고 3차원 공간에서 클러스터링을 수행한 다음 화소 각각에 클러스터 번호 부여
- **워터셰드(Watershed)** 확장해 영역 분할에 사용 가능
  - Watershed: 비가 오면 오목한 곳에 웅덩이가 생기는 현상을 모방한 연산
  - 낮은 곳부터 물을 채우는 연산을 반복하면 서로 다른 웅동이 찾을 수 o
    - (에지 강도 맵, 맵을 지형으로 간주)
  - ![image](https://github.com/user-attachments/assets/42fac92b-4d31-4e84-b802-b3ede9b3e4c6)
  - ![image](https://github.com/user-attachments/assets/69cfc6cf-ec93-4856-89c1-78c03e830bbc)

### 4.4.2 슈퍼 화소 분할(Super-pixel Segmantation)
- 때로는 아주 작은 영역으로 분할하여 다른 알고리즘의 입력으로 사용하는 경우 존재
- 작은 영역 화소보다 크지만 물체보다 작아  **슈퍼 화소(Super-pixel)** 라고 함
- **SLIC(Simple Linear Iterative Clustering)** :슈퍼 화소 알고리즘 중 하나
  - K-mean clustering 알고리즘과 비슷하게 작동
  - 처리 과정이 단순하고 성능 좋음
     
### 4.4.3 최적화 분할

## 4.5 대화식 분할
- 지금까지 다룬 분할 알고리즘은 자동으로 영상 전체를 여러 영역으로 나누지만, 때로는 한 물체의 분할에만 관심있는 경우 존재 (EX. 엑스레이 영상에서 특정 장기만 분할)
- 해당 경우에서 사용자는 분할 알고리즘이 필요로 하는 초기 정보 입력할 것 --> 초기 정보를 가지고 반자동으로 동작하는 분할 알고리즘은 사용자 의도에 맞게 물체를 분할

### 4.5.1 능동 외곽선
### 4.5.2 GrabCut

## 4.6 영역 특징

