---
layout: single
title:  "[Algorithm]프로그래머스 코딩테스트 고득점 KIT 여러 문제1[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit1 # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---

# 1. [폰켓몬] 문제
## 코드
```python
def solution(nums):
    set_nums = set(nums)
    answer = min(len(set_nums), len(nums)//2)
    return answer
```

## 풀이
처음엔 조합을 생각해서 NCN/2 만큼 for loop를 돌려서 해결하려했다.  
예를 들어 N이 6이라면 6C3번 선택하는 단계가 있을 것이다.  
그리고 그 15번의 선택 단계에서 set 함수를 사용해 중복을 제거한다.  
그리고 그 결과의 len 함수를 붙여 길이를 구하면 각 선택 단계에서의 몇 종류를 선택했는지 알 수 있을 것이다.  

하지만 for loop를 N/2 번 중첩 시켜야하는 것을 깨달았다.
N이 4개 일 때는 단순히 for loop를 두 번 사용하면 전부 해결될 거라 생각했던 탓이다.  

따라서 풀이방법을 바꾸었다.  
처음부터 set을 사용해 중복을 제거하면 가질 수 있는 폰켓몬 종류 수를 금방 알아챌 수 있었다.  

### 1) 주어진 폰켓몬 가짓수 > 뽑을 수 있는 폰켓몬(N/2)

예를 들어 첫 주어진 nums에서 [1, 2, 3, 4, 5, 5] 라고 주어졌다고 치자.  
내가 뽑을 수 있는 폰켓몬이 정해지지 않았다면 1, 2, 3, 4, 5를 전부 뽑아서 최대 5가지 종류를 가질 수 있다.  
하지만 N/2마리만 뽑을 수 있으므로 이 경우는 N/2가 내가 가질 수 있는 최대 종류 수이다.

### 2) 주어진 폰켓몬 가짓수 < 뽑을 수 있는 폰켓몬(N/2)

예를 들어 첫 주어진 nums에서 [1, 1, 1, 2, 2, 2] 라고 치자
그러면 아무리 N/2를 뽑는다 해도 최대 종류의 수는 2마리 이상을 넘을 수 없다.  

결과적으로 주어진 폰켓몬 가짓수, 뽑을 수 있는 폰켓몬 수 중에서 최솟값을 찾으면 된다.  

```python
set_nums = set(nums)
answer = min(len(set_nums), len(nums)//2)
```
따라서 위와 같은 코드로 둘 중 최솟값을 찾아 return 했다.

# 2. [완주하지 못한 선수] 문제
## 코드
```python
def solution(participant, completion):

    for i in range(len(completion)):
        x = completion[i]
        participant.remove(x)
    answer = participant[0]
    
    return answer
```
## 풀이
완주자 리스트에서 하나씩 뽑아와서 참여자 리스트에서 비교하고 하나씩 제거하려했다.  
하지만 하나 뽑아온 문자열을 참여자 리스트에서 모두 비교하니 시간초과로 실패했다.  

다음 풀이로 참여자와 완주자 리스트를 정렬하고 각 자리를 비교해서 값이 틀린 곳에
문제에 답이 있는 방식으로 접근했다.

```python
def solution(participant, completion):

    participant.sort()
    completion.sort()

    if len(participant) == 1:
        answer = participant[0]
    else:
        check = False
        for i in range(len(completion)):
            if participant[i] != completion[i]:
                check = True
                break


        if check:
            answer = participant[i]
        else:
            answer = participant[i+1]
    
    return answer
```

답이 굉장히 길어졌는데 모든 테스트에 대해 통과하려다 보니 그렇게 됐다...

먼저 이런 방식으로 체크하면 비교할 때 두 가지를 체크해야한다.

1) 각 인덱스 별로 비교하다가 중간에서 다른 값을 찾는 경우
2) 각 인덱스 별로 비교하다가 끝까지 다른 값을 찾지 못한 경우

1)의 경우 그대로 i가 완주하지 못한 참가자가 됐다.  
하지만 2)의 경우 완주하지 못한 참가자는 participant list의 마지막 인덱스가 됐다.

따라서 participant[i+1]로 인덱스를 하나 증가시켜 완주하지 못한 참가자를 판별했다.  

여기서 글을 쓰다보니 participant list의 마지막 인덱스가 완주하지 못한 사람인데
그렇다보니 [-1]로 인덱스를 지정하면 더 편하다는 것을 깨달았다. 그리고 이렇게 인덱스를 지정할 경우
참가자가 한 명인 테스트 케이스에 대해서도 따로 지정하지 않아도 된다는 것을 알았다...
이에 대한 코드는 아래에서 다시 적겠다.
{: .notice--success}

