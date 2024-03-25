---
layout: single
title:  "[SQL]프로그래머스 SQL 고득점 KIT(흉부외과 또는 일반외과 의사 목록 출력하기)"
categories: sql
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sqlkit_select_1  # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---

# 문제

>DOCTOR 테이블에서 진료과가 흉부외과(CS)이거나 일반외과(GS)인 의사의 이름, 의사ID, 진료과, 고용일자를 조회하는 SQL문을 작성해주세요. 이때 결과는 고용일자를 기준으로 내림차순 정렬하고, 고용일자가 같다면 이름을 기준으로 오름차순 정렬해주세요.

# 고찰

## 진료과가 CS or GS

WHERE 사용, 특히 IN 으로 표현하면 한 방에 가능하다.

## 이름, 의사ID, 진료과, 고용일자를 조회

SELECT 에서 위의 필드를 넣자.

## 이때 결과는 고용일자를 기준으로 내림차순 정렬, 고용일자가 같다면 이름을 기준 오름차순

ORDER BY에서 두 개 정렬을 사용해 내림차순, 오름차순 쓰기

# 코드 실행

```
SELECT DR_NAME, DR_ID, MCDP_CD, HIRE_YMD
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

처음에 위와 같이 코드를 짰다.

하지만 실패...

밑에 주의사항을 읽지 않아 실패

주의사항  
날짜 포맷은 예시와 동일하게 나와야합니다.
{: .notice--danger}

날짜 포맷 수정하는 법을 몰라서 서칭했다.

문제에서 고용일자(HIRE_YMD)는 Type이 DATE이다.

DATE 포맷은 출력하면 아래와 같이 나온다.

![img.png](/images/2024-03-21/dateformat.png)

하지만 문제에서는 YYYY-MM-DD 를 원했기 때문에 채점에 실패한 것이다.

date_format()를 쓰면 원하는 형식으로 출력이 가능하다.

따라서 문제에서 제시하는 형태로 바꾸려면 아래와 같이 코드를 수정해야한다.

```
SELECT DR_NAME, DR_ID, MCDP_CD, date_format(HIRE_YMD, '%Y-%m-%d')
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

SELECT에서 조회(또는 출력)할 때 HIRE_YMD를 '%Y-%m-%d' 형태로 출력해주세요 라고 수정한 것이다.

# 추가

## date_format에서 대문자와 소문자의 차이?
참고한 블로그에서 년도 부분은 대문자 Y 나머지는 소문자 m, d 를 사용했는데
이 부분이 바뀌면 출력이 안되는지 궁금해서 실험했다.

아무런 변화가 없을 것이라 생각했는데 큰 변화가 있었다...
{: .notice--success}
```
date_format(HIRE_YMD, '%Y-%m-%d') 를
date_format(HIRE_YMD, '%y-%M-%D') 로 변경해보았다.
```

![img.png](/images/2024-03-21/date_yMD_change.png)

>년도 부분은 YYYY에서 YY로 변했다.  
>월 부분은 숫자에서 영어 표기로 변했다.  
>일 부분도 숫자에서 영어 표기로 변하였다.  
>참고하면 좋을 듯 하다.

## date_format(HIRE_YMD, '%Y-%m-%d') 조회 후 필드 이름

다른 분을 참고했을 때 나와 달랐던 점이

```
date_format(HIRE_YMD, '%Y-%m-%d') 이 부분을
date_format(HIRE_YMD, '%Y-%m-%d') AS HIRE_YMD 라고 작성하였다.
```
>내가 짠 코드에서 코드 실행을 하면 확실히 date_format(HIRE_YMD, '%Y-%m-%d') 라고 뜬다.    
>이러한 사소한 것까지 반영된다면 확실히 틀린 답이 될 것 같다.  
>제출 후 채점하기에서는 통과했지만 주의해야겠다.  

## date_format 참고 사이트

>[여기](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_date-format)에서 굉장히 많은 포맷이 있다.
>전부 외우기는 어려울 듯 하고 자주 사용하는 것 위주로는 익숙해질 필요가 있을 듯 하다.

# 참고 사이트
[[MySQL] 날짜, 시간 표기 방식 지정하기 DATE_FORMAT()](https://lightblog.tistory.com/155)  

[12.7 Date and Time Functions](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_date-format)