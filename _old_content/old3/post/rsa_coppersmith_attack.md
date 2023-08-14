+++
title = "Coppersmith Attack"
description = "Cryptography"
date = 2023-07-02
toc = true

[taxonomies]
categories = ["Cryptography", "Cryptanalysis", "RSA"]
tags = ["cryptography", "Cryptanalysis"]

[extra]
math=true
+++

---
# 📌 Description
- <txtred>**Coppersmith Attack**</txtred>는 <txtylw>*Coppersmith Method*</txtylw> 를 기반으로 한 <txtylw>**RSA**</txtylw> 공격 방법입니다.
- <u>아주 작은 공개키 $e$ 를 사용</u>하거나, <u>비밀키에 관한 부분적인 정보를 알고 있을 때</u> 활용할 수 있습니다.

---
# 📃 Coppersmith Method
- 아래 <txtylw>정리</txtylw>는 다항식 $f$에 대해, $\bmod N$ 상에서의 <txtylw>해</txtylw>를 효율적으로 찾을 수 있음을 보여줍니다.

## 📜 Theorem 1 (Coppersmith)
> - 정수 $N$ 과 *일계수 다항식* $f \in \mathbb{Z}[x]$가 있다고 해보겠습니다.
> - $\frac{1}{d} > \epsilon > 0$인 $\epsilon$ 에 대하여, 값 $X=N^{\frac{1}{d} - \epsilon}$ 를 구합니다.
>     - 이 때, $d$ 는 <txtylw>차수</txtylw>를 의미합니다.
> - 주어진 $\langle N, f\rangle$에 대하여, $f(x_0) \equiv 0 \bmod N$인 모든 $x_0 < X$ 를 계산할 수 있습니다.
> - 이 때, <txtylw>실행시간</txtylw>은 $w=\min \big\\{ \frac{1}{\epsilon}, \log_2 N \big\\}$ 에 대한, $O(w)$ 차원 상의 격자(lattice)에 대한 <txtylw>*LLL 알고리즘*</txtylw> 의 <txtylw>실행시간</txtylw>에 의해 결정된다고 합니다.
- 즉, $X=N^{\frac{1}{d}}$ 보다 작은 값들 중 $\bmod N$ 공간에서의 다항식 $f$의 <txtylw>해</txtylw>를 구할 수 있음을 이용합니다.

---
# 🗡 Håstad's broadcast attack
- <txtylw>메시지</txtylw> $m$을 다수의 수신자($k$ 명)에게 보내는 상황을 가정해봅니다.
- <txtylw>메시지</txtylw>는 공개키 $e=3$ (가정)과 각각의 moduli $\langle N_i, e \rangle$를 이용해 <txtylw>암호화</txtylw> 됩니다.
- 만약 <txtred>공격자</txtred>가 $k \ge 3$ 개의 <txtylw>암호문</txtylw>을 획득할 경우, 아래와 같은 공격이 가능합니다.
> - $\langle N_i, e \rangle$ 를 이용해 생성한 <txtylw>암호문</txtylw>을 $C_i$ 라 하겠습니다.
> - 이 때, $e=3$ 이므로, $C_i = M^e = M^3 \bmod N_i$ 입니다.
> - <txtred>공격자</txtred>는 `CRT`를 이용해서 다음을 만족하는 $C$ 값을 구할 수 있습니다.
> $$
> \begin{align}
> C & \equiv C_i \bmod N_i \\ , \\ \\  C \in \mathbb{Z}_{N_1N_2N_3}^{*} \newline
> \therefore C & \equiv M^3 \bmod {N_1N_2N_3} \newline
> \end{align}
> $$
> - 그런데, $M < N_i$ 이므로 $M^3 < N_1N_2N_3$ 입니다.
> - 따라서, $C = M^3$ 가 되고, <txtred>공격자</txtred>는 $C$에 대한 <txtylw>세제곱근</txtylw>을 구해 <txtylw>메시지</txtylw> $m$을 복원할 수 있습니다.
- 만약 <txtylw>공개키</txtylw>가 $3$이 아닌 $k$라고 해도, $k$개의 암호문만 있으면 동일하게 적용 가능합니다.

