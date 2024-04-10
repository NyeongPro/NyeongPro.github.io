---
layout: single
title:  "[Algorithm]프로그래머스 배달 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm-delivery
---

# [배달] 문제
## 코드
```python
import heapq
import sys
def solution(N, road, K):
    answer = 0
    
    # 간선 정보를 담을 roads 리스트 초기화
    roads = [[] for _ in range(N+1)]

    # 1) 가장 작은 cost를 가진 길 탐색
    road = sorted(road, key=lambda x: (x[0], x[1], -x[2]))
    for i in range(len(road)-1):
        if road[i][0:2] == road[i+1][0:2]:
            continue
        else:
            a = road[i][0]
            b = road[i][1]
            c = road[i][2]
            
            roads[a].append([b, c])
            roads[b].append([a, c])
    a, b, c = road[-1]
    roads[a].append([b, c])
    roads[b].append([a, c])
    
    # 2) dijkstra 알고리즘
    def dijkstra(start):
        h = []
        
        # 무한대 값 설정
        INF = sys.maxsize
        
        # distance 리스트 모두 무한대로 초기화
        distance = [INF for _ in range(N+1)]
        
        # 시작점 초기화
        distance[start] = 0
        heapq.heappush(h, (0, start))
        
        # heap이 빌 때까지 루프
        while h:
            
            # heap에서 현재 node에 번호와 이 node에 대한 거리값 받아오기.
            dist, node = heapq.heappop(h)
            
            if distance[node] < dist:
                continue
            for b, c in roads[node]:
                if distance[b] > dist + c:
                    distance[b] = dist + c
                    heapq.heappush(h, (dist+c, b))
            
        return distance
    D = dijkstra(1)
    for i in range(1, N+1):
        if D[i] <= K:
            answer += 1 
    return answer
```
## 풀이
### 1) 가장 작은 cost를 가진 길 탐색

문제에서 같은 노드 사이에 여러 길이 주어질 수 있다고 했다.

따라서 이 여러 길 중에서 비용이 낮은 하나의 길만 선택하도록 했다.

road의 첫 번째, 두 번째 인덱스를 정렬해서 앞 뒤에 같은 인덱스가 위치하도록 하였다.
그리고 세 번째 인덱스는 거꾸로 정렬해서 같은 a, b를 가진 길 사이에서 맨 마지막 인덱스에 위치한 길이
비용이 제일 최소가 되도록 하였다.

### 2) dijkstra 알고리즘

distance 리스트를 활용해서 지속적으로 heap에 push하고 distance가 최소가 되도록
dijkstra 알고리즘을 사용하였다.

이렇게 구해진 최소값으로만 이루어진 distance 리스트를 반환하였다.

D = dijkstra(1) 코드를 통해 1번에서 출발해서 모든 노드들에 대해 비용이 최소가 되도록 하였다.

그리고 K보다 작은 비용을 가졌을 때 1씩 증가시켜 문제를 제출한다.
