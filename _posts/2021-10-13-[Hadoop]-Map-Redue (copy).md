---
title:  "[Hadoop] Map-Reduce"
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


## 1. 데이터의 모임에 가장 많이 수행되는 작업

1) Map - 데이터 변환
2) Filter - 조건에 맞는 데이터 추출
3) Reduce - 집계


## 2. Hadoop
- HDFS와 Map Reduce의 결합
- HDFS : Hadoop Distributed File Systerm, 데이터를 나눠 저장

### 📌 Hadoop의 등장 배경
- 과거에는 데이터를 한 곳에 저장했음
- 데이터가 늘어나는 경우 동적인 확장/백업의 문제가 발생함
- 데이터를 복제(RAID)하거나 나누어서 저장함
- 이때 나눠서 저장한 데이터를 Join해서 사용할 때 시간이 너무 오래 걸림
- 분산 처리의 필요성이 대두됨
- 이때 UNIX 운영체제 등장함



### 📌 Map-Reduxe Programming

- 데이터가 존재하는 곳에서 연산을 수행한 후(Map)
- **결과를 합치는(Reduce)** 형태의 프로그래밍
- 시간이 오래 걸리는 Join 작업을 하지 않기 때문에 작업 시간을 대폭 줄일 수 있음

#### 🌿 Map - Reduce는 정렬에 맞지 않음

- Map-Reduce가 부적합한 작업은 데이터 전체를 가지고 수행해야 하는 작업 - Sort🌱
- Map-Reduce의 형태로 정렬하는 방법 - Merge Sort🌱

#### 🌿 Edge Computing

- **데이터가 발생한 곳에서 처리**
- 대표적인 기술 : Tensorflow Lite, Tensorflow.js
- 이 방식으로 처리하는 이유 : **속도, 보안**
- 개인 데이터를 서버에 주지 않고 처리 가능함



### 📌 Hadoop의 약점과 Spark

- Hadoop은 Map 작업 후 결과를 **파일**에 저장함
- 파일에 저장하지 않고 **메모리에서(in - Memory)**  작업할 수 있도록 만들어진 Echo System - Spark🌱
- Hadoop은 Java로만 작업이 가능하지만 Spark는 Java, Python, R, Scala 등의 언어로도 작업이 가능함
- 초창기 Spark는 Scala를 이용하여 사용하였음
  - Scala : 함수형 프로그래밍이 가능한 JVM 기반 언어



## 3. Java와 다른 언어의 실행 상 차이점

- 이때 Java는 .Net 계열 포함 - C# 등

### 📌 C, Python 

- include, import를 이용해서 라이브러리 추가

- 라이브러리를 현재 메모리 공간으로 가져오는 개념

- 이런 언어는 <u>실행파일</u>을 만들어서 실행함

- 실행파일을 만들면 다른 것은 필요하지 않음

- 실행파일을 만들 때 배치 파일 만듦 - **bat(윈도우), sh 파일(리눅스. sh는 배치 명령어로 실행)🌱**

- 또는 배치파일을 별도의 파일로 만들어서 한번에 실행되게 배포하기도 함 -** exe, dmg 파일🌱**

- 서비스 만들 예정이면 bat, sh, exe, dmg 등 알아두자

- Windows : DLL(dynamic linking library) - 다른 

  

### 📌 Java, C#

- import, using의 개념은 **라이브러리에 링크를 설정**하는 개념
- 현재 실행 중인 메모리 공간에 라이브러리가 포함되지 않음
- 기본 라이브러리가 아닌 것은 같이 배포하고,
- 기본 라이브러리는 java의 경우 jre(JVM), C#의 경우 .net framework에서 빌려서 사용함



### 📌 JavaScript

- JavaScript : 웹 브라우저가 해석하고 웹 브라우저 안에서만 동작



## 4. Hadoop 설치하기🌱



### 📌 Unix / Linux / Mac에서 확장자가 sh인 파일