그리고 제출할 때 계속해서 하나가 런타임 에러가 났다.  
참가자가 한 명일 때의 테스트 케이스를 고려하지 못해서 인 것 같아 그에 대한 코드를 따로 짰다.  

그것이 바로 이 아래 코드이다.

```python
if len(participant) == 1:
        answer = participant[0]
    else:
```

위의 코드를 추가해서 참가자가 한 명일 때를 생각해주니 모든 테스트 케이스를 통과했다.

그리고 아까 위에서 리스트 마지막 인덱스를 [-1]로 하면 되겠다는 생각이 나서 이를 추가했더니
[i+1]에서 인덱스 에러가 났던 것도 사라졌다.  
따라서 조금은 더 깔끔해졌다.  

```python
def solution(participant, completion):

    participant.sort()
    completion.sort()
    
    check = False
    for i in range(len(completion)):
        if participant[i] != completion[i]:
            check = True
            break


    if check:
        answer = participant[i]
    else:
        answer = participant[-1]
    
    return answer
```
## 추가

hash 문제인데 나는 sort로 풀어서 다른 블로그를 참고하여 hash 방식으로 풀고자 한다.
이제껏 코딩할 때 hash를 사용해본적이 없어서 다른 블로그를 참조하며 공부했다.  

### 1) HashMap 만들기
* **HashMap**이란 Key-Value의 Pair를 관리하는 클래스이다.
* 이 문제에서 Key는 Hash 값이고, Value는 각 선수의 이름이다.

### 2) HashMap에 Participant 추가하기
* HashMap에 Participant를 전부 추가해보자.
* 위 코드의 동작 방식을 아래와 같이 설명하여 알아본다.

participant = ["C", "A", "B"]
completion = ["C", "B"]

#### Index 0 일 때

participant의 C를 가리키고 있다(Index 0이므로)

따라서 Key-Value 는 {17:"C"} 이다.

#### Index 1 일 때

participant의 A를 가리키고 있다(Index 1이므로)

따라서 Key-Value 는 {17 : "C", 5 : "A"} 이다.

#### Index 2 일 때

participant의 B를 가리키고 있다(Index 2이므로)

따라서 Key-Value 는 {17 : "C", 5 : "A", 13 : "B"} 이다.

Key-Value 쌍을 표로 정리하면 아래와 같겠네요.

![img.png](/images/2024-03-26/key-value-img.png)

그리고 모든 Hash(Key) 값을 더해주면 17+5+13 = 35가 됩니다.

### sumHash에서 완주한 선수 Hash 값 빼기

이제 위의 예제처럼 완주자 List에서 Index 0부터 1까지 돌아가며 Hash값을 빼줍니다.

완주자가 "C", "B" 였다면 "C"의 Key 값인 17과 "B"의 Key 값인 13을 35에서 빼줍니다.  

그렇다면 35 - 17 - 13 = 5 임을 알 수 있습니다.

이제 이 5를 통해 5라는 값을 Key 값으로 가진 참가자를 알 수 있겠네요.

5 라는 Key를 가진 사람은 "A"로 이렇게 완주자 List 에서 한 명을 찾아낼 수 있습니다.

### 코드
```python
def solution(participant, completion):
    hashDict = {}
    sumHash = 0
    
    # 1. Hash : Participant의 dictionary 만들기
    # 2. Participant의 sum(hash) 구하기
    for part in participant:
        hashDict[hash(part)] = part
        sumHash += hash(part)
    
    # 3. completion의 sum(hash) 빼기
    for comp in completion:
        sumHash -= hash(comp)
    
    # 4. 남은 값이 완주하지 못한 선수의 hash 값이 된다

    return hashDict[sumHash]
```

이렇게 보니 hash는 완전히 Dictionary를 쓸 때와 똑같네요.  
만들 때도 dict 형태로 만들고...  
대신 dict를 쓰면서 Key 값으로 값을 계산하고 이를 더하고 빼면서 추론하는게 신기하네요.  
잘 배워둬야 겠습니다.


# 3. [전화번호 목록] 문제
## 코드

위에서 배운 Hash로 풀어보려했으나 도저히 방법이 떠오르지 않아 생각난 방법대로 일단 풀어봄.  

