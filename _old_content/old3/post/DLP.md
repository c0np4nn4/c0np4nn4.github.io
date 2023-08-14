+++
title = "DLP"
description = "Cryptography"
date = 2023-06-28
toc = true

[taxonomies]
categories = ["Cryptography"]
tags = ["cryptography"]

[extra]
math=true
+++

---
> [Github: ashutosh1206/Crypton](https://github.com/ashutosh1206/Crypton/tree/master)을 참고해서 작성되었음을 밝힙니다.

# 📌 순환군
- `순환군 (cyclic group)`은 아래와 같이 정의됩니다.
> - `임의의 원소`를 이용하여, modulus 연산과 함께 <txtylw>모든 원소를 생성</txtylw>할 수 있는 ***Abelian Group***
- 이 때 `임의의 원소`를 *생성자 (generator)* 라 부르며, `g` 로 보통 표기합니다.

---
## 예시

- $\mathbb{Z}_{11}^{*}$를 `order` = 10 인 곱셈에 대한 유한 군(finite group)라 두겠습니다.
- $\mathbb{Z}_{11}^{*}$ 의 원소는 *{1, 2, 3, 4, ..., 10}* 입니다.
- 임의의 원소 $g' \in \mathbb{Z}_{11}^{*}$ 에 대해, $g' = 5$라고 해보겠습니다.
- $g'$ 를 거듭제곱 했을 때, 생성되는 값들을 살펴보면 아래와 같습니다.

> $$5^0 \bmod 11 = 1$$
> $$5^1 \bmod 11 = 5$$
> $$5^2 \bmod 11 = 3$$
> $$5^3 \bmod 11 = 4$$
> $$5^4 \bmod 11 = 9$$
> $$5^5 \bmod 11 = 1$$
> $$5^6 \bmod 11 = 5$$
> $$5^7 \bmod 11 = 3$$
> $$5^8 \bmod 11 = 4$$
> $$5^9 \bmod 11 = 9$$
> $$5^{10} \bmod 11 = 1$$
> $$\dots$$

- 유심히 보면, $5^0$ 과 $5^5$ 의 값이 같고 `1, 5, 3, 4, 9`가 반복되는 양상입니다.
- 이렇게 <txtylw>반복적으로 순환</txtylw>하는 특징이 `Group`에 대한 `Cyclic`입니다.

---

- 위 결과에서 알 수 있듯이 $5$는 <txtred>생성자가 아닙니다</txtred>.
- 이번에는 $g' = 2$ 로 두고 해보도록 하겠습니다.
> $$2^0 \bmod 11 = 1$$
> $$2^1 \bmod 11 = 2$$
> $$2^2 \bmod 11 = 4$$
> $$2^3 \bmod 11 = 8$$
> $$2^4 \bmod 11 = 5$$
> $$2^5 \bmod 11 = 10$$
> $$2^6 \bmod 11 = 9$$
> $$2^7 \bmod 11 = 7$$
> $$2^8 \bmod 11 = 3$$
> $$2^9 \bmod 11 = 6$$
> $$2^{10} \bmod 11 = 1$$
> $$\dots$$
- 따라서, $2$는 유한군 $\mathbb{Z}_{11}^{*}$의 <txtred>생성자</txtred> 입니다.

---
## 정리
- `예시`로부터 살펴본 내용 기반으로 `Cyclic Group`의 여러 성질들을 정리하면 아래와 같습니다.
> - 모든 소수 $p$에 대하여, $\mathbb{Z}_p^{*}$ 는 `Cyclic Group`

> - 원소 $g'$에 대한 `order`, $\text{ord}(g') = k$ 
>     - $e$가 항등원일 때, ${(g')}^{k} = e$ 를 만족하는 가장 작은 자연수 $k$

> - `Group` $G$ 의 모든 원소 $g'$에 대하여, $|G|$ 는 원소의 개수를 의미할 때
>     - $\mathbb{Z}_p^{*}$ 인 경우, $|G| = p - 1$
>     - [페르마의 소정리](https://ko.wikipedia.org/wiki/%ED%8E%98%EB%A5%B4%EB%A7%88%EC%9D%98_%EC%86%8C%EC%A0%95%EB%A6%AC)에 의해, $(g')^{|G|} \bmod p = 1$

- ***Lagrange's Theorem***
    - $\forall {g'} \in G, \text{ord}({g'})$ 는 ${|G|}$ 의 약수
    - 즉, <u>임의의 원소가 생성한 그룹의 원소 개수</u>는 <u>전체 원소 개수</u>의 <txtylw>약수</txtylw>

---
# 📌 이산 로그 문제
- `이산 로그 문제(Discrete Logarithm Problem)`은 아래 방정식에서 $x$의 해를 찾는 문제입니다.
> - $y, g, x$ 값은 `Cyclic Group` $G = \mathbb{Z}_p^{*}$ 상에서 정의된 값입니다.
> - $y, g, p$ 는 알고 있는 정보
> $$y = g^x \bmod p$$
> - $p$ 가 반드시 `소수`일 필요는 없습니다.
