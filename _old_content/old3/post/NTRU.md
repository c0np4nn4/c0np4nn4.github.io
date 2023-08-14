+++
title = "NTRU"
description = "Cryptography"
date = 2023-06-20
toc = true

[taxonomies]
categories = ["PQC", "Cryptography"]
tags = ["cryptography"]

[extra]
math=true
+++

---
# 📌 Description
- <txtred>**NTRU**</txtred>는 `Public-Key Cryptosystem` 입니다.

---
### 📍 Notation
> - $N, p, q$: 
>     - 정수 인자
>     - $p, q$는 반드시 <txtylw>소수</txtylw>일 필요는 없습니다.
>     - $\gcd (p, q) = 1$ 이고 $q > p$

> - $\mathcal{L_f}$, $\mathcal{L_g}$, $\mathcal{L_{\phi}}$, $\mathcal{L_m}$
>     - 정수 계수를 갖는 최고차항이 $N-1$인 다항식들의 집합들입니다.

> - Ring $\mathcal{R}=\mathbb{Z}\[X\] / (X^N - 1)$ 상에서의 원소들을 다룹니다.
>     - $F \in \mathcal{R}$ 인 $F$는 아래와 같이 <txtylw>벡터</txtylw>로 표현할 수도 있습니다.
>     - $F = \sum\limits_{i=0}^{N-1} F_i x^i = \[F_0, F_1, \dots, F_{N-1}\]$

> > - $\bmod (X^N-1)$ 상에서의 곱셈을 $\circledast$로 표기하고, *Star Multiplication* 라 읽습니다.
> >     - <txtylw>Cyclic Convolution product</txtylw> 연산이며, 아래와 같이 정의됩니다.
> >     - $F \circledast G = H$
> >         - $H_k = \sum\limits_{i=0}^{k} F_i G_{k-i} + \sum\limits_{i=k+1}^{N-1} F_i G_{N+k-i} = \sum\limits_{i + j \equiv k \bmod N} F_i Gj$ 
> >         - 이 때, $k$ 는 <txtylw>차수</txtylw>를 의미합니다.
> 
> > - 예를 들어, $N=3, F=F_0 + F_1 x + F_2 x^2, G=G_0 + G_1 x + G_2 x^2$ 에 대하여
> > $$
> > \begin{align}
> > F \circledast G &= (F_0 + F_1 x + F_2 x^2)(G_0 + G_1 x + G_2 x^2) \newline
> > &= F_0 G_0 + (F_0 G_1 + F_1 G_0)x + (F_0 G_2 + F_1 G_1 + F_2 G_0)x^2 + (F_1 G_2 + F_2 G_1)x^3 + F_2 G_2 x^4 \newline
> > &= F_0 G_0 + (F_0 G_1 + F_1 G_0)x + (F_0 G_2 + F_1 G_1 + F_2 G_0)x^2 \\ (\bmod x^3 - 1)
> > \end{align}
> > $$

> - 만약 $\bmod q$와 같이 정수로 modulus 연산을 할 경우, <txtylw>계수 (coefficient)</txtylw>에 대해 $\bmod q$를 적용해주면 됩니다.

---
### 🔑 키 생성, Key Creation
> - `Alice`와 `Bob`이 서로 안전한 통신을 하고자 하는 상황을 가정합니다.
> - `Bob`은 <txtylw>키 생성</txtylw>을 통해 자신의 <txtylw>비밀키</txtylw>($k_{priv}^{B}$)와 <txtylw>공개키</txtylw>($k^{B}_{pub}$)를 생성합니다.
>     - <txtylw>공개키</txtylw>를 이용하면, 누구나 `Bob`에게 데이터를 안전하게 보낼 수 있습니다.
>     - <txtylw>비밀키</txtylw>를 이용하면, 오직 `Bob`만이 ***암호화된 데이터*** 를 복호화할 수 있습니다.

> - 키 생성에는 $f, g \in \mathcal{L_g}$인 <txtylw>다항식</txtylw> $f, g$ 가 사용됩니다.
> - 이때, $f$ 는 아래와 같이 Ring $\mathcal{R}$ 상에서 $\bmod q$, $\bmod p$ 에 대한 <txtred>역원을 가진다</txtred>는 조건을 만족하는 다항식 입니다. $f_p$, $f_q$는 각각 $\bmod p$, $\bmod q$ 에서의 <txtylw>역원</txtylw>입니다.
> 
> $$
> \begin{align}
> f_p \circledast f & \equiv 1 \bmod p \newline
> f_q \circledast f & \equiv 1 \bmod q
> \end{align}
> $$

