# Chap4 에지와 영역

## Preview
- <b> 에지(Edge): </b> 물체 경계에 있는 점
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
    - **에지 강도**: 에지일 가능성, **에지 방향**: 에지의 진행 방향(그레디언트 방향을 90도 회전한 방향)
      ![image](https://github.com/user-attachments/assets/baa0a697-9be0-4dd1-8f89-eaca29c17362)




