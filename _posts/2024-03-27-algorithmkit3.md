---
layout: single
title:  "[Algorithm]프로그래머스 코딩테스트 고득점 KIT 여러 문제3[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit3
---

## 1. [H-Index] 문제
## 코드
```python
def solution(citations):
    
    answer = 0
    citations.sort(reverse=True)
    # print(citations)
    for i in range(len(citations)):
        minCit = min(citations[i], i+1)
        if minCit > answer:
            answer = minCit
        else:
            break
    
    
    return answer
```
## 풀이
citations 내림차순 정렬.

입출력 예시에서는 [6, 5, 3 ,1 ,0] 과 같이 정렬됨.

여기서 citations의 인덱스 값을 idx라고 하자.  

idx + 1 이 citations[idx] 값보다 크거나 같은 논문 수라는 점을 이용하였음.  

예시로 아래를 보자.  

```
idx            : 0 1 2 3 4
idx + 1        : 1 2 3 4 5   
citations[idx] : 6 5 3 1 0
```

위와같이 정리했을 때 idx = 2 일 때를 살펴보자.  
idx + 1 은 3이고 citations[idx]는 3이다.  
이때 내림차순으로 정렬했으므로 citations[idx] 왼쪽 요소들은 모두 citations[idx]보다 크거나 같다는 점 때문에  
citations[idx] 값 이상인 요소의 개수가 idx + 1(3)이 된다.  
즉 citations[idx] 번 이상 인용된 논문이 idx + 1개 만큼 된다.

이를 이해한 뒤에 idx를 하나씩 증가시키면서 상황을 살펴보자.  

```
idx 0 일 때  
idx + 1 = 1, citations[idx] = 6
```
이 경우 h는 1번 이상 인용된 논문이 1편이므로 값은 1이 된다.  

```
idx 1 일 때  
idx + 1 = 2, citations[idx] = 5
```
이 경우 h는 2번 이상 인용된 논문이 2편이므로 값은 2가 된다.  

```
idx 2 일 때  
idx + 1 = 3, citations[idx] = 3
```
이 경우 h는 3번 이상 인용된 논문이 3편이므로 값은 3이 된다.

여기서 두 개의 값 중 idx + 1 값이 결과값이 되는 것을 알 수 있다.

idx가 3이 되기 전 다른 예시를 한 번 살펴보자.  

```
idx            : 0 1 2 3 4
idx + 1        : 1 2 3 4 5   
citations[idx] : 0 0 0 0 0
```

위의 예시에서는 h 값은 0이 될 것이다.   
이때 이 0값은 citations[0]의 값이다.
idx가 하나씩 증가해도 citations[idx]값이 결과값이 된다.  

여기서 첫 번째 예시는 idx + 1이 결과값이었고 두 번째 예시는 citations[idx]가 결과값이었다.  

두 예시에서 결과값은 두 개의 값 중 최소값이라는 것을 알 수 있었고 answer 값에 min(citations[idx], idx + 1) 값을 assign했다.

이제 중요한 조건을 걸어주어야 하는데 아래 예시를 보자.  

만약 첫 번째 예시에서 citations[idx] 값보다 idx + 1 이 역전될 때는 어떨까.

```
idx 3 일 때  
idx + 1 = 4, citations[idx] = 1
```
이 경우 h는 3번 이상 인용된 논문이 3편이므로 값은 3이 된다.

idx가 3일 때는 h 값보다 citations[idx] 값이 낮아진 요소가 오므로 h는 값이 그대로이다.

이때는 두 개의 값 중 최소값이 결과값이 되지 않는다.  

이유는 H-Index는 최대값을 구하는 것인데 h = 3인 상황에서 최소값인 1을 넣을 이유가 없기 때문이다.

따라서 만약 새로 구한 값이 기존 값보다 작을 경우에는 assign 하지 않는다.

## 추가

### return len(citations) 코드 의문
다른사람 코드 분석 중에 인용된 논문이 하나면 h값은 1이라는데 잘 이해가 되지 않는다...  
citations = [0] 이라면  
0회 인용된 논문이 한 편이라는 뜻인데 이 경우 H-Index가 0이 되는게 아닌가...?  
그런데 다른 사람의 코드에서 마지막 줄에 return len(citations) 와 같은 경우가 있는데  
이때는 return 값이 1이므로 내가 생각하는 답과 달라진다고 생각하는데...  

위의 생각은 마지막 줄 return 값만 보고 생각해서 나온 바보같은 생각이었다.  
다른 코드에서 저 상황은 이미 걸러져서 return 된다...



