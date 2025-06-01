+++
title = "Modern Algebra. Appendix B"
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

# (<txtred>#</txtred>) Injective

<txtblu>단사</txtblu>(injective)는 쉽게 말해 "일대일"을 의미한다. 

좀 더 정형화하면 어떤 사상 $f:B \rightarrow C$에 대하여, $a, b \in B \text{ 이고 } f(a), f(b) \in C$일 때,

$$
a \neq b \rightarrow f(a) \neq f(b)
$$

을 의미하는데, 이에 대한 대우를 생각해보면 아래와 같다.

$$
f(a) = f(b) \rightarrow a = b
$$

{% detail(title="예제", default_open=false) %}

> 예제 1)
> 
> $f: \mathbb{R} \rightarrow \mathbb{R}$ 이고, $f(x) = 4x + 7$일 때,
> $$
> \begin{aligned}
> f(a) &= f(b) \newline
> \Leftrightarrow (4a + 7) &= (4b + 7) \newline
> a &= b
> \end{aligned}
> $$
> 따라서, 단사(*injective*) 이다.

> 예제 2)
> 
> $f: \mathbb{Z} \rightarrow \mathbb{Z}$ 이고, $f(x) = x^2$일 때,
> $$
> \begin{aligned}
> f(3)  &= f(-3) = 9 \newline
> \newline
> \therefore f(3) = f(-3) &\not \rightarrow 3 = -3
> \end{aligned}
> $$
> 따라서, 단사(*injective*)가 아니다.

{% end %}


---

# (<txtred>#</txtred>) Surjective

<txtblu>전사</txtblu>(surjective)는 쉽게 말해 전부 커버가 되는지를 의미한다.

좀 더 정형화하면 임의의 사상 $f: B \rightarrow C$에 대하여, $\forall c \in C$가 적어도 하나의 $b \in B$에 대한 $f$의 상이 될 때를 의미한다.
즉,

$$
\exists \\ b \in B : f(b) = c \\ \text{ for } \forall c \in C
$$

{% detail(title="예제", default_open=false) %}
> 예제 1)
> 
> 자연수 $N = \mathbb{Z}_{\ge 0}$에 대하여, $f: \mathbb{Z} \rightarrow N$, $f(x) = |x|$ 를 살펴보자.
> 
> $\mathbb{N}$의 임의의 원소는 적어도 하나의 $\mathbb{Z}$의 원소의 $f$에 대한 상임을 쉽게 알 수 있다.
> 
> 즉, $z \in \mathbb{Z}$ 가 $f$를 통해 대응되는 $\mathbb{N}$의 원소가 항상 존재하며 $z, -z$가 모두 하나의 $\mathbb{N}$의 원소($|z|$)에 대응된다.
> 
> 따라서, 전사(*surjective*)이다.
{% end %}

이제 전사의 필요충분조건을 살펴본다.

우선 <txtylw>치역</txtylw>은 다음과 같이 정의된다.

$$
\textsf{Im} f = \lbrace c | c = f(b) \text{ for } \forall b \in B \rbrace = \lbrace f(b) | b \in B \rbrace
$$

이를 이용해 임의의 사상 $f: B \rightarrow C$가 전사일 필요충분조건을 정리하면 아래와 같다.

$$
\textsf{Im} f = C
$$

즉, 공역(codomain)과 치역, 상이 모두 같은 경우를 의미한다고 볼 수 있다.

---

# (<txtred>#</txtred>) Bijective

<txtblu>전단사</txtblu>(*bijective*)는 함수 $f: B \rightarrow C$가 전사이면서 단사인 경우를 의미한다.

- 유한집합 $B, C$에 대하여, $f$가 전단사일 <txtylw>필요충분조건</txtylw>은 $B, C$가 같은 원소의 개수를 가지는 것이다.
- $B$가 유한집합이고 $C \subsetneq B$ 이면, 전단사함수 $f$가 존재할 수 없다.

$f$가 무한집합에서의 사상인 경우로 확장하여, 전단사 함수의 성질을 정리할 수 있다.

---

# (<txtred>Thm</txtred>) Bijective function

{% quote(cite="") %}
함수 $f: B \rightarrow C$가 전단사일 필요충분조건은 다음과 같다.

$g \\ \circ \\ f = \iota_B \text{ 이고 } f \\ \circ \\ g = \iota_C$인 함수 $g: C \rightarrow B$가 존재한다.
{% end %}

# (<txtblu>Thm-pf</txtblu>) Bijective function
필요충분조건에 대한 증명이므로 두 과정을 통해 증명을 진행한다.

## 1) $\Rightarrow$

<txtylw>목표</txtylw>: $f$가 전단사이면, 위 조건을 만족하는 함수 $g$가 존재한다는 것을 보인다.

먼저 $f$가 *bijective*라 가정한다. 그렇다면, 

1. 전사이므로 $\exists \\ b : f(b) = c \\ \text{ for } \\ \forall c \in C$ 이고,
2. 단사이므로 $\exists ! \\ b : f(b) = c$ ($b$가 *유일하게 존재함!*)

그러므로 $g(c) = b$인 함수를 (자연스럽게) 정의할 수 있다.

임의의 $c \in C$에 대하여 두 함수를 합성하여 아래와 같이 정리할 수 있다.

$$
(f \circ g)(c) = f(g(c)) = f(b) = c
$$

따라서, $f \circ g = \iota_B$이다.

유사한 방법으로 

$$
(g \circ f)(b) = g(f(b)) = g(c) = b
$$

따라서, $g \circ b = \iota_B$이다.

## 2) $\Leftarrow$

<txtylw>목표</txtylw>: 위 조건을 만족하는 $g: C \rightarrow B$가 존재하면, 함수 $f: B \rightarrow C$는 전단사임을 보인다.

함수 $g: C \rightarrow B$가 존재하고, $f$에 대하여 $f(a) = f(b)$라 가정하자.

$$
\begin{aligned}
f(a) &= f(b) \newline
g(f(a)) &= g(f(b)) \newline
\iota_B(a) &= \iota_B(b) \newline
a &= b \newline
\therefore f(a) = f(b) &\rightarrow a = b
\end{aligned}
$$

이를 통해 $f$가 <txtblu>단사</txtblu>(*injective*)임을 보인다.

또, 임의의 $c \in C$에 대하여, $g(c) \in B$임을 이용하여

$$
\begin{aligned}
f(g(c)) = (f \circ g)(c) = \iota_C(c) = c\in C
\end{aligned}
$$

임을 알 수 있다. 즉, $f$가 $C$에 사상하는 임의의 원소 ($g(c)$)가 존재함을 확인할 수 있다.
따라서, $f$는 <txtblu>전사</txtblu>(*surjective*)이다.

그러므로, $f$는 **<txtblu>전단사</txtblu>**(***bijective***)이다. $\blacksquare$
