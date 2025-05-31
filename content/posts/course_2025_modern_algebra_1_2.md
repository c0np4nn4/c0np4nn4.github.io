+++
title = "Modern Algebra (1-2)"
description = ""
date = "2025-06-01"

[taxonomies]
categories = ["Modern Algebra"]
tags = ["Math", "Course"]

[extra]
math = true
+++

> 현대대수학 (추상대수학) Hungerford 3판의 내용을 정리했습니다.

---

# (<txtgrn>Def</txtgrn>) Divisibility

정수 $a, b$에 대하여 $b \neq 0$이고 $a = bc$인 정수 $c$가 존재하면 $a$가 $b$로 <txtred>나누어진다</txtred>(divisible)고 하고,
$b$를 $a$의 약수(divisor) 또는 인수(factor)라 한다. 기호로 나타내면 $b | a$이다.

---


{% note(title="참고") %}
$a = bc$이고 $-a = b(-c)$이다. 즉, $a, -a$는 $b$를 공통 약수로 가짐을 알 수 있다. 

조금 더 일반적으로 말하면, **$a, -a$는 같은 약수를 갖는다.**
{% end %}

{% note(title="참고") %}
$b \neq 0$이고 $b|a$라 가정하자.

즉, $a = bc$ 이므로, $|a| = |b||c|$이고 $0 \le |b| \le |a|$이다.

이는 곧 $-|a|  \le b \le |a|$를 의미하고 아래 두 사실로 정리된다.

> 1. 영이 아닌 정수 $a$의 임의의 약수는 |a|와 같거나 작다.
> 2. 영이 아닌 임의의 정수는 오직 유한 개의 약수를 갖는다.

{% end %}

---

# (<txtgrn>Def</txtgrn>) Greatest Common Divisor

$a, b$를 적어도 하나는 $0$이 아닌 정수라 하자.

$a, b$가 모두 나누어지는 가장 큰 정수 $d$를 $a, b$의 <txtred>최대공약수</txtred>라 한다.

다르게 표현하면, $a,b$의 최대공약수 $d$는 다음 두 조건을 만족하는 정수이다.

> 1. $d|a$ 이고 $d|b$이다.
> 2. $c|a$ 이고 $c|b$ 이면 $c \le d$이다.

최대공약수 $d$는 $d = \gcd(a, b)$로 나타낸다.

---

