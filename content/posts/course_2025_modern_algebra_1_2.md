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

> 현대대수학 (추상대수학) Hungerford 3판의 내용 중 일부를 정리했습니다.

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

- 최대공약수가 1인 두 수를 <txtblu>서로 소</txtblu>(relatively prime)라 부른다.

---

# (<txtred>Thm</txtred>) (related with Bézout's identity)

{% quote(cite="") %}
적어도 하나는 $0$이 아닌 정수 $a, b$의 최대공약수를 $d = \gcd(a,b)$라 하자.

$d = au + bv$를 만족하는 정수 $u, v$가 (유일하지는 않지만) 존재한다.
{% end %}

우선 $a, b$의 일차 결합($ax + by$)의 집합 $S$를 아래와 같이 정의한다.

$$
S = \lbrace as + bt \\ | \\ s, t \in \mathbb{Z} \rbrace
$$

- <txtgrn>Step 1. $S$의 가장 작은 원소 $m$을 찾는다.</txtgrn>

<txtylw>Well-ordering axiom</txtylw>에 따르면, 공집합이 아닌 영 이상의 정수 집합의 부분집합은 항상 최소 원소를 갖는다.

$as + bt \in S$에 대하여 $s=a, t=b$를 대입하면 $a^2 + b^2 > 0$이다. 따라서 $S$는 항상 0 보다 큰 원소를 최소 하나 갖는다. 
이 원소를 $m = au + bv$라 두자.

- <txtgrn>Step 2. $m$이 $\gcd(a, b)$임을 증명한다.</txtgrn>

정수 $m$이 최대공약수 $\gcd(a, b)$임을 증명하기 위해 정의의 다음 두 조건을 만족함을 보인다.

> 1. $m|a$ 이고 $m|b$이다.
> 2. $c|a$ 이고 $c|b$ 이면 $c \le m$이다.

<txtblu>첫 번째 조건</txtblu>은 우선 $m|a$임을 보인다.

[나눗셈 알고리즘](@/posts/course_2025_modern_algebra_1_1.md)에 따라 아래와 같이 식을 정리할 수 있다. ($m > 0$)

$$
\begin{aligned}
a &= mq + r \quad  (0 \le r < m) \newline
r &= a - mq \newline
r &= a - (au+bv) q \newline
r &= a(\cdot) + b(\cdot)
\end{aligned}
$$

따라서, 나머지인 $r$도 $S$의 원소임을 알 수 있다.

하지만, 나눗셈 알고리즘의 조건인 $(r < m)$으로 인해 $S$의 양수 중 최소값인 $m$보다 작아야 하고 그 범위가 $(0 \le r)$이므로 반드시 $r=0$이어야 함을 알 수 있다.

따라서, $a = mq \Leftrightarrow m | a$ 이다.

$b$에 대해서도 같은 방법으로 손쉽게 $m | b$ 임을 보일 수 있다.

<txtblu>두 번째 조건</txtblu>는 $c|a, \\ c|b$에 이므로 두 값 $a, b$를 각각 $\begin{cases}a = ck \newline b = cs\end{cases}$로 두고 생각한다.

그렇다면 $m = au + bv = (ck)u + (cs)v = c(\cdot)$으로 정리할 수 있게 된다.

즉, $c | m$이므로 $c \le |m|$ 인데 $m > 0$이므로 $(\therefore) c \le m$이다.

위 두 조건 만족을 통해 $\gcd(a, b) = d$에 대하여 $d = au + bv$를 만족하는 $u, v$가 존재함($m$ 값이 존재함)을 알 수 있다. $\blacksquare$
