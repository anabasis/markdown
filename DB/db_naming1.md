# 데이터베이스 명명규칙

## 1. 명명규칙

### 1.1. 따옴표 사용금지

나쁜 예: 'FirstName' 혹은 "All Employees"

- 이름에 공백을 포함하지 않는다.
- 따옴표가 포함된 식별자로 SQL 작성이 어려워진다.

### 1.2. 식별자는 모두 소문자로 작성 되어야 함

- 테이블, 뷰, 컬럼 그리고 기타 모든 항목이 포함된다.
- 대소문자를 섞어 사용하면 따옴표를 사용해야 한다.(따옴표 사용금지)

### 1.3. 데이터타입(데이터유형)을 이름으로 작성하지 않음

나쁜 예: text 혹은 timestamp
데이터베이스 객체 이름, 특히 컬럼명은 필드나 객체를 설명하는 명사여야 한다.

### 1.4. 언더스코어(_)를 사용하여 단어를 분할

좋은 예: word_count 혹은 team_member_id
나쁜 예: wordcount 혹은 wordCount
참고: [snake_case](https://en.wikipedia.org/wiki/Snake_case).

### 1.5. 약어 사용을 지양하고 전체 단어를 사용해야 함

- 객체 이름은 전체 영어단어여야 한다.
- 특히 모음을 삭제하는 유형일 경우에는 지양한다.
- 대부분의 SQL 데이터베이스는 최소 30자로 된 이름을 지원한다.
- PostgreSQL은 식별자를 63자까지 지원한다.

### 1.6. 공통 약어를 사용함

몇몇 긴 단어에서는 그 단어 자체보다 약어가 흔히 사용된다.
“Internationalization” and “localization”
좋은 예: Internationalization > i18n
좋은 예: localization > l10n
하지만 약어의 의미전달이 의심스럽다면 전체 단어를 사용한다.

### 1.7. 예약어 사용금지

나쁜 예: user, lock, table 등.

예약어 단어 목록
[PostgreSQL](https://www.postgresql.org/docs/11/sql-keywords-appendix.html)
[MySQL](https://dev.mysql.com/doc/refman/5.7/en/keywords.html)
[Oracle](https://docs.oracle.com/database/121/SQLRF/ap_keywd001.htm#SQLRF55621)
[MSSQL](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/reserved-keywords-transact-sql?view=sql-server-ver15)

### 1.8. 단수형

데이터를 보유하는 테이블, 뷰 및 기타 객체는 복수가 아니라 단수형이어야 한다.
좋은 예: team
나쁜 예: teams
자바의 클래스나 파이썬의 변수명등으로 쉽게 변환된다.

### 1.9. 접두사와 접미사 사용금지

## 2. 키 필드

### 2.1. Primary keys

Primary key 필드명은 id 로 한다.

```sql
CREATE TABLE person (
id            bigint PRIMARY KEY,
full_name     text NOT NULL,
birth_date    date NOT NULL);
```

### 2.2. Foreign Keys

Foreign Key 필드명은 참조한 테이블과 필드의 조합이어야 한다.

```sql
CREATE TABLE team_member (
team_id       bigint NOT NULL REFERENCES team(id),
person_id     bigint NOT NULL REFERENCES person(id),
CONSTRAINT team_member_pkey PRIMARY KEY (team_id, person_id));
```

## 3. 명시적 이름 지정

### 3.1. 인덱스(Indexes)

인덱스명은 명시적으로 지정해줘야 하며, 테이블명과 컬럼명을 모두 포함해야 한다.

```sql
CREATE TABLE person (
  id          bigserial PRIMARY KEY,
  email       text NOT NULL,
  first_name  text NOT NULL,
  last_name   text NOT NULL,
  CONSTRAINT person_ck_email_lower_case CHECK (email = LOWER(email)));
CREATE INDEX person_ix_first_name_last_name ON person (first_name, last_name);
```

이름과 성에 대한 index person_ix_first_name_last_name가 사용 된다.

```sql
=# EXPLAIN SELECT * FROM person WHERE first_name = 'alice' AND last_name = 'smith';
                                QUERY PLAN                                          
-------------------------------------------------------------------------------------
 Index Scan using person_ix_first_name_last_name on person  (cost=0.15..8.17 rows=1 width=72)
   Index Cond: ((first_name = 'alice'::text) AND (last_name = 'smith'::text))
(2 rows)
```

### 3.2. 제약조건(Constraints)

제약조건에 대한 체크나 디버깅시 용이성을 위해 설명을 지정한다.

```sql
CREATE TABLE team (
  id          bigserial PRIMARY KEY,
  name        text NOT NULL);

CREATE TABLE team_member (
  team_id     bigint REFERENCES team(id),
  person_id   bigint REFERENCES person(id),
  CONSTRAINT team_member_pkey PRIMARY KEY (team_id, person_id));
```

PostgreSQL에서 외래키 작업 제한 설명 이름 지정

```sql
=# \d team_member
   Table "public.team_member"
  Column   |  Type  | Modifiers 
-----------+--------+-----------
 team_id   | bigint | not null
 person_id | bigint | not null
Indexes:
    "team_member_pkey" PRIMARY KEY, btree (team_id, person_id)
Foreign-key constraints:
    "team_member_person_id_fkey" FOREIGN KEY (person_id) REFERENCES person(id)
    "team_member_team_id_fkey" FOREIGN KEY (team_id) REFERENCES team(id)
```

제약 조건을 위반하는 행을 삽입하려고 시도할 경우 제약조건 이름으로 파악하기 쉽다.

```sql
INSERT INTO team_member(team_id, person_id) VALUES (1234, 5678);
ERROR:  insert or update on table "team_member" violates foreign key constraint "team_member_team_id_fkey"
DETAIL:  Key (team_id)=(1234) is not present in table "team".
```

마찬가지로, 3.1. 에서 생성한 person 테이블에 새로운 행을 추가할 때, 무엇이 잘못되었는지 알려주는 제약 조건 위반 오류가 표시된다.

```sql
-- This insert will work:
=> INSERT INTO person (email, first_name, last_name) VALUES ('alice@example.com', 'Alice', 'Anderson');
INSERT 0 1

-- This insert will not work:
=> INSERT INTO person (email, first_name, last_name) VALUES ('bob@EXAMPLE.com', 'Bob', 'Barker');
ERROR:  new row for relation "person" violates check constraint "person_ck_email_lower_case"
DETAIL:  Failing row contains (2, bob@EXAMPLE.com, Bob, Barker).
```