> - 위 수식을 만족하는 다항식 $f$ 를 구한 뒤에는, 아래 수식에 따라 다항식 $h$를 계산합니다.
> $$h \equiv f_q \circledast g \bmod q$$

> - 키 생성 이후를 정리하면 아래와 같습니다.
>     - <txtred>*공개키*</txtred> : $h$
>     - <txtred>*비밀키*</txtred> : $f$, ($f_p$ 도 저장하는 것이 좋음)
>     - 폐기: $g$, $f_q$

---
### 🔒 암호화, Encryption
- `Alice`가 `Bob`에게 <txtylw>비밀 메세지</txtylw> $m$ 을 보내려 한다고 가정하겠습니다.
> - `Alice`가 우선 알고 있어야 할 정보는 아래와 같습니다.
>     - 정수 $N, p, q$
>     - `Bob`의 <txtred>*공개키*</txtred> $h$
>     - <txtylw>다항식</txtylw>으로 표현된 $m \in \mathcal{L_m}$
>     - 무작위로 선택된 <txtylw>다항식</txtylw> $\phi \in \mathcal{L_{\phi}}$

> - <txtylw>암호문</txtylw> $c$ 는 아래 수식에 따라 계산됩니다.
> $$c \equiv p \phi \circledast h + m \\ (\bmod q)$$

> - $\phi$ 는 <txtylw>비밀 메세지</txtylw>를 감추고, 암호화에 무작위성을 부여하기 위해 사용됩니다.
> - 또, <txtylw><u>수식 상</u></txtylw>에서는 <txtylw>비밀 메세지</txtylw> $m$이 그대로 드러나있는 것처럼 보이지만, `Star Multiplication`에 의해서 <txtylw><u>다항식 표현</u></txtylw> 내에서는 정보가 감춰지게 됩니다.

---
### 🔓 복호화, Decryption
- `Bob`이 전달받은 <txtylw>***암호화된*** 비밀 메세지</txtylw> $c$ 를 복호화하는 과정입니다.
> - `Bob`은 자신의 <txtred>비밀키</txtred> $f$ 를 이용해서, 아래 수식과 같이 $a$를 우선 계산합니다.
> $$a \equiv f \circledast c \bmod q$$
> - 이 때, $a$의 <txtylw>계수</txtylw>는 $-q/2$ 에서 $q/2$ 사이의 값을 선택합니다.

> - $a$ 를 계산한 후에, 아래 수식을 이용하여 <txtylw>비밀 메세지</txtylw> $m$ 을 얻을 수 있습니다.
> $$f_p \circledast a \bmod p$$

#### 🔓 복호화 상세, Decryption (detail)
> - 우선 $a$ 에 대해 식을 전개합니다.
> $$
> \begin{align}
> a & \equiv f \circledast c \bmod q \newline
> & \equiv f \circledast (p\phi \circledast h + m) \bmod q \newline
> & = (f \circledast p\phi \circledast h) + (f \circledast m) \bmod q \newline
> & = (f \circledast p\phi \circledast (f_q \circledast g)) + (f \circledast m) \bmod q \newline
> & = (f \circledast fq \bmod q) \circledast (p\phi \circledast g) + (f \circledast m) \bmod q \newline
> & = (p\phi \circledast g) + (f \circledast m) \bmod q \newline
> \therefore a & \equiv (p\phi \circledast g) + (f \circledast m) \bmod q \newline
> \end{align}
> $$
> - <txtylw>다항식</txtylw> $a$의 <txtylw>계수</txtylw>가 모두 $-q/2$ 에서 $q/2$ 사이이므로, $\bmod q$ 를 하더라도 <txtred>계수가 그대로</txtred>있게 됩니다.
>    - 범위를 좀 더 정확히 적자면 $-\lfloor \frac{q-1}{2} \rfloor$ 이상 $\lceil \frac{q-1}{2} \rceil$ 이하 입니다.

> - 따라서, $a$ 에 대한 수식을 정리하면 아래와 같습니다.
> $$a = p\phi \circledast g + f \circledast m \\ \\ \text{in} \\ \\ \mathbb{Z}\[X\]/(X^N - 1)$$

