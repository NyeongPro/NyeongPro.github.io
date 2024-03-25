---
layout: single
title:  "[SQL]프로그래머스 SQL 고득점 KIT 배운 것들 정리4"
categories: sql
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sqlkit_summaxmin_2 # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect\
---

# 1. [최댓값 구하기] 문제

## 고찰

select 에서 DATETIME as 시간으로 이름 변경해주기.  

order by로 DATETIME 내림차순 가능하다면 내림차순 해주기.

limit 1 을 통해 제일 윗줄 한 줄만 출력하기.
## 코드 실행
```sql
SELECT DATETIME as 시간
from ANIMAL_INS 
order by DATETIME desc
limit 1
```
다행히 DATE 형식도 내림차순(가장 최근 시간)으로 정렬가능하여 쉽게 해결.  


# 2. [최솟값 구하기] 문제

## 고찰

위 최댓값 구하기에서 asc 만 바꾸면 될 것 같음.  

실행 시 그대로 성공함.  

왜 최댓값은 LEVEL 1이고 이것은 LEVEL 2 인지...

## 코드 실행

```sql
SELECT DATETIME as 시간
from ANIMAL_INS 
order by DATETIME asc
limit 1
```

order by와 limit 으로 조건에 맞는 한 행을 출력하기 너무 쉬워짐.  

# 3. [동물 수 구하기] 문제
## 고찰
count 절을 사용하면 될 것 같음.
## 코드 실행
```sql
SELECT count(ANIMAL_ID) as count
from ANIMAL_INS
```
위 코드로 너무 쉽게 해결.

# 4. [중복 제거하기] 문제
## 고찰
중복 제거는 distinct 구문 사용.
## 코드 실행
```sql
SELECT count(distinct NAME) as count 
from ANIMAL_INS 
```
## 추가

### 4.1) distinct 위치에 대한 문제

```sql
1)
SELECT distinct count(NAME) as count 
from ANIMAL_INS 
2)
SELECT count(NAME) as count distinct
from ANIMAL_INS 
```
위 두 코드 모두 오류가 떴음.  
본인이 어떤 필드에 대해 중복을 제거할 건지 정확히 구분하고 위치를 잘 생각 해야 할듯

### 4.2) NULL이 아닐 때

NAME이 NULL 일 때는 표현하지 않도록 해주어야 하는데 아무것도 해주지 않았음.  
아무것도 해주지 않아도 통과는 되지만 다른 문제에선 발목을 잡을 수 있으니 확실히 해두자.  

`WHERE NAME IS NOT NULL` 이라는 코드를 통해 NAME이 NULL이 아닐 때만 동작하도록 조건을 걸 수 있다. 

