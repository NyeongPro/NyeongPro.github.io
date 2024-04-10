---
layout: single
title:  "[Algorithm]프로그래머스 [3차] n진수 게임 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmn_number_game
---

# [[3차] n진수 게임] 문제
## 코드
```python
def solution(n, t, m, p):
    answer = ''
    numList = []
    
    # 1) 10부터 15까지 표현하기 위한 List
    alphabetList = ['A', 'B', 'C', 'D', 'E', 'F']
    
    num = 0
    
    # 2) 미리 n진법에 대한 숫자 구해두기.
    while len(numList) <= t * m:
        
        # 3) 숫자를 1씩 늘려가며 numList 채우기
        divideNum = num
        tmp = []
        # 5) 10이 넘어가는 숫자에 대해 변환
        # 4) 한 숫자에 대해 n진법으로 만들기
        while True:
            quotient = divideNum // n  # 몫 = 현재 숫자 // n
            remainder = divideNum % n
            tmp.append(remainder)
            divideNum = quotient
            if quotient == 0:
                break
                
        # 나머지를 반대로 뒤집기
        tmp.reverse()
        
        # 5) 10이 넘어가는 숫자에 대해 변환
        for x in tmp:
            if x >= 10:
                idx = x - 10
                numList.append(alphabetList[idx])
            else:
                numList.append(str(x))
                
        # 다음 숫자에 대해 n진법 변환하도록 1 증가.
        num += 1
    
    # 6) p 순서에 맞춰 numList에서 빼오기.
    for i in range(t):
        idx = i * m + (p - 1)
        answer += numList[idx]            

    return answer
```
## 풀이

이진법, 16진법으로 바꾸는 함수가 있었던 것 같습니다.

하지만 함수가 기억나지 않았고, 코테에서는 구글을 참고할 수 없기 때문에
코테에 임하는 느낌으로 전부 코드로 짜서 해결해봤습니다.

임의의 n에 대한 n진법으로 바꾸는 함수가 있는지는 따로 찾아보고, 일단 코드 풀이부터 하겠습니다.

### 1) 10부터 15까지 표현하기 위한 List
숫자 10부터 15까지는 영어 A~F로 표현해야하기 때문에 List를 미리 만들어 두고 활용했습니다.

### 2) 미리 n진법에 대한 숫자 구해두기
저는 미리 numList에 필요한만큼 숫자를 append 해두고 튜브가 말해야하는 순서를 아래에서 for 루프로 동작시켜서
numList에서 뽑아 활용할 것이기 때문에 미리 List를 만들어두었습니다.

예를 들어 인원이 두 명이고 튜브가 3번 말해야하고, 10진법으로 게임을 한다면
총 6개(t * m) 요소의 numList를 채워두어야합니다.

아래와 같이 말이죠.

```
0 1 2 3 4 5
A B A B A B
```
여기서 튜브는 A이고 B는 다른 인원입니다.

물론 p가 1이라 튜브가 처음에 말해야한다면, 숫자 5는 미리 채워두지 않아도 되긴합니다.

하지만 편의상 미리 다 채워두었습니다.

### 3) 숫자를 1씩 늘려가며 numList 채우기

숫자는 0부터 시작해서 필요한만큼까지 올라갑니다.

예를 들어 2진법이라면

0 / 1 / 1 0 / 1 1 / 1 0 0 / 1 0 1 / ...   

numList가 원하는만큼 채워질 때까지 0부터 1씩 증가하며 숫자를 n진법에 맞춰 채워야합니다.

그래서 num이 0부터 안쪽 while 루프를 마칠 때마다 1씩 증가시켜 n진법으로 만드는 작업을 수행합니다.

이번에 n진법으로 만들 숫자를 divideNum에 저장합니다.

그리고 각 숫자(num) 마다 tmp를 초기화합니다.

이 tmp에 n진법으로 만든 숫자를 저장합니다.

### 4) 한 숫자에 대해 n진법으로 만들기

아래에서는 한 숫자에 대해 n진법으로 만들어줍니다.

quotien는 몫입니다.  
remainder는 나머지입니다.

n진법으로 만드는 방법을 보면 아래와 같습니다.

제가 문제를 풀면서 메모했던 것을 보여드리겠습니다.

```
"""
13, 2진법 -> 1 1 0 1
몫 6, 나머지가 1

6/2
몫 3, 나머지가 0

3/2
몫 1 나머지가 1

1/2
몫 0 나머지가 1            

"""
```
만약 이번에 나눌 숫자(divideNum)이 13이었고, 2진법이라면 최종적으로 1 1 0 1을 얻어내야합니다.

그래서 2(n진법의 n)으로 계속해서 나누어줍니다.

그렇게 몫이 0이 될 때까지 나누어주고, 나머지만 살펴보면 됩니다.

나머지는 1 0 1 1이 나왔겠네요.

하지만 반대로 읽어주어야합니다. 그래야 정답이 됩니다.

따라서 tmp에 미리 1 0 1 1을 저장해주고 reverse함수를 통해 반대로 뒤집습니다.

이제 1 1 0 0 을 구했겠네요.

### 5) 10이 넘어가는 숫자에 대해 변환
이제 숫자는 구했습니다.

