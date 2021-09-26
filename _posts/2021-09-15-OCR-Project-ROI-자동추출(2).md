---
title:  "[OCR Project] ROI 자동 추출(2)"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: False
toc_sticky: False
toc_label: "주요 목차"
 
date: 2021-09-15
last_modified_at: 2021-09-15

use_math: true
comments: true

categories:
  - OCR Project
---


## ROI 자동으로 추출하기

- 아이디어 : 계량기 타입별로 고정된 데이터 영역을 도출하여 고정된 비율을 곱해 계량기를 검출함


1. 'G-Type' 글자가 잘 보이게 이미지 전처리
- 이미지 평활화
- 이진화 - OTSU 등
- OCR 모델이 모든 이미지에 대해 글자 영역을 잘 읽을 때까지 수행


2. OCR 모델로 'G' 또는 'G-Type' 문자 읽고 그 부분의 좌표 및 크기 반환받기
- 'G' 폰트별 OCR 모델 훈련
- 다른 모델 써보기 : tesseract, google attention OCR 등


3. 'G-Type' 문자 영역의 비율로 계량기 데이터 영역 도출
