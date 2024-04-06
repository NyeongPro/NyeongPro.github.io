---
layout: single
title:  "[Algorithm]프로그래머스 N개의 최소공배수 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm_n-Least-common-multiple
---

# [N개의 최소공배수] 문제
## 코드
```python
def solution(arr):
    answer = 0
    
    # 1) Least Common Multiple 값을 하나씩 증가시킬 예정
    LCM = 1
    
    # 2) 최소 공배수를 찾을 때까지 루프
    while True:
        
        # 3) for 루프를 중간에 빠져나왔는지 확인
        check = True
        
        # 4) arr의 모든 요소를 거쳤는지 확인
        for x in arr:
            if LCM % x != 0:
                check = False
                LCM += 1
                break
                
        # 5) 모든 요소를 돌아서 check값을 확인
        if check:
            return LCM
```
## 추가

### 다른 풀이

최소 공배수는 두 자연수의 곱 / 최대 공약수로 나타낼 수 있다.

그리고 최대 공약수는 **유클리드 호제법**을 이용해 구할 수 있다.

유클리드 호제법 : 2개의 자연수 a, b(a > b)에 대해서 a를 b로 나눈 나머지가 r일 때,
a와 b의 최대 공약수는 b와 r의 최대 공약수와 같다.

이를 코드로 구현하면 아래와 같다.

```python
def gcd(a, b):
    while b:
        r = a % b
        a, b = b, r
    return a
```
그리고 위의 코드는 python에서 math 라이브러리를 사용하면 된다.

따라서 math.gcd 함수를 사용한 코드는 아래와 같다.

```python
import math
def solution(arr):
    answer = 0
    LCM = arr[0]
    for b in arr[1:]:
        a = LCM
        LCM = LCM * b // math.gcd(a, b)
        
    return LCM
```


# 참고 사이트
[[프로그래머스]N개의 최소 공배수 python](https://hye-z.tistory.com/79)  
