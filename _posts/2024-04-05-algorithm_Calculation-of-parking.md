---
layout: single
title:  "[Algorithm]프로그래머스 주차 요금 계산 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithm_Calculation-of-parking
---

# [주차 요금 계산] 문제
## 코드
```python
import math
def solution(fees, records):
    answer = []
    
    # fees 리스트를 미리 할당함.
    hour, fee, perhour, perfee = fees[0], fees[1], fees[2], fees[3]
    
    
    carDict = {}  # HH, MM, hour, inout
    for record in records:
        
        # 1) 시간, 분, 차량번호, 입차/출차 받아옴.
        HH = int(record[:2])
        MM = int(record[3:5])
        carNum = record[6:10]
        inout = record[11:]
        
        # 2) if절로 IN일 때, OUT일 때 구분
        # 2.1) IN 일 때
        if inout == 'IN':
            if carNum in carDict:  # key가 이미 존재.
                carDict[carNum][0] = HH
                carDict[carNum][1] = MM
                carDict[carNum][3] = inout
            else:  # key가 없음(한 번도 입차하지 않음)
                carDict[carNum] = [HH, MM, 0, inout]
        # 2.2) OUT 일 때
        else:
            preHH = carDict[carNum][0]
            preMM = carDict[carNum][1]
            HH = HH - preHH
            MM = MM - preMM
            carDict[carNum][2] += HH * 60 + MM
            carDict[carNum][3] = inout
    
    # 3) IN 하고 OUT 안 한 차량에 대해 계산
    for _, value in carDict.items():
        if value[3] == "IN":
            HH = 23 - value[0]
            MM = 59 - value[1]
            value[2] += HH * 60 + MM
            
    # 차량 번호에 대해 정렬
    carDict_sort = sorted(carDict.items(), key=lambda x: x[0])
    
    # 4) 이용 시간에 따른 비용 계산하기
    for _, value in carDict_sort:
        totalHour = value[2]
        if totalHour <= hour:
            result = fee
        else:
            result = fee + math.ceil((totalHour - hour) / perhour) * perfee
        answer.append(result)
        
    return answer
```
## 풀이

### 1) 시간, 분, 차량번호, 입차/출차 받아옴.
slice를 통해 각각 시간, 분, 차량번호, 입출차 스트링을 받아왔습니다.

### 2) if절로 IN일 때, OUT일 때 구분

#### 2.1) IN 일 때 
IN 일 때는 처음 입차하는 차량일수도, 한 번 출차했다가 다시 입차할 수도 있다.  

따라서 key가 이미 존재한다면, 이미 한 번 입차했던 차량으로 carDict에 현재 값들을
할당해준다.

그리고 key가 없다면 처음 입차하는 차량으로 carDict에 key와 value들을 한 번에 할당해준다.

그리고 두 가지 경우 HH, MM 같이 시간과 관련된 경우 당연히 출차시 몇분을 이용했는지 계산을
해주기 위해 당연히 필요하다.
하지만 IN, OUT의 경우 만약 문제에서 IN이 들어오고 출차를 하지 않는 경우가 없었다면 이 부분은 필요없었을 것이다.
IN이 있었다면 무조건 OUT이 있기 마련인데, 이 문제의 경우 OUT이 없는 경우 23:59으로 계산해주어야한다.

따라서 이 차량이 IN만 있었을 경우를 대비해서 IN, OUT에 대해 저장해준다.

#### 2.2) OUT 일 때
OUT일 때는 문제 조건으로 무조건 IN이 한 번 들어왔다. 
따라서 key가 없을 때를 생각할 필요가 없다.

그리고 IN에서 저장했던 시간을 이용해 이용시간을 구할 수 있다.
이전 value에 저장된 HH, MM을 preHH, preMM이라 할당하고 현재 값과 차를 계산한다.

이제 HH는 시간이므로 60을 곱해주고 MM은 분이므로 그대로 더해주고 carDict에 넣어준다.

그리고 inout부분에 덮어준다. 

### 3) IN 하고 OUT 안 한 차량에 대해 계산
IN하고 OUT을 안한 경우 입차한 시간과 23시 59분 출차한 것으로 치고 계산해주어야한다.

### 4) 이용 시간에 따른 비용 계산하기
기본 이용 시간보다 작은 경우 기본 요금만 계산한다.

기본 이용 시간을 넘어섰을 때 문제의 요구 사항에 따라 넘친 시간에 단위 요금을 부과한다.

이때 import math를 통해 라이브러리를 추가하고, math.ceil 함수를 이용해 올림을 계산한다.
