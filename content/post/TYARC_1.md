+++
title = "Twenty Years of Attacks on the RSA Cryptosystem"
description = "Attacks on RSA"
date = 2023-05-13
toc = true

[taxonomies]
categories = ["Cryptography", "RSA"]
tags = ["Cryptanalysis", "paper"]

[extra]
math=true
+++


본 포스트는 *[Twenty Years of Attacks on the RSA Cryptosystem](https://crypto.stanford.edu/~dabo/pubs/papers/RSA-survey.pdf)* 를 읽고 정리한 내용입니다.

# 1. Introduction
- `RSA` 는 *1977* 년에 ***Ron Rivest***, ***Adi Shamir***, 그리고 ***Len Adleman*** 에 의해 만들어 졌습니다.
- `RSA` 는 <u>*privacy*</u> 와 디지털 정보에 대한 <u>*authenticity*</u> 를 위해 다양한 분야에서 이용되고 있습니다.
---
- `RSA` 의 등장 이후 20여년간 수 많은 공격 방법들이 연구되어왔습니다.
- `RSA` 자체보다는 `RSA`를 <txtred>잘 못 사용하는 경우</txtred>의 위험성을 보여줍니다.
- 본 논문에서는 이러한 `공격 방법`들을 조사하고, 이들의 <mark>수학적 원리</mark>에 대해 기술합니다.
- 논문 전체에서 통신의 당사자 및 공격자를 아래와 같이 지칭합니다.
  - `Alice`, `Bob`: *통신 당사자*
  - `Marvin`: *공격자*
---
- `RSA` 를 간단히 살펴봅니다.
  - 큰 수 $N=pq$ 은 각각 $n/2$ 비트의 큰 소수 $p, q$ 의 곱입니다. (보통 $N$ 은 $1024\text{bits}$ 입니다.)
  - $\phi(N) = (p-1)(q-1)$ 에 대하여, $ed \equiv 1 \bmod \phi(N)$ 을 만족하는 두 정수 $e, d$ 를 구합니다.
  - $\phi(N)$ 은 *multiplicative group* $\mathbb{Z}^{*}_{N}$ 의 ***order*** 입니다.
  - $\langle N, e \rangle$ 는 `공개키`로써, <txtylw>암호화</txtylw>에 사용됩니다.
  - $\langle N, d \rangle$ 는 `비밀키` 혹은 `개인키`로써, <txtylw>복호화</txtylw>에 사용됩니다.
---
- 평문(메시지) $M \in \mathbb{Z}^{*}_{N}$ 에 대한 <txtylw>암호화</txtylw> 는 아래와 같습니다.
$$M^e \equiv C \bmod N$$
- 암호문에 대한 <txtylw>복호화</txtylw>는 아래와 같습니다.
$$C^d = (M^{e})^{d} = M^{ed} \equiv M \bmod N$$
- 이 때, 마지막 등호는 `오일러 정리`에 따른 결과입니다.
---
- `RSA` 를 아래와 같이 정의해볼 수 있습니다.
$$x \longmapsto x^e \bmod N$$
- 만약 $d$ 값이 주어지면, 위에서 살펴본 방법대로 쉽게 *거꾸로* 갈 수 있습니다.
- 이 때, $d$ 를 <txtred>Trapdoor</txtred> 라 부릅니다.
- 본 논문에서는 이러한 <txtred>Trapdoor</txtred> 없이 *거꾸로* 가는 것이 얼마나 어려운지 조사했습니다.
  - 좀 더 자세히는, $\langle N, e, C \rangle$ 이 주어지고 $N$ 의 인수 $p, q$ 를 모를 때,
  - $C$ 의 $e$번째 `root` 를 찾는 것이 얼마나 어려운지 조사했습니다.
- $\mathbb{Z}^{*}_{N}$ 이 `유한`한 집합이므로, 평문 $M$ 을 찾을 때까지 *brute-force* 를 해보는 것도 가능합니다.
- 하지만, **매우** 비효율적이기 때문에 본 논문에서는 보다 `효율적`인 방법을 찾고자 했습니다.
---
- 본 논문에서는 `RSA`의 <txtylw>cryptosystem</txtylw> 보다는 <txtylw>function</txtylw> 에 집중했습니다.
- `RSA` <txtylw>function</txtylw> 에서는 주어진 $\langle N, e, C\rangle$ 에 대한 *거꾸로* 연산을 하기 어렵다는 말이 <u>공격자가 평문을 복원하는 공격을 할 수 없음</u>으로 이어집니다.
- 하지만, `RSA` <txtylw>cryptosystem</txtylw>에서는 평문 $M$ 에 관한 <u>**어떠한**</u> 정보도 얻을 수 없어야 합니다.
  - 이를 `Semantic Security` 라 부릅니다.
---
- `RSA` <txtylw>function</txtylw> 은 <txtred>trapdoor one-way function</txtred> 의 한 예입니다.
$$x \longmapsto x^e \bmod N$$
- 즉, ***쉽게 계산*** 할 수 있지만, <txtred>trapdoor</txtred> $d$ 가 없으면 특별한 경우를 제외하고는 *거꾸로* 연산이 매우 어렵습니다.
- 이를 활용해 <txtred>Digital Signature</txtred> 를 `RSA` 로 구현할 수 있습니다.
- `Alice` 는 자신의 <txtylw>비밀키</txtylw> $\langle N, d\rangle$ 로 평문 $M$ 에 대하여 아래와 같이 `signature`를 생성합니다.
$$S=M^d \bmod N$$
- `Alice` 의 <txtylw>공개키</txtylw> $\langle N, e \rangle$ 은 누구나 알 수 있습니다.
- 따라서, 누구나 $\langle M, S \rangle$ 에 대해 `Alice` 의`signature` 임을 아래와 같이 확인할 수 있습니다.
$$M=S^e \bmod N$$
---
- `RSA` 의 *key pair* 는 $\frac{N}{2}$ bits 의 두 ***소수*** 를 고른 뒤, 곱하여 $N$ 을 얻는 것으로 시작합니다.
- $e < \phi(N)$ 을 만족하는 $e$ 를 구합니다.
- $d \equiv e^{-1} \bmod \phi(N)$ 을 만족하는 $d$ 를 `확장 유클리드 알고리즘`으로 구합니다.
- ***소수*** 의 집합이 *충분히* 조밀하기 때문에, $\frac{N}{2}$ bits 의 무작위 정수를 선택한 뒤 `probabilistic primality test` 로 test 하여 소수를 빠르게 구할 수 있습니다.
---

## 1.1 Factoring Large Integers
- `RSA` 공개키 $\langle N, e \rangle$ 에 대한 첫 번째 공격으로 고려해볼만한 것은 $N$에 대한 <txtred>소인수분해</txtred> 입니다.
- $N$ 에 대한 소인수분해가 알려져있다면, $p, q$ 를 쉽게 구할 수 있습니다.
>  - $p, q$ 를 알면 $\phi(N) = (p-1)(q-1)$ 를 계산할 수 있습니다.
>  - $d \equiv e^{-1} \bmod \phi(N)$ 이기 때문에, <txtylw>비밀키</txtylw> $d$ 를 손쉽게 구할 수 있습니다.
- 본 논문에서는 $N$ 에 대한 소인수 분해를 `brute-force attack on RSA`라 명명합니다.
- `소인수분해` 알고리즘은 꾸준히 발전해왔으나, `RSA` 에 대한 위협이 될 정도는 <txtred>아닙니다</txtred>.
- 현재 가장 빠른 소인수분해 알고리즘으로 알려진 것은 `General Number Field Sieve(GNFS)` 입니다.
- `GNFS` 의 $n$ bits 정수에 대한 시간 복잡도는 아래와 같이 알려져 있다고 합니다.
$$\text{exp}\big((c + o(1))n^{1/3}\log^{2/3}n\big)\quad\quad (c<2)$$
- 따라서, 위 시간보다 더 오랜 시간이 걸리는 (i.e. `brute-force`) 방법은 고려할 대상이 아닙니다.
---
- 본 논문의 목적은 $N$ 을 소인수분해하지 않고도 평문 $M$ 을 복원하는 공격을 조사하는 것입니다.
- 그럼에도 불구하고, $N=pq$ 의 몇몇 경우가 <txtylw>간단히 소인수분해된다</txtylw>는 점을 살펴볼 가치는 있습니다.
  - 예를 들어, $p-1$ 이 $B$ 보다 작은 어떤 *소수* 들의 곱이라면,
  - $N$ 은 $B^3$ 이하의 시간에 소인수분해 될 수도 있습니다.
- 그래서 몇몇 `RSA` 구현들은 $p-1$ 에 대한 추가적인 검사를 하기도 합니다.
---
- 위에서 논한 내용으로 `RSA` 와 소인수 분해에 관해 아래의 `open problem` 이 있습니다.
> $\gcd(e, \phi(N))=1$ 을 만족하는 $N, e$ 에 대하여 `함수` $\mathcal{f}_{e, N}$ 를 아래와 같이 정의할 수 있습니다.

