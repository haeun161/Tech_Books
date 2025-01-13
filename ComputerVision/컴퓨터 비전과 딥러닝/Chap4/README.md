# Chap4 에지와 영역

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
- **1986, 캐니**  **, 캐니 알고리즘**
  - 가장 체계적인 검출 이론이라고 인정받은 알고리즘 제안
  - 최소 오류율과 위치 정확도, 한 두께라는 기준에 따라 목적 함수를 정의하고 에지 검출을 최적화 문제로 품
  - Edge Dectection에서 가우시안에 1차 미분 적용한 연산자가 최적이라는 사실을 수학적으로 증명
  - 한 두깨 에지를 출력하기 위해 Non-Maximum Suppression을 적용
  - **Non-Maximum Suppression**: 에지 방향에 수직인 두 이웃 화소의 에지 강도가 자신보다 작으면 에지로 살아남고, 그렇지 않으면 에지 아닌 화소로 바뀐다 => 두 이웃 화소와 비교해 자신이 최대가 아니면 억제
    ![image](https://github.com/user-attachments/assets/8a8cc7bc-a005-4714-a982-3eb0146103c8)
  - False Positive을 줄이기 위해 2개의 이력 임계값 T_low, T_high 이용한 Edge Detection 추가 적용
    - 에지 강도가 T_high 이상인 에지 화소에서 시작(에지일 가능성 높은 곳에서 추적 시작)
    - 시작 화소가 정해지면, 이후 추적은 T_low 임계값을 넘는 에지를 대상으로 진행
    - => 추적 이력이 있는 이웃을 가진 화소는 에지 강도가 낮더라도 실제 에지로 인정하는 전략
    -  T_high를 T_low 보다 2~3배로 설정하는 것 권고
- Edge Dectection 연구는 캐니 이후에 뚜렷한 개선 x

## 4.3 직선 검출






