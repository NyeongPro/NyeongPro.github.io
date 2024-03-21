---
layout: single
title:  "[SQL]코테 대비 SQL 공부"
categories: study
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sql-practice  # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---

여러 공기업 및 대기업 코딩테스트 대비 SQL을 스스로 공부하기 위해서 포스팅합니다.  
다른 블로그 참조하면서 공부하고 프로그래머스 SQL 고득점 Kit 활용해서 문제도 풀겠습니다.
{: .notice--info}
# 1. SQL 기본

## 1) TABLE

**SQL에서 테이블은 행(row)와 열(column)을 가진 표와 같이 생긴 데이터베이스.**  

각 테이블에서 특정한 개념에 대한 정보를 모아두고 데이터를 저장함.

아래는 **MySQL**과 **DBeaver** 프로그램을 활용해서 만들어 본 **employee** 테이블이다.  

![employee-tabel](/images/2024-03-21/employee-table.png)

## 2) COLUMN

**테이블 내에서 컬럼은 "필드" or "속성" 이라고 함.**  

테이블에서 저장하고 관리하는 각각의 정보 유형이다.  

"employee" 테이블에서 컬럼은 `부서`, `봉급`, `이름` 등이 될 수 있다.  

# 2. SQL 기본 문법

SQL에는 많은 문법이 있다.  

적는 순서는 다음과 같다.  

SELECT - DISTINCT - FROM - JOIN - ON - WHERE - GROUP BY - HAVING - ORDER BY - LIMIT - OFFSET  
{: .notice--success}

## 1) SELECT

**SQL에서 특정 데이터를 출력할 때 사용.**

아래 SQL 코드로 SELECT 문을 사용한다.

```
SELECT col(또는*)
FROM table
```

1행의 *를 통해 모든 column을 검색할 수 있다.

아래는 **employee** 테이블에서 모든 컬럼을 조회한 이미지이다.

![img.png](/images/2024-03-21/all-column-select.png)

### 1.1) SUM, MAX, MIN  

**각각 SUM(합), MAX(최댓값), MIN(최솟값) 을 구할 수 있는 집계함수.**

```
SUM(col)
MAX(col)
MIN(col)
```

위의 코드를 통해 각각 col에서 함수를 적용한다.

SELECT 문과 함께 아래와 같이 사용할 수 있다.

```
SELECT SUM(col)
FROM table
```

위의 코드를 해석하면 table 이라는 table에서 col 열의 합을 불러와 출력한다.

SUM, MAX, MIN 이외에 **AVG(col의 평균), COUNT(col의 개수)**를 사용할 수도 있다.
{: .notice--info}

아래는 봉급에 대해 각각 SUM, MAX, MIN, AVG, COUNT 한 내용이다.

![img.png](/images/2024-03-21/SUM_MAX_(sal)_etc.png)  

## 2) DISTINCT

DISTINCT는 중복제거 구문이다.  

**SELECT로 DB에서 컬럼을 가져올 때, 중복되는 값을 제거할 때 사용한다.**  

이에 DINSTINCT 구문을 사용한 COL(필드)에서 중복 값을 하나로 합쳐 한 번만 출력한다.  

사용법은 다음과 같다.
```
SELECT DISTINCT col
FROM table
```

아래 사진은 봉급에 대해 DISTINCT를 적용했다.  

**봉급에서 중복된 값을 제거하고 각 하나씩만 출력해준다.**

파이썬에서 집합(SET)과 매우 비슷하네요.

![img.png](/images/2024-03-21/distinct_ex.png)

## 3) FROM

FROM 은 조회할 테이블을 선택하는 구문이다.
사용법은 아래와 같이 매우 간단하다.

```
SELECT 이름
FROM employee
```

"employee"이라는 테이블 명을 가진 테이블에서 "이름"이라는 필드를 조회한다.

> 참고로
> ```
> SELECT *  
> FROM employee
> ```
> 위와 같이 "employee" 테이블의 모든 필드를 "*"를 사용하여 조회할 수도 있다.

## 4) WHERE

### 4.1) 기본 문법
```
SELECT *
FROM employee
```
위를 사용하여 "employee" 라는 테이블에서 모든 필드를 조회할 수 있다.  

그런데 만약 모든 정보가 아닌 "직책" 이라는 필드에서 '사원' 이라는 데이터만 조회하고 싶다면?  

또는 "봉급" 이라는 필드에서 "500" 이상을 가진 데이터만 조회하고 싶다면?  

**이럴 때 테이블에서 특정 조건에 맞는 데이터만 가져올 수 있도록 하는 조건문이 WHERE 구문이다.**  

