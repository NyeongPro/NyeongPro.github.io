---
layout: single
title:  "[Algorithm]프로그래머스 [카카오 인턴] 수식 최대화 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit18
---

# [[카카오 인턴] 수식 최대화] 문제
## 코드
```python
from itertools import permutations
def solution(expression):
    
    answer = []
    operators = ['+', '-', '*']
    expList = []
    left = 0
    n = len(expression)
    
    # 1) 
    for i in range(n):
        if expression[i] in operators:
            expList.append(int(expression[left:i]))
            expList.append(expression[i])
            left = i+1
        elif i == n-1:
            expList.append(int(expression[left:]))
    
    # 2) 연산자와 피연산자를 저장할 스택    
    operand_stack = []
    operator_stack = []
    
    # 3) 순열을 이용해서 가능한 모든 연산자 순열 구하기
    for operator in permutations(operators):
        
        # 4) 순열마다 스택 초기화
        operand_stack.clear()
        operator_stack.clear()
        result = 0
        
        for i in range(len(expList)):
            
            # 5) i가 짝수일 때, 홀수일 때
            if i % 2 == 0:  # 피연산자 파트
                operand_stack.append(expList[i])

            else:  # 연산자 파트
                
                # 6) 세 가지 경우에 따라 계산
                if not operator_stack:  # 연산자 파트에서 비었으면 걍 추가
                    operator_stack.append(expList[i])
                elif operator.index(expList[i]) > operator.index(operator_stack[-1]):  # 높은 거 뽑으면 그대로 푸쉬
                    operator_stack.append(expList[i])
                else:  # 만약 낮은거 뽑았다면 
                    op = operator_stack.pop()
                    operand_right = operand_stack.pop()
                    operand_left = operand_stack.pop()
                    if op == "*":
                        result = operand_left * operand_right
                    elif op == "+":
                        result = operand_left + operand_right
                    elif op == "-":
                        result = operand_left - operand_right
                    operand_stack.append(result)
                    
                    
                    if operator_stack: 
                        if operator.index(expList[i]) <= operator.index(operator_stack[-1]):
                            op = operator_stack.pop()
                            operand_right = operand_stack.pop()
                            operand_left = operand_stack.pop()
                            if op == "*":
                                result = operand_left * operand_right
                            elif op == "+":
                                result = operand_left + operand_right
                            elif op == "-":
                                result = operand_left - operand_right
                            operand_stack.append(result)
                    operator_stack.append(expList[i])
        # 7) 연산자 스택에 남은 연산자들 모두 계산
        while operator_stack:
            op = operator_stack.pop()
            operand_right = operand_stack.pop()
            operand_left = operand_stack.pop()
            if op == "*":
                result = operand_left * operand_right
            elif op == "+":
                result = operand_left + operand_right
            elif op == "-":
                result = operand_left - operand_right
            operand_stack.append(result)
        
        # 8) 절댓값을 취하고 추가하기.
        answer.append(abs(result))

    return max(answer)
```
## 풀이
문제는 간단한데 구현하기가 너무 어려웠네요...
다른 코드를 참고하지 않고 풀려다보니 코드가 매우 복잡합니다.  

### 1) 연산자와 피연산자 각각 저장하기.
operators에 사용할 연산자를 미리 저장해두었습니다.
그리고 expression에서 하나씩 뽑으면서 연산자였다면 그전까지 피연산자였으므로
slice를 통해 피연산자와 연산자를 expList에 저장했습니다.

expList를 출력하면 아래와 같습니다.

[100, '-', 200, '*', 300, '-', 500, '+', 20]

### 2) 연산자와 피연산자를 저장할 스택
스택의 특징을 활용해서 연산자의 우선순위를 정하고 사칙연산을 계산할 예정이라
연산자와 피연산자를 저장할 스택을 선언했습니다.

### 3) 순열을 이용해서 가능한 모든 연산자 순열 구하기
itertools에 permutations 를 활용하면 순열 형태로 얻어낼 수 있습니다.
permutations(operators)를 하면 '+', '-', '*' 에 대해 순열을 계산해줍니다.
따라서 3개의 연산자의 순열 6개(3!)의 경우를 저장합니다.

