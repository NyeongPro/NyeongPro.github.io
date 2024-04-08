---
layout: single
title:  "[Algorithm]프로그래머스 이중우선순위큐 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm_double-priority-queue
---

# [이중우선순위큐] 문제
## 코드
```python
from collections import deque
import heapq
def solution(operations):
    answer = deque()
    cnt = 0
    
    
    for oper in operations:
        # 1) I 일 때
        if oper[0] == 'I':
            answer.append(int(oper[2:]))
            answer = deque(sorted(answer))
            cnt += 1
        # 2) D 일 때
        else:
            if cnt == 0:
                continue
            # 최소값 제거(정렬 후 첫 번째 인덱스)
            elif oper == 'D -1':
                answer.popleft()
                cnt -= 1
            # 최대값 제거(정렬 후 마지막 번째 인덱스)
            elif oper == 'D 1':
                answer.pop()
                cnt -= 1
    

    return [0, 0] if cnt == 0 else [answer.pop(), answer.popleft()]
```
## 풀이

### 1) I 일 때
answer에 append 해줍니다.  

그리고 deque는 sort 정렬이 안되기 때문에 deque(sorted(answer))와 같이
리스트에서 sort 해주고 다시 deque 화 시킵니다.

그리고 I 를 한 번 했다는 뜻으로 cnt를 증가시킵니다.

### 2) D 일 때
만약 cnt가 0이라면, I와 D를 같은 수만큼 했기 때문이고, 이 상태에서는 D를 해주지 않습니다.

그리고 cnt가 0이 아니라면 D 할 수 있기 때문에 최솟값, 최댓값 제거에 따라 제거해줍니다.

일반 리스트에서 pop(0)와 같이 맨 왼쪽 인덱스를 pop하면 시간이 많이 걸리기 때문에 deque를 사용했습니다.


## 추가

최대값, 최소값에만 관심이 있어서 당연스래 heap으로 풀어야겠다고 생각했습니다.

파이썬은 minHeap을 자동으로 제공하고, maxHeap을 사용하려면 value를 -를 취하고 넣어주면 됩니다.

따라서 max와 min에 둘 다 접근해야하므로 두 개의 heap을 사용해서 각각 최소값, 최대값을 제거하는 쪽으로
진행했습니다.

하지만 계속해서 테스트1에서 통과를 못하더라구요.  

어쩔 수 없이 리스트에서 정렬하고 맨 왼쪽, 맨 오른쪽을 빼내면 각각, 최소값, 최대값을 제거하는 쪽으로 진행했습니다.

그리고 맨 왼쪽 인덱스를 pop(0)으로 제거하면 시간이 오래걸리므로 deque를 사용했구요.

추후에 heap으로 푸는 방법을 찾아보려했는데, 대부분 minHeap에서는 heap을 사용하지만 max 값을 구할 때는 정렬을 하더라구요...

또는 remove를 사용해서 값을 제거했습니다. 정렬 후 pop이나 remove 와 같은 것은 인덱스를 중간에 비우게 되므로, 

리스트를 다시 인덱스에 맞춰서 땡겨오는 작업을 하므로 시간복잡도가 올라가는 문제가 생겨
모든 부분을 heap으로 푸는 풀이를 찾아보았지만 찾지 못했습니다.

그래서 제가 제출한 답으로 문제를 끝냈습니다.