예를 들어 "직책" 필드에서 '사원' 데이터를 가진 데이터만 조회하려면 아래와 같다.

```
SELECT * FROM employee
WHERE 직책 = '사원'
```

그렇다면 "봉급" 필드의 500 이상 데이터만 가진 행을 조회할 때는 아래와 같다.

```
SELECT * FROM employee
WHERE 봉급 >= 500
```

WHERE 절에서는 여러 연산자가 사용된다.

대소를 비교하는 **>, <, >=, <=** 등이 있으며  
같거나 같지 않음을 위해 사용되는 **=, !=** 등이 있다.
{: .notice--info}  

### 4.2) 다중 조건

WHERE 절에서 여러 조건을 한 번에 사용해야 할 수 있다.  



#### 논리연산자(AND, OR)

예를 들어 **'사원' 이면서, '봉급'이 500이상인 데이터를 조회하라** 와 같을 수 있다.  

이러한 경우 **논리 연산자(AND, OR)**을 사용하여 해결할 수 있다.

아래와 같은 쿼리로 해결한다.

```
SELECT * FROM employee
WHERE 직책 = '사원' AND 봉급 >= 500
```
AND를 사용하면서 다중 조건을 해결한다.

이와같이 OR 논리 연산자도 사용할 수 있다.

예를 들어 **'사장' 이거나, '봉급'이 1000이상인 데이터를 조회하라**는 아래와 같다.

```
SELECT * FROM employee
WHERE 직책 = '사장' AND 봉급 >= 1000
```

#### BETWEEN A AND B

**데이터 값이 A 부터 B 사이에 있는 값을 조회할 때 사용한다.**

예를 들어 봉급이 200 이상 500 미만인 데이터를 조회하고 싶다고 가정하자.

이 경우 코드 상에서 아래와 같을 것이다. 
```
봉급 >= 200, 봉급 <= 500  
또는 200 <= 봉급 <= 500
``` 


위의 논리연산자를 활용한다면 아래와 같은 쿼리가 될 것이다.
```
SELECT * FROM employee
WHERE 봉급 >= 200 AND 봉급 <= 500
```

이런 상황에 BETWEEN 절을 사용할 수 있다.

```
SELECT * FROM employee
WHERE 봉급 BETWEEN 200 AND 500
```
어떤 값 사이에 있는 데이터를 조회할 때 직관적으로 보기 좋다.

#### IN 연산자
만약 봉급이 300, 400, 500 인 데이터를 조회하려면?

**위에서 배운 것을 활용하려면 논리연산자를 사용하면 된다.**

```
SELECT * FROM employee
WHERE 봉급 = 300
   OR 봉급 = 400
   OR 봉급 = 500
```

이런 경우 IN 연산자를 활용해 쉽게 쿼리를 작성할 수 있다.

```
SELECT * FROM employee
WHERE 봉급 IN (300, 400, 500)
```

# 5) GROUP BY

**GROUP BY는 특정 컬럼을 기준으로 집계 함수를 사용해 집계된 데이터를 추출할 때 사용된다.**

따라서 GROUP BY 절은 항상 집계 함수를 사용한다.
{: .notice--danger}

employee 테이블은 아래 사진처럼 주어졌다.  

![employee-tabel](/images/2024-03-21/employee-table.png)

**예를 들어 "나는 '부서'별로 '봉급'의 평균이 궁금해"**라고 할 수 있다.

이런 경우 GROUP BY를 사용해 ***부서 별*** 이라는 키워드를 해결 할 수 있다.  

예시의 쿼리를 작성하면 아래와 같다.  

```
SELECT 부서, AVG(봉급) AS 평균봉급
FROM employee
GROUP BY 부서
```

쿼리 후 조회된 데이터는 아래 사진과 같다.  

![employee-tabel](/images/2024-03-21/as-avg(sal).png)

SELECT 절에서 `SELECT 부서, AVG(봉급) AS 평균봉급` 이라고 작성했다.  

여기서 AS 평균봉급은 'AVG(봉급)' 이라는 필드를 '평균봉급'으로 이름을 바꾸고 조회되도록 해준다.  

만약 `AS 평균봉급`이라는 문구를 추가하지 않았다면 아래와 같이 `AVG(봉급)`이라고 뜰 것이다.  

![employee-tabel](/images/2024-03-21/avg(sal).png)

## 6) HAVING

**GROUP BY와 같이 사용하는 조건 구문인 HAVING 절은 그룹 별로 나누어진 데이터에 조건을 걸 수 있다.**

