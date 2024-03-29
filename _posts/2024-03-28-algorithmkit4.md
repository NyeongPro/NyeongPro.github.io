---
layout: single
title:  "[Algorithm]프로그래머스 코딩테스트 고득점 KIT 여러 문제4[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit4
---

# 1. [게임 맵 최단거리] 문제
## 코드
```python
result = []
def check(n, m, maps):
    
    # 맵을 벗어난 경우
    if n < 0 or m < 0 or n >= len(maps) or m >= len(maps[0]):
        return True
    # 벽에 간 경우
    if maps[n][m] == 0:
        return True
    
    return False

def dfs(n, m, maps, already_map, count):
    global result
    already_went_to_go = already_map
    # 맵을 벗어난 경우와 까만 벽에 간 경우 경로 이탈로 return
    if check(n, m, maps):
        return
    
    # 이미 왔던 곳 방문 시 return
    if already_went_to_go[n][m] == 1:
        return
    already_went_to_go[n][m] = 1  # 방문 표시

    
    # 목적지 도착
    if n == len(maps)-1 and m == len(maps[0])-1:
        result.append(count)
        return
        
    dfs(n, m+1, maps, already_went_to_go, count+1)  # 우로 이동
    dfs(n, m-1, maps, already_went_to_go, count+1)  # 좌로 이동 
    dfs(n+1, m, maps, already_went_to_go, count+1)  # 아래 이동
    dfs(n-1, m, maps, already_went_to_go, count+1)  # 위로 이동
    
def solution(maps):
    
    global result

    already_map = [[0 for _ in range(len(maps[0]))] for _ in range(len(maps))]  # 방문한 곳 영역 리스트 init
    
    dfs(0, 0, maps, already_map, 1)
    
    result.sort()
    if len(result) == 0:
        return -1
    else: 
        return result[0]
```
## 풀이
* check 함수를 통해 맵을 벗어나거나 까만 벽에 접근한 경우 return  
* dfs 풀이로 check로 먼저 확인 하고 방문한 곳이 전에 이미 왔던 곳이면 return  
* 목적지에 도착하면 그 동안 움직인 수 return  
* dfs로 상하좌우 다 움직이고 조건에 부합할 때까지 깊이 탐색하도록 했음.  

하지만 이미 갔던 곳에 대한 리스트를 하나의 [노드간의 이동] 에서만 사용토록 해야하는데 모든 탐색에서 공유해서 사용하는 것 같음...  

정확히는 모르겠지만 하나의 탐색이 끝나면 다른 탐색에 대한 정보가 돌아오지 않음.
예를 들어 문제에서 첫 번째 방법은 15칸 이동, 11칸 이동 두 가지 방법이 있음.
나의 코드는 하나의 경우가 해결되면 다른 칸 이동에 대한 정보가 오지 않음.
그래서 만약 dfs 순서를 위로 이동이 먼저 오게 바꾸면 위로 한 번 더 가는 15칸 방법이 결과값으로 뜨고
우로 이동하는 dfs를 제일 먼저 오게 두면 우로 먼저 가는 방법이 결과값으로 떠서 결과가 11칸 이동으로 뜨게됨...

각 dfs마다 이미 방문한 곳에 대한 정보를 초기화해야하는 것인지... 방법을 잘 모르겠음.


## 추가

### BFS 풀이 
블로그를 참조해 최단 거리 문제는 BFS 푸는게 효율적임을 알았다.  

```python
from collections import deque
def solution(maps):
    answer = 0
    
    # 인덱스로 따졌을 때 행렬의 상 하 좌 우 움직임.
    dn = [-1, 1, 0, 0]
    dm = [0, 0, -1, 1]
    
    queue = deque()
    queue.append((0, 0))
    row = len(maps)
    col = len(maps[0])
    
    # dn row 변화량, dm col 변화량
    while queue:
        n, m = queue.popleft()
        for i in range(4):
            N = dn[i] + n
            M = dm[i] + m
            if 0 <= N < row and 0 <= M < col and maps[N][M] != 0 and maps[N][M] == 1:
                maps[N][M] = maps[n][m] + 1
                queue.append((N, M))
                    
    return -1 if maps[row-1][col-1] == 1 else maps[row-1][col-1]
```
위의 코드로 해결했다.  

먼저 아래 코드를 통해 deque를 import 한다.
```python
from collections import deque
```

dequeue 는 기존 선입선출만 가능한 queue에서 양쪽에서 삽입, 삭제가 가능한 queue이다.
{: .notice--info}

다음으로 상하좌우를 움직이기 위한 dn, dm 리스트를 만든다.  
배열로 따졌을 때 [x-1][y] 가 위쪽 이동이다. 즉 한 칸 위에 행에 접근한다.  
하나만 더 살펴보면 [x][y+1]은 우측 이동으로, 열 기준 하나 증가하기 때문이다.  

