---
title:  "[Computer Architecture] 성능 향상을 위한 알고리즘 기법"
excerpt: "본 게시글은 「한 권으로 읽는 컴퓨터 구조와 프로그래밍」를 바탕으로 작성하였습니다."

toc: true
toc_sticky: true
toc_label: "주요 목차"
 
date: 2021-11-06
last_modified_at: 2021-11-06

use_math: true
comments: true

categories:
  - Computer Architecture
---



- 성능 향상을 위해서는 <u>효율적인 계산 방법</u>과 <u>계산을 회피하는 방법</u>을 고려함
- 그중 계산을 회피하는 방법에 대해 알아봄
- 계산을 간소화하는 프로그래밍 트릭
  - 지름길 계산(short cut)
  - 근사값 계산(approximating)



## 📙 목차

- 표 찾기
- 정수를 사용한 계산 방법
- 재귀적 분할
- 계산을 회피하는 그 밖의 수학적 기법들
- 무작위성



## 🌿 내용

### 🌱 표 찾기

#### 변환 

- x, y의 관계가 복잡한 수식으로 표현될 때, 
- 매번 계산하는 대신 <u>x값의 단위에 따라 대응하는 y값 테이블을 만들어두고 매칭함</u>



#### 텍스처 매핑

- MIP 매핑을 이용함
- 각 픽셀을 32비트 정사각형으롤 표현하면 R, G, B 값을 
