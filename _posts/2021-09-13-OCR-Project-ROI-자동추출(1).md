---
title:  "[OCR Project] ROI 자동 추출(1)"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: False
toc_sticky: False
toc_label: "주요 목차"
 
date: 2021-09-13
last_modified_at: 2021-09-13

use_math: true
comments: true

categories:
  - OCR Project
---


## 중간 점검

- 이번 주에 한 것 : 이미지 전처리 및 직선 검출
- 다음 주에 할 것 : ROI 추출 자동화

1. cv2.matchTemplate()

- 20장의 샘플이미지로 수행함
- 성능 향상을 위해선 더 많은 계량기 ROI 훈련 이미지가 필요함
- 회전, 크기 변화에 매우 민감함
- 계량기 타입별로 ROI 이미지가 달라져서 별도의 훈련이 필요함


2. cv2.CascadeClassifier()

- 시도해볼만함
- 성능 확인해보고 이용 여부 판단해 볼 예정
- 이용 시 계량기 ROI 영역을 이용하여 분류기 훈련 필요


3. selective search

- window sliding과 달리 이미지 픽셀의 컬러, 무늬, 크기, 형태에 따라 유사한 region을 계층적 그룹핑하여 계산하는 방식임
- 모든 계량기 이미지에 대해 일관된 전처리를 수행하여 일정한 특징을 검출할 수 있는지 확인 필요함
- 현재까지는 이미지 파일을 색상 공간을 hsv로 읽고 saturation으로 배열 변환했을 때, 액정 영역에서 특정 화소값 범위로 한정 할 수 있는지 확인하는 중