## 2. [최소직사각형] 문제
## 코드
```python
def solution(sizes):

    sizes.sort(key=lambda x: (x[0], x[1]), reverse=True)
    print(sizes)
    max_w = sizes[0][0]
    max_h = sizes[0][1]
    for i in range(1, len(sizes)):
        # 각 요소의 최대 길이가 가로가 되도록 미리 설정
        if sizes[i][0] < sizes[i][1]:
            w = sizes[i][1]
            h = sizes[i][0]
        else:
            w = sizes[i][0]
            h = sizes[i][1]
        # print("w, h =", w, h)
        
        if max_w >= w and max_h >= h:  # 가로, 세로 둘 다 작거나 같은 경우(명함이 잘 들어간다.)
            continue
        elif max_w < w and max_h < h:  # 가로, 세로 둘 다 큰 경우(눕혀도 안들어간다.)
            max_w = w
            max_h = h
        elif max_w < w and max_h >= h:  # 하나가 큰 경우
            max_w = w
        else:
            max_h = h

    # print(max_w, max_h)
    
    return max_w * max_h
```
## 풀이

레벨 1임에도 불구하고 나에겐 너무 어려웠다...  
모든 경우를 다 생각하고 그에 맞춰 하나하나 코드를 짜는 것이 비효율적이면서도  
생각하기 어려웠다.  

먼저 각 요소의 첫 번째 인덱스 기준으로 정렬하고 첫 번째 인덱스 값이 가장 큰 것을 기본 명함 크기로 잡았다.  
그 이후 4가지 조건에 맞게 지갑 가로, 세로 max 사이즈를 정하였다.
* 새로운 명함이 현재 지갑 max 사이즈에 비해 가로, 세로 둘 다 작은 경우 -> 아무런 변화없이 명함이 잘 들어갈 것.
* 새로운 명함이 지갑 max 사이즈 대비 가로, 세로 둘 다 큰 경우 -> max 사이즈를 모두 변경한다.
* 새로운 명함의 한 사이즈가 큰 경우 -> 가로, 세로 중 하나를 변경한다.

코드를 짜면서도 너무 비효율적이라 생각했다.  
좀 더 좋은 방안이 있을 것 같다.

## 추가

다른 사람의 코드를 참조해 최종 코드는 아래와 같다
```python
def solution(sizes):
    return max(max(x) for x in sizes) * max(min(x) for x in sizes)
```

내 풀이에 비해 압도적으로 간단하다...

sizes에서 한 요소를 뽑고 가로, 세로 중 큰 값을 모은다.  
그 모은 값에서 가장 큰 값을 고른다.

이후 한 요소를 뽑고 가로, 세로 중 작은 값을 모은다.  
그 모은 값에서 가장 큰 값을 고른다.  

그리고 그 값을 곱하면 답이된다.  

답을 봤을 때 이해할 수 있지만 생각하기에 어려웠던 것 같다.  

나는 한 쪽에서 제일 큰 값을 먼저 고를 때 sort로 정렬해서 구했다.

그 이후 다른 길이를 구할 때 4가지 조건에 근거해서 구했지만 각 요소에서 작은 값을 모으고
그 중에서 큰 값을 구하면 되는 문제였다.  


## 3. [모의고사] 문제
## 코드
```python
def solution(answers):
    answer = []
    
    supoja1 = [1, 2, 3, 4, 5]
    supoja2 = [2, 1, 2, 3, 2, 4, 2, 5]
    supoja3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]
    
    ansNum1 = 0
    ansNum2 = 0
    ansNum3 = 0

    for i in range(len(answers)):
        if answers[i] == supoja1[i % len(supoja1)]:
            ansNum1 += 1
        if answers[i] == supoja2[i % len(supoja2)]:
            ansNum2 += 1
        if answers[i] == supoja3[i % len(supoja3)]:
            ansNum3 += 1
    maxVal = max(ansNum1, ansNum2, ansNum3)
    
    if maxVal == ansNum1:
        answer.append(1)
    if maxVal == ansNum2:
        answer.append(2)
    if maxVal == ansNum3:
        answer.append(3)
    
    return answer
```
## 풀이

리스트로 각각 수포자가 찍는 방법을 미리 입력해두었다.  
맞춘 개수는 ansNUM{}로 설정하였다.  
for loop를 돌면서 각 자리마다 정답이 맞는지 확인하였다.  
각자 5번마다, 8번마다, 8번마다 찍는 루틴이 반복되므로 %len(supoja{}) 를 통해 나머지를 구해서 인덱스를 넣어주었다.  

이후 가장 많이 맞춘 수에 따라서 if절에서 비교해서 가장 많이 맞춘 인물이 누구인지 판단하고 정답에 append 해주었다.  