queue = deque()를 통해 deque 를 선언한다.  

첫 위치는 0, 0이기 때문에 queue에 먼저 (0, 0)을 선언한다.  

여기서((0, 0)) 이 아닌 (0, 0) 과 같이 선언하면 풀이에 오류가 생긴다.  
큐에 삽입할 때 하나의 argument를 삽입해야하는데 2개를 삽입해서 생기는 오류이다.  
TypeError: append() takes exactly one argument (2 given)
{: .notice--danger}

* while을 통해 queue에 아무것도 존재하지 않을 때까지 반복한다.  
* n, m 변수에 queue에서 뽑아온 값을 할당한다.  
* 이는 현재 row, col에 대한 값이다.

이후 for loop를 4번 돌면서 상하좌우 4가지 중 갈 수 있는 곳을 선정한다.  

N, M은 각각 상하좌우 중 한 곳으로 이동했을 때의 row, col 값이다. 

이후 맵의 크기를 넘어갔는지, 까만 벽으로 진입했는지 확인한다.  
그리고 상하좌우 중 하나로 이동한 곳의 값이 1인지 확인한다.  
이는 아래에서 `maps[N][M] = maps[n][m] + 1`를 보면 알 수 있다.  
한 번 이동한 곳은 1을 더해주고 만약 1이 아니라면 이미 한 번 방문한 곳임을 알 수 있으므로 만약 방문하려는 곳이 1이 아니라면 접근하면 안된다.  

이후 이동한 곳을 다시 한 번 삽입한다.  
위에서 이 값을 다시 한 번 뽑아서 지속적으로 영역을 넓혀가는 것이다.  

마지막으로 골인 지점의 값이 1이라면 한 번도 방문하지 못한 것이므로 -1을 return하고
1 값이 아니라면 방문을 했다는 것이므로 그대로 return 하여 몇 칸만에 접근했는지 표현한다. 


# 2. [네트워크] 문제
## 코드
```python
def dfs(visited, comNum, n, computers):
    
    visited[comNum] = True
    for i in range(n):
        if comNum == i:
            continue
        elif computers[comNum][i] == 1:
            if visited[i] == False:
                dfs(visited, i, n, computers)
    return

def solution(n, computers):
    
    answer = 0
    visited = [False for _ in range(n)]
    
    for comNum in range(n):
        if visited[comNum] == False:
            dfs(visited, comNum, n, computers)
            answer += 1
            
    return answer
```
## 풀이
풀다가 잘 안풀려서 블로그를 참조했습니다.  
DFS로 끝까지 들어가서 그 컴퓨터 번호가 visited 했다는 것만 표시하고 나와서 네트워크가 하나 늘었다.
즉 answer += 1 해주면 된다는 개념만 참고하고 나머지는 생각해서 짰습니다.  

먼저 컴퓨터가 5개 존재한다고 가정하고, 0번컴퓨터~4번 컴퓨터가 있다고 가정합니다.
그리고 visited를 False로 컴퓨터 개수만큼 초기화 합니다.    

이제 for loop를 5번 돌려 각 컴퓨터의 연결도 정보를 통해 접근합니다.
만약 visited[0] == False라면(즉, 0번 컴퓨터에 한 번도 접근하지 않았다면) dfs로 접근합니다.
dfs에서는 깊이 탐색을 통해 depth 가 끝까지 닿을 때까지 계속해서 컴퓨터가 연결되어 있는 모든 컴퓨터에 접근합니다.
그리고 나서 dfs가 끝나 나오게 됐다면, 한 컴퓨터를 통해 끝까지 탐색하고 visited를 모두 찍은 상태가 됩니다.
이후 그렇게 쭉 탐색했던 네트워크는 하나의 네트워크가 됐다고 생각하면 +1 해주면 됩니다.  

그 네트워크가 어떻게 생겼건 몇 번 네트워크인지 생각할 필요가 없이
총 네트워크가 몇 개 인지만 파악하면 되기 때문입니다.
{: .notice--success}

# 3. [단어 변환] 문제
## 코드
```python
from collections import deque
def check(currentWord, checkWord):  # 한 알파벳만 다른 경우
    cnt = 0
    for i in range(len(currentWord)):
        if currentWord[i] != checkWord[i]:
            cnt += 1
    # print("cnt =", cnt)
    return True if cnt == 1 else False
        
def solution(begin, target, words):
    
    queue = deque()
    queue.append(begin)
    words.append(begin)
    n = len(words)
    visited = [0 for _ in range(n)]  # 방문횟수
    while queue:
        word = queue.popleft()
        for i in range(n-1):
            if check(word, words[i]) and visited[i] == 0:  # words[i]가 변경 가능 단어인 경우, 한 번도 방문하지 않은 경우
                visited[i] = 1 + visited[words.index(word)]
                if words[i] == target:
                    return visited[i]
                queue.append(words[i])
    return 0
```
## 풀이

