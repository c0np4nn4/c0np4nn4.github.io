+++
title = "CB24150, 4, SQL Basic"
date = "2023-10-17"
+++

# DDL(Data-Definition Language)
- `Table`의 `Schema`를 생성/수정/삭제 하는 Query 입니다.
- `Create`
    ```sql
    CREATE TABLE T (
        uid  INT,
        nickname CHAR(20),
        level INT check (level > 0),
        balance REAL
        primary key (uid, nickname)
        foreign key (uid) references TT (uid)
    );
    ```
- `ALTER`
    - *Column* 추가
        ```sql
        ALTER TABLE T ADD friends_count INT;
        ```

    - *Column* 삭제
        ```sql
        ALTER TABLE T DROP friends_count;
        ```
- `DROP`
    - *Table* 전체 삭제
        ```sql
        DROP TABLE T;
        ```
    - *Table* 내용 전부 삭제
        ```sql
        DELETE FROM T;
        ```
---
# DML(Data-Manipulation Language)
- `Tuple`을 다루는 Query 입니다.
- `Insertion`
   ```sql
   INSERT INTO T 
   values(100, 'nickname_haha', 1, 0.0);
   ``` 

- `Deletion`
    ```sql
    DELETE FROM T 
    where uid=100;
    ```

- `Update`
    ```sql
    update T 
    set balance = balance * 100
    where level=10;
    ```
---
# SFW Query
- ***SQL Query***의 기본 구조입니다.
    ```sql
    SELECT  <columns>
    FROM    <table name>
    WHERE   <condition>
    ```
- 이를 *relational algebra*로 표현하면 아래와 같습니다.
$$\pi_{\text{columns}}(\sigma_{\text{conditions}}(\text{table name}))$$
- 따라서,  *Table*, *Row*, *Column*의 세 가지 정보를 모두 알고 있어야 합니다.

---
## Useful operators: DISTINCT, ORDER BY, LIKE
- `DISTINCT`
    - *Query* 결과에서 중복되는 값을 제거합니다.
        ```sql
        SELECT DISTINCT School
        FROM Students
        ```
- `ORDER BY`
    - *Query* 결과를 정렬합니다.
        ```sql
        SELECT      name, gpa, age
        FROM        Students
        WHERE       school = "PNU"
        ORDER BY    gpa DESC
        ```
- `LIKE`
    - 지정된 *문자열*을 찾습니다.
        ```sql
        SELECT      *
        FROM        People
        WHERE       name LIKE 'A_%d'
        ```
    - `%`: 하나 이상의 문자열을 의미합니다.
    - `_`: 하나의 문자열을 의미합니다.

---
## NULL
- 값이 없을 때, `NULL`을 넣어서 표현할 수 있습니다.
- **not null constraint**
    ```sql
    CREATE TABLE Student (
        sid         INT NOT NULL,
        lastName    CHAR(20),
        firstName   CHAR(20),
        age         INT,
        school      VARCHAR(20),
    );
    ```
- **Operation Rule**
    - 수식 연산을 하면 무조건 결과로 `NULL`이 나옵니다.
        > NULL * 100 $\rightarrow$ NULL
        > 
        > NULL * 0 $\rightarrow$ NULL
        > 
        > NULL + 100 $\rightarrow$ NULL
    - 비교 연산을 하면 무조건 결과로 `UNKNOWN`이 나옵니다.
        > NULL > 100 $\rightarrow$ UNKNOWN
        >
        > NULL = NULL $\rightarrow$ UNKNOWN
- 아래 *Query*로 `NULL`값을 갖는 *Row*를 선택할 수 있습니다
    ```sql
    SELECT name
    FROM students
    WHERE gpa IS NULL
    ```
- ***Boolean Operation***에 관해 `UNKOWN` = 0.5 을 갖습니다.
    - **AND = MIN**, **OR = MAX**, **NOT(x) = 1-x**로 생각하고 연산을 진행하면 됩니다.

---
# Aggregations
- `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`
    ```sql
    SELECT avg(score) FROM students
    ```
- `GROUP BY`
    - 특정 *Attribute*의 값들을 *grouping* 합니다.
    - `GROUP BY` 후의 `SELECT`는 *group*이 된 *attribute* 또는 **Aggregation function**에만 가능합니다.
        ```sql
        select gender, avg(gpa)
        from student
        where gpa > 3
        group by gender
        ```
- `HAVING`
    - `GROUP BY`로 만들어진 *group*에 대한 **constraint**를 명시합니다.
        ```sql
        select avg(gpa), gender
        from student
        where gpa > 4
        group by gender
        having grade > 3
        ```
