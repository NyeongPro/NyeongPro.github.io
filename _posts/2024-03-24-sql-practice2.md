---
layout: single
title:  "[SQL]코테 대비 SQL 공부2"
categories: study
tag: [sql, 코테, 코딩테스트]
redirect_from:
  - /study/sql-practice  # categories 또는 md 파일 이름이 변경되더라도 이 포스트로 올 수 있도록 redirect
---

지난 포스팅에서 못다룬 JOIN에 대해서 공부해본다.
{: .notice--info}

# JOIN

JOIN은 관계형 DB에서 많이 사용한다.  
여러 조인의 종류가 있으므로 잘 공부해본다.

JOIN 실습을 위해 아래와 같이 테이블 두 개를 만들었다.

### 상품 테이블과 세부사항 테이블

![img.png](/images/2024-03-24/product-table.png) ![img.png](/images/2024-03-24/detail-table.png)


## 1) INNER JOIN

INNER JOIN은 일반적으로 사용하는 JOIN이다.

핵심적으로 JOIN 뒤에 **ON**을 사용하는데 이는 두 테이블이 결합하는 조건을 나타낸다.

내가 만든 테이블을 예시로 해보면 어느정도 이해가 된다.

나는 두 테이블의 ID가 겹치므로 ID가 같은 것들만 보이도록 해보겠다.

```sql
SELECT product.ID, product.상품명, detail.대표상품, detail.유통기한
FROM 상품 AS product
JOIN 세부사항 AS detail
ON product.id = detail.id
```
위의 코드로 아래와 같은 결과를 얻었다.  

![img.png](/images/2024-03-24/inner-join-ex.png)

먼저 코드를 해석해보자.

FROM 부터 살펴보면 상품 테이블을 조회할건데, 이름을 product라고 지칭했다.  
아랫줄에서 JOIN할 대상을 세부사항으로 정하고, 이름을 detail로 설정했다.

그리고 SELECT에서 각 테이블에게 새로 설정한 이름을 가지고 각 테이블에서 뽑아올 필드를 설정한다.  
예를 들어 product.ID를 보면 상품 테이블(새로 설정한 이름 product)의 ID를 뽑아온다.  

하나 더 살펴보면 detail.대표상품을 보면, 세부사항 테이블(새로 설정한 이름 detail)의 대표상품을 뽑아온다.


이렇게 테이블 이름을 AS로 설정한다.  
그 이후 [새로 설정한 이름].[뽑아올 필드 이름] 을 통해 내가 조회하고자 하는 테이블의 필드를 정할 수 있다.  
{: .notice--success}

>이때 필드를 조회할 때 ON 조건이 들어간다.  
나는 PRODUCT.ID를 조회했는데 ID 필드 결과를 보면 1~6 까지밖에 조회되지 않았다.  
product(상품 테이블)은 분명 1~10까지 데이터가 있는데 말이다.  
**이것은 ON 조건에서 두 테이블에서 ID가 같은 것만 조회하도록 내가 설정했기 때문이다.**  
세부사항 테이블에서는 7부터 10까지 ID가 없기 때문에 조회되지 않는다.



<span style="font-size: 20px; color: black">
**그렇다면 ON 조건을 빼면 어떻게 되는지 궁금해졌다.**
</span>

![img.png](/images/2024-03-24/on-remove.png)  

완전히 다른 형태로 나오게 됐다...  

내 생각에 상품 테이블의 한 행마다 세부사항 테이블의 모든 행 하나씩 엮여서 조회되는 것 같다.  

그 이유는 일단 총 60줄이 출력됐다. 아마도 상품 테이블 10행 * 세부사항 테이블 6행 = 60 인 것 같다.  

그리고 1번 ID에 대표상품(세부 사항 데이터)가 6개 모두 출력.  

그리고 2번 ID에도, 3번, ... 10번 ID 까지 모두 6개가 동일하게 출렸됐다.  

그리고 JOIN의 뜻도 있고...  