BFS 풀이법으로 풀었습니다.  
* import 통해 deque import 했습니다.  
* check 함수를 만들고 비교할 문자열을 받아서 각 자리의 알파벳 차이의 총 개수가 1개일 때 True를 반환하여 변환할 수 있는 단어인지 확인했습니다.
* BFS 처럼 큐를 선언하고 첫 단어를 queue에 삽입했습니다.
* words에 첫 단어(begin)을 삽입했습니다.

첫 단어를 words에 삽입한 이유를 설명하겠습니다.   
먼저 visited 리스트에서 단어를 한 번 변환할 때마다 1씩 증가시킬 예정입니다.
첫 초기화를 0으로 하고 변환해서 1이 증가됐다면 더 이상 0이 아니기 때문에 방문했다는 뜻이 됩니다.
또한 1씩 증가시키면 이 값, 변환한 수 자체가 정답이 되기 때문에 visited를 1씩 증가시키며 카운트합니다.
이때 카운트할 때 변환되기 전 단어의 인덱스를 알아야 그 인덱스에서 탐색을 진행하며 이전에 몇 번 움직였는지 확인해주어야합니다.   

예를 들어 설명하면, hit -> hot -> dot 순으로 움직인다고 가정하겠습니다.
설명 편의를 위해 dot 상황부터 살펴보겠습니다. dot까지 변환된 수는 **hot가 변환된 수 + 1**이겠지요?
hot가 변환된 수는 ```visited[words에서 hot의 인덱스]``` 입니다.
따라서 `visited[words에서 dot의 인덱스] = visited[words에서 hot의 인덱스] + 1` 입니다.
그런데 제가 이 루프를 begin 부터 시작하려면 words에서 hit(begin)의 인덱스도 포함되어야 하기 때문에 words에 begin을 미리 append 하였습니다.
이를 다른 방식으로 해결하려면 먼저 hit에서 다른 부분으로 변경시킨 뒤에 그 뒤에부터 BFS를 적용해주면 됩니다.  
하지만 저는 한 루프에서 BFS를 돌리기 위해 words 리스트에 begin을 추가한 것입니다.
 
* visited 리스트에 0으로 words의 개수만큼 초기화했습니다.  
* while queue: 를 선언해서 queue 가 없을 때까지 loop를 돌렸습니다.  
* if 에서 check(변환 가능한 단어인지?)와 visited가 0임(방문하지 않은 단어)을 확인했습니다.
* 그리고 변환 이전 단어의 visited 값(이전까지 변환된 수)에 +1을 해주어서 이때까지 변환된 수를 visited에 저장합니다.
* 그리고 만약 변환 후에 단어가 targer이라면 그 즉시 종료했습니다.

BFS기 때문에 모든 경우의 변환한 수가 같은 depth에서 1씩 증가한다고 생각했고, 그렇다면 DFS와 다르게 target을 만나는 즉시 그 depth가 최소 depth라고 판단했습니다.
결과적으로는 정답이 잘 나왔네요.
{: .notice--success}

## 추가

### 다른 코드 참조
```python
from collections import deque

def solution(begin, target, words):
    if target not in words:
        return 0

    q = deque()
    q.append([begin, 0])

    while q:
        x, cnt = q.popleft()

        if x == target:
            return cnt # target값이 되면 cnt 출력

        for i in range(0, len(words)):
            diff = 0
            word = words[i]
            for j in range(len(x)):
                if x[j] != word[j]:
                    diff += 1
            if diff == 1:
                q.append([word, cnt + 1])
    return 0 # target값이 있으나 변환될 수 없다면
```

#### not in 구문
리스트에 targer이 없다면 바로 0으로 return 하는게 좋겠죠.  
제가 생각한 코드는 모든 리스트를 전부 돌아도 target을 못찾았을 때 return 0를 합니다.  
만약 리스트가 매우 길어진다면 이 코드로 인해 효율이 많이 올라가겠네요.
```python
if target not in words:
    return 0
```

#### 단어와 변환된 수 묶음을 큐로 넣기
저는 단어 리스트 words와 visited 리스트를 따로 두고 words의 인덱스와 visited를 통해 변환된 수를 구했습니다.  
이 때문에 begin은 words에 없는 문제 때문에 begin을 words에 먼저 append 하였습니다.  
하지만 위의 코드는 단어와 변환된 수를 같이 묶어서 이동하기 때문에 처음에 begin을 append 할 필요가 없네요.  

#### 해석
단어와 변환된 수를 묶음으로 해서 처음에 [begin, 0] 을 집어넣고 시작합니다.  