여기서 HAVING 절과 WHERE 절의 다른 점은 있다.  
HAVING 절은 GROUP BY 절과 함께 사용해야 한다.  
<span style="font-size: 13px; color: red">
**그리고 WHERE 절이 먼저 조건을 거르고 GROUP BY가 그룹을 만든 뒤에 HAVING 조건이 적용된다는 것을 기억하자.**
</span>
{: .notice--danger}

예시로 5) GROUP BY 에서 사용한 쿼리를 그대로 사용해보자.

```
SELECT 부서, AVG(봉급) AS 평균봉급
FROM employee
GROUP BY 부서
```

쿼리 후 조회된 데이터는 아래 사진과 같다.  

![employee-tabel](/images/2024-03-21/as-avg(sal).png)

그런데 여기서 평균봉급이 300 이상인 부서만 조회하고 싶다고 가정하자.

즉, 이미 그룹화된 데이터에서 조건을 거는 것이다.

아래 쿼리로 바꾸면 될 것 같다.

```
SELECT 부서, AVG(봉급) AS 평균봉급
FROM employee
GROUP BY 부서
HAVING AVG(봉급) >= 300
```

이후 조회된 데이터는 아래와 같다.

![employee-tabel](/images/2024-03-21/HAVING-avg(sal).png)

## 7) ORDER BY

SELECT 문을 사용할 때 출력되는 결과물은 테이블에 입력된 순서대로 출력된다.  

**하지만 이 데이터를 오름차순 혹은 내림차순으로 정렬해야할 때가 있다.**

이 경우 ORDER BY 절을 사용해 해결한다.

참고로 ORDER BY 절은 항상 SELECT 문의 마지막에 위치한다.
{: .notice--info}

ORDER BY의 기본 문법은 아래와 같다.

```
SELECT *
FROM table
ORDER BY col [ASC/DESC]
```

ORDER BY의 마지막 부분에서 ASC/DESC는 각각 오름차순, 내림차순이다.

그리고 오름차순이 기본 세팅이므로 아무것도 쓰지 않을 시 자동으로 오름차순으로 정렬된다.

예시로 나는 'employee' 테이블에서 봉급을 내림차순으로 정렬하고 싶네 라고 가정하자.

아래 사진은 예시에 대한 쿼리와 그에 대한 값이다.

![img.png](/images/2024-03-21/ORDERBY_exam.png)

사진처럼 ORDER BY를 활용해 봉급에 대해 내림차순으로 정렬하였다.

추가적으로 ORDER BY는 여러 개 활용할 수 있다.

위 이미지를 보면 봉급이 400, 300 등 여러 겹치는 모습을 볼 수 있다.

이렇게 겹치는 경우엔 이름을 가나다 순(오름차순)으로 정렬해야겠다고 생각할 수 있다.

이때 ORDER BY에 두 개 이상의 정렬 기준을 넣어서 해결할 수 있다.

아래 사진은 두 개의 정렬을 넣어 1차로 봉급 내림차순 정렬 후 2차로 이름 오름차순으로 정렬하였다.

![img.png](/images/2024-03-21/ORDERBY_2add.png)

ORDER BY 절에서 , 로 두 개의 정렬을 구분하였다.

이름을 오름차순(ASC)를 넣으니 위 사진과 다르게 같은 봉급 값 안에서 이름 기준으로 새로 정렬되었다.

ORDER BY 첫 번째 기준부터 정렬 -> 값이 겹치면 두 번째 기준으로 정렬 -> ...  
이를 잘 기억하면 되겠다.  
{: .notice--danger}  

## 8) JOIN, ON, LIMIT, OFFSET
위에서 중간 중간 들어가는 위 4개의 구문이 있다.  

여기서 JOIN이 문제도 어렵고 코딩테스트에서 꽤나 중요한 것 같아서 따로 포스팅하려 한다.

다음 포스팅에서도 JOIN을 비롯한 4개의 구문에 대해 공부해보려 한다.

# 참고 사이트
[SQL 코딩테스트 대비 문법 정리](https://velog.io/@cji3604/SQL-%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8C%80%EB%B9%84-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC)  

[[DB SQL] 중복제거 (DISTINCT)](https://bio-info.tistory.com/110)  

[[SQL] SELECT 문 간단하게 파헤치기](https://121202.tistory.com/26)  

[[MS SQL Server] #6_SELECT문에 WHERE절 사용하기](https://doorbw.tistory.com/214)  

[[MSSQL] GROUP BY 절 사용법 (그룹별 집계)](https://gent.tistory.com/505)  

[[Oracle] 오라클 GROUP BY, HAVING 절 사용법 (WHERE, 조건절)](https://gent.tistory.com/366)  

[[SQL] 4. 정렬해서 출력하기](https://gomguard.tistory.com/93)  