## 4. [소수 찾기] 문제
## 코드
```python
from itertools import permutations
from math import sqrt

def isPrime(num):
    if num < 2:
        return False
    for i in range(2, int(sqrt(num))+1):
        if num % i == 0:
            return False
    
    return True
    
def solution(numbers):
    answer = 0
    # print(numList)
    perList = []
    for i in range(1, len(numbers)+1):
        tmp = list(set(map("".join, permutations(numbers, i))))
        for x in tmp:
            perList.append(x)
            
    print(perList)
    
    perNumList = list(set(map(int, perList)))
    print(perNumList)
    
    for x in perNumList:
        if isPrime(x):
            answer += 1
    
    return answer
```
## 풀이

모든 순열을 찾는 것이 너무 어려웠네요.  
구현해보고 싶었지만 포기하고 순열 찾는 방법을 조사했습니다.  

파이썬 라이브러리의 itertools에 permutations를 사용하면 모든 순열을 구할 수 있었습니다.  

그렇게 모든 순열을 구하고 int를 통해 숫자로 변경했습니다.  
또한 set함수를 이용해 중복을 제거해준 뒤 perNumList에 저장했습니다.  

마지막으로 perNumList가 소수인지 찾는 함수를 따로 구현해서 소수 개수만큼
결과값에 더해주었습니다.  

## 추가

### permutations 함수 공부
permutations는 순열이라는 뜻 그대로 순열을 구해주는 함수입니다.  

사용법은 from itertools import permutations를 통해 import해준 뒤 사용하면 됩니다.  
itertools에는 다른 유용한 함수가 존재합니다.
간단히 아래에서 살펴보고 넘어가겠습니다.  

#### 1) combinations()

combinations(iterable, r) 로 사용합니다.

iterable에서 원소 개수가 r 개인 조합 뽑기 입니다.

nCr이라 생각하면 됩니다.

#### 2) combinations_with_replacement(iterable, r)

iterable에서 원소 개수가 r개인 중복 조합 뽑기 입니다.

같은 원소를 중복해서 뽑을 수 있습니다.  

#### 3) permutations(iterable, r)

iterable에서 원소 개수가 r 개인 순열 뽑기 입니다.  

nPr 입니다.

#### 4) product(iterable1, iterable2, repeat=1)

여러 iterable의 데카르트곱 반환입니다.  

### 오류 해결

소수를 구할 때 2부터 num-1 까지 나누어줬을 때 나누어 떨어지는 경우가 없으면 소수입니다.  

이때 num-1 까지 전부 비교하지 않고 num의 root 값, 즉, 2~sqrt(num)까지 비교하면 좀 더 비교 횟수를 줄일 수 있습니다.  

여기서 저는 소수인지 아닌지 반환하는 함수 isPrime을 
만들 때 범위를 (2, int(sqrt(num))) 로 했었는데 이 경우 하나를 놓치는 경우가 생길 수 있습니다.  

예를 들어 num이 49라고 가정해봅시다.  
그러면 2, 3, 4, 5, 6, 7까지 나누어보아야 합니다. 2, 3, 4, 5, 6까지는 나누어 떨어지지 않기 때문에 7도 나누어 보아야하는데
파이썬 range 의 특성상 (2, 7)로 범위를 잡으면 7이 들어가지 않기 때문에 7로 나누어야할 상황에 나누지 않고 함수가 종료되는 상황이
생기게 됩니다. 이때문에 몇몇 테스크 케이스가 오류가 발생했고 이를 (2, int(sqrt(num))+1)로 고쳐 해결했습니다.

## 5. [타겟 넘버] 문제
## 코드
```python
def search(numbers, inNumList, cnt): 
    
    # print("inNumList = ",inNumList)
    if cnt >= len(numbers):
        return inNumList
    
    numList = []
    if cnt == 0:
        numList.append(-inNumList[0])
        numList.append(inNumList[0])
    else:
        for x in inNumList:
            numList.append(x-numbers[cnt])
            numList.append(x+numbers[cnt])
        
    cnt += 1
    
    
    return search(numbers, numList, cnt)
    
def solution(numbers, target):
    answer = 0
    numList = [numbers[0]]
    
    result = search(numbers, numList, 0)
    
    for x in result:
        if x == target:
            answer += 1
    return answer
```
## 풀이
탐색 문제라 재귀함수 꼴로 문제가 풀릴 것 같아서 재귀함수를 통해 문제를 풀고자 하였습니다.  
해설을 안보고 혼자서 풀어본 것이라 효율성도 떨어지고 코드도 복잡합니다...  

접근방법은 다음과 같습니다.  
[4, 1, 2] 라는 numbers가 있다고 가정하겠습니다.  
그렇다면 경우의 수를 적어보면 아래와 같습니다.

```
-4 -1 -2 = -7
-4 +1 +2 = -1
+4 -1 -2 = +1
+4 +1 +2 = +7
----------------------
-4 -1 +2 = -3
-4 +1 -2 = -5
+4 -1 +2 = +5
+4 +1 -2 = +3
```

