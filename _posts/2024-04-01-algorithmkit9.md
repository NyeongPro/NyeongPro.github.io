---
layout: single
title:  "[Algorithm]프로그래머스 안전지대 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit9

---

# [안전지대] 문제
## 코드
```python
def solution(board):
    answer = 0
    n = len(board)
    dangerArea = [[0 for _ in range(n)] for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            if board[i][j] == 1:
                for k in range(-1, 2):
                    for w in range(-1, 2):
                        if i+k < 0 or j+w < 0:
                            continue
                        try:
                            if dangerArea[i+k][j+w] == 0:
                                dangerArea[i+k][j+w] = 'x'
                        except:
                            pass
    for i in range(n):
        answer += dangerArea[i].count('x')

    return n*n - answer
```
## 풀이
만약 board의 i, j 가 1이라면 i-1~i+1과 j-1~j+1 부분을 전부 지뢰가 있는 것으로 표시하고
지뢰가 있는 곳들을 세주면 되는 문제입니다.  

다만 0, 0과 같이 구석진 곳이나 board의 가장자리 부분은 인덱스를 -1이나 +1을 더해주면
인덱스 오류가 나기 때문에 조건을 걸어주어야합니다.

그런데 각 모서리나 상하좌우 가장자리 부분 모두 조건식이 다르기 때문에 귀찮아서
인덱스에러를 try-except 구문으로 퉁쳐서 작성했습니다.

> try 안에 오류가 날 수도 있는 구문을 넣습니다.  
> except에 오류가 났을 때 처리할 구문을 넣어줍니다.  
> 저는 pass만 넣었으므로 아무일도 일어나지 않았습니다.


## 추가

### 다른 풀이
```python
def solution(board):
    answer = 0
    n = len(board)
    dx = [-1, 1, 0, 0, -1, -1, 1, 1] # 8방향 탐색
    dy = [0, 0, -1, 1, -1, 1, -1, 1]
    
    # 지뢰 설치
    boom = []
    for i in range(len(board)):
        for j in range(len(board)):
            if board[i][j] == 1:
                boom.append((i,j)) # 지뢰일때의 인덱스 append
                
    # 지뢰가 설치된 곳 주변에 폭탄 설치
    for x, y in boom:
        for i in range(8):
            nx = x + dx[i]
            ny = y + dy[i]
            if 0 <= nx < n and 0 <= ny < n:
                board[nx][ny] = 1

    # 폭탄이 설치되지 않은 곳만 카운팅
    for i in range(n):
        answer += board[i].count(1)

    return n*n - answer
```
이 풀이는 try-except를 쓰지 않고도 풀었습니다.  
애초에 if문으로 에러 상황을 안나오도록 하는 것이 더 좋은 것 같아서 참고해봤습니다.  

먼저 8방향 탐색에 대한 리스트를 미리 만들어둡니다.
x와 y의 변화량에 대해 할당해줍니다.

그리고 boom이라는 리스트에 지뢰가 설치된 인덱스를 미리 append 해두었네요.   

>저도 제 코드를 짤 때 이전 지뢰에 의해 1로 바뀐 영향 때문에 if 절을 추가해서 조건을 걸어두었는데, 
>만약 미리 지뢰에 대한 인덱스를 얻어둔 리스트가 있다면 이를 신경쓰지 않아도 되므로 더 좋은 코드 같습니다.
>다만, 미리 리스트에 대해 저장 공간을 사용한다는 점, 그리고 board의 길이 n만큼 n^2만큼 시간복잡도가 필요하므로
>시간이 중요한 문제에서는 잘 생각해야할듯 합니다.

boom 리스트에서 지뢰에 대한 인덱스를 for in 구문으로 지속적으로 받아옵니다.
그리고 x + dx[i] 와 같이 이동할 인덱스를 지정합니다.  

이때 이 이동한 인덱스 nx가 0보다 크거나 같고, n보다 작거나 같아야합니다.
이렇게 인덱스를 활용하니 인덱스가 벗어난 범위를 쉽게 파악할 수 있네요.  

만약 이 인덱스를 if 절로 만들었다면, x가 벗어날 때와 안 벗어날 때,
y가 벗어날 때, 안 벗어날 때와 같이 매우 복잡하게 조건을 달았어야 했지만 nx, ny를 활용해서
그 점을 막은게 좋은 코드인 것 같습니다.

그리고 board에서 위험지역을 1로 바꾸었으니 1인 지점을 전부 카운트해서
안전 지역의 위치를 계산했습니다.  

# 참고 사이트
[[프로그래머스] 안전지대(Lv.0) - Python](https://wise-99.tistory.com/19)