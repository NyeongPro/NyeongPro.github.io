---
layout: single
title:  "[Algorithm]프로그래머스 메뉴 리뉴얼 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm-menu_renewal
---

# [메뉴 리뉴얼] 문제
## 코드
```python
from itertools import combinations
def solution(orders, course):
    answer = []
    
    # 1) 문제에서 주어진 조합된 메뉴의 개수
    for n in course:
        
        # 2) 딕셔너리 활용
        orderDict = {}
        
        # n마다 최대값 초기화
        maxVal = 0
        
        # 3) orders에서 어떤 조합으로 메뉴 시킨지 확인
        for order in orders:
            
            # 미리 스트링을 오름차순으로 정렬
            order = "".join(sorted(order))
            
            # 4) 조합을 사용해서 n개씩 뽑을 수 있는 모든 경우를 확인한다.
            for combiMenu in combinations(order, n):
                print(combiMenu)
                orderDict["".join(combiMenu)] = orderDict.get("".join(combiMenu), 0) + 1
                
        # 5) value 들 중에서 제일 많이 주문한 조합
        if orderDict.values():
            maxVal = max(orderDict.values())
        for key, value in orderDict.items():
            if value == maxVal and maxVal >= 2:
                answer.append(key)
    
    answer.sort()
    return answer
```
## 풀이

### 1) 문제에서 주어진 조합된 메뉴의 개수

course에서 2, 3, 4와 같이 주어졌다면, 각각 단품 메뉴 두 개, 세 개, 네 개로 구성된 코스를 찾아야한다.

따라서 이 값을 n으로 받아주고, 필요한 메뉴의 개수를 파악한다.

### 2) 딕셔너리 활용

이 문제에서 딕셔너리를 활용했다.

만약 어떤 사람이 "ABC"에 대해 주문했고, 2개의 메뉴로 코스를 구성해야한다면
"AB", "AC", "BC"와 같이 구성해볼 수 있다.

이때 이 "AB", "AC", "BC"를 Key로 활용해서 value 값에 이 메뉴를 한 번 시켜먹은 사람이 있다라고 할 예정이다.

### 3) orders에서 어떤 조합으로 메뉴 시킨지 확인

매번 n마다 orders에서 order를 하나씩 받아온다.

order에는 어떤 사람이 이렇게 주문해서 먹었다 라는 정보가 담겨있다.

"ABCFG" 처럼 말이다.

그리고 미리 string 형태를 오름차순으로 정렬해둔다.

이는 입출력 예 3번을 보면 알 수 있다.

orders를 보면 "XWY"와 "WXA"가 있다. 만약 이를 미리 정렬해주지 않는다면, 
combinations를 통해 요소를 뽑아오기 때문에 "XW"와 "WX"가 동시에 존재한다.

하지만 메뉴 X와 메뉴 W를 주문한것과, 메뉴 W와 메뉴 X를 주문한 것은 완전히 같기 때문에
이를 미리 정렬해서 방지해준다.

또한 result를 출력할 때 이 부분에서도 스트링이 정렬돼서 출력해야하기 때문에 일석이조이다.

> 그리고 스트링은 sort() 함수를 통해 정렬할 수 없기 때문에 sorted() 함수를 사용해 정렬한다.
>
> 다만 sorted() 함수를 사용하면 list 형태로 반환하기 때문에 이를 다시 join 함수를 사용해 string 형태로 바꾸어주었다.

### 4) 조합을 사용해서 n개씩 뽑을 수 있는 모든 경우를 확인한다.

조합을 사용하면 "ABC"를 "AB", "AC", "BC" 형태로 뽑아올 수 있다.

이 "AB" 한 요소를 combiMenu라고 이름짓고 이를 딕셔너리의 Key로 사용한다.

하지만 이때, "".join(combiMenu) 를 Key로 집어넣는다.

이유는 combiMenu를 출력해보면 아래와 같이 튜플 형태로 나온다.

```
('A', 'B', 'C', 'F')
```

따라서 이를 다시 스트링 형태로 바꾸어주어야 한다.

그리고 get 함수를 통해 key가 존재한다면 그 key의 value 값에 +1을 해주고
만약에 key가 존재하지 않았다면, 0을 반환하도록 하는 get 함수를 사용했다.

이 get 함수를 쓰지 않는다면 아래와 같이 해결할 수도 있다.

```python
orderDict["".join(combiMenu)] = orderDict.get("".join(combiMenu), 0) + 1

--------------------------------

if "".join(combiMenu) in orderDict:
    orderDict["".join(combiMenu)] += 1
else:
    orderDict["".join(combiMenu)] = 1
```

딕셔너리에서 key가 존재하는지 아닌지 판단할 때 in 연산자를 사용해서 판단한다.

### 5) value 들 중에서 제일 많이 주문한 조합

values() 를 사용해서 현재 딕셔너리의 value들을 모두 가져온다.

그 중에서 최대값을 maxVal에 저장한다.

그리고 아래에서 maxVal과 같은 값을 가진 key들만 answer에 추가해주면 된다.

그리고 maxVal >= 2 조건을 추가해야만, 문제 요구 사항중에서 "또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다."
를 해결할 수 있다.