BFS의 기본 형태로 while q:를 통해 queue가 빌 때까지 loop 합니다.  
밑에서 가능한 경우를 계속해서 큐에 append 하므로 모든 경우가 다 가능하게 되는 것입니다.  

그리고 큐에 넣은 단어(x 변수로 받음) x가 taget 인지 확인하고 target이라면 역시 바로 그때까지 변환된 수 cnt를 return 하네요.  

최소 수를 구하는 BFS의 강점이 여기서 나타나는 것 같습니다.  
모든 경우를 depth를 1씩 증가시키면서 모두 확인하기 때문에 확인되는 그 즉시 그 결과가 최소한의 노드를 지나간 결과가 되기 때문이죠.  

아래에서는 비슷하게 알파벳이 1개만 차이나는지 확인하고 한 개만 차이난다면 그 값을 또 다시 큐에 집어넣고 loop를 통해 지속적으로 확인하네요.  

# 4. [같은 숫자는 싫어] 문제
## 코드
```python
def solution(arr):
    answer = []
    x = arr[0]
    answer.append(x)
    
    for i in range(len(arr)-1):
        x = arr[i+1]

        y = answer[-1]
        if x != y:
            answer.append(x)
            
    return answer
```
## 풀이
answer 에는 처음에는 아무것도 없으니 arr의 0번 인덱스 숫자는 무조건 연속적으로 겹치지 않습니다.  
따라서 처음에 넣고 시작합니다.  

그리고 arr[i+1] 값을 x에 저장해주고 answer에 append를 시도합니다.
이때 answer의 마지막 인덱스(-1)와 넣으려는 값이 같다면 겹치게 되므로
같지 않을 때만 넣어줍니다.

처음에 풀이할 때 스택/큐이다 보니까. arr에서 arr.pop(0)을 하면 매번 제일 왼쪽 것부터 뽑아내고
while arr: 로 루프를 돌리면 arr가 pop에 의해 다 뽑힐때까지 도니까 편할 것이라 생각했습니다.  
하지만 이 경우 시간초과가 걸립니다.  
제 생각에 pop(0)은 arr에서 굉장히 시간이 많이 걸릴 것으로 생각됩니다.  
왜냐하면 pop(0)을 하면 0번째 인덱스 값을 뽑아내고 나머지 인덱스 1부터 끝까지 요소들을 한 칸씩 왼쪽으로 옮겨줘야하기 때문입니다.  
이 경우 배열의 크기만큼 옮겨줘야 하므로 시간이 굉장히 오래 걸리는 것 같습니다.  
편할 것이라 생각했지만 그냥 pop(0)으로 접근하기 보다 인덱스로 바로 접근하는 것이 시간측면에서는 우위에 있는 것 같습니다.  
pop을 시간초과없이 사용하려면 제일 오른쪽 요소를 빼낼 때, 즉 pop()을 쓸 때만큼은 효율적일 것 같습니다.  


## 추가

다른 사람 풀이를 조금 참고했습니다.  
answer[-1:]은 안에 값이 없어도 슬라이스가 되기 때문에 값이 없을 때도 마지막 칸 하나만 가져오는 것이 가능합니다.  

따라서 이 경우를 제 코드에 적용해보면 아래와 같이 될 것 같습니다.  
즉 첫 번째를 미리 넣어줄 필요가 없어지게 되는 겁니다.  

```python
def solution(arr):
    answer = []
    
    for i in range(len(arr)):
        x = arr[i]
        
        y = answer[-1:]
        # print("i = {}, x = {}, y = {}".format(i, x, y))
        if [x] != y:
            answer.append(x)
            
    return answer
```

다만 주의할 것이 있습니다!
중간에 주석처리해놓은 print를 출력해보면 y값은 [값] 형태로 나오게 됩니다.  
따라서 if <span style="font-size: 24px; color: red">
**x**
</span> != y: 부분을 if <span style="font-size: 20px; color: red">
**[x]**
</span> != y: 로 바꾸어 주어야 정답이 됩니다!
{: .notice--danger}


y 값이 **[값]** 형태로 나오는 건 아래에 이미지로 넣어놓겠습니다.  

![img.png](/images/2024-03-28/y-val.png)

# 참고 사이트
[프로그래머스 - 네트워크 (DFS, BFS) Python](https://velog.io/@timointhebush/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-DFS-BFS-Python)  

[[프로그래머스]LEVEL3 단어변환 - 파이썬](https://reliablecho-programming.tistory.com/115)  

[파이썬 - 같은 숫자는 싫어(lv.1)](https://deblisher.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EA%B0%99%EC%9D%80-%EC%88%AB%EC%9E%90%EB%8A%94-%EC%8B%AB%EC%96%B4lv1)