> - $p$에 관한 항을 제거하기 위해 $\bmod p$ 를 하면 아래와 같습니다.
> $$a \bmod p \equiv f \circledast m \bmod p$$
> - 그리고, $f$ 를 제거하기 위해 $\bmod p$에서의 <txtylw>역원</txtylw>인 $f_p$로 `Star Multiplication` 해주면 $m$을 복원할 수 있습니다.
> $$
> \begin{align}
> & f_p \circledast a \bmod p \newline
> \equiv \\ & f_p \circledast (f \circledast m) \bmod p \newline 
> \equiv \\ & m \bmod p
> \end{align}
> $$

> - 마지막에 $\bmod p$를 하기 때문에, <txtylw>원본 메세지</txtylw>의 복호화가 제대로 되는지 의심할 수 있습니다.
> - 하지만, $a$ 의 계수가 $-q/2$ 에서 $q/2$ 사이라는 점, $q > p$ 라는 점에 의해 안전하게 복구됨을 알 수 있습니다.

---

# ⚙ 인자 선택, Parameters
- 앞서 살펴본 `NTRU Algorithm`을 진행하기 위해 <txtylw>인자</txtylw>를 선택할 때의 고려 사항에 관해 살펴봅니다.
> - $N, q$ 는 보통 2의 거듭제곱 ($2^k$)로 정해집니다.
> - $p$ 는 $q$ 보다 매우 작은 홀수로 정해지며, $\gcd(p, q) = 1$을 만족해야 합니다.

---

## ⚙ Norm estimate
> - 우선 <txtylw>*Width*</txtylw> 라 불리는 값을 정의합니다. <txtylw>*Width*</txtylw>는 <txtylw>다항식의 계수</txtylw> 중 <u>가장 큰 수</u>와 <u>가장 작은 수</u>의 차이를 의미하며, 아래와 같이 표기합니다.
> $$\mid F\mid_{\infty}=\max\limits_{0\le i \le N-1} \\{ F_i \\} - \min\limits_{0\le i \le N-1} \\{ F_i \\} $$

> - 그리고, 아래와 <txtylw>다항식 계수의 평균값</txtylw>인 $\bar{F}$ 를 구해서 아래의 값을 정의합니다.
> $$\mid F\mid_{2}=\big(\sum\limits_{i=0}^{N-1} (F_i - \bar{F})^2 \big)^{\frac{1}{2}}$$

- 앞서 구한 $\mid F\mid_{\infty}, \mid F\mid_{2}$ 를 기반으로, `Don Coppersmith`의 아래 명제를 이해할 수 있습니다.
> - $\epsilon > 0$인 경우, $\epsilon$과 $N$값에 따라 상수 $\gamma_1, \gamma_2 > 0$이 존재하며, <txtylw>무작위로 선택된 두 다항식</txtylw> $F, G \in \mathcal{R}$이 아래의 부등식을 만족한다.
> $$\gamma_1 \mid F\mid_{2} \mid G\mid_{2} \\ \le \\ \mid F \circledast G \mid_{\infty} \\ \le \\ \gamma_2 \mid F\mid_{2}\mid G\mid_{2}$$

---

## ⚙ Sample Spaces
> - 평문 $m$에 관한 $\mathcal{L_m}$ 에 대해 아래와 같이 정의합니다.
> $$\mathcal{L_m} = \big{\\{} m \in \mathcal{R}: \text{coefficients are lying between} -\frac{1}{2} (p-1) \\ \text{and} \\ \frac{1}{2}(p-1) \big{\\}}$$
> - 따라서, $\bmod p$ 에 의해 평문 $m$이 변하는 일도 없습니다.

> > - 나머지 집합 $\mathcal{L_f}, \mathcal{L_g}, \mathcal{L_{\phi}}$ 에 관해서는 아래의 수식을 이용합니다.
> > $$\mathcal{L}(d_1, d_2) := \big\\{ \mathcal{F} \in \mathcal{R} \mid \mathcal{F} \\ \text{has} \\
> > \begin{cases}
> > d_1 \\ \text{coefficients equal to 1} \newline
> > d_2 \\ \text{coefficients equal to -1} \newline
> > \text{rests are equal to 0} \newline
> > \end{cases}
> > \big\\}$$
> > - 즉, <u><txtylw>계수</txtylw>가 **1**인 항이 $d_1$개</u>, <u><txtylw>계수</txtylw>가 **-1**인 항이 $d_2$개</u>, <u>나머지는 모두 0</u>임을 나타내는 수식입니다.
> 
> > - 예를 들어, $N$ = 5, $d_1 = 2$, $d_2 = 1$ 인 경우 아래와 같이 생각할 수 있습니다.
> > $$\mathcal{L}(2, 1) \ni 1 * x^4 - x^3 + 0 * x^2 + 0 * x^1 + 1$$
> > - 벡터로 나타내면, $[1, -1, 0, 0, 1]$ 입니다.