### 4) 순열마다 스택 초기화
하나의 순열마다 스택을 초기화합니다.
아래에서 보시면 아시겠지만 6개의 순열에서 하나의 순열을 operator로 뽑아옵니다.
만약 '+', '-', '*' 라고 가정하면, 저는 이를 *가 우선순위가 제일 높고 +가 우선순위가 제일 낮다라고 두고
아래에서 계산했습니다. 따라서 하나의 경우에 대해 문제에서 주어진 식을 계산하고
answer에 저장한다음 다음 순열의 경우에서 같은 식을 계산하기 위해 스택을 초기화합니다.

즉 6번의 순열을 연산자의 우선순위라 두고 각 경우마다 주어진 식을 계산합니다.

### 5) i가 짝수일 때, 홀수일 때
i가 짝수라면 피연산자입니다.  
i가 홀수라면 연산자입니다.

피연산자 -> 연산자 -> 피연산자 -> ... 순으로 저장했기때문입니다.

### 6) 세 가지 경우에 따라 계산

세 가지 경우를 설명드리기 전에 스택을 활용하여 사칙연산을 하는 방법에 대해 간략하게만 설명드릴게요.

식을 적을 때 중위 표기법, 후위 표기법 이라는 게 있어요.

중위 표기법은 우리가 식을 적을 때 매번 사용하는 표기법입니다.  
1+2*3 과 같이요.

후위 표기법은 조금 달라져요.  
123*+ 가 됩니다.

후위 표기법은 컴퓨터에서 연산을 하기 쉽게 표현하는 방법입니다.

이제 왼쪽에서 후위표기법을 스캔한다고 치면, 123까지 와서 연산자*를 만나죠.
그러면 연산자 앞에 피연산자 두 개를 pop하고 연산자와 계산을 해줍니다.

그러면 1이 있고 2, 3이 pop되고 *가 연산자이므로 2*3=6 이 계산됩니다.
이 계산된 6은 다시 push해서 16+이 되고, 1, 6 pop 하고 +랑 만나서
7이 됩니다.

이런 방식이 후위 표기법입니다.

