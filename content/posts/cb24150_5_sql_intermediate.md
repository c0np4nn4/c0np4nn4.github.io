+++
title = "CB24150, 5, SQL Intermediate"
date = "2023-10-20"
+++

# Join Expression
## `Cross Join` 
*(a.k.a Cartesian Product)*
```sql
select name, building
from student cross join department;
```

```sql
select name, building
from student, department;
```

## `Natural Join`
    - 두 *Relation* 에서 ***공통되는 Attribute***가 있을 때 **Join**
```sql
select name, cid
from instructor natural join teaches;
```

```sql
select name, cid
from instructor I, teaches T
where I.id = T.id;
```

- 만약 특정 *Attribute* 에 대한 equaty 검사를 해야 한다면, `Using` 키워드를 사용하면 됩니다.

```sql
select name, title
from student natural join takes, course
where takes.course_id = course.course_id;
```

- 위와 같이 *course_id* 에 대한 검사만을 진행한다면 아래와 같이 바꿔서 적어볼 수 있습니다.

```sql
select name, title
from (student natural join takes) 
        join course using course_id;
```

## `Inner Join`
- *Intersection*

```sql
select *
from student, takes
where student.id = takes.id;
```

- `on` 키워드를 사용하여 아래와 같이 적을 수 있습니다.
```sql
select *
from student join takes
    on student.id = takes.id;
```

## `Outer Join`
- *Full*, *Left*, *Right*  의 종류가 있으며 아래와 같이 적으면 됨
- **Full**
    ```sql
    select name, course
    from student S full join enroll E
    on S.name = E.stdName
    ```
- **Left**
    ```sql
    select name, course
    from student S left join enroll E
    on S.name = E.stdName
    ```
- **Right**
    ```sql
    select name, course
    from student S right join enroll E
    on S.name = E.stdName
    ```
---

# Set Operation
- $\cup$(Union), $\cap$(Intersect), $-$(Set Difference) 연산에 대한 SQL 문입니다.

## `Union`
- *course_id*가 111,  222인 수업을 수강하는 모든 학생들의 이름을 검색합니다.
- 두 *Relation* 에 대한 합집합(Union)을 구합니다.
    ```sql
    select name
    from student, enroll
    where sid = student_id and course_id = 111

    union

    select name
    from student, enroll
    where sid = student_id and course_id = 222
    ```

## `Intersect`
- *course_id*가 111,  222인 수업을 **모두 수강**하는 학생들의 이름을 검색합니다.
- 두 *Relation* 에 대한 교집합(Intersect)을 구합니다.
    ```sql
    select name
    from student, enroll
    where sid = student_id and course_id = 111

    intersect

    select name
    from student, enroll
    where sid = student_id and course_id = 222
    ```

## `Set Difference`
- *course_id*가 111는 <u>수강하는</u> 학생들 중, 222인 수업은 <u>수강하지 않는</u> 학생들의 이름을 검색합니다.
- 두 *Relation* 에 대한 교집합(Intersect)을 구합니다.
    ```sql
    select name
    from student, enroll
    where sid = student_id and course_id = 111

    except

    select name
    from student, enroll
    where sid = student_id and course_id = 222
    ```

---

# Subquery
- 이름 그대로, `Query`문 내에 또 다른 `Query`문이 존재하는 경우입니다.

## WHERE Caluse
- *Where* 절에서 사용되는 *Subquery* 는 아래 종류들이 있습니다.
### Scalar Subquery
- *Subquery* 가 어떤 **Scalar Value**를 반환합니다.
> 평균 수입보다 더 많은 수입을 버는 고객들의 *customerID* 를 검색하시오.

```sql
select C1.customerID
from Customer C1
where C1.income > (select avg(c2.income)
                    from customer C2);
```

### Membership / Comparison
- *Subquery* 가 어떤 **Relation**을 반환합니다.
- 이 때,  아래의 키워드를 사용할 수 있습니다.
    - `IN`
        - *Relation*에 속하는 값인지 검사합니다.
    - `NOT IN`
    - `EXISTS`
        - *Relation*이 ***Empty***인지 아닌지 검사합니다.
    - `NOT EXISTS`
    - `ANY`
        - **= Some**
    - `ALL`
        - **= ALL**
### Unique
- *Subquery* 내부에 ***중복되는 값***이 있는지 검사합니다.

## SELECT Caluse
- ***Scalar Subquery***가 이용될 수 있습니다.

## FROM Caluse
- ***Relation as a result Subquery***가 이용될 수 있습니다.

## DML
## Correlated subquery
## Quantifier

---

# Views

---

# Integrity Constraints