```python
def solution(phone_book):
    
    answer = True
    check = False
    for x in phone_book:
        if check == True:
            break
        for i in range(len(phone_book)):
            if x == phone_book[i]:
                continue
            if len(x) >= len(phone_book[i]):
                continue
            if x == phone_book[i][0:len(x)]:
                answer = False
                check = True
                break

    return answer
```
## 풀이

for loop 에서 phone_book의 첫 번째 인덱스 값부터 하나를 뽑아서
다른 인덱스의 값의 앞 부분만 비교함.
이때 다른 인덱스의 값을 슬라이스했음.
슬라이스 간격은 처음에 뽑았던 값의 길이와 동일함.
하나라도 접두어인 경우라면 모든 for loop를 종료시키도록 하여
시간이 덜 들도록 하였음.

하지만 효율성 테스트 4개 중 2개가 시간 초과 걸림...

## 추가

###  다른 풀이1

sort를 활용해서 풀어보자

sort는 문자열에서 한 문자 단위를 기준으로 sort 해준다.  

예를 들어 "ABCE", "ABCD" 두 문자열이 있다고 가정하자.  

그렇다면 첫 번째 인덱스를 먼저 비교한다. 서로 "A"이므로 다음 인덱스  
그렇다면 두 번째 인덱스를 비교한다. 서로 "B"이므로 다음 인덱스
이렇게 네 번째 인덱스에 도달하면 "E"와 "D" 이므로 D가 앞서게 된다.

따라서 "ABCD"가 앞서게 되고 그 이후 "ABCE"가 오게된다.

이를 활용하면 두 문자열에서 일정 인덱스까지 겹친다는 것을 빠르게 파악할 수 있다.  

그러면 문제의 예시로 다시 살펴보자.  

첫 번째 입출력을 예시로 보면

["119", "97674223", "1195524421"] 이 입력됐다.

이 리스트를 sort 하면 

["119", "1195524421", "97674223"] 로 정렬이 된다.

이때 앞뒤만 확인하면 접두어가 있는지 아닌지 빠르게 파악이 가능하다.

위의 내가 짠 코드에서 for loop를 두 번 돌려서 내가 아닌 인자에 대해 모두 확인할 필요가 없어진다.

내 코드에서 sort를 넣는다면 아래와 같은 코드가 될 것 같다.

```python
def solution(phone_book):
    
    answer = True
    phone_book.sort()
    
    for i in range(len(phone_book)-1):
        if phone_book[i] == phone_book[i+1][0:len(phone_book[i])]:
            answer = False
            break

    return answer
```
인덱스 기준으로 오른쪽 자료(i+1)를 앞에서 부터 왼쪽 자료의 길이만큼 자르면 접두어를 확인할 수 있다. 

### 다른 풀이2

Hash 풀이를 배우기 위해 다른 블로그를 참고했다.

코드는 아래와 같다.

```python
def solution(phone_book):
    # 1. Hash map을 만든다
    hash_map = {}
    for phone_number in phone_book:
        hash_map[phone_number] = 0
    
    # 2. 접두어가 Hash map에 존재하는지 찾는다
    for phone_number in phone_book:
        jubdoo = ""
        for number in phone_number:
            jubdoo += number
            # 3. 접두어를 찾아야 한다 (기존 번호와 같은 경우 제외)
            if jubdoo in hash_map and jubdoo != phone_number:
                return False
    return True
```

#### 1) hashMap 만들기, 모든 전화번호 Hashing 하기

- Key는 phone_number로 Value는 1로 설정한다.  
- HashMap은 위와 같이 만들어주는게 정석이랍니다...  
- Value의 값은 숫자가 1개 존재한다는 의미라고 합니다.

#### 2) 접두어가 HashMap에 존재하는지 확인하기.

길이, 1부터 (전체길이-1) 까지 substring을 뗀다.  

그리고 HashMap에 존재하는지 확인한다.  

if jubdoo in hash_map and jubdoo != phone_number 를 통해
지금 들고 있는 substring이 hash_map의 key 속에 있는지 확인한다.  

그리고 같은 문자열이 아닐 때인지도 같이 조사한다.

# 참고 사이트
[[프로그래머스] 완주하지 못한 선수 문제 풀이(해시 Lv. 1) - 파이썬 Python](https://coding-grandpa.tistory.com/85)  

[[프로그래머스] 전화번호 목록 문제 풀이(해시 Lv. 2) - 파이썬 Python](https://coding-grandpa.tistory.com/86)
