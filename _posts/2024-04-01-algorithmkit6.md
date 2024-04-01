---
layout: single
title:  "[Algorithm]프로그래머스 의상 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit6
---

# [의상] 문제
## 코드
```python
def solution(clothes):
    answer = 1
    kindCloth = []
    for cloth, kind in clothes:
        kindCloth.append(kind)
    setKindCloth = list(set(kindCloth))
    for i in range(len(setKindCloth)):
        count = kindCloth.count(setKindCloth[i])
        answer *= (count+1)
    
    return answer-1
```
## 풀이
해시 문제이지만 해시로 풀지는 않았습니다.  

아래처럼 상황이 주어졌다고 가정하겠습니다.  

얼굴 : 동그란 안경, 검정 선글라스  
상의 : 파란색 티셔츠  
하의 : 청바지  
겉옷 : 긴 코트  

여기서 각 종류마다 '없음' 을 추가 시키고 각 종류에서 하나씩 뽑는 경우를 생각하겠습니다.  
그렇다면 예를 들어 얼굴:'없음', 상의:'없음', 하의:'청바지', 겉옷:'긴 코트' 와 같이 뽑힐 수 있겠네요.  
이러한 경우들이 모두 하나의 조합이 됩니다. 

따라서 모든 경우의 수는 각 종류에서 하나씩 뽑아서 곱해주면 됩니다.  

그래서 각 종류의 +1을 해준 것들을 곱해줍니다.  '없음'이라는 경우가 각 종류마다 하나씩 들어가기 때문이죠.  

그래서 저 위의 예시는 (2+1) * (1+1) * (1+1) * (1+1)이 됩니다.  
여기서 모두 '없음'인 경우는 더해주면 안되므로 저 곱셈식에서 -1을 해줍니다.  

따라서 (2+1) * (1+1) * (1+1) * (1+1) - 1 이 answer가 됩니다.  

이 개념을 생각해서 문제를 풀이했습니다.  

먼저 같은 이름을 가진 의상은 존재하지 않는다고 제한되어 있습니다.  
따라서 옷의 각 종류마다 개수만 세주면 되기 때문에 이름은 아예 배제해도 됩니다.  

그래서 kindCloth 라는 list를 만들고 이 리스트에 옷의 종류만 append합니다.  

그럼 입출력 예제 1번에서 아래와 같은 예시가 주어집니다.  

```python
[["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]
```
그러면 이 kindCloth 리스트에는 ["headgear", "eyewear", "headgear"] 순서대로 append 되겠네요.  
이제 (headgear의 개수 + 1) * (eyewear + 1) - 1 만 해주면 답이 됩니다.  
그래서 set 함수로 중복을 제거해서 각 요소에 맞게 count 함수를 써서 개수를 구해주고, 각 개수에 +1을 해주고 계속 곱해주었습니다.  

마지막으로 return 할 때 -1을 해주어 모두 '없음' 일 때를 배제했습니다.  


## 추가

### 다른 풀이1
```python
def solution(clothes):
    clothes_type = {}

    for c, t in clothes:
        if t not in clothes_type:
            clothes_type[t] = 2
        else:
            clothes_type[t] += 1

    cnt = 1
    for num in clothes_type.values():
        cnt *= num
    return cnt - 1
```
옷의 이름인 c를 신경쓰지 않는 것은 똑같다.  
하지만 dictionary를 이용하고 key값을 이용해 같은 옷의 종류일 때 그 value에 접근이 용이하도록 하였다.  

처음에 t(옷의 종류)가 clothes_type에 없는 경우 2로 설정한다. 왜냐하면 없었다면 이번에 하나를 추가하면서도
'없음' 이라는 것도 하나 추가로 들어가기 때문에 처음부터 2로 설정한다. 그 후 이미 있던 것에 접근하면 
같은 종류에 옷이 하나 더 추가되기 때문에 +=1을 해준다.  

그리고 dictionary.value() 함수를 이용해서 모든 dict의 value에 접근한다.  

### 다른 풀이2
```python
def solution(clothes):
    # 1. 옷을 종류별로 구분하기
    hash_map = {}
    for clothe, type in clothes:
        hash_map[type] = hash_map.get(type, 0) + 1
        
    # 2. 입지 않는 경우를 추가하여 모든 조합 계산하기
    answer = 1
    for type in hash_map:   
        answer *= (hash_map[type] + 1)
    
    # 3. 아무종류의 옷도 입지 않는 경우 제외하기
    return answer - 1
```
#### 1) HashMap 만들기
Key-Value 쌍을 가지는 dict 만들기
여기서 Key는 옷의 종류, Value는 옷 종류의 가짓수를 의미한다.

#### 2) clothes배열에 존재하는 옷 종류에 대해 count 테이블 만들기
종류
"headgear" : 2
"eyegear" : 1  

위와 같이 만들기

이러한 Key-Value 테이블을 만드는 것을 Hashing 한다라고 표현함.  
HashMap.get(type, 0) 함수를 활용함.
* 이 함수는 type이라는 Key에 해당하는 Value를 가져오고, 만약 type이라는 Key가 존재하지 않는다면 0을 가져온다.  
* Value는 곧 Key라는 옷 종류의 개수를 의미하므로 +1을 해주면서 다시 넣어준다.  
* 만약 그 Key가 없었다면 HashMap.get은 0을 반환하고 +1을 해주면서 옷 종류의 개수는 그대로 1이된다.  

#### 3) 모든 경우의 수를 구하기
모든 경우의 수를 구하는 것은 위에서도 설명했듯이 각 옷 종류의 개수+1 만큼 각각 곱해주면 된다.  
그래서 headgear가 2개였고, eyewear가 1개 였으므로 각각 1씩 더해주어서
(2+1) * (1+1) = 6이 된다.

#### 4) 옷을 하나도 안입은 경우 제외
3번에서 6개의 경우가 나왔다. 이 경우는 모든 종류의 옷을 하나도 안 입은 경우도 포함되어있기 때문에
-1을 빼주면서 return하면 된다.

# 참고 사이트
[[알고리즘] 프로그래머스 의상 파이썬](https://velog.io/@helenason/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%9D%98%EC%83%81-%ED%8C%8C%EC%9D%B4%EC%8D%AC)

[[프로그래머스] 위장 (해시 Lv. 2) - 파이썬 Python](https://coding-grandpa.tistory.com/88)
