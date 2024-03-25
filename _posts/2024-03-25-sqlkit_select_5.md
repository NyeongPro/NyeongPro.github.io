---
layout: single
title:  "[SQL]프로그래머스 SQL 고득점 KIT 배운 것들 정리2"
categories: sql
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sqlkit_select_5  # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---


# 1. [조건에 맞는 도서 리스트 출력하기] 문제

## 고찰
select에서 원하는 부분 조회  
단, 주의사항으로 날짜 출력 데이터 포맷 맞출 것.  
where 구문으로 카테고리, 2021 조건 맞출 것.  
order by로 오름차순 정렬.

## 코드 실행
```sql
SELECT BOOK_ID, date_format(PUBLISHED_DATE, '%Y-%m-%d')
FROM BOOK
WHERE CATEGORY = "인문" AND date_format(PUBLISHED_DATE, '%Y') = '2021'
order by PUBLISHED_DATE ASC
```

## 추가
where 절에서 '년도, 달, 일' 만 뽑을 때 **year, month, day** 쓰는 것이 아직 익숙하지 않음.  
year로 바꿨으면 더 고민없이 조건을 걸 수 있었을듯.

# 2. [과일로 만든 아이스크림 고르기] 문제

## 고찰

select 에서 flavor만 조회한다.  
where 절에서 3000 이상인 것들만 조회한다.  
또한 성분 타입이 과일인 것들만 조회한다.
from, join을 통해 테이블을 결합한다.

## 코드 실행

```sql
SELECT FH.FLAVOR
FROM FIRST_HALF as FH
left join ICECREAM_INFO as II
on FH.FLAVOR = II.FLAVOR
where TOTAL_ORDER > 3000 AND II.INGREDIENT_TYPE = 'fruit_based'
order by total_order desc
```
## 추가

flavor가 기본키와 외래키로 묶여 있어서 서로 연결시키는 방법이 따로 있는 줄 알고 조금 헤맸다.  
기본적으로 생각했던 join을 통해 테이블을 묶고 where에서 조건을 전부 해결해주면 됐다.  


# 3. [인기있는 아이스크림] 문제

## 고찰

총주문량을 먼저 내림차순, 같다면 출하 번호를 기준으로 오름차순  

order by에서 ,를 통해 두 기준을 각각 정렬한다.

## 코드 실행
```sql
SELECT FLAVOR
from FIRST_HALF 
order by TOTAL_ORDER desc, SHIPMENT_ID asc
```

# 4. [강원도에 위치한 생산공장 목록 출력하기] 문제

## 고찰
select로 조회하고자 하는 것들 추가.  
공장 ID를 오름차순 정렬

where에서 substr을 이용하거나 like를 이용해 강원도로 시작하는 것을 찾으면 될듯.  

## 코드 실행
```sql
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
from FOOD_FACTORY
where ADDRESS like '강원도%'
order by FACTORY_ID
```
## 추가

```sql
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
from FOOD_FACTORY
where substr(ADDRESS, 1, 3) = "강원도"
order by FACTORY_ID
```

추가로 substr을 이용해 첫 번째 글자부터 세 번째 글자를 잘라오고  

강원도이면 조회하도록 하였음.  

하지만 like를 이용하는게 더 깔끔한듯.

# 참고 사이트
