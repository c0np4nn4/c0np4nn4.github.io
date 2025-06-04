+++
title = "Modern Algebra (7-1) 군의 정의와 예"
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

군은 하나의 연산을 갖는 대수체계이다.

### (<txtgrn>Example</txtgrn>) permutation
집합 $T$의 치환(permutation)은 그 원소들의 순열을 의미한다.
예를 들어, $T=\lbrace 1, 2, 3\rbrace$에는 아래와 같이 여섯 가지의 가능한 치환이 있다.

$$
\begin{aligned}
123 && 132 && 213 && 231 && 312 && 321
\end{aligned}
$$

어떤 전단사(*bijective*)함수 $f: T \rightarrow T$를 이러한 치환을 나타내는 함수로 정의할 수 있다.
예를 들어, $f(1) = 2, f(2) = 3, f(3) = 1$은 $(123) \rightarrow (231)$로의 치환을 나타낸다.

이러한 치환을 두 행으로 나누어 표현하면 아래와 같다.

$$
\begin{pmatrix}
1 & 2 & 3 \newline
2 & 3 & 1 
\end{pmatrix}
$$

첫째 행이 원래 순열이고, 둘째 행이 치환된 순열이다.

이와 같이 모든 치환을 나타내면 아래와 같다.

$$
\begin{pmatrix}
1 & 2 & 3 \newline
1 & 2 & 3 
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 \newline
1 & 3 & 2 
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 \newline
2 & 1 & 3 
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 \newline
2 & 1 & 3 
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 \newline
3 & 1 & 2 
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 \newline
3 & 2 & 1 
\end{pmatrix}
$$

두 전단사 함수의 합성은 마찬가지로 전단사 함수가 되므로, 임의의 두 치환의 합성은 상기한 여섯 개의 치환 중 하나가 된다.
예를 들어, $f = \begin{pmatrix} 1 & 2 & 3 \newline 3 & 1 & 2 \end{pmatrix}, \quad g = \begin{pmatrix} 1 & 2 & 3 \newline 1 & 3 & 2 \end{pmatrix}$에 대하여

$$
\begin{aligned}
f \circ g = \begin{pmatrix} 1 & 2 & 3 \newline 3 & 1 & 2 \end{pmatrix} \circ \begin{pmatrix} 1 & 2 & 3 \newline 1 & 3 & 2 \end{pmatrix} \newline
\newline
(f \circ g)(1) = f(g(1)) = f(1) = 3 \newline
(f \circ g)(2) = f(g(2)) = f(3) = 2 \newline
(f \circ g)(3) = f(g(3)) = f(2) = 1
\end{aligned}
$$

따라서, $f \circ g = \begin{pmatrix} 1 & 2 & 3 \newline 3 & 2 & 1 \end{pmatrix}$ 이다.

위 여섯 가지의 치환 집합을 $S_3$로 나타낼 수 있고, 연산 $(\circ)$는 집합 $S_3$의 연산이다.
그리고, 아래 두 성질들을 만족한다.

1. (<txtylw>닫힘</txtylw>) $f, g \in S_3$ 이면 $f \circ g \in S_3$이다. 
2. (<txtylw>결합법칙</txtylw>) $f, g, h \in S_3$일 때, $f \circ (g \circ h) = (f \circ g) \circ h$ 
3. (<txtylw>항등원</txtylw>) $I = \begin{pmatrix}1 & 2 & 3 \newline 1 & 2 & 3\end{pmatrix}$에 대하여, 모든 $f \in S_3$ 는 $f \circ I = I \circ f = f$를 만족한다. 
4. (<txtylw>역원</txtylw>) 임의의 전단사 함수는 <txtred>역원</txtred>을 갖는다. 따라서, $f \in S_3$ 이면 $f \circ g = g \circ f = I$인 $g \in S_3$가 존재한다. 

{% tip(title="대칭군") %}
에제에서 살펴본 이러한 군을 `n차 대칭군`(symmetric group of degree $n$)이라 한다.
{% end %}

---

> 위 예제를 토대로, 주요 성질을 추상화하여 군을 정의할 수 있다.

# (<txtgrn>Def</txtgrn>) Group
다음 공리를 만족하는 이항연산 $\circ$ 를 갖는 공집합이 아닌 집합 $G$를 군(Group)이라 한다.

1. (<txtylw>닫힘</txtylw>) $f, g \in G$ 이면 $f \circ g \in G$이다. 
2. (<txtylw>결합법칙</txtylw>) $f, g, h \in G$일 때, $f \circ (g \circ h) = (f \circ g) \circ h$ 
3. (<txtylw>항등원</txtylw>) 모든 원소 $f \in G$에 대하여 $f \circ I = I \circ f = f$를 만족하는 $I \in G$가 존재한다.
4. (<txtylw>역원</txtylw>) 각 원소 $f \in G$에 대하여  $f \circ g = g \circ f = I$인 $g \in G$가 존재한다. 

그리고 <txtylw>교환법칙</txtylw>도 성립하면 `Abelian Group`이라 부른다.

`5.` (<txtylw>교환법칙</txtylw>) 모든 $f, g \in G$에 대하여 $f \circ g = g \circ f$이다.

---

이 밖에도 *dihedral group*, *general linear group* (e.g. $GL(2, \mathbb{Z}_2$) 등이 존재한다.

---

# (<txtred>Thm</txtred>) Cartesian product on Groups
$G$와 $H$를 각각 연산 $*$, $\circ$를 갖는 군이라 하자. $G \times H$의 연산자 $\triangle$을

$$
(g, h) \triangle (g', h') = (g * g', h \circ h')
$$

와 같이 정의하면 $G \times H$는 군이다.

$G$, $H$가 Abelian group이면 $G \times H$도 abelian group이며, 두 군이 유한이면 $G\times H$도 유한군이 된다. 이 때 $|G \times H| = |G||H|$를 만족한다.
