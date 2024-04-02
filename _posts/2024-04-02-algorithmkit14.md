---
layout: single
title:  "[Algorithm]프로그래머스 카펫 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit14

---

# [카펫] 문제
## 코드
```python
def solution(brown, yellow):
    answer = []
    
    for y in range(1, brown):
        for x in range(1, brown):
            if 2*x + 2*(y-2) == brown:
                if (x-2) * (y-2) == yellow:
                    return [x, y]
    
    return answer
```
## 풀이
단순 계산 문제였습니다.  
저는 아래 주석처럼 예시를 보면서 가로, 세로를 방정식으로 표현하도록 했습니다.  

![img.png](/images/2024-04-02/carpetex.png)

카펫의 가로, 세로를 x, y라고 둡니다.  
노란색 부분은 m, n 으로 두었습니다.  

그리고 가로 두 번, 세로 두 번-겹치는 부분으로 브라운의 개수를 구했습니다.

가로 두 번(2x) + 세로 두 번-겹치는 부분(2(y-2)) 입니다.
노란색 부분을 보면 가장자리부분만 빼주면 됩니다.  
따라서 m = (x-2), n = (y-2) 가 됩니다.

이대로 for 루프를 두 번 돌면서 모든 x,y에 대한 경우를 구해보며
값이 들어맞을 때까지 루프합니다.
