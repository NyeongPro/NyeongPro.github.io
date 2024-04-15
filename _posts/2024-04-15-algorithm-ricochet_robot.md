---
layout: single
title:  "[Algorithm]프로그래머스 리코쳇 로봇 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm-ricochet_robot
---

# [리코쳇 로봇] 문제
## 코드
```python
from collections import deque

# 1) 다음 방문할 곳 가능한지
def next_loc_check(m, n, board):
    M = (len(board))
    N = len(board[0]) 
    if 0 <= m < M and 0 <= n < N and board[m][n] != 'D':
        return True
    return False
def solution(board):
    answer = 0
    M = (len(board))
    N = len(board[0])
    
    visited = [[-1 for _ in range(N)] for _ in range(M)]
    
    # 2) 상하좌우 이동
    m_dir = [-1, 1, 0, 0]
    n_dir = [0, 0, -1, 1]    
    
    # 3) R 위치 확인
    for m in range(M):
        for n in range(N):
            if board[m][n] == 'R':
                start = (m, n)
                visited[m][n] = 0
                break
    
    q = deque()
    q.append(start)
    
    
    # 4) BFS를 통해 상하좌우로 확장
    while q:
        m, n = q.popleft()
        
        for i in range(4):
            check = False
            dm = m_dir[i] + m
            dn = n_dir[i] + n
            
            while True:
                # 이동 후에 갈 수 있는 곳인지 확인.
                if next_loc_check(dm, dn, board):
                    check = True
                    dm = dm + m_dir[i]
                    dn = dn + n_dir[i]
                    continue
                break
            
            # 이동하기 전으로 되돌림.
            dm = dm - m_dir[i]
            dn = dn - n_dir[i]
            
            # 도착했는지 확인.
            if board[dm][dn] == 'G':
                return visited[m][n] + 1
            
            # 도착 지점이 아니므로 방문 가능한 곳인지 확인.
            if check and visited[dm][dn] == -1:
                visited[dm][dn] = visited[m][n] + 1
                q.append((dm, dn))
            
    
    return -1
```
## 풀이

### 1) 다음 방문할 곳 가능한지

상하좌우로 한 칸 이동할 시 그 위치가 이동 가능한 곳인지 확인.

게임판을 넘어서면 안되고, D 위치에 이동 불가능함.

### 2) 상하좌우 이동

m은 행 이동과 관련됨, 즉 상하.  
n 은 열 이동과 관련됨, 즉 좌우  

### 3) R 위치 확인

R 위치가 고정되었다는 사항이 없기 때문에 게임판을 탐색하며 위치 확인.

그 위치를 start에 할당함.

그리고 visited에 초기값 설정함.

다른 문제에서는 visited를 0으로 초기화했었음.

이때는 처음 위치에서 시작할 때부터 한 칸으로 시작했기 때문에 초기값을 1로 설정할 수 있지만,
이 문제는 처음 위치가 0으로 시작하기 때문에 visited를 -1로 초기화했음.

그리고 처음 시작점을 0으로 초기화하였음.

### 4) BFS를 통해 상하좌우로 확장

deque를 활용해 상하좌우 방향으로 갈 수 있는 곳을 점점 확장함.

그리고 상하좌우 한 방향이라도 이동했다면, 그 방향으로 막힐 때까지 쭉 이동해야하기 때문에
한 번 이동했다가 그 방향으로 이동이 가능했다면, while 구문에서 갈 수 있을 때까지 계속 이동함.

그리고 한 칸 이동시켜보고 그 곳이 갈 수 있는지 아닌지 확인하기때문에,
while 구문을 빠져나왔다면 이동 전으로 되돌리기 위해 -값으로 되돌렸음.

아래에서 도착했는지 확인함.

도착했는지 확인하는 구문을 통과했다는 것은 도착하지 못했으므로, 이미 방문한 곳이 아니라면 다음 이동장소로 갈 수 있도록 append 해줌.

모든 q를 돌았음에도 도착지점에서 return이 되지 않았으면 도착할 수 없는 환경이므로, return -1을 해줌.