하지만 10이 넘어가는 숫자에 대해 작업을 해주어야합니다.

그래서 만약 10이 넘어가면 alphabetList에 저장해둔 것을 통해 각 인덱스별로 append 해줍니다.

만약 tmp에서 얻어온 숫자가 12였다면 'C'를 저장해야겠지요.

이때 x - 10 을 해주면 2를 얻게되고 alphabetList[2]는 'C' 이므로 맞아떨어지네요.

### 6) p 순서에 맞춰 numList에서 빼오기.

이제 numList에는 n진법에 맞춰 원하는 길이만큼의 숫자가 들어가있습니다.

그리고 인원수에 맞게 튜브의 차례에 맞게 인덱스를 맞춰주고 numList에 들어가있는게 str이므로 더해주어 정답을 제출했습니다.

## 추가

### 다른 참고 풀이
```python
def change(number,base):
    noti = "0123456789ABCDEF"
    q,r = divmod(number,base)
    n = noti[r]
    return change(q, base) + n if q else n


def solution(n, t, m, p):
    answer = []
    changeNum = []
    
    # 1) 
    for i in range(0,t*m,1):
        changeNum.append(change(i,n))
    text = "".join(changeNum)
    i = 0
    while(len(answer) < t):
        answer.append(text[i:i + m][p - 1])
        i = i + m
    return "".join(answer)
```

### 1) t * m개 만큼 만들기

다른 풀이에서도 t * m개만큼 미리 만들어 둔다.

그리고 원하는 숫자에 대해 몫과 나머지를 구하는 함수를 change라고 미리 만들어두고 활용한다.

changeNum에 change에서 받아온 숫자들을 지속적으로 담는다.

즉 숫자 0부터 t * m 까지 change에 넣는데, 이를 n진법의 n과 같이 매개변수로 제공한다.

### 2) 재귀함수를 활용해서 숫자를 n진법으로 만들기
change함수에서 재귀함수를 통해 어떤 숫자를 n진법으로 만든다.

noti에 미리 0부터 F까지 저장해두었다.

이렇게하면 내가 만든 코드에서처럼 10이상일때만 영어로 바꾸지 않아도 되고, if절을 사용하지 않아도 될 것 같다.

그리고 divmod는 몫과 나머지를 동시에 받는 함수이다.

따라서 나눠질 숫자와 나누려하는 숫자를 동시에 넣으면 몫(q)와 나머지(r)를 반환한다.

그리고 밑에서 q가 0이 될 때까지, 즉 몫이 0일 때까지 계속해서 재귀함수를 호출한다.

이렇게 되면 처음에 나누었던 나머지가 뒤로 가게 된다.

즉 `change(q, base) + n` 이 부분에서 왼쪽에 나중에 나온 나머지가 붙고, 오른쪽에 먼저 나눈 나머지가 나오기 때문에 나처럼 뒤집을 필요가 없어진다.

### 3) join을 통해 "string" 형태 만들기
changeNum 리스트에 append를 통해 각 숫자마다 n진법화한 숫자가 담기므로
changeNum은 아래와 같은 형태이다.

```
['0', '1', '10', '11', '100', '101', '110', '111']
```
이는 테스트1에서 2진법으로 숫자를 만든 것이다.

따라서 이를 각 요소별로 인덱스로 모두 접근해야하기 때문에 연결시켜준다.

join은 join 앞에 어떠한 문자를 넣어서 각 리스트 요소 사이에 어떠한 문자로 연결시켜준다.

만약 "-".join 이었다면 0-1-10-11-100... 처럼 됐을 것이다.

확인해보면 아래와 같다.

```
changeNum : ['0', '1', '10', '11', '100', '101', '110', '111']
text : 0-1-10-11-100-101-110-111
```

따라서 "".join을 통해 011011100... 처럼 하나의 string으로 연결됐을 것이다.

### 4) 튜브가 말해야하는만큼 답하기

answer에 튜브가 말해야하는 숫자를 담는다.  

그리고 이게 t보다 작을 때까지 지속적으로 answer에 연결해준다.

### slice의 n 번째 인덱스

```python
text[i:i + m][p - 1]
```
이 부분을 처음봐서 기록합니다.

어떤 문자열 또는 list에서 slice로 나누고 나서, 다시 인덱스별로 쪼갤 수 있습니다.

예를 들어 아래와 같습니다.

```python
test = "ABCDEFGHIJK"
print(test[0:4][2])
```
이를 찍어보면 test[0:4] : "ABCD" 이고 이 중에서 2번 인덱스이므로 'C'가 출력됩니다.

즉, slice로 string을 자르면 또 string이고 그 string안에서도 인덱스로 요소를 뽑아올 수 있다.

그렇다면 slice에 slice도 될 것 같네요.

```python
test = "ABCDEFGHIJK"
print(test[0:4][2:])
```
이를 찍어보면 "ABCD" 에 [2:]로 slice했으므로 CD가 출력됩니다.

### 역순으로 담기.
tmp.reverse() 함수 말고도, tmp = tmp[::-1]로 활용가능하다.


# 참고 사이트
[[Python/프로그래머스/Level2/KAKAO] n진수 게임](https://minnit-develop.tistory.com/9)