---
layout: single
title:  "[Algorithm]프로그래머스 코딩테스트 고득점 KIT 여러 문제2[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit2
---

# 1. [K번째수] 문제
## 코드
```python
def solution(array, commands):
    
    answer = []
    for com in commands:
        slice_arr = array[(com[0]-1):com[1]]
        slice_arr.sort()
        # print("slice_arr =", slice_arr)
        answer.append(slice_arr[com[2]-1])
        # print("slice_arr[com[2]-1] =", slice_arr[com[2]-1])
    
    return answer
```
## 풀이

commands를 for loop 에서 com으로 받고 각 요소에 맞게 slice한다.  
이후 sort()를 통해 정렬한다.  
append()를 통해 각 com 마다 원하는 값을 추가한다.  

중간에 정렬하는 것을 모르고 계속 해매다가 문제 다시 보고 sort() 추가해서 해결했네요.

# 2. [가장 큰 수] 문제
## 코드
```python
def solution(numbers):
    
    answer = ''
    numbers.sort(key=lambda x: str(x)[0], reverse=True)
    print(numbers)
    
    return answer
```

## 풀이
위의 코드를 통해 먼저 첫 번째 자리에 따른 내림차순 정렬을 하였다.  
이 경우 두 번째 테스트 케이스에서 아래와 같이 결과를 얻는다.

[9, 5, 3, 30, 34]

9, 5 순서는 맞지만 그 뒤에 3, 30, 34는 순서가 맞지 않는다...

두 번째 자리에 대해서 또 sort 를 하고 싶지만 3은 두 번째 자리에 대한 값이 없기 때문에
index 범위를 벗어나 오류가 발생한다.  

또한 두 번째 자리를 비교한다고 해도 숫자가 큰 순서대로 비교하다가 두 번째 자리가
0인 경우와 아예 없는 경우는 또 아예 없는 경우가 앞선 자리에 들어가야하기 때문에 난감했다...  

그렇다면 원소의 값이 1000이하이므로 for loop를 통해
첫 번째 자리, 두 번째 자리 세 번째 자리 각각 비교해서 문제를 푸는 것인지 의문이다.

## 추가

### 1) 블로그 참조 풀이
도저히 방법이 생각나지 않아서 블로그를 참조했다.

문자열 곱셈을 문자형태의 숫자를 대소비교...  
혼자 생각했으면 절대 생각못했을 것 같다.

1000이하이므로 3번씩 문자열을 써서 길이를 늘린다.  
3 ----> 3 3 3 |  
30 ---> 3 0 3 | 0 3 0  
34 ---> 3 4 3 | 4 3 4   

각 숫자를 문자열로보고 세 번씩 곱하면 위와 같고  
앞 세 자리를 따오면 333, 303, 343 이다.  
이에 대한 대소비교를 하면 343 > 333 > 303 이고 문제에서 원하는  
34, 3, 30 을 얻을 수 있다.

따라서 문제를 해결할 코드는 아래와 같다.   
```python
def solution(numbers):

    sort_numbers = sorted(list(map(str, numbers)), key=lambda x: x * 3, reverse=True)
    # print(numbers)
    answer = "".join(sort_numbers)
    # print(answer)
    return str(int(answer))
```
기존 코드에서 numbers.sort()를 사용했지만 이렇게 하면 리스트안에 
요소들은 int 형태로 존재하고 int 형태로 존재하는 원소들을 join하면 오류가 난다.
따라서 sorted 형태로 정렬할 때 list(map(str))을 사용해서 str로 저장하면서도
list 형태로 저장해둔다. 그리고 위에서 설명했던 것처럼 문자열에 *3을 해서 문제에서 요구하는
정렬을 수행한다.

마지막으로 return str(int(answer))를 해줘야 제출할 때 테스트 11번을 통과할 수 있다.
이는 특이 케이스인 [0, 0, 0] 과 같이 모두 0으로 존재하는 경우에 대비하기 위함이다.
마지막에 str(int(answer))를 해주지 않을 경우 문자열을 join 하므로 "000" 이라는 문자열이 생기고 이는
숫자가 아니므로 테스트를 통과하지 못한다. 물론 문제에서 숫자가 너무 길어 문자열로
표현해달라고 했지만 **숫자**를 문자열로 표현하는 것일뿐 문자열로 표현되기전에
숫자는 숫자여야한다. 따라서 000은 숫자가 아니므로 이와 같은 경우를 0으로 바꾼 뒤에 "0"으로 문자열로
바꾸어주어야한다.

꽤나 생각하기 어려운 문제였다...

### 2) join 함수 사용법

'구분자'.join(리스트)

1)
''.join(list) 를 사용하면 ['a', 'b', 'c'] list를 'abc' 문자열로 합쳐서 반환해준다.

2)
'_'.join(list)를 사용하면 ['a', 'b', 'c'] list를 'a_b_c' 문자열로 합쳐서 반환해준다.

# 참고 사이트
[[프로그래머스] 가장 큰 수 정렬문제 python (200713)](https://huidea.tistory.com/4)    

[[python] 파이썬 join 함수 정리 및 예제 (문자열 합치기)](https://blockdmask.tistory.com/468)
