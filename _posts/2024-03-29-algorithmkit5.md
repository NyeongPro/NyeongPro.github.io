---
layout: single
title:  "[Algorithm]프로그래머스 코딩테스트 고득점 KIT 여러 문제5[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit5
---

# 1. [더 맵게] 문제
## 코드
```python
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    
    while True:
        if scoville[0] >= K:
            return answer
        
        mix = heapq.heappop(scoville) + heapq.heappop(scoville) * 2
        heapq.heappush(scoville, mix)
        answer += 1
        
        if len(scoville) == 1:
            return -1 if scoville[0] < K else answer
```
## 풀이

heap 유형이라 쓰여져 있고 효율성을 따질 때 매 loop 마다 sort를 쓰면 시간초과가 날 것이라 예상했지만
heap 유형에 대해 잘 몰라서 일단 생각나는대로 풀어보았습니다.  

sort(reverse=True)로 내림차순으로 정렬하고 pop()을 두 번 써서 제일 작은 수 두개를 뽑고
섞어준뒤에 다시 append 하였습니다. append할 때마다 answer를 1씩 추가해주었습니다.
그리고 한 번 섞었으면 다시 loop를 돌면서 내림차순으로 sort를 진행하였습니다.

역시 정확성에서는 통과하지만 효율성에서는 통과하지 못했습니다.

따라서 heap에 대해 배우고 pop과 append 하는 부분을 heappop(), heappush로 대체하였습니다.

그리고 효율성까지도 통과하였습니다.


## 추가

### heap에 대하여

완전 이진 트리의 일종으로 **우선순위 큐**를 위하여 만들어진 자료구조.  
여러 개의 값들 중에서 최대값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.  

힙은 일종의 반정렬 상태(느슨한 정렬 상태)를 유지한다.
* 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다.
* 부모 노드의 키 값이 자식 노드의 키 값보다 항상 크거나 작은 이진 트리이다.

그리고 힙에서는 중복된 값을 허용한다.  

### heap 사용법

import heapq를 통해 사용한다.
* heappush : heappush(heap, value), value를 heap에 넣는다.
* heappop : heappop(heap), heap에서 가장 작은 값을 반환, 비어 있는 경우 IndexError 호출
heap[0]은 최소값 호출이다.
* heapify : heapify(list), 기존 리스트를 힙으로 변환한다.

### Max heap 사용법, 역순으로 정렬

heap을 그대로 사용하면 Min heap으로 적용된다. 따라서 heappop을 사용하면 가장 작은 값을 반환하게 된다.
따라서 최대값을 사용하는 문제에서 역순으로 정렬할 필요가 있다.  

힙에 tuple을 원소로 추가하거나 삭제할 때, tuple 내에서 맨 앞에 있는 원소를 기준으로 최소 힙이 구성되는 원리를 이용한다.  
즉 (1, val1)과 (2, val2)라는 튜플 두 개가 heap에 들어가면 val1, val2와는 상관없이 1, 2라는 숫자 두 개에 대해서 최소 최대를 보고 heap이 min heap으로 정렬된다.
따라서 1이 원소로 들어가 있는 (1, val1)이 heap[0]에 들어가있을 것이다.  이를 이용하면 Max heap을 만들 수 있다.
만드는 법은 아래와 같다.

```python
for num in nums:
    heapq.heappush(heap, (-num, num))  # (우선 순위, 값)
```
이렇게 하면 num이 클수록 -num은 작아지므로 num이 최댓값이면 -num은 자동으로 최소값이 된다.
그렇다면 위에 예시처럼 앞의 요소가 최소값을 기준으로 heap이 정렬되므로 num이 최대면 -num이 최소값이 됨에 따라 맨 앞에 정렬되게 된다.

이렇게 max heap을 만들 수 있다.

그리고 주의할 점으로 우선 순위 값(-num)은 관심이 없으므로 num 값만을 불러올 수 있도록 heappop을 할 때 아래와 같이 사용한다.  
**print(heappop(heap)[1])**  
리스트에서 [[1, val1], [2, val2]]와 같이 되어 있을 때 val1, val2에 대해 궁금하면 list[0] 또는 list[1]을 가져온 뒤에 또 그 안에 요소의 인덱스를 다시 찾는 2차원 배열과 같은 원리이다.
{: .notice--success}


# 2. [체육복] 문제
## 코드와 풀이

