+++
title = "CB24150, 2, Relational Model"
date = "2023-10-17"
+++

# Data Model
- ***Relational Model***: 가장 널리 사용되는 Data Model 입니다.
    - *Entity-relationship*, *semi-structured*, *object-based* 등등 도 있습니다.
- 일반적으로 아래 세 가지에 대해 기술합니다.
    - `Structure`
    - `Constraints`
    - `Operations`

## Structure
- ***Schema***: 주어진 데이터 모델을 활용하여 특정 데이터 집합에 대해 기술합니다.
- 데이터가 저장되는 <u>구조</u>에 관한 부분입니다.

## Constraints
- 저장되는 데이터에 대한 <u>조건(제한)</u>에 관한 부분입니다.
- *Data Integrity*와 관련이 있습니다.

## Operations
- 데이터에 대한 <u>연산</u>을 정의한 부분입니다.
- 두 가지 종류의 연산이 존재합니다.
    - *Retrieve*
    - *Change*
- ***Declarative Language*** 를 사용합니다.

---

# Relational Data Model
- ***Table***의 집합으로 이루어진 `Data Model` 입니다.
- `Relation`은 각 행(row)의 데이터 간의 관계를 의미합니다.

## Structure
- Row, Column 을 가진 `Table` <u>구조</u>로 이루어져있습니다.
- 아래와 같이 용어들이 정리됩니다.
    - *Table* = *Relation*
    - *Row* = *Tuple* = *Record*
    - *Column* = *Attribute* = *Field*
    - *Degree*: column 개수
    - *Cardinality*: row 개수
- *Relation Schema*
    - `Relation`의 이름 + `Attribute` + `Domain`
    - e.g. `Student(sid int, sname varchar(10), age int, gpa float)`
- *Domain*
    - *Data Type* 이라 생각하면 됩니다.
- ***Properties***
    - *Relation*은 **tuple의 집합**이기 때문에 아래 성질들을 갖습니다.
        - *order*: tuple 들의 순서가 중요하지 않습니다.
        - *unique*: 동일한 tuple 은 존재할 수 없습니다.
        - *atomic*: attribute 는 atomic 해야 합니다. (name + age 를 나타내는 attr는 안됨)
- *Instance*
    - 한 시점의 ***DB Contents***를 의미합니다.

## Constraints
- `Key`를 이용해서 정의됩니다.
- <`Key`에 관한 포함관계 다이어그램 이미지 추가 예정>
    - `Super Key`
        - *Relation* 상에서 **unique**하게 *tuple*을 지정할 수 있도록 하는 <u>Attribute의 집합</u>입니다.
        - 하나 이상이 될 수 있으며, 최소 하나의 원소가 *uniqueness*를 만족해야 합니다.

    - `Candidate Key`
        - `Super Key` 중 하나의 원소를 가지는 부분집합이라 생각할 수 있습니다.
        - **minimal** 을 만족하는 Key 입니다.

    - `Primary Key`
        - `Candidate Key` 중 <u>선택된</u> 하나의 Key 입니다.
        - *tuple*을 식별(***Identify***)할 수 있는 Key 입니다.

    - `Foreign Key`
        - 다른 `Table`의 `Primary Key`를 column 의 value로 갖는 *Attribute*를 의미합니다.
        - **constraint**: `Foreign Key`가 갖고 있는 데이터는 실제로 다른 `Table`에 존재해야 합니다.
- 또한, `Integrity Constraint`도 있습니다.
    - *Entity Integrity Constraint*
        - `Primary Key`는 절대 **NULL**값을 가질 수 없습니다.
    - *Referential Integrity Constraint*
        - `Foreign Key`에서의 **constraint**과 동일합니다.

## Operations
- *Operation* 은 이후 4장에서 ***SQL***과 함께 살펴봅니다.
