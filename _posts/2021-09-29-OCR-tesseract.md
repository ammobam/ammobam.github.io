---
title:  "[OCR Project] OCR tesseract 수행 및 개선사항"
excerpt: "이미지 전처리 및 OCR 모델 훈련 프로젝트 수행일지입니다."

toc: False
toc_sticky: False
toc_label: "주요 목차"

date: 2021-09-29
last_modified_at: 2021-09-29

use_math: true
comments: true

categories:
  - OCR Project
---

## 🚀OCR 모델 : tesseract

### 환경 준비
- Window version으로 설치
  [tesseract-ocr-w64-setup-v5.0.0-alpha.20210811.exe](https://github.com/UB-Mannheim/tesseract/wiki)

- OpenCV 또는 PIL 이용

- pip install pytesseract

- 한글 인식을 위해 학습된 한글데이터 파일 다운로드
  [kor.traineddata](https://github.com/tesseract-ocr/tessdata/blob/main/kor.traineddata)

- kor.traineddata 파일을 다음 경로로 복사
  C:\Program Files\Tesseract-OCR



### tesseract 샘플 소스코드

- (참고자료) https://github.com/madmaze/pytesseract

```
# PATH 설정
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract'

# config parameters
# -l : 사용할 언어 설정
# --oem 1 : LSTM OCR 엔진 사용 설정
config = ('-l kor+eng --oem 1 --psm 3')

# tesseract 수행
text = pytesseract.image_to_string(src_roi, config=config)
# text = pytesseract.image_to_string(src_roi, lang='kor')
```



### OCR 처리할 이미지 준비

- 배열을 PIL 이미지로 변환하는 방법
```python
	from PIL import Image
	# arr : 이미지 배열
	img = Image.fromarray(arr, 'RGB')
	img.show()
```




## 🚀[전처리 >> OCR 수행] 개선사항

### 1. 전처리 이미지에서 글자가 다소 깨짐

```src = x.resize_check(x.printGray())``` 

- 여기서 모든 이미지의 가로픽셀이 700으로 줄어들음.
- 이미지를 줄이지 않고 좌표에 원래 비율을 적용하자.



### 2. 기울어진 이미지

- 이미지 투시변환 및 회전 필요




## 질문

### PIL.Image 클래스가 반환하는 객체는 어떤 타입일까?

- OpenCV 로 대체할 수 있을까?

```
from PIL import Image

Image.open('..\data\ElectricityMeter\847207D64AF9_P1134.jpg')

# <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=1024x1024 at 0x25844C2A100>
```



### 이미지 라이브러리 비교 : PIL vs OpenCV

- (참고자료) https://www.kaggle.com/vfdev5/pil-vs-opencv
- (참고자료) https://chacha95.github.io/2019-08-01-PIL-vs-OpenCV/

- OpenCV는 numpy array를 이용함
- PIL은 Image array를 이용함. numpy array로 이용하려면 변환해야 함.
- 어떤 라이브러리를 사용하는 것이 좋을까? 속도, 자원 등 고려해보자.



