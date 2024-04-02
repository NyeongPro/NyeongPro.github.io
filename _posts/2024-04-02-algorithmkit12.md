---
layout: single
title:  "[Algorithm]프로그래머스 프로세스 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit12
---

# [프로세스] 문제
## 코드
```python
from collections import deque

def solution(priorities, location):
    answer = 0
    # 1) 디큐 선언, (idx, 우선순위) 큐에 삽입.
    queue = deque()
    for priority in enumerate(priorities):
        queue.append(priority)
    
    # print(queue)
    while queue:
        # 2) loc과 priority
        loc, priority = queue.popleft()
        # print(loc, priority)
        check = False
        # 3) 큐 내에 우선순위 확인.
        for i in range(len(queue)):
            if priority < queue[i][1]:
                check = True
                break
                
        # 4) 우선순위가 높은 프로세스가 존재하는 경우와 아닌 경우
        if check:  # 우선순위 높은 프로세스가 존재한다.
            queue.append((loc, priority))
        else:
            answer += 1
            if loc == location:
                return answer
            
    
    return answer
```
## 풀이
### 1) # 1) 디큐 선언, (idx, 우선순위) 큐에 삽입.
실행 대기 큐를 구현하기 위해 deque를 선언했습니다.  

그리고 enumerate 함수를 활용해서 priority를 받음과 
동시에 그때의 인덱스 또한 받은 (idx, priority) 형태의 튜플을
queue 에 삽입했습니다.  

### 2) # 2) loc과 priority
location과 비교할 인덱스를 loc에 받았고, 우선순위도 비교해야하므로 
priority에 받았습니다. 그리고 deque이기 때문에 왼쪽부터 popleft를 사용해서 뽑아내고
큐에 삽입할 때는 append를 아래에서 사용합니다.

### 3) 큐 내에 우선순위 확인.
for 루프를 통해 queue의 길이만큼 loop합니다. 이 루프에서는 현재 프로세스가 가지고 있는 우선순위(priority)보다
큰 우선순위를 가지고 있는 프로세스가 있는지 확인합니다.
만약 현재 우선순위보다 큰 우선순위를 가지고 있는 프로세스가 있다면 check를 True로 변경하고 break하여
남은 프로세스를 확인하는 것을 멈춥니다. 하나라도 우선순위가 큰 프로세스가 있다면 다른 프로세스는 확인할 것도 없이
현재 프로세스를 다시 큐에 넣어줘야하기 때문입니다.

### 4) 우선순위가 높은 프로세스가 존재하는 경우와 아닌 경우
if문을 통해 check가 True였는지 False였는지 확인합니다.
위에서 True로 변경한 상황은 우선순위가 높은 프로세스가 존재했을 때입니다.

그러므로 check가 True였다면 현재 프로세스를 큐에 append하고 다음을 기약해야합니다.

그리고 만약 check가 False였다면 현재 프로세스는 우선순위가 가장 큰 경우, 또는 같은 우선순위가 존재하는 경우로
프로세스를 실행하면 되는 상태입니다.

따라서 실행됐기 때문에 한 번 실행됐다는 의미로 answer += 1을 통해 실행 횟수를 1회 증가시킵니다.
증가한 뒤 현재 프로세스의 loc(처음 존재했던 리스트의 Index)와 location이 같은지 확인하고
같다면 문제에서 요구한 프로세스가 몇 번째 실행됐는지를 바로 return 해주면 됩니다.

## 추가

### 다른 풀이1
```python
from collections import deque

def solution(priorities, location):
    answer = 0
    # 1) 디큐 선언, (idx, 우선순위) 큐에 삽입.
    queue = deque()
    for priority in enumerate(priorities):
        queue.append(priority)
    
    # print(queue)
    while queue:
        # 2) loc과 priority
        loc, priority = queue.popleft()
        # print(loc, priority)
        # 3) 큐 내에 우선순위 확인.
        if any(priority < q[1] for q in queue):
            queue.append((loc, priority))
        else:
            answer += 1
            if loc == location:
                return answer
            
    return answer
```
다른 풀이의 일부를 참고하여 내 코드에 적용했습니다.
고친 부분은 3) 큐 내의 우선순위 확인 부분입니다.

저는 True, False를 저장할 check를 선언하고 for loop를 통해 큐 내의 우선순위를 확인하면서
현재 프로세스보다 우선순위가 큰 프로세스가 있는지 확인하였습니다.

여기서 any() 함수를 쓰면 코드가 좀 더 깔끔해집니다.
any() 함수는 함수안에 하나라도 True라면 True를 반환해줍니다.

따라서 queue 내에서 하나씩 받아온 요소를 q라고 하고 q[1](priority 부분)과
현재 프로세스의 우선순위 priority를 비교합니다.
그리고 하나라도 크다면 any() 함수의 특성에 의해 True를 반환하겠네요.

만약 모두 만족해야 True를 반환하고 싶다면 all()을 사용합니다.

> * any() 하나라도 True라면 True
> * all() 모두 True이면 True

# 참고 사이트
[[알고리즘] 프로그래머스 프로세스 파이썬](https://velog.io/@helenason/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%ED%8C%8C%EC%9D%B4%EC%8D%AC)  

