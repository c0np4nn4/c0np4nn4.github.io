+++
title = "CB24150, 3, Relational Algebra"
date = "2023-10-17"
+++

# Relational Query Language
- `SQL`과 같은 *Query Language*를 활용하면 `DB` 내 데이터에 대한 검색 및 수정이 가능합니다.
- `SQL Engine`은 아래와 같이 크게 세 부분으로 나누어 볼 수 있습니다.
- <이미지 추가 예정>
    - ***SQL Query***: *Declarative Language* 로 작성된 Query 문입니다.
    - ***Relational Algebra Plan***: *Query*를 *Relational Algebra(RA) Expression*으로 번역 및 최적화합니다.
    - ***Execution***: *RA Plan*의 Operator를 실행합니다.
- 즉, `Relational Algebra`는 ***Query***를 ***Expression***으로 변환할 수 있게끔 합니다.
- `Relational Algebra`에서 *Operation*들은 <u>하나 또는 둘</u>의 *Relation*을 입력받아서 새로운 *Relation*을 결과값으로 연산합니다.
    - **Unary Operation**: $op(R_i) = R'$
    - **Binary Operation**: $R_1 \\ \\ op \\ \\ R_2 = R'$

---

# Relational Algebra
- `Query`에서 *Relation* 들을 입력값으로 받고 새로운 *Relation*을 제공하게 됩니다.
$$Q(R_1, R_2, \dots, R_n) = R_{result}$$

- 입력받은 *Relation* 및 *Query*에 따라 새로운 *Relation*이 갖는 <u>schema</u>가 결정됩니다.

## Operators
- 총 5개의 ***Core Operator***가 있고,  추가적으로 ***Additional Operators***가 있습니다.
- <각 operator 에 대한 이미지 추가 에정>

### Unary: Selection & Projection
- `Selection`
    - **조건**에 따라 특정 **Row**를 반환하는 *operator*입니다.
    $$\sigma_{condition}(relation)$$
    - **condition**은 *Boolean Expression*입니다.

- `Projection`
    - 지정된 특정 **Colum*operator*입니다.
    $$\pi_{columns}(relation)$$
    - **columns**는 `,`로 구분하여 여러 개를 지정할 수 있습니다.

- 두 *operator*를 함께 사용하여 원하는 정보를 검색할 수 있습니다.
    - 단, 두 연산은 *commutative* 하지 않습니다.

- `DBMS`가 데이터를 저장하는 방식에 따라,  두 연산 중 어떤 것을 먼저 하는 것이 유리한지 결정할 수 있습니다.
    - `Row-oriented`: ***Selection*** $\rightarrow$ ***Projection***
    - `column-oriented`: ***Projection*** $\rightarrow$ ***Selection***

### Binary: Union & Intersect & Set Difference & Cartesian Product
- `Union`, `Intersection`, `Set`
    - 두 *Relations*가 서로 **Compatible**할 때,  아래 수식이 성립합니다.
    $$A \\ \\ op \\ \\ B \ R_{result}$$
        - 이 때,  $op$는 $\cap$, $\cup$, $-$ 중 하나입니다.
        - **Compatible**은 *Attribute*가 동일함을 의미합니다.

- `Cartesian Product`
    - 서로 다른 두 *Relations* $A(a_1, \dots, a_n), B(a_{n+1}, \dots, a_m)$ 에 대해 아래 수식을 만족하는 *operator*입니다.
    $$A \times B = R(a_1, \dots, a_m)$$
    - $A$의 각 *tuple*에 대해 $B$의 모든 *tuples*가 pair를 이루게 됩니다.
    - 따라서, 반환되는 *relation*의 크기는 두 *relations* 각각의 **cardinality**를 곱한 값($m \times n$)입니다.

### Additional: Rename
- *Relation*에 새로운 이름을 붙여주는 *operator*입니다.
$$\rho_{\text{new_name}}(Relation)$$

### Additional: Join
- `Cartesian Product`의 ***사이즈가 불필요하게 커진다***는 단점을 보완할 수 있는 *operator*입니다.
- `Natural Join`
    - 중복되는 **Attribute**를 결과에서 제거합니다.
    $$R \Join S = \pi_{A}(\sigma_{\theta}(R \times S))$$
    - `Selection` $\sigma_{\theta}$는 공통 *Attribute*에 대한 *equality* 검사를 합니다.
    - `Projection` $\pi_{A}$는 공통 *Attribute*를 제거합니다.
- `Theta Join`
    - `Cartesian Product` + 추가적인 조건 $\theta$ 인 *operator*입니다.
    $$R \Join_{\theta} S = \sigma_{\theta}(R \times S)$$
    - `Cartesian Product`와 유사한 결과를 확인할 수 있습니다.
- `EquiJoin`
    - `Theta Join`의 $\theta$가 *equality check*를 하는 경우입니다.
---
- `Outer Join`
    - 상기한 세 `Join`이 ***Intersect***에 해당한다면, `Outer Join`은 ***Union***에 해당합니다.
    - `Left`, `Right`, `Full`의 종류를 가지며, 데이터가 없는 경우에는 **NULL**로 채워지게 됩니다.

---

# Exercise 1
- 아래와 같은 Schema가 있다고 해보겠습니다.
> Account = {accNumber, type, balance, branchName}
- 가장 큰 `balance`를 갖고 있는 계좌의 `accNumber`를 찾기 위해 아래와 같이 식을 만들 수 있습니다.
$$
\begin{align}
R_1 &= \rho_{\text{A}}(\text{Account}) \times \rho_{\text{D}}(\text{Account}) \newline
R_2 &= \sigma_{\text{A.balance} < \text{D.balance}}(R_1) \newline
R_3 &= \pi_{\text{accNumber}}(\text{Account}) - \pi_{\text{A.accNumber}}(R_2) \newline
\end{align}
$$
    - $R_2$: 다른 계정보다 적은 돈을 가진 계정들만을 모두 찾습니다.
    - $R_3$: *Set Difference*를 이용해 가장 큰 `balance`를 가진 계좌를 찾습니다.

---

# Exercise 2
- 아래와 같은 Schema가 있다고 해보겠습니다.
> Student(sid, lastName, firstName, gpa)
> 
> Teaches(tid, iid, dept, cid, semester)
> 
> Took(sid, tid, grade)

1. 두 수업에서 각각 <u>80점보다 높은 점수</u>와 <u>50점보다 낮은 점수</u>를 받은 모든 학생의 `sid`를 찾으시오.
$$
\begin{align}
R_1 &= \sigma_{\text{grade > 80}}(\text{Took})\newline
R_2 &= \sigma_{\text{grade < 50}}(\text{Took})\newline
R_3 &= R_1 \times R_2 \newline
R_4 &= \sigma_{R_1\text{.tid} \neq R_2\text{.tid} \\ \wedge \\ R_1\text{.sid} = R_2\text{.sid}}(R_3) \newline
R_5 &= \pi_{R_1\text{.sid}}(R_4) \newline
\end{align}
$$

2. ***CSE 101***를 수강한 모든 학생들의 `sid`를 찾으시오.

---