### 실패 코드 1
```python
def solution(n, lost, reserve):
    answer = n - len(lost)
    reserve.sort()
    
    while reserve:
        res = reserve.pop()
        if res in lost:
            lost.remove(res)
            answer +=1 
        elif res+1 in lost:
            lost.remove(res+1)
            answer += 1
        elif res-1 in lost:
            lost.remove(res-1)
            answer += 1
            
    return answer
```
reserve에 값이 있는 동안에 loop를 돌린다.
그리고 정렬을 해두었기 때문에 pop() 하면 제일 큰 값부터 차례대로 뽑는다.
그리고 먼저 본인 체육복이 있는 경우 본인이 사용해야하므로 if 절에서 처리하고 
나머지 부분은 본인보다 숫자가 1 큰 사람부터 차례로 빌려준다.  

여기서 실패사항은 아래 테스트 케이스에서 드러난다.

n > 5
lost > [1, 2, 3]
reserve > [2, 3, 4]
기댓값 > 4

이 테스트 케이스에서 loop를 돌면 4번 학생이 3번 학생에게 빌려주게 된다.  
하지만 3번 학생은 본인 여분 체육복이 있으므로 본인것을 사용해야한다.  
따라서 4번은 결국 아무도 빌려주지 않게 되고 체육 수업을 들을 수 있는 사람은 4가 되므로 기댓값은 4가 된다.  

loop 안에서 if 절로 먼저 본인 체육복을 입도록 설정하면 당연히 될 것이라 생각했다.  
하지만 본인보다 숫자가 하나 큰 사람이 빌려주게 되는 예외 상황을 해결할 수는 없었다.

만약 이 경우를 체크하지 못한다면 제출했을 때 테스트 5, 12번을 통과못하게 되는 경우일 것이다.
참고하길 바람.


### 실패 코드 2
```python
def solution(n, lost, reserve):
    answer = n - len(lost)
    reserve.sort()
    
    for res in reserve:
        if res in lost:
            lost.remove(res)
            reserve.remove(res)
            answer += 1
    
    while reserve:
        res = reserve.pop()
        if res+1 in lost:
            lost.remove(res+1)
            answer += 1
        elif res-1 in lost:
            lost.remove(res-1)
            answer += 1
            
    return answer
```
이 경우는 테스트 1, 7, 24에서 실패했다.  
여러 찾아본 결과 for 루프에서 reserve 에 대해 요소를 뽑아내고 있는 상황에서
reserve.remove(res) 등으로 리스트 자체를 변경한다면 for 루프에 이상이 생길 확률이 대단히 높다고 한다.
따라서 for 루프 내에서 for 루프에 대한 조건을 변경하는 것은 자제해야겠다.  

### 성공 코드
```python
def solution(n, lost, reserve):
    answer = n - len(lost)
    reserve.sort()

    equalList = []
    for res in reserve:
        if res in lost:
            equalList.append(res)
    answer += len(equalList) 
    
    for rem in equalList:
        reserve.remove(rem)
        lost.remove(rem)
    
    while reserve:
        res = reserve.pop()
        if res+1 in lost:
            lost.remove(res+1)
            answer += 1
        elif res-1 in lost:
            lost.remove(res-1)
            answer += 1
            
    return answer
```
첫 번째 실패 코드에서 먼저 배운 것들 토대로 본인 체육복을 본인이 입는 경우만 생각해서 모두 제거해준다.
그리고 두 번째 실패 코드에서 배운 것을 토대로 for 루프안에서 수정하는 상황을 없애기 위해 제거해야할 번호를 먼저 append로 추가하고 나중에 제거 해주었다.

## 추가

테스트 5, 12에 대해서 오래 고민한 것 같다. 이론상으로 문제가 없다고 생각했지만
번호가 큰 사람의 if, elif, elif를 먼저 돌기 때문에 이 경우에서 본인의 여분 체육복을 입지 못했다.  
한 사람마다 if, elif 등 먼저 돌기 때문에 생기는 문제에 대해서 잘 인지하고 있어야겠다.  

결국 본인 체육복은 본인만 입어야한다 와 같은 예외 상황에 대해 먼저 해결할 수 있도록 코드를 짜야겠다.
{: .notice--success}

