# K-최근접 이웃(KNN-K Nearest Neighbors)
- KNN은 거리 가반 모델로, 데이터 간의 거리를 활용하여 새로운 데이터를 예측하는 모델
- 가까이에 있는 데이터를 고려하여 예측값이 결정된다
- 다중분류 문제에서 가장 간편하게 적용할 수 있는 알고리즘
  - 데이터 크지 않고 예측이 까다롭지 않은 상황에서 신속하고 쉽게 예측 모델 구현 가능
- ![image](https://github.com/user-attachments/assets/aea640aa-6533-4a8d-a5d5-b5f786fbd24e)

## 장점
- 수식에 대한 설명이 필요 없을 만큼 직관적이고 간단
- 선형모델과 다르게 별도의 가정이 존재하지 않아서 더 자유롭다
  - 선형 회귀는 독립변수와 종속변수의 선형 관계를 가정

## 단점
- 데이터가 커질수록 상당히 느려질 수 있다
- 아웃라이어에 취약

## 유용한 곳
- 주로 분류에 사용되며, Logisitc Regression으로 해결할 수 없는 3개 이상의 목표변수들로 분류 가능
- 작은 데이터셋에 적
 
