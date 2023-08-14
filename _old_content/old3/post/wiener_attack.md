+++
title = "Wiener's Attack"
description = "Attacks on RSA"
date = 2023-08-05
toc = true

[taxonomies]
categories = ["Cryptography", "RSA"]
tags = ["Cryptanalysis"]

[extra]
math=true
+++

---
# Wiener's Attack

- 위너의 공격(암호학자 마이클 J. 위너의 이름에서 따옴)은 RSA에 대한 암호 공격의 한 유형입니다. 이 공격은 연속 분수 방법을 사용하여 d가 작을 때 비공개 키 d를 노출시킵니다.

---
# Background of RSA
- <txtylw>Alice</txtylw>와 <txtylw>Bob</txtylw>이 있다고 해봅시다.
- 둘 사이에 `RSA`를 이용한 비밀 통신을 하려고 합니다.
- 먼저 <txtylw>Bob</txtylw>은 두 소수 $p$와 $q$를 선택합니다. 
- 그 다음 $N = pq$를 계산하고, $(N, e)$는 <txtred>공개키</txtred>로써 공개합니다.
    - 이 정보를 공개함으로써 누구나 밥에게 메시지를 암호화할 수 있습니다. 
- $d$는 $ed=1 \pmod{\lambda(N)}$을 만족하는 값입니다
    - 여기서 $\lambda(N)$은 카마이클 함수(Carmichale's function)를 나타내며, 종종 오일러 피 함수 $\varphi(N)$가 사용됩니다.
    - $e$와 $\lambda(N)$은 <txtylw>서로소</txtylw>로, <txtred>역수</txtred>가 존재하게 됩니다. 
- N의 인수분해와 비공개 키 $d$는 <txtred>비밀</txtred>로 유지되므로 오직 <txtylw>Bob</txtylw>만이 메시지를 복호화할 수 있습니다. 
- <txtred>비공개 키</txtred>를 (d, N)으로 표시합니다. 
- 메시지 M의 암호화는 $C \equiv M^{e} \pmod{N}$ 으로 주어지며, 복호화는 $C^{d} \equiv (M^{e})^{d} \equiv M^{ed} \equiv M \pmod{N}$으로 주어집니다 
    - 페르마의 소정리를 사용합니다.

---
# Small private Key
- <txtred>Decryption</txtred> 연산의 속도 향상을 위해 $d$ 값을 작게 설정할 수도 있습니다.
- 하지만, 이는 곧 <txtylw>Wiener's Attack</txtylw> 에 따라 ***RSA 시스템이 뚫릴 수 있는 위험*** 으로 직결됩니다.
- <txtylw>Wiener</txtylw>는 $d < \frac{1}{3}N^{\frac{1}{4}}$ 일 때, $d$ 값을 충분히 효율적으로 복구할 수 있음을 증명했습니다.
    - 또한, 아주 큰 공개키($e$)와 `CRT`를 이용해서 본인이 제시한 공격을 피하는 방법에 대해서도 제시했습니다.

---
# How Wiener's attack works
- `RSA`에서 카마이클 함수는 아래와 같습니다.
$$
\begin{align}
\lambda(N)&=\text{lcm}{(p-1, q-1)}=\frac{(p-1)(q-1)}{G}=\frac{\phi(N)}{G} \newline
\text{where} \\ \\ \\ G&=\gcd(p-1, q-1)
\end{align}
$$

- $e$와 $d$의 관계에 따라 아래와 같이 식을 전개할 수 있습니다.

$$
\begin{align}
ed &\equiv 1 \bmod (\lambda(N)) \newline
ed &= K \times \lambda(N) + 1 \newline
ed &= \frac{K}{G} (p-1)(q-1) + 1
\end{align}
$$

- 만약 $k=\frac{K}{\gcd(K, G)}$ 그리고 $g=\frac{G}{\gcd(K, G)}$ 로 두면,
$$
\begin{align}
ed = \frac{k}{g}(p-1)(q-1) + 1
\end{align}
$$

- $dpq$ 로 양 변을 나누면,
$$
\begin{align}
\frac{e}{pq} &= \frac{k}{dg}(1-\delta) \newline
\text{where} \\ \\ \\ \delta &= \frac{p+q-1-\frac{g}{k}}{pq}
\end{align}
$$

- 따라서, $\frac{e}{pq}$ 가 $\frac{k}{dg}$ 보다 아주 조금 작은 값임을 알 수 있습니다.
- 또, $\frac{e}{pq} = \frac{e}{N}$ 이므로, <txtylw>좌항</txtylw>은 모두 알고 있는 값으로 구성됨을 확인할 수 있습니다.
