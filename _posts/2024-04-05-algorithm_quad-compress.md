---
layout: single
title:  "[Algorithm]프로그래머스 쿼드압축 후 개수 세기 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm_quad_compress
---

# [쿼드압축 후 개수 세기] 문제
## 코드
```python
# 1) 한 영역 S를 4개의 영역으로 나누기
def quadDivide(area):
    n = len(area)
    s1, s2, s3, s4 = [], [], [], []

    # 2) 4개 영역 일반화 하기
    half_n = n//2
    for i in range(half_n):
        s1.append(area[i][:half_n])
        s2.append(area[i][half_n:])
        s3.append(area[half_n+i][:half_n])
        s4.append(area[half_n+i][half_n:])
    return s1, s2, s3, s4


def DFS(area, answer):
    n = len(area)
    # print("area = {} / n = {}".format(area, n))
    # 3) 한 칸이면 0 또는 1 올리고 종료
    if n == 1:
        answer[area[0][0]] += 1
        # print("here1, area[0][0] = ", area[0][0])
        return
    
    # 4) 한 칸이 아니라면 4개 영역으로 나눌지 말지 확인하기
    else:
        cnt_zero = 0
        cnt_one = 0
        # 영역에서 0 또는 1 개수 확인하기.
        for arr in area:
            cnt_zero += arr.count(0)
            cnt_one += arr.count(1)
        # 모두 0인지 확인하기
        if cnt_zero == n * n:
            answer[0] += 1
            return
        # 모두 1인지 확인하기.
        elif cnt_one == n * n:
            answer[1] += 1
            return
        
        # 5) 모두 0, 모두 1이 아닌 경우
        s1, s2, s3, s4 = quadDivide(area)
        DFS(s1, answer)
        DFS(s2, answer)
        DFS(s3, answer)
        DFS(s4, answer)

def solution(arr):
    answer = [0, 0]

    DFS(arr, answer)
    return answer
```
## 풀이
### 고찰
DFS로 풀어야겠다고 생각이 들었습니다. 영역을 4개로 나누고 
계속해서 파고들어서 개수를 센다고 생각했거든요.  

그래서 아래와 같이 먼저 흐름을 적었습니다.
```
if 지금 접근한 영역이 한 칸이니? 
  False(한 칸이 아니다!) -> 
      if 현 영역에서 n * n 개만큼 0이 있니? 또는 1이 있니?
        True -> answer에 0 또는 1이 하나 있었다고 기록하기 -> 그 영역 다시는 안보기.
        
        False -> 그 영역 쿼드로 만들기.
            다시 4개의 영역을 S1~S4로 두고 같은 거 반복하기.    
  True(한 칸이다!) ->  그 영역은 그냥 한 칸이라 끝. -> 0/1 인지 보고 카운트 추가.
```

코드에서 위 흐름을 순서대로 풀이하겠습니다.

### 1) 한 영역 S를 4개의 영역으로 나누기
큰 영역 S를 4개의 영역으로 나누는 것을 함수로 구현했습니다.

먼저 나눈 영역에 s1부터 s4로 이름지었습니다.  

![img.png](/images/2024-04-05/quad_area_img.png)

같은 행에서 반으로 나누어서 두 개에 영역에 나누어 넣어야합니다.  

예시로 문제 첫 번째 예시 4 * 4 arr를 보면 첫 행이 1 1 0 0 으로 이루어져있습니다.

여기서 왼쪽 절반 1 1 은 S1에 들어가고 오른쪽 절반 0 0 은 S2에 들어갑니다.

두 번째 예시 8 * 8을 한 번 보면 아래와 같이 첫 4개의 영역이 나누어 집니다.

```
s1 = [0][0:4] / [1][0:4] / [2][0:4] / [3][0:4]
s2 = [0][4:] / [1][4:] / [2][4:] / [3][4:]
s3 = [4][0:4] / [5]0:4] / [6][0:4] / [7][0:4]
s4 = [4][4:] / [5][4:] / [6][4:] / [7][4:]
```

### 2) 4개 영역 일반화 하기
위를 토대로 4개 영역 slice를 일반화 하였습니다.

n의 절반값을 활용해야하기 때문에 미리 선언해두었습니다.

그리고 아래에서 이 4개의 영역을 return 해주었습니다.

### 3) 한 칸이면 0 또는 1 올리고 종료
현재 접근한 영역이 한 칸이라면 4개 영역으로 나눌 필요없이 바로 0 또는 1을 판별하고
answer에 1을 증가하면 됩니다.

그리고 return하여 DFS를 종료합니다.

### 4) 한 칸이 아니라면 4개 영역으로 나눌지 말지 확인하기
한 칸이 아니라면 이 영역이 모두 1인지 또는 모두 0인지 확인해야합니다.

만약 2 * 2 영역에서 0이 4개가 나왔다면 이 영역은 모두 0이므로 0 하나만 증가시키면 되겠죠.

만약 반대로 1이 4개가 나왔다면 이 영역은 모두 1이므로 1을 하나만 증가시키면 됩니다.

그리고 이 영역에 대해서 나눌 필요도 없이 바로 종료하면 되겠죠.

### 5) 모두 0, 모두 1이 아닌 경우
위에서 return으로 종료되지 않았다면 4개의 영역을 나눠서 다시 접근해야합니다.

따라서 위에서 만든 quadDivide 함수로 접근하여 영역을 4개로 구분합니다.

그리고 다시 DFS를 적용해서 모두 0 또는 모두 1 또는 한 칸인 경우까지 계속해서
깊이를 증가시켜 0과 1의 개수를 찾아냅니다.

