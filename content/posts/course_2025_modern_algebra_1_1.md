+++
title = "Modern Algebra (1-1)"
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

# <txtylw>(Axiom) Well-Ordering</txtylw>
영 이상의 모든 정수 집합 ($\mathbb{Z}_{\ge 0}$)의 공집합이 아닌 임의의 부분집합은 항상 최소원소를 포함한다.

- $\mathbb{Z}$, $\mathbb{Q}$에서는 성립하지 않는다.

---

# <txtred>(Thm)</txtred> Division Algorithm
정수 $a, b$에 대하여 $b > 0$ 일 때
$$
a = bq + r \quad 0 \le r < b
$$
를 만족하는 정수 $q, r$이 유일하게 존재한다.

---

# <txtblu>(Thm-pf)</txtblu> Division Algorithm
$a, b \in \mathbb{Z}$ 이고 $b > 0$ 이라 하자. 그리고 집합 $S$를 아래와 같이 가정한다.
$$
S = \lbrace a - bx: x \in \mathbb{Z}, a - bx \ge 0 \rbrace
$$

- <txtgrn>Step 1. $S \neq \emptyset$ 임을 보인다.</txtgrn>

$a + b |a| \ge 0$ 임을 보이면, $x = -|a|$일 때 $S$가 원소를 최소 하나 포함함을 보일 수 있다. $b$는 정수이므로 $b > 0$는 곧 $b \ge 1$을 의미한다.

$$
\begin{aligned}
b &\ge 1\newline
b|a| &\ge |a| \newline
b|a| &\ge -a \newline
a + b|a| &\ge 0
\end{aligned}
$$

- <txtgrn>Step 2. $a = bq + r$이고 $r \ge 0$ 을 만족하는 $q, r$을 구한다.</txtgrn>

<txtylw>Well-Ordering axiom</txtylw>에 의하여 $S$가 최소원소를 포함한다.

이 최소원소를 $r \in S$이라 하면, 임의의 $x$에 대하여 $a - bx = r$임을 알 수 있다. 이 때 $x=q$로 두면, 다음이 성립한다.

$$
r = a - bq \text{ 이고 } r \ge 0 \quad \Leftrightarrow \quad a = bq + r \text{ 이고 } r \ge 0
$$

- <txtgrn>Step 3. $r<b$ 를 증명한다.</txtgrn>

<txtylw>모순에 의한 증명</txtylw>으로 $r<b$를 보인다. 즉, $r \ge b$로 가정한 뒤 모순이 있음을 보임으로써 증명한다.

그럼 $r \ge b$ 이므로, $r-b \ge 0$이 성립한다. 이를 다시 정리하면

$$
\begin{aligned}
0 \le &r - b = (a - bq) - b = a - b(q+1) \newline
(\therefore) \quad &r-b \in S
\end{aligned}
$$

즉, $S$의 원소 중 $r$보다 작은 원소인 $r-b$가 존재함을 의미한다.

그런데, <txtgrn>Step 2</txtgrn>에서 $r$이 $S$의 최소원소라고 하였으므로 이는 모순이다 ($\Rightarrow \Leftarrow$).

따라서, $r < b$ 이고 <txtgrn>Step 2</txtgrn>의 결론과 함께 정리하면 아래와 같다.

$$
a = bq + r \text{ 이고 } 0 \le r < b
$$

분명히 위 식을 만족하는 정수 $q, r$이 존재한다.

- <txtgrn>Step 4. $q, r$의 유일성</txtgrn>

<txtgrn>Step 3</txtgrn>의 결과에 대해, $a = bq_1 + r_1 \text{ 이고 } 0 \le r_1 <b$ 인 정수 $q_1, r_1$을 가정하고 $q_1 = q$, $r_1 = r$임을 증명하자.

$$
\begin{aligned}
a = bq + r &= bq_1 + r_1 \newline
b(q - q_1) &= r_1 - r \newline
\end{aligned}
$$

그리고

$$
\begin{aligned}
0 \le r < b \newline
0 \le r_1 < b \newline
\end{aligned}
$$

첫 번째 부등식에 $-1$을 곱한 뒤 두 부등식을 더하면

$$
\begin{aligned}
&b \le -r < 0 \newline
&0 \le r_1 < b \newline
\Rightarrow &-b < r_1 - r < b \newline
\Rightarrow &-b < b(q - q_1) < b \newline
\Rightarrow &-1 < q - q_1 < 1
\end{aligned}
$$

따라서, $q - q_1 = 0$ 이므로 $q = q_1$이다. 손쉽게 $r = r_1$임을 알 수 있다.

그러므로, $q, r$은 유일하게 존재한다. $\blacksquare$