결과적으로 상품 테이블의 행에 세부사항 테이블 모든 행이 하나씩 엮이는 것 같다.

한 테이블의 행에 엮을(JOIN) 테이블의 한 행을 모두 JOIN 해버리고 싶으면 쓰면 될 것 같다.  
실제로 이렇게 쓰는지는 모르겠다. 데이터가 막 섞여서 도움되는 내용이 없을 것 같아서...
{: .notice--success}

INNER JOIN을 집합 관계로 표현하면 이해하기 쉬울 듯 하다.

![img.png](/images/2024-03-24/inner-join-set-relation.png)

## 2) LEFT OUTER, RIGHT OUTER 조인

위에서 본 것처럼 INNER JOIN을 사용할 때 ON 조건을 ID가 같은 것으로 설정하면 ID가 같지 않은 7~10은 보이지 않게 된다.    
하지만 이 행에 대해 보고 싶은 경우가 생길 수 있다. 
이럴 때 한 쪽 테이블 기준으로 합치는 조인을 사용할 수 있다.

아래 쿼리를 이용해 left join을 실행했고, 이미지는 아래와 같다.

```sql
SELECT product.ID, product.상품명, detail.대표상품, detail.유통기한
FROM 상품 AS product
lEFT JOIN 세부사항 AS detail
ON product.id = detail.id
```
![img.png](/images/2024-03-24/left-join-ex.png)

LEFT JOIN을 했기 때문에 기존 product 테이블의 1~10행이 모두 보인다.  
그러면서 ON 조건에서 id가 같은 행에 대해 대표상품과 유통기한까지 보이고 있다.  
7~10행의 상품에 대해서는 대표상품과 유통기한이 없었기 때문에 NULL로 표시되고 있다.  

그렇다면 RIGHT JOIN을 하게 된다면 아마 1~6행만 보이게 될 것이다.  

```sql
SELECT product.ID, product.상품명, detail.대표상품, detail.유통기한
FROM 상품 AS product
RIGHT JOIN 세부사항 AS detail
ON product.id = detail.id
```
![img.png](/images/2024-03-24/right-join-ex.png)

예상대로 다른 테이블(세부사항 테이블)은 기존 1~6행만 있었고
이 테이블에 대해 JOIN을 실행했으므로 1~6행에 대해서만 조회되었다.  

> 결과적으로 FROM에서 언급한 테이블이 LEFT 테이블이 되고  
> LEFT(RIGHT) JOIN 이후 언급한 테이블은 RIGHT 테이블이 된다.  
> 이때 LEFT 조인을 하면 LEFT 테이블 기준으로 조건에 맞는 필드가 JOIN되고  
> 그 반대도 마찬가지이다.

그래서 **[A LEFT JOIN B]과 [B LEFT JOIN A]**는 완전히 같은 식이 된다.
{: .notice--info}

LEFT JOIN과 RIGHT JOIN을 집합 관계로 표현하면 아래과 같겠다.  

![img.png](/images/2024-03-24/a-left_and_a-right-set.png)

## CROSS JOIN(카티전 조인)

집합에서 집합 곱의 개념이다.

A = {a, b, c, d}  
B = {1, 2, 3} 

A, B가 위와 같을 때 A CROSS JOIN B는 아래의 결과와 같다.

(a, 1), (a, 2), (a, 3),  
(b, 1), (b, 2), (b, 3),  
(c, 1), (c, 2), (c, 3),  
(d, 1), (d, 2), (d, 3)  

사용방법은 두 가지가 있고 아래와 같다.

```sql
1)
SELECT product.ID, product.상품명, detail.대표상품, detail.유통기한
FROM 상품 AS product
CROSS JOIN 세부사항 AS detail
-------------------------------
2)
SELECT product.ID, product.상품명, detail.대표상품, detail.유통기한
FROM 상품 AS product, 세부사항 AS detail
```

# 참고 사이트
[[MySQL] 7장 조인 : JOIN (INNER, LEFT, RIGHT)](https://futurists.tistory.com/17)