더 자세한 설명은 [여기](https://todaycode.tistory.com/73)를 참고하시면 좋을 것 같아요.

이제 제가 이를 토대로 3가지 경우로 나누어서 if 절을 구성하였습니다.  

#### 6.1) 연산자 스택이 비었다.
이때는 연산자 스택에서 우선순위가 본인이 제일 높기 때문에(애초에 비교할 연산자도 없기 때문에)
연산자 스택에 그냥 저장합니다.

#### 6.2) 연산자 스택에 있는 연산자보다 연산자 본인이 우선순위가 제일 높다.
이때는 그냥 연산자를 스택에 푸쉬합니다.

#### 6.3) 연산자 스택에 있는 연산자가 우선순위가 더 높거나 같다.
이 경우가 제일 중요합니다. 연산자 스택에 방금 뽑은 연산자보다 우선순위가 높거나 같으면 그냥 푸쉬하면 안됩니다.

예를 들어 연산자 스택에 + *가 들어있다고 가정하겠습니다.
피연산자 스택에는 1, 2, 3이 들어있다고 가정하겠습니다.  

처음에 +는 연산자 스택이 비어있으니 그냥 푸쉬했습니다.  
그리고 *는 +보다 연산자 우선순위가 높으니 푸쉬합니다.
이 상태에서 *가 들어온 경우와 +가 들어온 경우 둘 다 비교해볼게요.

##### *가 들어온 경우
그러면 + * 가 있는 상황에 * 가 들어왔으므로 우선순위가 같네요?
그러면 기존 스택에 있는 *를 pop하고 이 연산자를 가지고 피연산자와 한 번 계산을 해주어야해요.
그래서 피연산자 두 개를 pop 합니다.
3과 2가 pop 되겠네요. 그리고 연산자 스택에서 뽑은 *와 계산해줘야해요.  
그러면 2*3이 되니까 6이 되겠네요. 그리고 이 값을 다시 피연산자 스택에 푸쉬해야합니다.

그러면 결국 피연산자 스택은 1, 6이 되고  
연산자 스택은 +가 됩니다. 그리고 아까 뽑은 피연산자 *가 남아있습니다. 
하지만 +보다 우선순위가 높으므로 그냥 푸쉬합니다.
그래서 결국 연산자 스택은 다시 + *가 됩니다.

##### +가 들어온 경우
이제 연산자 스택에서 하나를 뽑으면 피연산자 스택에서 두 개를 뽑고 계산하는 것은
알았으니 연산자 스택에서 하나를 뽑았으면 피연산자에서 두 개 뽑고 계산했다고 가정할게요.
그리고 연산자 스택이 어떻게 되는지만 살펴볼게요.

처음에 연산자 스택이 + * 였습니다. 이제 +가 들어오면 연산자 스택에 *보다 우선순위가 낮으므로
연산자 스택에 *를 뽑아서 피연산자랑 계산합니다.
그리고 +를 이제 넣으려고 보니까, 우선순위가 같은 +가 연산자 스택에 있네요?
그러면 연산자 스택에 있는 +를 뽑아내고 피연산자랑 계산합니다.
이제 연산자 스택에 본인밖에 없으므로 +를 스택에 푸쉬합니다.

이렇게 연산자 스택에 있는 연산자들과 방금 뽑은 연산자간의 우선순위를 생각해서
계산을 해줘야합니다.

#### 이렇게 계산해야하는 이유?
처음에 1+2*3+4 이런 식이 있다고 가정해볼게요.

저 식에 하나씩 접근하면서 스택을 쌓아볼게요.

피연산자 스택 : 1    
연산자 스택 :     
----------------------------    
피연산자 스택 : 1    
연산자 스택 : +    
----------------------------   
피연산자 스택 : 1 2   
연산자 스택 : +    
----------------------------  
피연산자 스택 : 1 2   
연산자 스택 : + *   
----------------------------  
피연산자 스택 : 1 2 3  
연산자 스택 : + *   
----------------------------  
이제 여기서 +를 뽑습니다. 하지만 여기서 연산자 스택에 그냥 +를 넣으면 스택은 나중에 들어간 요소가 먼저 나오므로
+보다 *를 먼저 뽑을 수가 없게 되요. 그러면 우선순위를 신경쓸 수가 없겠죠?
그래서 *를 뽑아야 하는 겁니다.
그래서 뽑고나서 피연산자와 계산까지 한 경우를 보여드릴게요.  

피연산자 스택 : 1 6  
연산자 스택 : +   
현재 뽑은 연산자 +  

이렇게 보면 우선순위가 높은 연산자를 먼저 빼내야하는 것을 알 수 있죠?

근데, 우선순위가 같은 연산자는 왜 빼내야 할까요?

그것은 식에서 왼쪽부터 계산하기 때문입니다.  

저 식에서 +는 두 개가 있습니다. 하지만 보통 같은 우선순위라면 왼쪽 + 부터 계산하죠.  
즉 같은 우선순위 연산자라도 왼쪽에 있는 연산자가 우선순위가 높기 때문에
스택에 먼저 들어간 같은 우선순위 연산자를 pop 해줘야하는 겁니다. 
그 경우를 아래에서 살펴볼게요.      

피연산자 스택 : 7  
연산자 스택 : +  

연산자 스택에서 +를 뽑은 뒤 피연산자를 계산했습니다.  
그리고 현재 뽑은 연산자를 연산자 스택에 푸쉬한 경우입니다.

### 7) 연산자 스택에 남은 연산자들 모두 계산
expList 모두 for 루프를 돌아도 연산자 스택에 연산자가 남아있는 경우가 있습니다.

연산자 스택에서 뽑는 경우는 나중에 뽑은 연산자가 우선순위가 낮거나 같은 경우입니다.

그래서 + * 가 있는 상황에 *만 들어왔다고 가정하겠습니다.  
그러면 *가 우선순위가 같으므로 연산자 스택에 있는 *를 뽑아냅니다.
그리고 +랑 비교했을 때 우선순위가 높으므로 그대로 남겠네요.

그래서 최종적으로 + * 가 스택에 남아있습니다.

이 남아있는채로 끝낼 수는 없겠죠?

그래서 연산자 스택에 무언가 남지 않을 때까지 while 구문을 돌려서 모두 계산해줍니다.

### 8) 절댓값을 취하고 추가하기.
문제의 요구사항에 따라 절대값을 취하고 추가해줍니다.  

아래에서 max값을 취해 answer 중에서 제일 최대값을 얻어내 return 합니다.