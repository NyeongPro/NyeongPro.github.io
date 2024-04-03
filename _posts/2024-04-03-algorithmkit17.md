---
layout: single
title:  "[Algorithm]프로그래머스 튜플 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit17
---

# [튜플] 문제
## 코드
```python
def solution(s):
    
    answer = []
    s = s[1:-1]  # 양 끝 괄호 제거.
    numList = []
    for i in range(len(s)):
        if s[i] == '{':
            left = i
        if s[i] == '}':
            right = i
            numList.append(s[left+1:right])
    numList.sort(key=lambda x: len(x))
    
    setNum = set()
    for num in numList:
        commaDivide = list(map(int, num.split(',')))
        result = set(commaDivide) - setNum
        setNum.update(result)
        answer.append(list(result)[0])

    return answer
```

## 풀이
문제를 읽고 매우 쉬울 것이라 생각했는데 많이 헤맸습니다...  

이 문제를 보고 split이나 slice를 잘 써야 파이썬 문제를 쉽게 해결할 수 있다고 생각했습니다.
{: .notice--success}

저는 각 튜플을 감싸는 중괄호 안에 숫자들만 쏙쏙 모아서 중복을 제거하면서 추가하면 되겠다고 생각했습니다.

### 1) 양 끝 괄호 제거
안의 튜플을 감싸는 양 끝 중괄호는 불필요하여 slice로 제거했습니다.

**slice는 리스트에도 사용가능하고 string 형태에서도 사용이 가능했습니다.** 

string에서 pop 함수가 사용이 불가하여 s를 list(s)로 리스트로 만들고 pop(), pop(0)을 수행하는 
복잡한 바보짓을 했습니다. 문제 풀다가 문득 slice를 사용하면 될 거 같아서 변경했습니다.  

### 2) '{' , '}' 기준으로 모으기
s 안에 {2, 1}, {2, 1, 3}과 같이 모여져있습니다. 이를 {} 를 기준으로 numList에 모았습니다.
여기서도 '{' 일 때의 인덱스와 '}'일 때의 인덱스를 알아내고 s를 slice하여 쉽게 얻어냈습니다.

numList를 찍어보면 아래와 같습니다.
```
['2', '2,1', '2,1,3', '2,1,3,4']
```
중괄호와 같이 필요없는 것들을 제외하고 숫자만 string 형태로 모여져 있습니다.
string을 슬라이스했으므로 또 string 형태로 모여져 있습니다.

### 3) 길이가 짧은 튜플 순으로 정렬
첫 번째 예제는 "{2},{2,1},{2,1,3},{2,1,3,4}" 로 앞에서부터 추가하면  
2 -> 1 -> 3 -> 4 가 차례대로 들어가서 result와 맞습니다.

하지만 네 번째 예제를 보면 "{\{4,2,3},{3},{2,3,4,1},{2,3}\}" 로 앞에서 얻은 튜플부터 추가하면  
4 -> 2 -> 3 -> 1 이 되므로 result와 값이 달라집니다.

따라서 result와 결과값을 맞추려면 네 번째 예제를 {3}, {2, 3}, {4, 2, 3}, {2, 3, 4, 1} 순으로
정렬해주어야합니다.

따라서 numList를 길이 순으로 정렬해서 해결했습니다.

### 4) comma 기준으로 split하기
백준 예제를 풀 때 input().split() 을 많이 써봤는데도
이 예제에서 바로 split(',')를 생각하지 못했습니다. 잘 안쓰다보니 생각하기 어려웠나봅니다.  

처음에는  if 절로 ',' 기준으로 왼쪽 자르고 오른쪽 잘라서 숫자만 얻으려 했습니다.
하지만 이렇게 풀다보니 숫자가 하나만 들어가 있는 '2'는 ','를 만날 일이 없어서 이를 구별짓는게
쉽지 않더라구요.

그러다가 split으로 list화 시키면 깔끔할 것 같아서 list화 하였고 숫자를 얻어야하므로 map에서 int로 바꿔주는 함수를 적용했습니다.

### 5) 안겹치는 숫자 얻어내기
전 포스팅에서 교집합과 update 함수에 대해 공부할 때 차집합에 대해서도 잠깐 참고했었는데
여기에서 차집합을 쓰면 좋겠다는 생각이 들어 적용해보았습니다.  

