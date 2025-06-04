+++
title = "Modern Algebra (7-2) 군의 기본성질"
description = ""
date = "2025-06-04"

[taxonomies]
categories = ["Modern Algebra"]
tags = ["Math", "Course"]

[extra]
math = true
+++

> 현대대수학 (추상대수학) Hungerford 3판의 내용 중 일부를 정리했습니다.

---

**군의 기본성질**

---


# (<txtred>Thm</txtred>) Uniqueness of Identity and Inverse element
군 $G$에 대하여 $a, b ,c \in G$라 하자.

1. $G$의 항등원 $e$는 유일하다.
2. $G$에서 소거법칙이 성립한다. 즉,
$$
\begin{aligned}
a \\ b=a \\ c \quad \Leftrightarrow \quad b=c \newline 
b \\ a=c \\ a \quad \Leftrightarrow \quad b=c
\end{aligned}
$$
3. $G$의 각 원소 ($a \in G$)의 역원 $a^{-1} \in G$는 유일하다.

---

## (<txtblu>Thm-pf</txtblu>) Uniqueness of Identity and Inverse element
1. 군의 정의에 따라 군 $G$는 적어도 하나의 항등원 $e$를 갖는다.
만약 $e, e'$가 각각 군 $G$의 항등원이라 가정하면,
$$
\begin{aligned}
e \\ e' &= e' \\ e = e \newline 
e' \\ e &= e \\ e' = e' \newline 
\end{aligned}
$$
두 식이 동일하므로, $e = e'$이다. 즉, 하나의 항등원만 존재한다.
2. 군의 정의에 따라 원소 $a$는 역원 $a^{-1}$를 갖는다.
등식의 양 변에 역원을 곱해주면 쉽게 보일 수 있다.
3. 원소 $a \in G$의 역원을 $d, d'$라 가정하자. 즉,
$$
da = e = d'a
$$
를 만족하고, `2.`에서 증명한 것처럼 양 변에 $a^{-1}$을 오른쪽에 곱하면 $d=d'$임을 보일 수 있다.
따라서 $a$의 역원은 하나만 존재한다. $\blacksquare$

---

# (<txtylw>Cor</txtylw>) Inverse
군 $G$에 대하여 $a, b \in G$이면
1. $(ab)^{-1} = b^{-1}a^{-1}$
2. $(a^{-1})^{-1} = a$

---

## (<txtblu>Cor-pf</txtblu>) Inverse
1. 역원 관계에 있는 두 원소의 곱은 항등원이다.
$$
(ab) \cdot (b^{-1}a^{-1}) \\ = \\ a(bb^{-1}) a^{-1} \\ = \\ aea^{-1} \\ = \\ aa^{-1} \\ = \\ e
$$
마찬가지로
$$
(b^{-1}a^{-1}) \cdot (ab)  \\ = \\ b^{-1}eb \\ = \\ b^{-1}b \\ = \\ e
$$
따라서, $b^{-1}a^{-1}$이 $ab$의 역원이고 상기한 정리에 따라 <txtred>유일</txtred>하다.
2. $a$에 대한 역원으로 $a^{-1}$를 생각하면 아래 식이 성립한다.
$$
a \\ a^{-1} = a^{-1} \\ a = e
$$
또, $a^{-1}$에 대한 역원으로 $(a^{-1})^{-1}$을 생각하면 아래 식이 성립한다.
$$
a^{-1} \\ (a^{-1})^{-1} = e
$$
따라서 식을 정리하면, $a^{-1} \\ a = a^{-1} \\ (a^{-1})^{-1}$이므로, 상기한 정리의 `2.` (소거 법칙)으로 증명할 수 있다.
$\blacksquare$
---

# (<txtred>Thm</txtred>) Law of exponents in Group
군 $G$에 대하여 $a \in G$라 하자. 임의의 $m, n \in \mathbb{Z}$에 대하여
$$
a^m a^n = a^{m+n}, \quad (a^m)^n = a^{mn}
$$

---

**원소의 위수**

---

# (<txtred>Thm</txtred>) Order of an element
군 $G$에 대하여 $a \in G$일 때
1. $a$가 무한위수를 가지면 $k \in \mathbb{Z}$인 모든 원소 $a^k$는 모두 다르다. 
2. $i \neq j$에 대하여 $a^i = a^j$이면 $a$는 유한위수를 갖는다.

---

## (<txtblu>Thm-pf</txtblu>) Order of an element
`1.`, `2.`가 서로 대우 관계임에 주목하여 `2.`만 증명한다.
$i > j$에 대하여 $a^i = a^j$라 가정하면, 양 변에 $a^{-j}$ ($a^j$의 역원)을 곱해 다음을 구할 수 있다.
$$
a^{i-j} = a^{j-j} = a^0 = e
$$
이 때 $i - j > 0$ 이므로, $a$가 유한위수를 가짐을 알 수 있다.

---

