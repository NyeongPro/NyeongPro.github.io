---
layout: single
title:  "[SQL]프로그래머스 SQL 고득점 KIT(평균 일일 대여 요금 구하기)"
categories: study
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sqlkit_select_3  # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---

# 문제

> CAR_RENTAL_COMPANY_CAR 테이블에서 자동차 종류가 'SUV'인 자동차들의 평균 일일 대여 요금을 출력하는 SQL문을 작성해주세요. 이때 평균 일일 대여 요금은 소수 첫 번째 자리에서 반올림하고, 컬럼명은 AVERAGE_FEE 로 지정해주세요.
# 고찰

## 자동차 종류가 'SUV'

where 절 사용

단순하게 생각했으나 문제를 제대로 살펴보니 SUV에도 여러 옵션이 붙으면 가격이 달라지므로  
SUV를 그룹지어 생각해야할듯함.

## 평균 일일 대여 요금 출력

select 에서 avg를 활용해 출력.

## 평균 일일 대여 요금은 소수 첫 번째 자리 반올림

처음 보는 것이라 서칭을 통해 찾아야겠다.

### MYSQL 반올림 함수 

ROUND() 를 사용해 반올림을 구현한다.

ROUND() 함수는 2개의 인자를 받고 각각 **숫자, 반올림할 자릿수** 이다.

**따라서 ROUND(1234.56789, 4) 는 1234.5679 를 출력할 것이다.**

반올림할 자릿수는 생략과 - 값도 가능하다.  
생략은 소수점 첫 번째 자리를 뜻하고 -는 소수점 기준 왼쪽으로 몇칸을 갈지이다.  
따라서 ROUND(1234.56789, -2)는 1200이다.
{: .notice--info}

## 컬럼명은 AVERAGE_FEE

AS 문을 활용해 출력될 때 이름 변경

# 코드 실행

처음에 GROUP BY로 CAR_TYPE을 그룹지으면 되겠다는 생각에 
```
SELECT ROUND(AVG(DAILY_FEE)) as AVERAGE_FEE
FROM CAR_RENTAL_COMPANY_CAR
GROUP BY CAR_TYPE
```
와 같이 코드를 실행함

하지만 이렇게 하면 모든 CAR_TYPE을 그룹지어서 CAR_TYPE이 총 4개였다면 행도 4개가 찍히게 됨.  

따라서 먼저 WHERE 절로 CAR_TYPE을 SUV만 검색하도록 조건을 건 뒤에 GRUOP BY로 그룹화해야  

SUV에 대한 것만 뜨면서 SUV를 그룹지은 AVG가 뜨게됨.

# 참고 사이트