만약 A 집합 {1, 2, 3}과 B 집합 {1, 2, 3, 4} 가 있을 때 A 집합에 비해 B 집합에서 추가된 것을
바로 알아낼 수 있으면 좋겠죠?

for 루프로 하나씩 뽑아내면서 비교해도 됩니다. 정렬을 사용해도 되구요.
이전 문제들에서는 정렬을 사용했던 것 같은데 이번엔 집합을 사용해보고 싶었습니다.

B와 A 가 둘 다 집합일 때 B - A를 하면 차집합이 적용됩니다. 따라서 위의 예제에서 {4}만 남겠네요.

{4}를 result 변수에 할당했습니다. 그리고 계속해서 숫자를 추가하면서 비교해야하기 때문에
setNum.update(result)를 통해 추가된 숫자를 update 했습니다.

그리고 set 형태는 인덱스가 없습니다.
* 집합은 인덱스가 업다.
* 집합은 중복이 없다.  

위와 같은 집합의 특성때문에 인덱스로 집합의 요소를 얻어낼 수 없습니다.

따라서 집합을 list로 만들고 요소를 뽑아와서 answer에 append 하였습니다. 

## 추가

### 다른 코드
```python
def solution(s):
    answer = []
 
    # 1) 맨 앞과 뒤의 {{, }} 를 지워줌
    slice_s = s[2:-2]
    # 2) '},{' 를 기준으로 split 하기
    split_s = slice_s.split("},{")
    split_s.sort(key=len)
 
    for sp in split_s:
        sp_s = sp.split(",")
        for num in sp_s:
            if int(num) not in answer:
                answer.append(int(num))
 
    return answer
```

#### 1) 맨 앞과 뒤의 \{\{, \}\} 를 지워줌
아래에서 '},{' 를 기준으로 split 할 거라 이 코드에서는 앞에 '\{\{'와 뒤에 '\}\}'를 모두 제거해주었네요.

#### 2) '},{' 를 기준으로 split 하기
문제를 볼 때 ',' 를 기준으로 잘라야겠다고만 생각이 들었지 '},{'와 같이 3개를 묶여서 구분된다는 생각을 전혀 못했네요.  
역시 다른 사람 풀이를 참고하면 여러 시야가 넓어지는 것 같아요.

#### 3) 길이를 기준으로 sort 하기
```python
split_s.sort(key=len)
```
위의 코드로 길이를 기준으로 split_s 를 정렬했습니다.  


> 위 코드도 제가 사용하지 못했던 코드네요.
>
> key=lambda만 사용해봐서 제 코드는 아래와 같이 사용했습니다.
>
> ```python
> numList.sort(key=lambda x: len(x))
> ```
> 하지만 key=lambda 라는 것 자체가 **이름 안지은 함수**를 여기서만 잠깐 사용하겠다는 것이고,
> lambda a: b -> a : 입력인자, b : 계산되고 반환되는 값 입니다.
>
> 여기서도 key에 함수를 잠깐 집어넣는 것이니까, 여기에 기존 함수 len을 넣는 것이 가능하겠네요.
> 참고로 key=abs 와 같이 절대값을 기준으로 정렬하는 것도 가능합니다.

여기서 split_s를 찍으면 아래와 같습니다.
```python
['3', '2,3', '4,2,3', '2,3,4,1']
```
이 형태는 제가 만든 형태와 완전히 같네요. 물론 저는 '{'인 부분과 '}'인 부분의 인덱스를 얻어내서
slice하고 ','를 기준으로 split 해서 얻었지만요.

여기서는 바로 '},{' 를 기준으로 split 하니까 바로 얻었네요.

### 4) 존재하지 않는 요소 얻어내기
split_s 라는 리스트에서 하나의 string 을 얻어내고 그 string을 num으로 받습니다.
num은 ','를 기준으로 숫자가 있기 때문에  ','를 기준으로 얻어내고 존재하지 않을 때 int로 바꿔 append합니다.


# 참고 사이트
[[python] sorted(), sort(), key 사용법](https://velog.io/@rockwellvinca/python-sorted-sort-key-%EC%82%AC%EC%9A%A9%EB%B2%95)  

[[프로그래머스 LV 2] 튜플 파이썬/python](https://foameraserblue.tistory.com/83)  
