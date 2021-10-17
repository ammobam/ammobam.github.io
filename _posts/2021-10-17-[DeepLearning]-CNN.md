---
title:  "[DeepLearning] CNN 개요"
excerpt: "본 게시글은 「케라스 창시자에게 배우는 딥러닝」을 참고하여 작성하였습니다."

toc: True
toc_sticky: True
toc_label: "주요 목차"

date: 2021-10-17
last_modified_at: 2021-10-17

use_math: true
comments: true

categories:
  - DeepLearning
---

- Convolutional Neural Network
- Convolutional layer를 포함한 신경망 구조



## 🚀 이미지 데이터에 CNN을 이용하는 이유

### 📌 이미지 데이터의 특징

- 이미지 데이터는 ```(가로, 세로, 색상채널)```의 **3차원 데이터**임
- 이미지 데이터는 다음과 같은 특징이 있음
  - 이미지 내에서 가까운 픽셀끼리 유사한 값을 가질 확률이 높음
  - 이미지 내에서 먼 픽셀끼리는 관련이 없음
  - 색상채널 사이에 밀접한 관련이 있음
    - RGB 채널에서 노란색 영역(R 255, G 255, B 0)의 경우, R채널의 값과 G 채널의 값이 높고 B채널의 값은 낮은 픽셀이 연속하여 나타날 가능성이 높음

### 📌 기존의 완전연결층의 문제점

- 기존의 완전연결층(fully connected layer)은 1차원 데이터를 처리함
- 3차원 데이터를 1차원으로 펼치는 과정에서 이미지 데이터의 특징 정보를 잃음
- 

### 📌 Convolution layer(합성곱층)의 등장

- 이미지 데이터의 손실을 최소화하기 위해 3차원 데이터를 처리하는 Convolution layer(합성곱층)이 등장함
- 기존의 완전연결층이 픽셀 하나의 값에 대해 처리하였다면, <u>합성곱층은 일정한 영역(수용영역, receptive field)의 픽셀 묶음을 기본 단위로 하여 처리함</u>
- 각 픽셀과 그 주변 픽셀의 관계에 대한 정보를 반영한 처리가 가능하여 이미지 데이터의 분석에 적합함

### 📌 Kernel

- 수용영역(receptive field)은 각 픽셀과 그 주변 픽셀 묶음임
- 

### 📌 Pooling

- 그러나 합성곱층의 뉴런은 입력 이미지의 각 픽셀에 대해 수용영역



