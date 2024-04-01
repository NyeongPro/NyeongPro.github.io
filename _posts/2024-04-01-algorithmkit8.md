---
layout: single
title:  "[Algorithm]프로그래머스 하샤드 수 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit8
---

# [하샤드 수] 문제
## 코드
```python
def solution(x):

    X = map(int, str(x))
    sumX = sum(X)
    print(x, sumX)
    
    return True if x % sumX == 0 else False
```
## 풀이
list와 map 함수를 이용해 양의 정수 x를 각 자리수 별로 구분짓는다.

그리고 sum 함수를 통해 리스트에 있는 모든 수를 더한다.  

나눠지면 True 를 리턴하고 아니라면 False를 리턴한다.  