- Shell Programming 파일 - 명령어의 집합으로 만들어진 텍스트 파일
- 실행을 할 때는 bash 파일경로의 형태로 함
- Mac에서 Tomcat 같은 WAS 연동을 할 때도 sh 파일을 선택하면 됨



### 📌 현재 디렉토리에 있는 파일을 실행할 때

- ./ 파일명의 형태로 입력할 것



### 📌 Hadoop 설치 및 확인

#### 1. 서버 만들기

- Hadoop은 외부에서 접속하는 프로그램이라서 ssh 설정을 해야 함

- ssh : secure shell

  - 원격지 호스트 컴퓨터에 접속하기 위해 사용하는 인터넷 프로토콜 서버가 되기 위해서는 <u>방화벽에서 ssh가 제외되어야 함</u>
  - 클라우드 접속할 때 ssh로 주로 접속함

- FireWall (방화벽) : 외부에서 내부 컴퓨터에 접근할 때 거치도록 하는 SW / HW

- Proxy : 내부에서 외부 컴퓨터에 접근할 때 거치도록 하는 SW / HW

- 중견 기업 이상은 FireWall과 Proxy가 다 설정되어 있음

- 내 컴퓨터가 ssh 서버가 될 수 있도록 설정

  ```sudo apt-get install openssh-server``` (프로그램 설치)

  ```sudo service ssh start``` (서버 실행)

  

- 본인 컴퓨터 IP 확인

  ```ifconfig``` (리눅스에서 IP 확인)

  - 위 명령이 제대로 동작하지 않는 경우 net-tools 설치

    ```sudo apt install net-tools``` (net-tools 설치)

  - 다른 컴퓨터에 ssh로 접속 - 터미널에서 수행 🌱 (클라우드)

    ```ssh 다른컴퓨터ip주소 -I 계정```

#### 2. ssh-client 설치

- 클라이언트 설치

  ```sudo apt install openssh-client -y```

- 클라이언트를 설치하는 이유?

  - Hadoop을 공부할 때는 대부분 여러 개의 컴퓨터로 실습함
  - 서로 접속이 가능해야 하여 client도 설치
  - 물리적인 컴퓨터 또는 virtual machine 여러 개 실행 중일 수 있음

#### 3. 현재 유저를 비밀번호 없이 ssh로 접속할 수 있도록 설정

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa (작은 따옴표 2개임)

cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

chmod 0600 ~/.ssh/authorized_keys # 파일의 허가모드 변경	# 정처기🌱
# 1: 디렉토리 여부, 2: 소유자, 3: 그룹 사용자, 4: 기타 사용자의 권한으로 8진수로 설정함
# rwx(read, writr, x) 권한을 순서대로 설정함
# chmod 0777 모든 허가

ssh local host
```



#### 4. Hadoop 권한 문제 발생 시

```bash
sudo apt-get install ssh
sudo apt-get install pdsh
```

- ```nano ~/.bashrc```를 실행시켜 bashrc 파일을 열고 맨 아래에 추가

- nano 대신 vi를 사용하는 경우도 있음

  ```export PDSH_RCMD_TYPE=ssh # 단축키 추가```

  ```nano ~/.bashrc```

#### 5. JDK 설치

#### 6. 인코딩 확인

- UTF-8의 인코딩만 확인 가능한 명령어

  ```echo $LANG```

#### 7. Hadoop 설치

1. 다운로드
   - http://www.apache/org/dyn/closer.cgi/hadoop/common
   - 🙄
2. 압축해제
3. 접속이 쉬운 위치로 옮기고 바로가기 링크 설정
4. 설정



---

Python 3.10 나오면 Switch 찾아보기

---

## 사례

- 신문사 3개 크롤링
- 이 경우, 3개 신문사에서 크롤링은 독자적 수행 가능
- 각 크롤링 데이터의 순서를 유지할 필요가 없다면, 
- 크롤링 하는 작업은 각각 스레드로 만들어서 작업을 수행하여 **효율적으로 크롤링 가능**

---

## 빅데이터?

- 저장
- 처리
- 분석





