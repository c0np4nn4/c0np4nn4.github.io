+++
title = "Elliptic Curve Cryptography (1)"
description = "ECC"
date = 2023-03-21
toc = true

[taxonomies]
categories = ["Cryptography"]
tags = ["Cryptography", "ECC"]

[extra]
math=true
+++

---

# Weierstrass Form
$$Y^2 = X^3 + a X + b$$

- `Singularity` 를 방지하고자 아래 조건이 추가됩니다.

$$4a^3 + 27b^2 \neq 0$$

- `Singular Point` 는 기울기가 무한대가 되는 점으로, ECC 상에서의 연산을 방해하는 점이라고 합니다.

---

# Point Addition, Negation
- ***ECC*** 에서의 덧셈은 기하학적으로 아래와 같이 설명할 수 있습니다.

> - 두 점 $P, Q$ 를 지나는 직선이 타원곡선과 만나는 점을 $R$ 이라 둡니다.
>
> - 그리고, 점 $R$ 의 $y$ 축에 대한 대칭점을 $R'$ 라 둡니다.
>
> - 타원곡선 상에서의 덧셈은 다음과 같이 정의됩니다.
>
> - $P + Q = R'$

즉, 단순한 덧셈이 아니라 아래의 알고리즘을 따라 구현할 수 있습니다.

```python
from collections import namedtuple
from Crypto.Util.number import inverse

Point = namedtuple("Point", "x y")

# Put Infinity Point as (0, 0)
ZeroPoint = Point(0, 0)

# Curve E: Y^2 = X^3 + A X + B
A =
B =

# Mod p 
p =

def add(P:Point, Q:Point):
    x1, y1 = P
    x2, y2 = Q

    if P == ZeroPoint:
        return Q

    elif Q == ZeroPoint:
        return P

    else:
        if x1 == x2 and y1 == -1 * y2:
            return ZeroPoint
        else:
            lam = 0

            if P != Q:
                lam = (Q.y - y1) * inverse(x2 - x1, p)
                lam %= p

            elif P == Q:
                lam = (3 * pow(x1, 2, p) + A) * inverse(2 * y1, p)
                lam %= p
            
            x3 = pow(lam, 2, p) - x1 - x2
            x3 %= p

            y3 = lam * (x1 - x3) - y1
            y3 %= p

            return Point(x3, y3)
```


# Scalar Multiplication
- 타원 곡선 상의 임의의 한 점 $P$ 에 대한 Scalar Multiplication 은 다음과 같이 정의할 수 있습니다.
$$nP = P + P + P + \cdots + P$$

- 즉, 한 점에 대한 반복적인 `Addition` 연산입니다.

- 이에 대한 효율적인 연산 방법으로 `Double and Add` 알고리즘이 알려져있고, 코드는 아래와 같습니다.

```python
def mul(P: Point, n: int):
    Q, R = P, Point(0, 0)

    while n > 0:
        if n % 2 == 1:
            R = add(R, Q)

        Q = add(Q, Q)

        n //= 2

    return R
```
---

# ECDLP
- `Elliptic Curve Discrete Logarithm Problem` 을 의미하며 <mark>Scalar Multiplication</mark>에서의 $n$ 을 찾기가 어렵다는 문제입니다.
- 순환군 $G$ 가 generator 로 $P$ 를 가질 때의 `Discrete Logarithm Problem` 의 특별한 경우입니다.

