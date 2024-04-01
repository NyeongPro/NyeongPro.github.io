---
layout: single
title:  "[Algorithm]프로그래머스 베스트앨범 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit7
---

# [베스트앨범] 문제
## 코드
```python
def solution(genres, plays):
    answer = []
    genresPlays = {}
    
    # 1) Hash로 각 장르의 플레이된 총 수 구하기
    for i in range(len(genres)):
        if genres[i] not in genresPlays:
            genresPlays[genres[i]] = plays[i]
        else:
            genresPlays[genres[i]] += plays[i]
    
    # 2) HashMap 리스트화 및 정렬
    genresPlaysList = list(genresPlays.items())
    sortedList = sorted(genresPlaysList, key=lambda x:-x[1])
    # print(sortedList)
    
    # 3) 정렬 형태로 만들기
    genres_play_idx  = []
    for i in range(len(genres)):
        genres_play_idx.append([genresPlays[genres[i]], plays[i], i])
    genres_play_idx.sort(key=lambda x: (-x[0], -x[1], x[2]))
    
    # 4) 정렬된 정보를 통해 베스트 앨범에 들어갈 노래 고유번호 append하기
    preCount = 0
    preGenres = 0
    for i in range(len(genres_play_idx)):
        if preGenres == genres_play_idx[i][0]:  # 같은 장르로 들어오는 경우
            if preCount == 1:  # 한 번 더 수록할 수 있는 경우
                preCount = 2
                answer.append(genres_play_idx[i][2])   
        else:  # 다른 장르로 들어오는 경우
            preCount = 1  # 현재 방문한 새로운 장르의 노래를 1번 수록했다.
            preGenres = genres_play_idx[i][0]
            answer.append(genres_play_idx[i][2])
            
    return answer
```
## 풀이

### 1) Hash로 각 장르의 플레이된 총 수 구하기
HashMap을 통해서 장르는 Key로 넣고 플레이된 총 수는 Value로 설정했습니다.  
그리고 Key값이 일치하는 곳에 plays의 값을 더해주어 play된 총 수를 구했습니다.  

### 2) HashMap 리스트화 및 정렬
dict 형태의 HashMap을 Value 값 기준으로 정렬해봤지만 오류로 안되므로 dict를 리스트화 시키고
정렬했습니다.  
dict를 리스트로 만들고 value 값으로 내림차순 정렬하면 아래와 같이 나옵니다.  

![img.png](/images/2024-04-01/dict-sortedlist.png)

### 3) 정렬 형태로 만들기
1순위로 장르, 2순위로 플레이된 수, 3순위로 인덱스 순으로 정렬을 해야합니다.  
각 장르별로 플레이를 합친 순위대로,  
장르 안에서는 플레이된 수 순위대로,  
장르 안의 플레이된 수가 같다면 인덱스가 낮은 순으로,  
정렬을 해야하기 때문에, sort 함수를 쓰기위해 3가지 정보를 리스트에 담았습니다.  

그리고 장르를 문자열 그대로, "pop", "classic"으로 넣으면 순위를 알 수 없기 때문에
각 문자열을 key로 가지는 value 값을 대신해서 넣어주었습니다. 이렇게 하면 "pop"의 경우 3100회가
플레이 됐고, "classic"의 경우 1450회가 플레이 됐으므로 이 숫자만으로 순위를 정할 수 있습니다.  

### 4) 정렬된 정보를 통해 베스트 앨범에 들어갈 노래 고유번호 append하기
for 루프에서 길이만큼 loop 하면서 각 장르별 최대 노래 2개까지 append 하도록 설계했습니다.  

## 추가

### 다른 풀이
```python
def solution(genres, plays):
    answer = []
    playDic = {}  # {장르 : 총 재생 횟수}
    dic = {}      # {장르 : [(플레이 횟수, 고유번호)]}
    
    # 해시 만들기
    for i in range(len(genres)):
        playDic[genres[i]] = playDic.get(genres[i], 0) + plays[i]
        dic[genres[i]] = dic.get(genres[i], []) + [(plays[i], i)]
    
    # 재생 횟수 내림차순으로 장르별 정렬
    genreSort = sorted(playDic.items(), key = lambda x: x[1], reverse = True)
    
    # 재생 횟수 내림차순, 인덱스 오름차순 정렬
    # 장르별로 최대 2개 선택
    for (genre, totalPlay) in genreSort:
        dic[genre] = sorted(dic[genre], key = lambda x: (-x[0], x[1]))
        answer += [idx for (play, idx) in dic[genre][:2]]
    
    return answer
```
#### 1) dict 2개 만들기
각 정보를 담을 dict를 두 개를 만들었다.  

> 그리고 dic = {} 을 보면 {장르 : [(플레이 횟수, 고유번호)]} 와 같이 value 부분에 (값1, 값2) 형태로 넣을 수 있었다.
> dictionary 형태에 key 하나에 value 하나라고 생각하다보니, "pop"이라는 장르에 여러 개의 곡이 있다면 이 여러 곡을 각자 하나씩 표현할 방법이 없다고 생각했다.  
> 
> 하지만 value를 하나의 리스트로 표현하면 가능해보인다. 아래 이미지를 보면 바로 이해할 것이다.
> value를 하나의 리스트라고 보고 그 리스트에 () 형태의 튜플 형태로 계속해서 추가하면 된다.
> 즉 value가 int 형태일 때 value 에 계속해서 숫자를 더하듯이...

