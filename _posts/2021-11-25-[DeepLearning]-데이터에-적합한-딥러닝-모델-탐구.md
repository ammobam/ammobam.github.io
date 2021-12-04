---
title:  "[DeepLearning] 데이터에 적합한 딥러닝 모델 탐구"
excerpt: "데이터에 따른 DeepLearning 적용 방법을 탐구한 글입니다."

toc: true
toc_sticky: true
toc_label: "주요 목차"
 
date: 2021-11-25
last_modified_at: 2021-12-03

use_math: true
comments: true

categories:
  - DeepLearning
---



## 고려할 것

- 데이터의 수
  - 데이터가 충분히 많다면 딥러닝을 이용한 분석 수행
  - 데이터가 적다면 직관적인 해석이 가능한 머신러닝 이용한 분석 수행
  - 데이터가 아주 적다면 정성 분석을 수행



## 딥러닝 개요

### 기본 딥러닝 모델의 종류 (분류 기준 : 층 개수)

- 단층 퍼셉트론
  - 입력층, 출력층으로 구성됨
  - 은닉층이 없음
- 다층 퍼셉트론
  - 입력층, 은닉층, 출력층이 존재함
  - ANN
    - 입력층, 은닉층, 출력층이 각각 1개로 구성됨
  - DNN
    - 입력층 1개, 은닉층 n개, 출력층 1개로 구성됨 (이때, n>2)
  - CNN
    - 노드 1개에 데이터 1개가 아니라, kernel단위로 데이터를 입력 받음
    - 데이터와 주변 관계를 포함한 분석이 가능함
  - RNN
    - 은닉층과 출력층에서 이전 층과, 그 이전 층의 값을 입력으로 받음
    - 데이터의 순서를 포함한 분석이 가능함



### 딥러닝 모델의 응용

- Generative Model, 생성모델 [(참고글)](https://minsuksung-ai.tistory.com/12)

<img src="\assets\posting_img\\generative_models.png" alt="generative_models" style="zoom:67%;" />

- 생성모델에서 생성한 데이터의 분포가 학습 데이터 분포와의 차이가 적을 수록 실제 데이터와 유사해짐
- Explicit dencity - 데이터의 분포 기반으로 생성
  - Tractable density - 데이터의 분포를 직접 구하는 방법
  - Approximate density - 분포를 추정하는 방법
    - 그중 VAE는 분포를 Gaussian Distribution으로 추정하여 사용함
    - Tractable density 보다 성능은 떨어지지만 분포를 가정하는 방법 중 가장 성능이 뛰어남
- Implicit dencity - 데이터의 분포를 모르는 상태에서의 생성



- GAN

  - Generative adversarial networks, 적대적 생성 신경망
  - 두 개의 모델이 서로 다른 목적을 갖고 경쟁하면서 훈련함
  - Generator(생성자)
    - 원본 데이터로부터 새로운 데이터를 생성함. 판별자가 구분 불가능한 수준의 데이터를 생성하는 것이 목표임.
    - 손실함수를 최소화하려는 방향으로 학습
  - Discriminator(판별자)
    - 원본 데이터와 생성한 데이터를 구분하도록 훈련함
    - 손실함수를 최대화하려는 방향으로 학습



- VAE

