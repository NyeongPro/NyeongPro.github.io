---
layout: single
title:  "[Algorithm]프로그래머스 우박수열 정적분 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm-sequence_integral
---

# [우박수열 정적분] 문제
## 코드

```python
def solution(k, ranges):
    answer = []
    points = []
    areaList = []
    
    # 1) 초기 설정.
    x = 0
    y = k
    points.append((x, y))
    
    # 2) 콜라츠 수열 만들기
    while y != 1:
        x += 1
        if y % 2 == 0:
            y = y // 2
        else:
            y = y * 3 + 1
        points.append((x, y))
    
    # 3) x 길이가 1짜리 정적분하기
    for i in range(len(points)-1):
        maxH = max(points[i+1][1], points[i][1])
        minH = min(points[i+1][1], points[i][1])
        area = (maxH - minH) * 1 / 2 + 1 * minH
        areaList.append(area)
    
    # 4) 주어진 범위의 정적분 계산하기
    for ran in ranges:
        result = 0
        a = ran[0]
        b = (len(points) - 1) + ran[1]
        if a > b:
            answer.append(-1)
        else:
            for i in range(a, b):
                result += areaList[i]
            answer.append(result)
        
    return answer
```
## 풀이

### 1) 초기 설정.
점(x, y)에 대해 찍을 예정.

그리고 처음 점(0, k)에 대해 초기화 후 시작.

### 2) 콜라츠 수열 만들기

k(y 좌표)에 대해 짝수라면 2로 나누기, 홀수라면 3을 곱하고 1더하기

최종적으로 k가 1이 될 때까지 수행.

### 3) x 길이가 1짜리 정적분하기

미리 areaList에 아래 길이가 1로 정적분을 수행함.

예를 들어 areaList[0]에는 x가 0~1 사이에 대해 정적분을 수행한 값임.

그래서 areaList[0]에는 삼각형 부분 하나와 사각형 부분 하나를 더한 값을 넣어줌.

숫자로 보면, 삼각형 = (16-5) * 1 / 2, 사각형 5 * 1 이 들어감.


### 4) 주어진 범위의 정적분 계산하기

x 를 a 부터 b 까지 적분함.  
a 는 문제에서 주어진 범위의 첫 번째 인덱스이다.  
b 는 n(k가 1이 될 때까지의 길이) + 두 번째 인덱스 값이다.  
두 번째 인덱스는 마이너스값으로 주어지기 때문에 더해준다.

a보다 b가 작다면 인덱스 범위를 벗어난 것이므로 -1을 append해주고,
아니라면 원하는 범위의 정적분 값들을 모두 더해주고 append해준다.

