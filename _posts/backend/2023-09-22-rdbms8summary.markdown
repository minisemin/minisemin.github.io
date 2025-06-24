---
layout: post
title:  "백엔드 기초 요약"
date:   2023-09-21 04:40:48 +0900
categories: [backend]
---

&nbsp;
&nbsp;

> 이번 포스트는 YC Tech Academy 3주차 발표자료로,
> 앞서 작성한 주요 내용을 요약했습니다.
>
> 자세한 내용은 앞선 포스트들을 참고해 주세요.
<details>
<summary>앞서 다룬 주제 펼쳐보기</summary>
<div markdown="1">
- 데이터베이스 / 데이터 모델 / RDBMS ([link](#http://127.0.0.1:4000/backend/2023/09/18/rdbms1.html))
- SQLite 다운로드 / SQLite 실행 ([link](#http://127.0.0.1:4000/backend/2023/09/19/rdbms2.html))
- CREATE TABLE 및 관련된 명령어 ([link](#http://127.0.0.1:4000/backend/2023/09/20/rdbms3.html))
- INSERT / SELECT 및 관련된 명령어 ([link](#http://127.0.0.1:4000/backend/2023/09/21/rdbms4.html))
- Foreign Key / Reference ([link](#http://127.0.0.1:4000/backend/2023/09/21/rdbms5.html))
- Index ([link](#http://127.0.0.1:4000/backend/2023/09/21/rdbms6.html))
- Transaction / Rollback ([link](#http://127.0.0.1:4000/backend/2023/09/21/rdbms7.html))
</div>
</details>

&nbsp;
&nbsp;

#### Table of Contents
---
1. [RDBMS](#rdbms)
2. [CREATE TABLE and KEYS](#create-table-and-keys)
3. [Indexing](#indexing)
4. [Transaction](#transaction)
5. [Spring Data JPA 활용하기](#spring-data-jpa-활용하기)

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## RDBMS
- RDBMS란 Relational Database Managing System의 약자로, Relational Database를 관리하는 시스템이다.

- Relational Database 는 여러 Data Model 중 표를 활용하여 Data를 표현하는 모델이다.

  - 예시) scholarship Table

      | studentId | Name | GPA   | Amount   |
      |---|------|--------------|
      | 1 | Matt  | 4.0 | 10,000 |
      | 2 | John  | 3.0  | 5,000 |
      | 3 | Tom  | 3.5  | 7,000 |
      | 4 | Kathy  | 1.5  |  |
      | 5 | Amy | 4.0 | 10,000 |
- SQL, Structured Query Language는 RDBMS에 사용하는 대표적인 프로그래밍 언어이다.

&nbsp;

## CREATE TABLE and KEYS

`CREATE TABLE();`은 SQL에서 Table을 생성하는 명령어로, 괄호 안에 Schema를 Constraint와 함께 적는다.
```sql
CREATE TABLE scholarship (
  studentId INT PRIMARY KEY,
  name VARCHAR(50) UNIQUE NOT NULL,
  GPA DECIMAL(3,1) NOT NULL,
  amount DECIMAL(10, 2)
);
```

- Schema: studentId, name, GPA, amount
- Constraint: NOT NULL, UNIQUE, PRIMARY KEY 등

&nbsp;

### UNIQUE

Table 내 데이터들의 중복을 방지하기 위해 설정하는 Constraint로, `UNIQUE`가 붙은 Attribute는 중복된 값을 입력할 수 없다.

- Unique는 Table 하나 당 한 개 이상일 수 있다.
- Unique는 Null 값을 가질 수 있다.
- Unique 값은 변형될 수 있다.
- Unique의 값 중복될 수 없다.

&nbsp;

### PRIMARY KEY

주로 Id로 사용되는 Attribute로, 각 행을 고유하게 식별하기 위한 Key이다. 우연히 모든 Attribute가 중복되더라도 Primary Key는 중복될 수 없으며, `UNIQUE`보다 제약사항이 강하다.

- PRIMARY KEY는 Table 하나 당 한 개만 존재할 수 있다.
- PRIMARY KEY는 Null 값을 가질 수 없다.
- PRIMARY KEY의 값은 변형될 수 없다.
- PRIMARY KEY의 값 중복될 수 없다.

&nbsp;

### FOREIGN KEY
한 Table의 Attribute가 다른 Table에 이미 존재하는 Attribute를 참조할 때 사용한다. 예를 들어, 장학생(`scholarship`) Table과 수업(`courses`) Table이 학번(`studentId`)이라는 Attribute를 공유한다면, `courses` Table의 `studentId`는 FOREIGN KEY를 참조(Reference)한다.

예를 들어, **`studentId`가 1인 'Matt'**는 'Math,' 'PE,' 'Music' 과목을 수강했다.

> Courses

  | studentId | Course   | Credit | Grade  |
  |----|----------|--------|-------|
  | <span style="background:yellow">1</span>  | <span style="background:yellow">Math</span>     | <span style="background:yellow">3</span>      | <span style="background:yellow">A</span>     |
  | 2  | English  | 2      | B     |
  | 3  | Physics  | 4      | A-    |
  | 4  | History  | 3      | B+    |
  | 5  | Chemistry| 3      | B-    |
  | <span style="background:yellow">1</span>  | <span style="background:yellow">PE</span>       | <span style="background:yellow">1</span>      | <span style="background:yellow">A</span>     |
  | 2  | Biology  | 4      | C+    |
  | 3  | Drama    | 2      | A     |
  | 5  | Art      | 2      | B     |
  | <span style="background:yellow">1</span>  | <span style="background:yellow">Music</span>    | <span style="background:yellow">2</span>      | <span style="background:yellow">A-</span>    |

```sql
  CREATE TABLE courses (
      courseId INT PRIMARY KEY,
      studentId INT,
      course VARCHAR(50),
      credit INT,
      grade CHAR(2),
      FOREIGN KEY (studentId) REFERENCES scholarship(id)
  );
```

- Foreign Key는 Table 하나 당 한 개 이상일 수 있다.
- Foreign Key는 Null 값을 가질 수 있다.
- Foreign Key는 중복될 수 있다.


FOREIGN KEY는 두 테이블 사이의 관계를 파악할 때 유리하다. 예를 들어, 장학금을 받은 학생의 총 이수 Credit을 쉽게 Query할 수 있도록 도와준다.

```sql
SELECT
    s.name, s.GPA,
    SUM(c.credit) AS total_credits
FROM
    scholarship s
JOIN
    courses c ON s.id = c.studentId
WHERE
    s.id IN (SELECT id FROM scholarship)
GROUP BY
    s.id;
```
결과:
```
name   GPA  total_credits
-----  ---  -------------
Matt   4.0  6
John   3.0  6
Tom    3.5  6
Kathy  1.5  3
Amy    4.0  5
```

&nbsp;

## Indexing

Index는 `SELECT` Query를 수행할 때 속도를 높일 수 있도록 포인터를 사용하는 특화된 Table이다.

생성 방법:
```sql
CREATE [UNIQUE] INDEX [IF NOT EXISTS] index_name
  ON table_name
    (column1 [ASC | DESC],
     column2 [ASC | DESC],
     ...
     column_n  [ASC | DESC])
  [ WHERE conditions ];
```

Index는 데이터 접근이 빠른 대신 업데이트가 비효율적이기 때문에 되도록 정적이고 (`UPDATE`나 `DELETE`가 없는) 사이즈가 큰 데이터베이스에 사용하기 적합하다.

사용하지 않는 Index는 삭제해주는 것이 좋다.
```sql
DROP Index index_name ON table_name;
```


### Index의 종류
- Single-Column: 디폴트 옵션으로, 하나의 column에 대한 index를 생성한다.
- Composite: 두 개 이상의 column에 대한 index를 생성한다.
- Unique: 중복된 값을 허락하지 않는다.
- Implicit: 테이블을 생성하면 자동으로 만들어지는 Index로, Primary Key나 Unique Constraint를 자동으로 저장한다.

&nbsp;

## Transaction

Transaction은 데이터베이스를 조작하는 단위로, 데이터베이스가 업데이트될 때 오류를 최소화하도록 고안된 특성을 가지고 있다. (간단히 말하자면, 동시에 소비자 A와 B가 마지막 남은 상품을 구매하다가 재고가 -1이 되는 머리아픈 상황을 방지하기 위한 장치들을 가지고 있다.)

![동시에 상품 구매](https://media.istockphoto.com/id/148051378/photo/shopping-violence.jpg?s=612x612&w=0&k=20&c=EfUE9wG_dFwICf0hkZyVQJB4XSjlRZ1VeLQy-HS578I=)
###### 출처: iStock

### Transaction의 특성

- **Atomicity:** 하나의 Transaction은 수행하는 작업<span style="color:blue">(예: 장바구니 담기 - 결제 - 승인)</span>이 모두 완료되어야 성공하고, 실패할 경우 Transaction 시작 전 상태로 복구한다.

- **Consistency:** Transaction이 성공적으로 완료되면 결과가 DB에 반영된다. <span style="color:blue">(상품 구매가 완료되면 재고는 1 줄어야 한다.)</span>

- **Isolation:** Transaction들은 서로 독립적으로 운영된다. <span style="color:blue">(재고가 1개 남은 상품을 장바구니에 담은 사람이 결제를 완료/취소하기 전까지 다음 사람이 장바구니에 담을 수 없다.)</span>

- **Durability:** 시스템에 오류가 생겨도 Commit된 Transaction 결과는 사라지지 않는다. <span style="color:blue">(결제를 완료했다면 이 정보는 사라지지 않아야 한다.)</span>

&nbsp;

### Transaction에 사용하는 명령어

- `COMMIT`: 변경 사항 저장하기
- `ROLLBACK`: 변경 사항을 마지막 세이브포인트로 되돌리기
- `SAVEPOINT`: Transaction 내의 특정 세이브포인트 생성
- `SET TRANSACTION`: Transaction에 이름 부여

&nbsp;

## Spring Data JPA 활용하기


### Hibernate
- Spring은 Java 오브젝트를 데이터베이스로 변화시키는 ORM(Object-Relational Mapping) 프로세스를 사용한다.
- Hibernate는 JPA를 사용하는 가장 유명한 ORM 프레임워크다.

&nbsp;

### JPA

- JPA(Jpa Repository)를 사용하기 쉽게 만든 모듈으로, Spring에서 DB 사용을 클래스 레벨에서 추상화한 것이다.
- CRUD 메소드 등이 포함되어 있으며, Query를 자동으로 생성해준다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {
}

```

&nbsp;

### JPA Entity

- Java class 위에 @Entity라는 annotation을 작성하면 class가 Table로 변환된다.

  ```java
  @Entity   /**Following is the database**/
  @Table(name="STUDENT")    /**Table name**/
  public class Student {
      @Id    /**PRIMARY KEY**/
      @GeneratedValue(strategy=GenerationType.AUTO)    /**AUTO_INCREMENT**/
      private Long id;    /**Attribute name and type**/

      /**VARCHAR(50) UNIQUE NOT NULL**/
      @Column(name="STUDENT_NAME", length=50, nullable=false, unique=true)
      private String name;    /**Attribute name and type**/

      // getters and setters
  }
  ```

&nbsp;

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### 자료 출처
- [[YC Tech Academy 과정/3주차 주제] RDBMS / Indexing / JPA](#https://ha-pu.tistory.com/3)
- [[Pre-W2] Relational Database Management System - YC Tech Academy Backend Career Project](#https://toosalty00.hashnode.dev/pre-w2-relational-database-management-system)
