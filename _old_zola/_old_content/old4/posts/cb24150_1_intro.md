+++
title = "CB24150, 1, Introduction"
date = "2023-09-10"
+++

# DB and DBMS

<center>
<img style="width: 10rem" alt="database icon" src="https://thenounproject.com/api/private/icons/5180256/edit/?backgroundShape=SQUARE&backgroundShapeColor=%23000000&backgroundShapeOpacity=0&exportSize=752&flipX=false&flipY=false&foregroundColor=%23000000&foregroundOpacity=1&imageFormat=png&rotation=0" />
</center>

**DB** 는 한 기관이 운영 목적으로 다수의 어플리케이션을 통해 접근하는 데이터의 집합입니다.

<center>
<img style="width: 10rem" alt="dbms icon" src="https://thenounproject.com/api/private/icons/5364302/edit/?backgroundShape=SQUARE&backgroundShapeColor=%23000000&backgroundShapeOpacity=0&exportSize=752&flipX=false&flipY=false&foregroundColor=%23000000&foregroundOpacity=1&imageFormat=png&rotation=0" />
</center>

그리고, **DBMS** 는 이러한 **DB** 들을 관리하기 위한 소프트웨어 입니다.
잘 알려진 예시로는, *Oracle*, *Microsoft SQL Server*, *MySQL*, *PostgreSQL*, *SQLite* 등이 있습니다.

# DBMS necessary
만약 **DBMS** 없이 **DB**를 관리하게 된다면 아래와 같은 구조로 데이터를 관리하게 됩니다.

<center>
<img alt="wo DBMS" src="../../Class/CB24150_DB/1_1.png" />
</center>

즉, 다수의 어플리케이션이 각각의 파일을 관리하기 시작하면서 *Inconsistency*, *Redundancy* 등의 문제가 발생할 수 있습니다.

이를 중앙화된 형태로 개선하고, **DBMS**가 아래의 기능들을 수행하면서 해소할 수 있습니다.
- Efficient access
- Data integrity and security
- Concurrency
- Crash recovery

---

# Data Models
데이터를 기술하기 이후나 개념의 집합입니다.

***Relational Data Model(관계형 데이터 모델)*** 은 가장 잘 알려진 구조이며, 데이터를 `테이블`에 저장합니다.

`schema`는 *"어떤 식으로 데이터를 저장할 것인지를 기술"* 합니다.