![img.png](/images/2024-04-01/dic-valueinlist.png)

#### 2) HashMap 만들기
playDic 부분을 보면 playDic.get(genres[i], 0) + plays[i] 를 사용하였다.  
```python
if genres[i] not in genresPlays:
            genresPlays[genres[i]] = plays[i]
        else:
            genresPlays[genres[i]] += plays[i]
```
위의 구문과 완전히 같은 코드라 보면 된다.  

playDic과 dic 딕셔너리에 각각 인덱스 i에 대한 
genres 값과 palys 값 그리고 고유번호 (인덱스 i 값)을 넣는다.

> HashMap.get 함수는 key 값을 쭉 흝고 genres[i]라는 key가 있다면 그때의 value를
> 없다면 그 옆의 인자(0)을 뱉어내는 함수이다. 이 함수를 통해 
> 내가 생각한 if, else구문으로 있을 때 없을 때를 구별지을 필요가 없어진다.

#### 3) 재생 횟수 내림차순으로 정렬

```python
genreSort = sorted(playDic.items(), key = lambda x: x[1], reverse = True)
```
위의 코드를 통해서 playDic 딕셔너리를 value 값 기준으로 내림차순 정렬했다.  
playDic 은 각 장르별 총 재생횟수가 value 값으로 들어가있는 딕셔너리이다.  

> value 값으로 정렬이 불가능한 줄 알았지만 items() 함수를 사용해 정렬이 가능했다.  
> items() 함수를 사용하면 (key, value) 에 대한 값으로 반환해준다.  
> 따라서 반환된 값의 첫 번째 인덱스는 key이고 두 번째 인덱스는 value이므로 아래와 같이 활용할 수 있다.

```python
# key에 의한 정렬
dic1 = sorted(dic.items(), key=lambda x: x[0])

# value에 의한 정렬
dic2 = sorted(dic.items(), key=lambda x: x[1])
```  

내림차순은 reverse=True를 추가하거나, -x[0] 와 같이 표현하면 됩니다.

```python
# key에 의한 내림차순 정렬1
dic1 = sorted(dic.items(), key=lambda x: -x[0])

# key에 의한 내림차순 정렬2
dic1 = sorted(dic.items(), key=lambda x: x[0], reverse=True)
```

#### 4) 장르 안에 재생횟수 기준 내림차순, 인덱스 오름차순 정렬
genreSort는 각 장르의 총 재생횟수 기준으로 내림차순 정렬되었기 때문에
for (genre, totalPlay) in genreSort: 를 통해 뽑으면 장르 총 재생횟수가 큰 순서대로 원소가 구성될 것입니다.
여기서 totalPlay는 내림차순 정렬할 때만 사용했기 때문에 이후에는 필요가 없습니다.  
genre는 이제 dic 딕셔너리의 key 값으로 사용됩니다.  
dic[genre]는 genre라는 변수에 저장된 하나의 장르 속에 노래별 재생횟수와 인덱스가 포함되어있습니다.  
그러므로 dic[genre]를 출력해보면 위에서 보여준 아래 이미지의 value 부분과 같게 나올 것입니다.  

![img.png](/images/2024-04-01/dic-valueinlist.png)

그리고 여기서 value부분은 리스트로 이루어져 있고 각각의 요소는 튜플 형태입니다.  
(500, 0), (150, 2), ..., 와 같이 이루어져 있습니다.  

이제 이 형태를 sorted(dic[genre], key = lambda x: (-x[0], x[1])) 를 통해
재생횟수는 내림차순으로 인덱스(고유번호)는 오름차순으로 정렬하면 정렬은 끝이 납니다.  

그리고 슬라이스를 이용했네요. 최대 2개까지 자르고 리스트로 만들어서 answer에 이어줬습니다.  

> 제가 푼 풀이와 비교했을 때 각 정렬해야하는 것 기준으로 정렬하는 개념은 완전히 같았습니다.
> 하지만 다른 풀이에서는 제가 리스트로 만들어서 정렬한 것과 달리 딕셔너리를 만들어서 안에 튜플로 계속해서 추가했습니다.  
> 
> 이를 통해 플레이 횟수와 고유번호가 필요할 때 key 값만으로도 빠르게 접근이 가능하도록 했고
> 마지막에 최대 2개까지 출력하도록 하는 부분에서 각 key 별로 value에 접근이 가능하기 때문에 슬라이스를
> 활용할 수 있는 것까지 더욱 직관적이고 코드도 깔끔했습니다.  

* 딕셔너리에서 value 값에 리스트를 넣어 활용할 수 있다.
* 하나의 리스트에서 정렬 및 슬라이스를 통해 최댓값부터 n개, 최솟값부터 n개에 활용하자.
* 딕셔너리에서 items()에서 나온 key와 value를 x[0], x[1] 를 활용해 정렬까지 할 수 있다.
* dic.get을 활용해 key값이 없을 때 원하는 값이 나오도록 설정할 수 있다.
* 리스트에 + ["string"] 또는 + [숫자] 코드를 작성하면 append와 같은 효과를 보인다.

```python
testList = []
print(testList)
testList = testList + ["a"]
print(testList)
testList += ["1"]
print(testList)
testList += [1]
print(testList)
```

![img.png](/images/2024-04-01/list-plusoperand-ex.png)

# 참고 사이트
[[Python][해시] 프로그래머스 - 베스트 앨범(Level3)](https://johnyejin.tistory.com/50)