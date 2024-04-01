---
layout: single
title:  "[Algorithm]프로그래머스 합성수 찾기 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit10
---

# [합성수 찾기] 문제
## 코드
```python
def solution(n):
    answer = 0
    
    for i in range(4, n+1):
        cnt = 0
        for j in range(1, i+1):
            if i % j == 0:
                cnt += 1
            if cnt == 3:
                answer +=1
                break
    
    return answer
```
## 풀이
2번의 for loop를 사용했습니다.

첫 번째 loop 에서 n보다 작은 수를 하나 하나 찾기위해 loop를 돌립니다.  

아래 loop 에서 첫 번째 loop에서 받아온 수가 합성수인지 확인합니다.  
1부터 i 본인까지 약수의 개수를 cnt에 저장합니다. 그리고 cnt가 3이 되는 순간 종료해도 됩니다.
break를 통해 종료하고 답을 하나 증가시킵니다.

모든 n보다 작은 자연수에 대해 조사하고 answer를 증가시킨뒤 return 합니다.