---
## 🗡 일반화
> - <txtylw>암호화</txtylw>이전에 적용하는 <txtylw>단순한 linear padding</txtylw>은 <txtred>RSA 공격에 안전하지 않음</txtred>을 확인할 수 있습니다.
>     - 원래 <txtylw>암호화</txtylw>는 $C = M^e \bmod N$ 입니다.
>     - <txtylw>padding</txtylw> 함수를 $f$ 라 하고, 이를 적용한 결과를 $M' = f(M)$ 이라 정의하겠습니다.
>         - (e.g.) $f = 2^{\text{length_of_bits}(M)} + M$
>     - <txtylw>padding 을 적용한 암호화</txtylw>는 아래와 같습니다.
> $$C = f(M)^e = (M')^{e}\bmod N$$

> - <txtred>공격자</txtred>가 $k$개의 <txtylw>암호문</txtylw> $C_i = f_i(M)^e$를 획득한다고 가정해보겠습니다. ($1 \le i \le k$)
> - 앞선 방법과 비슷하게, <txtred>공격자</txtred>가 <txtylw>***충분한 수*** 의 암호문</txtylw>을 획득하게 되면 <txtylw>평문</txtylw> $M_i$를 복원할 수 있습니다.
>     - 좀 더 일반적으로 말하자면, `Håstad`는 <u><txtylw>*서로소의 곱</txtylw>을 moduli 로 갖는 <txtylw>단일 변수 방정식*</txtylw></u> 이 *충분히 많은 방정식들이 제공된다면* 풀릴 수 있음을 증명했습니다.
>     - 이를 통해, <txtylw>RSA</txtylw>에서 <txtylw>padding</txtylw>을 더할 때는, <txtred>Random</txtred>한 방법을 적용해야 함을 알 수 있습니다.

## 📜 Theorem 2 (Håstad)
> - 서로소 $N_1, N_2, \dots, N_k$ 에 대하여, $N_{\min} = \min\limits_i \\{ N_i \\}$ 를 구합니다.
> - 차수가 $q$ 이하인 $k$ 개의 <txtylw>다항식</txtylw> $g_i(x) = \in \mathbb{Z}/N_i[x]$ 가 있습니다.
> - $i \in \\{1, 2, \dots, k\\}$ 에 대하여, $g_i(M) \equiv 0 \bmod N_i$ 를 만족하는 $M < N_{\min}$ 가 <txtylw>유일하게 존재한다</txtylw>고 가정합니다.
> - 또한, $k > q$ 가 성립함을 가정합니다.
> - 모든 $i$에 대하여, $\langle N_i, g_i(x)\rangle$ 이 주어졌을 때, $M$을 계산하는 효과적인 알고리즘이 존재합니다.

### 증명
- $N_i$ 가 <txtylw>서로소</txtylw>이기 때문에, `CRT`를 이용해 아래를 만족하는 <txtylw>계수</txtylw> $T_i$ 를 계산할 수 있습니다.
$$
T_i \equiv 
\begin{cases}
1 \bmod N_i \newline
0 \bmod N_j \newline
\end{cases}
\\ \\ \\ \text{where} \\ \\ \\ i \neq j
$$
- $g(x) = \sum{T_i \cdot g_i(x)}$ 라 두면, $g(M) \equiv 0 \bmod \prod N_i$ 를 만족합니다.
$$
\begin{align}
g(M) & = \sum{T_i \cdot g_i(x)} \newline
& = T_0 \cdot g_0(x) + T_1 \cdot g_1(x) + \cdots + T_k \cdot g_k(x) \newline
\end{align}
$$
