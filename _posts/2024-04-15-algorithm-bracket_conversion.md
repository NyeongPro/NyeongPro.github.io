---
layout: single
title:  "[Algorithm]프로그래머스 괄호 변환 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm-bracket_conversion
---

# [괄호 변환] 문제
## 코드
```python
def check_correct_bracket(bracket):
    stack = []
    
    for x in bracket:
        if x == '(':
            stack.append(x)
        elif x == ')':
            if not stack:
                return False
            stack.pop()
    return True
    
def solution(p):
    answer = ''
    if check_correct_bracket(p):
        return p
    
    def bracket(p):
        # 1. 빈 문자열 반환
        if not p:
            return ""
        
        # 2. u, v 분리
        left = 0
        right = 0
        for i in range(len(p)):
            if p[i] == '(':
                left += 1
            elif p[i] == ')':
                right += 1
            if left == right and left != 0:
                u = p[:i+1]
                v = p[i+1:]
                break
        # 3. u 올바른 문자열 확인
        if check_correct_bracket(u):
            return u + bracket(v)
        
        # 4. u가 올바르지 않은 문자열
        tmp = '(' + bracket(v) + ')'

        u = u[1:-1]
        new_u = ""
        for x in u:
            if x == '(':
                new_u += ')'
            elif x == ')':
                new_u += '('
        return tmp + new_u

    return bracket(p)
```
## 풀이

문제에서 주어진 과정을 충실히 따라서 코드를 구성했습니다.

### 1. 빈 문자열 반환

문자열이 비었다면 빈 문자열을 반환합니다.

사실 제일 쉬운 부분인데 이 부분을 놓쳐서 계속 해맸습니다.

if not 을 이용해 빈 문자열을 확인하고, 빈 문자열을 반환해야하는데 
`return ''` 이 아닌 `return`으로만 작성하였습니다.

이에 재귀함수가 작동하다가, 아무것도 반환되지 않아서 문자열과 Nonetype이 더해지는 상황이 발생했습니다.

이에 오류가 떠서 문제가 쉽게 해결되지 않았으나 빈 문자열을 확실하게 반환하여 해결했습니다.

### 2. u, v 분리

u, v를 분리하였습니다.

문제에서 더 이상 분리되지 않는 균형잡힌 괄호 문자열 u가 잘 이해가 안됐었습니다만, 왼쪽에서 u를 잘라내고 균형잡힌 괄호 문자열을 만드려면
왼쪽 괄호 개수와 오른쪽 괄호 개수가 같아야한다는 점과, 더 이상 잘리지 않으려면 최소한의 괄호만 가져야한다고 생각했기 때문에
왼쪽에서부터 개수를 세면서 개수가 딱 떨어지는 지점에서 바로 u, v를 분리했습니다.

### 3. u 올바른 문자열 확인

올바른 괄호 문자열인지 함수로 만들어 확인했습니다.

check_correct_bracket 이라는 함수를 만들었습니다.

그리고 stack의 개념을 활용해 괄호가 잘 이루어져 있는지 확인했습니다.

'('은 append하고 ')' 은 pop 합니다.

pop 할 때 스택이 비었다면, 올바른 괄호가 아닙니다.

물론 원래는 전부 pop 했음에도 스택이 남아있다면 또 문제가 되지만, 문제에서 정확하게 왼쪽 괄호와 오른쪽 괄호가 같은 균형잡힌 괄호 문자열만 들어오기 때문에
이는 구현하지 않았습니다.

### 4. u가 올바르지 않은 문자열

4-1, 4-2, 4-3 까지 tmp에 한 번에 할당합니다.

그리고 처음과 끝 문자를 제거하고 나머지 문자열에 대해서는 괄호를 뒤집어 붙여줍니다.