제가 절취선을 그린 것을 기준으로 위아래를 살펴보면 첫 번째 요소(+-4)와 두 번째 요소(+-1)의 순서가 완벽히 똑같음을 알 수 있습니다.
대신 3번째 요소가 -2, +2를 반복하기 때문에 절취선 위 아래 각각 4가지 * 2가지 = 8가지가 된 것입니다.  
저는 이 점에서 [4, 1] 이라는 리스트에서 2라는 요소가 하나 추가됐다고 생각했습니다.  
잘 설명이 안되는데 예시를 들겠습니다.  

리스트에서 모든 요소를 더하는 함수를 만들고 싶습니다.  
[4, 1, 2]라는 리스트를 예시로 들겠습니다.  

하나씩 다 더하면 됩니다. 4+1+2 = 7이 됩니다.  
그런데 여기서 첫 번째 요소를 더하고 다음 요소를 더하는 식으로 방법을 바꾸겠습니다.  

처음 적용시키면 4+1 = 5 가 됩니다.  
그리고 5를 첫 번째 인덱스에 넣으면 [5, 2] 가 됩니다.  
이를 다시 적용하면 5+2 = 7이 됩니다.

즉 [4, 1]이라는 리스트에 이 더하는 함수를 먼저 적용시켜 [5]라는 리스트를 만들고
여기에 2 요소를 추가해 [5, 2]를 다시 만든뒤에 다시 적용하면 5+2 = 7 이 됩니다.

이처럼 이 문제에 [4, 1] 리스트에 이 문제를 적용시켜 4가지 경우를 만든 뒤에  
이 경우에 각각 [[4, 1], 2] 와 같이 2 요소를 추가해 똑같은 함수를 적용합니다.
즉 재귀함수를 적용합니다. 그래서 [4, 1]이라는 요소에 +2, -2 를 적용해서 4가지 * 2가지 = 8가지 경우의 수가 나오게 됩니다.  

처음 함수를 들어가면(cnt=0이라면) +inNumList[0], -inNumList[0] 을 초기에 먼저 만들어줍니다.
그 이후부터는 다음 요소를 더하고 빼고(즉 *2가지가 됨)하면서 재귀함수를 지속적으로 들어가고
cnt를 통해 몇 번 재귀함수를 들어갈지 조건을 걸어주었습니다.

## 추가

### count 함수 사용
```python
for x in result:
    if x == target:
        answer += 1
```
위 코드는 하나씩 뽑아와서 같을 때마다 숫자가 늘어가 결과적으로 같은 것의 개수를 구할 때 사용했다.  

위는 그냥 `result.count(target)` 단 한 줄로 표현이 가능하다.

### BFS 풀이
```python
def solution(numbers, target):
    leaves = [0]
    for num in numbers:
        temp = []

        # 자손 노드
        for leaf in leaves:
            temp.append(leaf + num)  # 더하는 경우
            temp.append(leaf - num)  # 빼는 경우

        leaves = temp
    return leaves.count(target)
```
numbers가 [4, 1, 2]라고 가정
leaves = [0] -> leaf는 0 한 번 뽑아온다.

0에 4빼기, 4 더하기 두 가지를 temp 리스트에 append
[-4, 4] 가 저장된다.

-4에 1빼기 더하기 두 가지를 temp 리스트에 append
+4에 1빼기 더하기 두 가지를 temp 리스트에 append
총 4가지가 저장된다.

또 이 4가지에 2를 더하고 빼기.  
그러면 8가지가 된다.
...이후 반복된다.

### DFS 풀이
```python
answer = 0
def dfs(idx, numbers, target, value):
    global answer
    n = len(numbers)
    if n == idx and target == value:
        answer += 1
        return
    elif n == idx:
        return
    else:
        dfs(idx+1, numbers, target, value+numbers[idx])
        dfs(idx+1, numbers, target, value-numbers[idx])
        
def solution(numbers, target):
    global answer
    dfs(0, numbers, target, 0)
    return answer
```
numbers = [4, 1, 2, 1] 일 때  
dfs(0, numbers, target, value+numbers[idx])  
dfs(1, numbers, target, value+numbers[idx])  
dfs(2, numbers, target, value+numbers[idx])  
dfs(3, numbers, target, value+numbers[idx])  
----------------------------------------------  
dfs(4, numbers, target, value+numbers[idx]) 일 때 idx와 len(numbers) 같아진다.  
그리고 else: 의 dfs로 가기전에 return 으로 걸러진다.   
그리고 target이 맞는지 아닌지 확인하고 맞다면 글로벌 변수에 1이 더해진다.
# 참고 사이트
[[프로그래머스] 타겟 넘버 (Python)](https://hkim-data.tistory.com/35)  

[[프로그래머스] 타겟 넘버 (python) - BFS/DFS](https://daeun-computer-uneasy.tistory.com/69)
