+++
title = "Baby Step Giant Step"
description = "Cryptography"
date = 2023-06-29
toc = true

[taxonomies]
categories = ["Cryptography"]
tags = ["Cryptanalysis"]

[extra]
math=true
+++

---

# DLP
- `DLP` 문제는 아래의 수식에서 $y, g, p$ 를 알고 있을 때, $x$ 값을 구하는 문제입니다.
> $$y = g^x \bmod p$$
- <txtylw>Brute Force</txtylw> 이를 해결하려면 $O(n)$의 시간이 필요합니다.
    - $n$은 *order of group*
- 이를 $O(\sqrt{n})$로 해결할 수 있는 `Baby-Step Giant-Step`에 대해 살펴봅니다.

---

# Baby-step Giant-step
- <txtylw>order</txtylw>가 $n$인 어떤 <txtylw>순환군(cylclic group)</txtylw> $G$, <txtylw>생성자(generator)</txtylw> $\alpha$, 임의 원소 $\beta$에 대하여 아래 수식의 $x$ 를 찾는 문제가 있다고 해보겠습니다.
$${\alpha}^{x} = \beta \bmod n$$
- `Baby-Step Giant-Step` 알고리즘은 $x$ 를 다시 쓰는 것에서부터 시작합니다.
- 우선 $m$ 이라는 값을 아래와 같이 정의합니다.
    - $m=\lceil \sqrt{n} \rceil$
- 만약 $x$에 대해 $m$으로 나눈 것을 수식으로 표현하면 다음과 같습니다.
$$x = m * q + r$$
- $r$ 은 나머지 이므로, $0 \le r < m$ 입니다.
- 또, 연산이 $\bmod n$에서 일어나므로, 자연스레 $0 \le q < m$ 입니다.
- 따라서, 원래 식을 아래와 같이 수정할 수 있습니다.
> $$
> \begin{align}
> \alpha^{x} &= \beta \newline
> \alpha^{mq + r} &= \beta \newline
> \alpha^{mq} * \alpha^{r} &= \beta \newline
> \alpha^{r} &= \beta * (\alpha^{-m})^q \newline
> \end{align}
> $$

---
## Algorithm
- 핵심은 search space 가 줄어들었다는 것입니다.
- $\beta$, $m$ 값은 이미 알고 있기 때문에, $(q, r)$ 값에 대해서만 고려하면 됩니다.
> - 1. $\sqrt{n}$ 크기의 $r$ 에 대해, $\alpha^{r}$ 값을 미리 계산해둡니다.
> - 2. $\sqrt{n}$ 크기의 $q$ 에 대해, $\beta * (\alpha^{-m})^q$ 를 계산합니다.
> - 3. 만약 등호가 성립하는 $(q, r)$ 쌍이 있다면, $x = m*q + r$ 를 계산하여 $x$ 를 구할 수 있습니다.

- [[python2 구현, github]](https://github.com/ashutosh1206/Crypton/tree/master/Discrete-Logarithm-Problem/Algo-Baby-Step-Giant-Step#implementation)

---
## Reference
- [Crypton](https://github.com/ashutosh1206/Crypton/tree/master/Discrete-Logarithm-Problem/Algo-Baby-Step-Giant-Step#baby-step-giant-step-algorithm)
- [Wikipedia](https://en.wikipedia.org/wiki/Baby-step_giant-step)
- [CMU pdf](http://www.cs.umd.edu/~gasarch/COURSES/198/Su14/baby.pdf)
