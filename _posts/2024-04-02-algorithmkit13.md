---
layout: single
title:  "[Algorithm]프로그래머스 삼각 달팽이 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit13
---

# [삼각 달팽이] 문제
## 코드
```python
def solution(n):
    answer = []
    
    
    triMap = [[0 for _ in range(i)] for i in range(1,  n+1)]
    totalNum = 0
    for i in range(1, n+1):
        totalNum += i
    
    # 초기값
    triMap[0][0] = 1  
    i, j = 0, 0
    cnt = 1
    while cnt != totalNum:
        # 왼쪽 아래 진행
        while i < n-1:
            if triMap[i+1][j] != 0:  # 다음 갈 곳을 방문했으면 종료
                break

            i += 1
            triMap[i][j] = triMap[i-1][j] + 1  # 방문할 곳 = 방문전 값 + 1
            cnt += 1
        # 오른쪽 진행
        while j < n-1:
            if triMap[i][j+1] != 0:
                break

            j += 1
            triMap[i][j] = triMap[i][j-1] + 1
            cnt += 1
        # 왼쪽 위 진행
        while i>=0:
            if triMap[i-1][j-1] != 0:
                break
            i -= 1
            j -= 1
            triMap[i][j] = triMap[i+1][j+1] + 1
            cnt += 1
        
    for level in triMap:
        for x in level:
            answer.append(x)
    
    return answer

```
## 풀이
### 1) 초기값 설정
* triMap에서 각 층마다 0으로 초기화 합니다.
n=4 일 때 출력하면 아래와 같습니다.
```
[[0], [0, 0], [0, 0, 0], [0, 0, 0, 0]]
```
* totalNum은 모든 작업을 마치고 while loop를 빠져나오기 위해 설정했습니다.
n=4 기준 각 층별로 1, 2, 3, 4 칸이 있고 모든 칸 수는 1+2+3+4 입니다.
n=5 기준은 1+2+3+4+5 이므로 이 총 합을 totalNum에 저장했습니다.

* triMap 첫 층 첫 칸 1로 초기화
* 이후 while loop에서 사용할 i, j 0으로 초기화
* cnt는 한 칸 채울 때마다 1씩 증가시키고 전체 칸과 비교

### 2) 전체 루프 시작
왼쪽 아래 방향, 오른쪽 방향, 왼쪽 위 방향 3가지로 나눔.
삼각형의 껍질부터 채우고 안쪽 한 겹을 채우고... 한 겹을 채우고...
라고 생각하면 한 겹을 채우는 루프를 시작함.

n=4일 때는 1, 2, 3, 4, 5, 6, 7 ,8, 9 가 한 겹임.
그리고 10이 안쪽 한 겹임.

### 3) 전체 루프의 동작을 하나씩 구분
n=4 기준으로 설명하면,  
1, 2, 3, 4 는 왼쪽 아래 방향 채우기  
(4), 5, 6, 7은 오른쪽 방향 채우기
(7), 8, 9는 왼쪽 위 방향 채우기

괄호로 표시한 숫자는 이미 전 단계에서 채워져서 이후 채워지지는 않지만
편의상 적었음.  

while 안의 구문은 그렇게 어렵지는 않음.  
인덱스를 주의하면서 원하는 방향으로 진행하며 전 단계에서 +1 을 해주었음.
그리고 범위를 벗어나거나, 이미 방문한 칸에 접근하면 break로 중지시키고 다음 단계로 진행하도록 하였음.

### 4) 각 층으로 나눠진 리스트 정리
[[1], [2, 9], ...] 와 같이 표현된 triMap 리스트를 문제가 요구하는 형태로 변경하기 위한 for 루프.

