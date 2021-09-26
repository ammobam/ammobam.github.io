---
title:  "[OCR Project] 멘토링"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: true
toc_sticky: true
toc_label: "주요 목차"
 
date: 2021-09-23
last_modified_at: 2021-09-23

use_math: true
comments: true

categories:
  - OCR Project
---

## 계량기 영역 도출
- 네 개 사각형을 기준으로 검출하는 방식을 제안함

<img src="..\assets\posting_img\210923 four box.png" alt="210925 four box" style="zoom: 33%;" />

- (1) 이미지 전처리
	- 이진화, 경계선 또렷하게. 네 개 사각형 잘 나오게 그림자, 빛 처리
- (2) 네 개 사각형에 대한 영역 검출
- (3) 비율을 적용하여 계량기 영역 도출



## 데이터 영역에서 기울기 자동 변환을 위한 작업
- (1) 이미지 전처리 - 모폴로지. 문자영역이 잘 뭉쳐지는 커널, 파라미터 찾기.
- (2) 기울기 기준 탐색
	- (2)-1. contouring 수행 - cv2.approxPolyDP
	- (2)-2. hough 변환을 이용한 직선 검출
	- (2)-3. [Form OCR을 이용한 회전](https://www.pyimagesearch.com/2020/09/07/ocr-a-document-form-or-invoice-with-tesseract-opencv-and-python/)


- (3) 이미지 3분할하여 각 영역에서 기준 기울기 찾기
- (4) 이미지 3분할 하여 각 영역에 속한 문자를 기울기 변환
- (5) 각 영역에 OCR 수행하여 성능 확인



## OCR 모델 개선
- (1) 이미지 훈련 데이터 늘리기 - GAN
- (2) 이미지 훈련 데이터 늘리기 - 폰트별 이미지 데이터 생성
- (3) 적절한 전처리



## Android 앱
- (1) 방법1📌 : 촬영 - 이미지 서버 전송 - 서버에서 전처리 및 모델링 수행 - 디바이스 전송 - 출력
- (2) 방법2 : 촬영 - 안드로이드 상에서 이미지 전처리 및 모델링 수행 - 출력
- 예상 이슈 : 속도, 자원의 효율적 이용 필요. 알고리즘 공부.
