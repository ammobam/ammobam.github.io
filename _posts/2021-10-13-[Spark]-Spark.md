---
title:  "[Spark] Spark"
excerpt: "본 게시글은 K-Digital Training 수업을 바탕으로 작성하였습니다."

toc: True
toc_sticky: True
toc_label: "주요 목차"

date: 2021-10-13
last_modified_at: 2021-10-13

use_math: true
comments: true

categories:
  - ETC
---

## 🚀 Spark 특징

- **In Memory 기반 데이터 처리 방식**
  - cf) Hadoop은 파일 시스템 기반 데이터 처리 방식
- 다양한 언어 지원
  - cf) Hadoop은 Java만 지원
- Hadoop보다 처리 속도가 빠르고, 코드를 간결하게 작성할 수 있음
- 온라인 작업 처리
  - OLAP : OnLine Analytical Processiong (온라인 분석 작업, 일반적으로 읽기 전용)
  - OLTP : OnLine Transaction Processing (온라인 처리 작업, 일반적으로 수정 전용)
- Hadoop이나 Spark는 작업의 일관성이나 순서를 보장하지 못함
  - 실시간 작업 처리에는 부적합함

## 🚀 Spark가 제공하는 자료형

### 📌 RDD

- **데이터와, 데이터를 다루는 부분을 함께 제공함**

  - C언어에서 자료형과, 데이터를 다루는 부분을 별도로 제공함

  - Java의 기본형도 별도로 제공했었음

  - Java는 Wrapper 클래스로 자료형과 다루는 부분을 같이 제공함

  - 예시

    - ```10.7 >> 2``` 실수를 shift하면 오류 발생함

    - method로 제공하면 처리할 수 없는 자료형을 입력하는 실수가 발생할 수 있음

    - 하지만 Class로 만들어서 제공하면 입력 자료형을 제한하기 용이함
    - OOP(객체 지향 프로그래밍)을 하는 이유

- **원본 데이터를 수정하지 않고 복사해서 사용함**
  - 복사하여 사용할 때 작업 내용을 보관하고 있음
  - 중간에 장애가 발생하면 스스로 에러를 복구할 수 있음
  - cf) Database의 Transaction - commit, rollback, savepoint

- RDD는 작업을 2가지로 나눠서 수행함
  - **Transformation / Action** 🌱
  - Transformation 작업은 여러 번 수행할 수 있음
  - Action은 1번만 수행할 수 있음
  - 변환 작업을 여러 개 묶어서 수행하는 것을 **파이프라인 처리**라고 함
  - 지연실행방식 이용함
    - Transformation / Action 작업을 순차적으로 수행하는 코드를 작성하면, <u>실제로 Transformation은 Action 직전에 수행됨</u>
    - 파이프라인 처리는 대부분 지연실행방식으로 동작함
    - Transformation을 미리 수행하면 비효율적이기 때문임
    - 지연 실행을 이용하면 모든 Transformation 작업을 확인하고 최적화한 수 Action을 수행할 수 있음
  - (사례) 여러 대의 서버에 저장된 로그 파일로 특정 상품의 판매 건수를 계산하는 경우
    - 처리 방법은 2가지
    - (방법1) 모든 컴퓨터의 데이터를 모아서 특정 상품을 추출, 판매 건수 계산
    - (방법2) 각 컴퓨터에서 특정 상품을 추출, 판매 건수를 계산한 후, 컴퓨터별 데이터를 합하여 계산
    - 필터(조건).합계()
    - 필터(조건).정렬()
  - cf) int와 integer를 따로 만들어둔 이유?🌱



### 📌 데이터프레임, 데이터셋

- RDD의 확장



## 🚀 Spark 설치

### 1. Java, Scala 설치

​	```sudo apt install scala```

​	```scala -version```

### 2. Spark 설치

- Spark 다운로드

- 압축해제 및 필요한 디렉토리로 이동

- bashrc 파일에 압축 해제한 디렉토리 등록

  ```bash
  nano ~/.bashrc # 배치파일 접근
  ...
  export SPARK_HOME=~/spark	# ~/spark : 복사된 디렉토리
  export PATH=SPARK:$SPARK_HOME/bin	# 경로 설정
  export PATH=SPARK:$SPARK_HOME/sbin
  export PYSPARK_PYTHON=/usr/bin/python3	# 파이썬 쓰기 전 등록
  ```

- bashrc 파일에 변경 내용 적용

  ```source ~/.bashrc```

### 3. Python 파일을 생성하여 spark 연동

- pyspark 라이브러리 설치

  ```bash
  pip install pyspark
  ~$ cd spark
  ~/spark$
  ```

  

- python 파일을 만들기
  ```bash
  # 파일 확인
  ~$ cd spark
  ~/spark$ nano wordcount.py
  ```

- python 파일 작성
  ```python
  from operator import add
  from pyspark import SparkContect, SparkConf
  import sys
  
  class WordCount:
      def getSparkContext(self, appName, master):
          conf = SparkConf().setAppName(appName).setMaster(master)
          return SparkContext(conf=conf)
      def getInputRDD(self, sc, input):
          return sc.tectFile(input)
      def process(self, inputRDD):
          words = inputRDD.flatMap(lambda s : s.split(" "))
          wcPair = words.map(lambda s: (s,1))
          return wcPair.reducwByKey(add)
  
  if __name__=="__main__":
      wc = WordCount()
      sc = wc.getSparkContext("WordCount", sys.argv[1])
      inputRDD = wc.getInputRDD(Sc, sys.argv[2])
      resultRDD = wc.process(inputRDD)
      resultRDD.saveAsTextFile(sys.argv[3])
      sc.stop()
  ```


- 파일이 저장된 디렉토리에서 spark의 bin/spark-submit 명령으로 실행

  ```bash
  bin/spark-submit 소스코드파일경로 입력파일경로 출력파일경로
  				# sys.argv[1]  # sys.argv[2]  # sys.argv[3]
  ```
  
- 출력 파일은 디렉토리에 head part-0000 부터 일련번호 형태로 생성함

- 파일 정상 실행 시, ```ls```로 확인했을 때 testresult가 만들어져야 함

- 확인

  ```head testresult/part-00000```

  ```~/spark/testresult$ nano part-00000```

  

### 4. Python 셸에서 실행

- Spark 설치 디렉토리에서 수행할 것

- bin/pyspark를 실행해서 python 셸로 진입

- 코드 작성

  ```python
  inputRDD = sc.textFile("README.md")
  words = inputRDD.flatMap(lambda str : str.split(" "))
  wcPair = words.map(lambda s: (s,1))
  resultRDD = wcPair.reduceByKet(lambda x, y:x + y)
  resultRDD.saveAsTextFile("testresult")	# 이전에 저장된 적 없는 이름이면 오류남
  ```
  

- 종료 : [Control + D]

- 결과 확인

  - Spark 디렉토리로 이동해서 수행

  ```bash
  head testresult/part-00000
  nano testresult/part-00000
  ```



### 5. Spark 머신러닝을 위한 ml lib, ml 패키지

- mllib은 초창기 등장한 패키지. Spark 3.0에서 제거될 예정임.
- ml 패키지를 이용할 것













