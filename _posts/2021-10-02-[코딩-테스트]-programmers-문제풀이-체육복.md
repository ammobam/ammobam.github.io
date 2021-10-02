---
title:  "[코딩 테스트] programmers 문제풀이 체육복"
excerpt: "본 게시글은 programmers 문제풀이입니다."

toc: true
toc_sticky: true
toc_label: "주요 목차"
 
date: 2021-10-02
last_modified_at: 2021-10-02

use_math: true
comments: true

categories:
  - Coding Interview
---

## 문제
[코딩테스트 연습 > 탐욕법(Greedy) > 체육복(click)](https://programmers.co.kr/learn/courses/30/lessons/42862?language=python3)



## 풀이

### 아이디어

- 도난과 여벌 준비의 결과, 각 학생이 갖고 있는 체육복 수를 리스트로 만듦
- 옷이 0벌인 학생은 자신의 앞 번호 학생이 2벌 가졌을 때 빌림
- 앞 번호 학생의 옷이 없을 때, 뒷 번호 학생에게 빌림

### 주의할 점

- **1번 학생의 경우 앞 번호가 없음. n번 학생의 경우 뒷 번호가 없음.**

- 학생 번호가 1번부터 n번까지 있을 때, 리스트 인덱스는 0번부터 n-1번까지 있음
- 1번 학생은 리스트에서 인덱스 0번에 해당하고, 1번 학생의 앞 번호 학생은 인덱스 -1번이 됨
- 리스트는 인덱스 -1을 가지면 맨 끝값을 조회하므로 처리가 필요함
- 학생 수 + 1 길이의 리스트를 만들고, 체육복을 빌려줄 수 없는 유령학생으로 설정함



### 소스코드

```python
# 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있음
# 전체 학생의 수 n,
# 체육복을 도난당한 학생들의 번호가 담긴 배열 lost,
# 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때,
# 체육수업을 들을 수 있는 학생의 최댓값을 return

def solution(n: int, lost, reserve):
    
    # 문제 조건 예외 처리
    if n < 2 or n > 30 or len(lost) < 1 or len(lost) > n or len(reserve) < 1 or len(reserve) > n:
        pass
        # print("입력값의 범위가 잘못되었습니다.")

    else:
        # 도난당한 경우 0, 아닌 경우 1인 리스트 생성
        donan = [1] * n + [0]
        for i in lost:
            donan[i-1] = 0
            
        # 여벌 옷이 없는 경우 0, 있는 경우 1인 리스트 생성
        yeobeol = [0] * n + [0]
        for i in reserve:
            yeobeol[i-1] = 1

        # 현재 각 학생이 가진 체육복 수 리스트 생성
        clothes = [donan[i] + yeobeol[i] for i in range(n+1)]


        for i in range(0, n):
            # 도난당한 학생이 여벌 옷도 없는 경우
            if clothes[i] == 0:
                # 앞번호 친구가 가진 옷이 2벌이면 빌림
                if (clothes[i-1] == 2):
                    clothes[i-1] = 1
                    clothes[i] = 1
                # 앞번호 친구에게 빌리지 못한 경우, 뒷번호 친구가 가진 옷이 2벌이면 빌림
                elif (clothes[i+1]==2):
                    clothes[i+1] = 1
                    clothes[i] = 1

        # print(clothes)
    # 서로 빌려준 결과, 
    # 각각의 학생이 가진 옷이 0벌인 경우는 0, 0벌이 아닌 경우는 1의 값을 갖도록 하고
    # 그 값을 모두 더하여 반환함
    return sum(1 if x!=0 else 0 for x in clothes)
```



## 결과

| 테스트 1 〉  | 통과 (0.01ms, 10.3MB) |
| ------------ | --------------------- |
| 테스트 2 〉  | 통과 (0.02ms, 10.3MB) |
| 테스트 3 〉  | 통과 (0.02ms, 10.3MB) |
| 테스트 4 〉  | 통과 (0.01ms, 10.3MB) |
| 테스트 5 〉  | 통과 (0.01ms, 10.3MB) |
| 테스트 6 〉  | 통과 (0.01ms, 10.3MB) |
| 테스트 7 〉  | 통과 (0.02ms, 10.3MB) |
| 테스트 8 〉  | 통과 (0.02ms, 10.3MB) |
| 테스트 9 〉  | 통과 (0.01ms, 10.3MB) |
| 테스트 10 〉 | 통과 (0.02ms, 10.3MB) |
| 테스트 11 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 12 〉 | 통과 (0.01ms, 10.2MB) |
| 테스트 13 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 14 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 15 〉 | 통과 (0.01ms, 10.2MB) |
| 테스트 16 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 17 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 18 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 19 〉 | 통과 (0.01ms, 10.3MB) |
| 테스트 20 〉 | 통과 (0.01ms, 10.3MB) |