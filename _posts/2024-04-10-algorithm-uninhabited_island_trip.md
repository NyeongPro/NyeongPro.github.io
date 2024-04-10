---
layout: single
title:  "[Algorithm]프로그래머스 무인도 여행 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmuninhabited_island_trip
---

# [무인도 여행] 문제
## 코드
```python
from collections import deque
def solution(maps):
    answer = []
    mapList = []
    
    # 1) masp 2차원 리스트로 만들기
    for row in maps:
        tmp = []
        for x in row:
            if x.isdigit():
                tmp.append(int(x))
            else:
                tmp.append(x)
        mapList.append(tmp)
    
    # 행과 열 길이
    row = len(mapList)
    col = len(mapList[0])
    
    # 인덱스 기준으로 상하좌우 이동하기.
    drow = [-1, 1, 0, 0]
    dcol = [0, 0, -1, 1]
    
    
    for i in range(row):
        for j in range(col):
            
            # 2) BFS로 진입하기.
            if mapList[i][j] != 'X':
                
                # 한 무인도에 대해 result와 queue 초기화하기
                result = 0
                queue = deque()
                queue.append((i, j))
                while queue:
                    m, n = queue.popleft()
                    
                    # 3) 'X' 일 때 지나가기
                    if mapList[m][n] == 'X':
                        continue
                    result += mapList[m][n]
                    mapList[m][n] = 'X'
                    
                    # 4) 상하좌우 살피고, 갈 수 있는 곳 queue에 다시 append
                    for k in range(4):
                        dm = m + drow[k]
                        dn = n + dcol[k]
                        
                        if 0 <= dm < row and 0 <= dn < col and mapList[dm][dn] != 'X':
                            queue.append((dm, dn))
                answer.append(result)
                
    return sorted(answer) if answer else [-1]
```
## 풀이
### 1) masp 2차원 리스트로 만들기
각 요소별로 인덱스로 접근하기 편하도록 미리 2차원 리스트로 구성

### 2) BFS로 진입하기
deque를 사용해서 근처 상하좌우에 이동할 수 있는지 보면서 지속적으로 옆에 붙어있는 땅을
넓혀감.

### 3) 'X' 일 때 지나가기
이를 놓쳐서 많이 해맸다.

아래에서 보면 if절로 mapList[m][n] != 'X'일 때만 queue에 append 하므로,
queue에 있는 mapList[m][n]이 'X'가 될 일이 없다고 생각했다.

하지만 입출력 예 #1 에서 큰 무인도 오른쪽 아랫부분 531 부분에서 오류가 발생하는 것을 확인했다.

이는 queue에 5(인덱스로 1, 3)과 3(인덱스로 2, 2)가 동시에 append 되어있다.

이 상황에 상하좌우 순으로 queue에 들어가므로 3에 먼저 진입하게 되는데,
3에서 1(인덱스로 2, 3)에 접근해서 1을 'X'로 표시하며 방문 표시를 하게된다.

이때 5에서 다시금 기존 1위치에 진입한다. 하지만 이미 3에서 접근했으므로 1은 'X'가 되있는데,
이 'X'를 result에 더하게되는 오류를 범하게 된다.

따라서 queue내에서도 방문을 확인하는 절차를 꼭 거쳐야겠다.

### 4) 상하좌우 살피고, 갈 수 있는 곳 queue에 다시 append
for 루프로 상하좌우를 전부 살핀다.

그리고 이 움직인 곳의 인덱스가 갈 수 있는 곳이면서, 'X'가 아닌 곳을 선정해서
queue에 append한다.