# 3. [정수 삼각형] 문제
## 코드
```python
def solution(triangle):
    
    for i in range(len(triangle) - 1):
        result = []
        for j in range(i+1):
            result.append(triangle[i][j] + triangle[i+1][j])
            result.append(triangle[i][j] + triangle[i+1][j+1])
        triangle[i+1][0] = result[0]
        triangle[i+1][-1] = result[-1]
        for k in range(1, int((i+1)*2)//2):
            triangle[i+1][k] = max(result[k*2-1], result[k*2])

    return max(triangle[-1])
```
## 풀이
문제가 원하는 동적계획법으로 푼지는 모르겠습니다.
그냥 한 칸씩 더해가면서 그 중에서 제일 큰 값인지 덮어쓰면서 진행했습니다.
인덱스를 만들기가 쉽지는 않았지만 아래와 같이 주석을 넣어가면서 인덱스를 구성했습니다.

```
"""
i = 0

tri[0][0] + tri[1][0] / tri[0][0] + tri[1][1]
두 개를 저장

i = 1
tri[1][0] + tri[2][0] (j = 0)/ tri[1][0] + tri[2][1] (j = 0)
tri[1][1] + tri[2][1] / tri[1][1] + tri[2][2] (j = 1)
값은 4개지만 결과는 3개만 보여야함.
즉 3번째 라인 8 1 0 에서 1로 가는 길 두 개중에 최댓값 하나만 골라야함.

i = 2
tri[2][0] + tri[3][0] / tri[2][0] + tri[3][1]
tri[2][1] + tri[3][1] / tri[2][1] + tri[3][2]
tri[2][2] + tri[3][2] / tri[2][2] + tri[3][3]

"""
```
인덱스를 숫자로 구성해보니 for loop 두 개에서 i, j로 코드를 짜면 될 거라는 생각을 해서 구현했습니다.
그리고 이렇게 구성하면 모두 갈 수 있는 경우에 대해서 전부 계산하게 됩니다.  

예를 들어 문제 예시 삼각형 세 번째 라인인 8 1 0에 가는 경우의 수는 4개입니다.  

7 -> 3 -> 8  
7 -> 3 -> 1  
7 -> 8 -> 1  
7 -> 8 -> 0  

특히 두 번째 세 번째 경우는 같은 목적지로 가기 때문에 저 둘 중 큰 값을 넣어주어야 최종적으로 간 경우에서 최댓값을 구할 수 있습니다.
여기서 또한 주석에 인덱스를 구성해서 아래와 같이 몇 번 경우를 묶어서 max값을 취할지 정했습니다.
```
'''
idx 기준 0, max(1, 2), max(3, 4), 5
idx 기준 0, max(1, 2), max(3, 4), max(5, 6), max(7, 8), max(9, 10), 11
'''
```
따라서 첫 번째 경우와 마지막 경우는 길이 하나뿐이므로 그대로 triangle 리스트에 경로의 숫자 최대값을 덮어쓰기했고
중간 부분에서 둘 중 큰 값을 정해줘야하는 부분은 다시 for loop를 돌려 해결했습니다.

이렇게 for loop를 많이 써서 당연히 효율성 테스트에서 실패할 줄 알았는데
효율성도 모두 통과했네요...

## 추가

다른 블로그를 참조해 동적계획법에 대한 풀이를 공부했습니다.

### 동적계획법(Dynamic Programming)

메모리 공간을 더 사용해서 연산 속도를 비약적으로 증가시킨다.  
한 번 계산한 문제에 대해 다시 계산하지 않도록 하기 때문이다.  

우선 다음과 같은 2가지 조건을 만족할 때 동적계획법을 사용할 수 있다.

* 큰 문제를 작은 문제로 나눌 수 있다.  
* 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다.  

위 조건을 만족하는 대표적인 문제가 피보나치 수열문제이다.  
피보나치는 이전 값들을 더해서 만들어진다.  
n차항 = n-1차항 + n-2차항으로 이루어진다.  
따라서 n차항보다 큰 차항, 큰 문제를 해결할 때 n차항, 작은 문제가 포함된다.  
그리고 그러한 정답들이 전혀 바뀌지 않는다.  

n차항까지 구해놨다면 그 이전 차항은 구하지 않아도 된다. 이미 한 번 계산돼서 변하지 않기 때문이다.  