> > - 위 수식과 함께 인자 $d_f, d_g, d_{\phi}$ 를 추가로 활용하여 <txtred>*sample space*</txtred> 를 아래와 같이 정의합니다.
> > $$
> > \begin{align}
> > \mathcal{L_f} & = \mathcal{L}(d_f, d_f - 1) \newline
> > \mathcal{L_g} & = \mathcal{L}(d_g, d_g) \newline
> > \mathcal{L_{\phi}} & = \mathcal{L}(d_{\phi}, d_{\phi}) \newline
> > \end{align}
> > $$
> 
> > - 이 때, $\mathcal{L_f} = \mathcal{L}(d_f, d_f)$ 로 정하게 되면, $f(1) = 0$ 을 만족하게 된다고 합니다.
> > - 즉 $f = (x-1)k(x)$ 가 되는데, 이렇게 되면 modulo $x^N -1$ 상에서 $\gcd(x^N-1, f) \neq 1$가 되므로, <txtylw>역원</txtylw>을 갖지 못합니다.
> > - 앞서 <txtred>키 생성</txtred> 에서 $f_p$, $f_q$라는 <txtylw>역원</txtylw>을 사용함을 상기할 필요가 있습니다.
> > - 따라서, $\mathcal{L_f} = \mathcal{L}(d_f, d_f - 1)$ 로 정의합니다.

> - 각 <txtred>*Sample space*</txtred>의 원소인 $f, g, \phi$ 는 각각 아래와 같은 $L^2$ norm 을 갖습니다.
> $$
> \begin{align}
> |f \mid_2 & = \sqrt{2d_f - 1 - N^{-1}} \newline
> |g \mid_2 & = \sqrt{2d_g} \newline
> |\phi \mid_2 & = \sqrt{2d_{\phi}}
> \end{align}
> $$

---

## ⚙ Decryption Criterion
> - <txtylw>복호화 단계</txtylw>에서 $a = p\phi \circledast g + f \circledast m \bmod q$ 를 할 때, $\bmod q$ 에 의해 값이 바뀌지 않도록 해야 합니다.
> - 이를 위해서는, $|p\phi \circledast g + f \circledast m\mid_{\infty} < q$ 를 만족해야 합니다.
>     - 만약 $q$ 이상이 되면, $\bmod q$에 의해 값이 줄어들기 때문으로 이해할 수 있습니다.
>         - $q \equiv 0 \bmod q$
>         - $q + 1 \equiv 1 \bmod q$
>         - $q + 2 \equiv 2 \bmod q$
> - 이는 아래 두 식을 만족하도록 <txtylw>인자를 선택</txtylw>하면 거의 항상 참이 된다고 합니다.
> $$
> \begin{align}
> & \mid f \circledast m \mid_{\infty} \\  \le \\  q / 4 \newline
> & \mid p\phi \circledast g \mid_{\infty} \\  \le \\ q / 4 \newline
> \end{align}
> $$

> - 앞서 살펴본 `Don Coppersmith`의 <txtylw>명제</txtylw>의 부등식에 의해, 수 $\epsilon$에 따라 $1-\epsilon$의 확률로 아래의 값을 얻을 수 있습니다.
> $$
> \begin{align}
> & \mid f \mid_{2} \mid m \mid_{2} \\ \approx \\  \frac{q}{4\gamma_2} \newline
> & \mid \phi \mid_{2} \mid g \mid_{2} \\ \approx \\  \frac{q}{4p\gamma_2} \newline
> \end{align}
> $$

---
# 🛡 Security Analysis

---
# 🛠 Practical Implementations of NTRU

---
# 🎇 Additional Topics

---
# 📚 Reference
- [NTRU a lattice-based encryption system, Eindhoven University of Technology](https://pure.tue.nl/ws/portalfiles/portal/69853173/Wit_2014.pdf)
- [The Mathematics of the NTRU PKC, Abderrahmane Nitaj](https://nitaj.users.lmno.cnrs.fr/ntru3final.pdf)
- [NTRU: A Ring-Based PKC, HPS98, pdf](https://www.ntru.org/f/hps98.pdf)
- [NTRU: A Ring-Based PKC, HPS98, slideplayer](https://slideplayer.com/slide/5368908/)
