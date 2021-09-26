---
title:  "[OCR Project] ROI 자동 추출(2)"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: true
toc_sticky: true
toc_label: "주요 목차"
 
date: 2021-09-15
last_modified_at: 2021-09-15

use_math: true
comments: true

categories:
	- OCR Project
---

## ROI 추출 수행

- 계량기 이미지로부터 OCR을 수행할 영역을 추출하고자 함
- 계량기 액정을 기준 영역으로 정하고 좌표를 추출함
- 기준 영역의 좌표와 액정의 가로, 세로 높이를 비율로 이용하여 대략적인 OCR을 수행할 영역을 추출함
- 기울어진 사진들 때문에 영역을 조금 넓게 잡음
- 900여 장의 이미지에 대해 검토한 결과, 좌표와 비율은 다음이 적절함

```
# 액정 영역 좌표 : x1, y1, x2, y2

width = x2 - x1 	# 액정 가로길이
height = y2 - y1	# 액정 세로길이

# OCR 수행 영역 좌표
x1_ = int(x1 - 0.4*width)
x2_ = int(x2 + 0.4*width)
y1_ = int(y1 - 0.7*height)
y2_ = int(y2 + 3*height)
```



### ROI 추출 이미지 사례

![ROI추출(1)](..\assets\posting_img\ROI추출(1).JPG){: width="100" height="100"} 
![ROI추출(2)](..\assets\posting_img\ROI추출(2).JPG){: width="100" height="100"} 
![ROI추출(3)](..\assets\posting_img\ROI추출(3).JPG){: width="100" height="100"}



## 앞으로 수행할 것

- 이제 이 영역에서 OCR을 수행하면서 이미지 전처리 수행할 예정임
- 이미지 전처리는 이진화, 침식/팽창, 컨투어링, OCR 인식 순으로 해볼 것
- 왜곡이 심한 이미지에 대해서는 멘토링 받았던 것처럼 액정 영역을 다각형으로 추출한 다음, 투시변환을 수행할 예정