# (<txtred>Thm</txtred>) Finite order of an element
군 $G$에 대하여 $a \in G$가 유한위수 $n$을 갖는 원소라면,
1. $a^k = e   \\ \Leftrightarrow \\ n | k$
2. $a^i = a^j \\ \Leftrightarrow \\ i \equiv j ( \bmod n)$
3. $d \ge 1, \\ n = td \\ \Rightarrow \\ \textsf{ord}(a^t) = d$

---

## (<txtblu>Thm-pf</txtblu>) Finite order of an element
1. (<txtylw>$\Leftarrow$</txtylw>) $n|k \\ \Leftrightarrow \\ k = n \cdot t$이므로,
$$
a^k = a^{nt} = (a^n)^t = e^t = e
$$
(<txtylw>$\Rightarrow$</txtylw>) $a^k = e$ 일 때, 나눗셈 알고리즘에 따라 $k = n\cdot q + r$이고 $0 \le r < n$이다. 즉,
$$
e = a^k = a^{n \cdot q + r} = (a^n)^q \cdot a^r = e \cdot a^r = a^r
$$
이를 만족하는 $r$은 <txtylw>영</txtylw>밖에 없으므로($\because$ 위수(order)의 정의: $n$이 항등원이 되는 최소의 양의 정수),
$$
k = n\cdot q + 0 = nq
$$
따라서, $n | k$이다.
2. (<txtylw>$\Leftrightarrow$</txtylw>) $a^i = a^j$이면 $a^{i-j} = e$이다. 즉, `1.`의 정리에 의해
$$
a^{i-j} = e \\ \Leftrightarrow \\ n | (i-j)
$$
약수는 곧 $(i-j) = n \cdot t$임을 의미하므로, $i \equiv j (\bmod n)$ 이다. $\newline$
필요충분조건임을 보이는 것은 어렵지 않다.
3. $|a| = n$이므로, $a^t$에 대하여 $(a^t)^d = a^{td} = a^n = e$를 만족하는 정수 $d$가 있음을 보여야 한다.  $\newline$
(<txtylw>*가정*</txtylw>) $(a^t)^k = e$를 만족시키는 양의 정수 $k$를 가정하자. $\newline$
그러면 `1.`에 의해 $n | tk$이고, 정리하면 아래와 같다.
$$
t \\ k = n \cdot \mathcal{r} = (td) \cdot \mathcal{r}
$$
따라서, $k=dr$이다. 다시 말해, $a^t$에 제곱승되는 $k$는 $d$를 약수로 가짐을 의미한다. $\newline$
정리 자체는 원소의 유한위수 $n$이 $n = td$로 인수분해 될 때, $a^t$의 위수가 정확히 $d$임을 의미한다.

---

# (<txtylw>Cor</txtylw>) Finite order of an element in Abelian group
군 $G$를 임의의 원소가 유한위수를 갖는 <txtred>**Abelian Group**</txtred>이라 하자.
$c \in G$가 $G$에서 <txtgrn>최대위수</txtgrn>를 갖는 원소(즉, 모든 $a \in G$에 대하여 $|a| \le |c|$)이면 $G$의 임의의 원소의 위수는 $|c|$의 약수이다.

---

## (<txtblu>Cor-pf</txtblu>) Finite order of an element in Abelian group
(증명방법: <txtylw>**모순에 의한 증명**</txtylw>)

임의의 $a \in G$가 $|a|\not{\mid} \\ |c|$라고 가정해보자.
그러면 $|c|$의 소인수분해에서 나타나는 소수 $p$의 거듭제곱보다 더 큰 $p$의 거듭제곱이 정수 $|a|$의 소인수분해에서 나타나는 소수 $p$가 존재해야한다.
수식으로 쓰면 아래와 같다.
$$
\begin{aligned}
|a| &= p^r \cdot m \newline
|c| &= p^s \cdot n
\end{aligned}
$$
이 때 $m, n$은 $\gcd(p, m) = \gcd(p, n) = 1$인 값이고 $r > s$이다.

상기한 정리의 `3.`으로 부터 또 아래 두 사실을 알 수 있다.
$$
\begin{aligned}
\textsf{ord}(a^m)     &= p^r \newline
\textsf{ord}(c^{p^s}) &= n \newline
\end{aligned}
$$

(<txtylw>***증명 필요***</txtylw>) 위수가 서로소인 두 원소의 곱의 위수는 두 위수의 곱이므로,
$$
\textsf{ord}(a^m \cdot c^{p^s}) = p^r n
$$

따라서, $a^m \cdot c^{p^s} \in G$의 위수 <txtgrn>$p^r n$</txtgrn>이 $c$의 위수인 <txtblu>$p^s n$</txtblu>보다 크기 때문에($\because r > s$) $|c|$에 대한 가정에 모순이다 $\newline$ 
(<txtred>$\Rightarrow \Leftarrow$</txtred>)

따라서, $|a| \mid |c|$ 이다. $\blacksquare$
