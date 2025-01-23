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