# 4. [가장 먼 노드] 문제
## 코드
```python
from collections import deque

def solution(n, edge):
    answer = 0
    edges = [[] for _ in range(n+1)]
    
    for e in edge:  # [a, b] 형태 간선을 i-노드에 대한 정보로 리스트화
        aNode, bNode = e
        edges[aNode].append(bNode)
        edges[bNode].append(aNode)    
    
    queue = deque()
    distance = [0 for _ in range(n+1)]
    queue.append([1])
    distance[1] = 1
    
    while queue:
        connectPath = queue.popleft()  # 3번 노드의 경우 [1, 2, 4, 6] 연결되어 있어 1, 2, 4, 6으로 갈 예정
        for nowNode in connectPath:  # 이번에 방문해서 다른 곳 가기 위해 대기.
            possibleNode = edges[nowNode]  # 첫 루프에서 1번 노드에 있으므로 [2, 3] 갈 수 있음.
            tmp = []
            for node in possibleNode:
                if distance[node] == 0:  # 방문하지 않은 경우
                    distance[node] = 1 + distance[nowNode]
                    tmp.append(node)
            queue.append(tmp)

        
    maxVal = max(distance)
    
    
    return distance.count(maxVal)
```
## 풀이
많이 해맨 문제입니다.
기존에 시도할 때 BFS풀이로 풀어야겠다는 생각이 바로 들었습니다.
그래서 queue에 모든 edges를 다 넣고 1에서 시작해서 다음 칸에 갈 수 있는 조건문을 걸었습니다.
큐에서 popleft로 한 edge를 빼고 현재 노드에서 갈 수 있는지 확인했습니다. 그리고 못 가는 edge를 뽑았다면 다시
append해서 큐의 뒤쪽으로 보냈습니다.  

왜냐하면 처음에 pop 해서 이미 큐에서 사라졌지만 이번 조건에서 사용하지 못했기 때문에 다음 번에 또 pop 해서 확인하려고 했기 때문입니다.
그렇게 큐에서 popleft하고 조건에 안맞으면 append 해서 큐의 전형적인 선입선출 느낌으로 loop를 돌렸습니다.  
조건문이 많아져서 그런지 몇몇 케이스는 실패했고 시간초과가 많이 돼서 저 방식으로 하지는 못했습니다.  

따라서 다른 블로그를 좀 참고했습니다.

기존 edge는 [[1, 2], [2, 3], ... ,] 와 같이 서로 연결된 간선을 무작위로 보여주기 때문에 활용하기 힘듭니다.  
그렇기에 제가 처음 시도했던 방법이 계속 삽입하고 빼내고 빼낸 것을 썼으면 그대로 pop되고 못썼으면 다시 append 한 것입니다.
안 쓴 요소는 계속해서 쓸 수 있기 때문입니다.  

하지만 블로그를 참조하니 아래와 같이 리스트에 각 간선의 정보를 리스트화 할 수 있었습니다.

```python
for e in edge:  # [a, b] 형태 간선을 i-노드에 대한 정보로 리스트화
        aNode, bNode = e
        edges[aNode].append(bNode)
        edges[bNode].append(aNode)
```

위와 같이 하면 edges의 인덱스로 접근하면 몇 번 노드에 몇 번 간선이 연결되어 있는지 바로 확인할 수 있었습니다.  
이 포인트를 체크하고 아래 부분은 BFS 접근법으로 스스로 생각해서 풀었습니다.  

내가 갈 수 있는 노드를 지속적으로 append 한다고 했을 때 내가 갈 수 있는 노드들을 popleft로 빼냅니다.
그리고 그것을 connectPath라고 이름 지었습니다. 연결된 모든 길들에서 in 연산을 통해 하나씩 빼냅니다.
하나를 빼냈다면 그 노드에서 연결될 수 있는 모든 노드들을 한 칸씩 이동하고 distance를 +1 씩 할 겁니다.

그래서 nowNode(현재 위치한 노드)에서 for loop를 통해 한 칸씩 이동합니다.
이동할 때는 그 노드를 들렀는지 확인하고 안들렀다면(distance == 0)이라면 접근하고 distance를 +1 합니다.  
0이 아니라는 것은 +1이 됐다는 것이고 적어도 한 번 들른 것입니다.  

그리고 내가 들렀던 곳은 이후에도 더 연결된 곳이 있을 수 있으므로 tmp에 append 한 뒤
다시 큐에 삽입해서 지속적으로 distance를 늘리는 작업을 진행합니다. 

# 참고 사이트

[[프로그래머스] - 정수 삼각형(파이썬)](https://velog.io/@ssulee0206/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%A0%95%EC%88%98-%EC%82%BC%EA%B0%81%ED%98%95%ED%8C%8C%EC%9D%B4%EC%8D%AC)  

[계속 5,12번 안되시는 분들](https://school.programmers.co.kr/questions/50407)  

[[Python] Dynamic Programming(동적계획법) 알고리즘](https://techblog-history-younghunjo1.tistory.com/183)

