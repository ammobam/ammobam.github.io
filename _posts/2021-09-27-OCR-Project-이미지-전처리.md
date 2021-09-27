---
title:  "[OCR Project] OCR을 위한 전처리 수행"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: False
toc_sticky: False
toc_label: "주요 목차"
 
date: 2021-09-27
last_modified_at: 2021-09-27

use_math: true
comments: true

categories:
  - OCR Project
---



[관련 소스코드(Click)📌](https://github.com/ammobam/OCR_ElectricityMeter_imgprep/blob/main/prep_OCR.py)



## OCR로 읽어낼 데이터 영역 추출을 위한 전처리 과정

### 요약
- GrayScaling >> Binary >>  Morphology operation >> Contouring
- 데이터 영역의 문자가 배경과 구분되도록 이미지를 흑백으로 읽어와서 이진화함
- 모폴로지 연산을 통해 문자 영역의 픽셀이 서로 뭉쳐지도록 함 
- 문자 뭉치의 외곽선을 추출함



### 상세 내용

1. GrayScaling
	- GrayScale 이미지는 단일 채널이기 때문에 이미지의 이진화 수행이 가능함

2. Binary
	- 데이터 영역에서 그림자 등 노이즈의 영향을 덜 받기 위해 **적응형 이진화**를 수행함
	- 일반적인 이진화는 전체 이미지에 대해 하나의 경계값을 설정하여 수행함
	- 적응형 이진화는 일정한 사이즈의 작은 블록(blockSize) 단위로 경계값을 설정하여 이진화를 수행하기 때문에, 이미지 일부에 드리워진 그림자 처리 등에 강함
	- 이때 전체 이미지에 대해 비슷한 수준의 이진화 수행을 위해 작은 블록 사이즈는 이미지의 크기에 비례하여 설정되도록 처리함


```python
    # 파라미터
    # maxValue : 이진화 결과 영상의 최댓값
	max_val = 255
    # 임계값 조정을 위한 상수
    C = 16
    # blockSize : 이미지 임계값을 설정할 단위 블록. 3이상의 홀수여야 함
    bin = 10
    if (src.shape[1] // bin) % 2 == 0:
    	block_size = src.shape[1] // bin + 1
    else:
    	block_size = src.shape[1] // bin
    
    # 적응형 이진화 수행
    cv2.adaptiveThreshold(src, max_val, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, block_size, C)
```

3. Morphology Operation
	- 문자 영역에 대해 Contouring을 수행하기 위해 모폴로지 연산을 통해 문자 영역이 한 덩어리로 뭉치도록 처리함
	- 모폴로지 기본 연산은 침식(erode)과 팽창(dilate)이 있음
	- openCV는 팽창 수행 결과에서 침식 수행 결과를 제외하여 외곽선을 검출하는cv2.MORPH_GRADIENT 연산을 지원함
	- 반복 수행해본 결과 가로 방향으로 침식을 더 강하게 하는 것이 문자를 적절히 뭉치게 하여 직접 팽창과 침식을 수행한 값의 차를 구해 외곽선을 검출함
	- 외곽선에 대해 다시 한 번 가로 축에 대해 강한 closing 연산을 수행하여 객체 내부에 작은 구멍을 메움


```python
    # MORPH_DILATE
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (3, 3))
    src_dilate = cv2.morphologyEx(src_binary, cv2.MORPH_DILATE, kernel)

    # MORPH_ERODE
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 3))
    src_erode = cv2.morphologyEx(src_binary, cv2.MORPH_ERODE, kernel)

    # MORPH_GRADIENT : dilate - erode. Get outline.
    src_gradient = src_dilate - src_erode

    # MORPH_CLOSE : dilate -> erode. Lump pixels.
    c_kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (7, 1))
    src_close = cv2.morphologyEx(src_gradient, cv2.MORPH_CLOSE, c_kernel)
```



4. Contouring
	- 지금까지의 전처리를 통해 뭉쳐진 문자 영역의 외곽선을 검출함
	- cv2.findContours 함수는 외곽선 점들의 좌표와 계층 정보를 리턴함
	- 문자 영역의 단순한 외곽선을 검출하기 위해 mode는 cv2.RETR_LIST, method는 cv2.CHAIN_APPROX_TC89_L1를 이용함
        - mode : 외곽선을 검출하는 방식
        - method : 검출된 외곽선 점들의 좌표를 근사화하는 방법
		- 문자 영역을 추출하는데 계층 정보는 필요하지 않았음
	- cv2.drawContours 함수를 이용하여 외곽선 좌표를 변환하여 이미지로 출력함 

```
	# contouring
    contours, _ = cv2.findContours(src, mode=cv2.RETR_LIST, method=cv2.CHAIN_APPROX_TC89_L1)

	# draw contour
	src_contour = cv2.drawContours(src, contours, contourIdx=-1, color=(0, 0, 255), thickness=2)
```


## 프로젝트 수행 중 느낀 점🐥
### Class의 필요성
- 이전 프로젝트는 셀 단위로 실행되는 Colab, Jupyter Notebook을 이용하여 class 단위의 필요성을 못 느꼈음
- PyCharm을 이용하면서 프로젝트 수행 단계별로 class를 만들어 이용하니 중복된 코드를 피할 수 있고, 코드 리뷰, 코드 공유가 쉬워짐
- 이전에 만든 class를 불러와 쓸 때 다시 읽어보면서 코드 최적화와 예외 처리에 좀더 신경 쓸 수 있음
- 팀원과 코드를 공유하면서 좀더 직관적인 class, mehod 이름을 붙이려 노력함
- 「Clean Code」를 읽어볼 예정임

### 이미지 처리와 노하우
- 모폴로지 연산과 컨투어링의 파라미터를 조정함에 따라 결과물이 크게 달라짐
- 이미지 처리는 적절한 파라미터 값을 설정하는 것이 매우 중요하여 노하우가 필요한 영역임을 실감함
- 앞으로도 openCV를 비롯한 이미지 라이브러리를 공부하면서 이미지 연산에 대해 배우고 싶은데, 대부분의 이미지 처리 라이브러리는 C++ 언어로 수행됨
- 추후 C++을 공부해보려 함

## 앞으로 할 일📌
- OCR 돌려보고, 성능이 많이 떨어지는 경우 투시 변환할 예정임
- 액정 부분에 꼭 맞는 다각형을 그리고, 액정 ROI 좌표에 대해 투시변환 할 것임

