+++
title = "Paper Review: A Family of Optimal Locally Recoverable Codes"
description = "Tamo-Barg Code"
date = "2025-05-26"
draft = false

[taxonomies]
categories = ["Paper Review"]
tags = ["Information Theory", "Coding Theory", "LRC", "Reed-Solomon Code"]

[extra]
math = true
+++

# 1. Introduction

# 2. Preliminaries of LRC Codes

## 📌 Definition
어떤 코드 $\mathcal{C} \in \mathbb{F}^n_q$가 <span style="color: tomato">*locality*</span> $r$ 을 갖는다는 것의 의미는,
코드워드의 모든 심볼 $\forall x \in \mathcal{C}$이 $r$개의 서로 다른 심볼들로 이루어진 <span style="color: skyblue">부분집합</span>을 통해 복원될 수 있음을 의미한다.
(다시 말해, $r$개의 서로 다른 심볼 $x_{i_1}, x_{i_2}, \dots, x_{i_r}$에 대한 함수 $f: (x_{i_1}, x_{i_2}, \dots, x_{i_r}) \rightarrow x$가 존재함을 의미한다.)

좀 더 자세히 얘기하자면, $i$번째에 위치하는 코드워드의 심볼 $x_i$에 대하여, 아래 조건을 만족하는 <span style="color: skyblue">부분집합</span> $I_i$ 가 존재한다고 가정하자.

$$
\begin{aligned}
I_i &\in [n] \backslash i \newline
|I_i| &\le r
\end{aligned}
$$

이 때, 코드 $\mathcal{C}$를 $I_i$의 위치로만 제한하면, $x_i$ 의 값을 복원할 수 있다.
이러한 <span style="color: skyblue">부분집합</span> $I_i$를 <span style="color: skyblue">*recovering set*</span>이라 한다.

***LRC***에 대한 보다 형식적인 정의는 다음과 같다.
임의의 필드 위 원소 $a \in \mathbb{F}_q$에 대하여 다음의 코드워드 집합을 가정한다.

$$
C(i, a) = \lbrace x \in \mathcal{C}: x_i = a \rbrace, \quad i \in [n]
$$

$C(i, a)$는 코드 $\mathcal{C}$에 속하는 임의의 코드워드 $x$들 중, 임의의 위치 $i$에 대한 심볼 값이 $x_i = a$로 동일한 코드워드들의 집합을 의미한다.

만약 모든 $i \in [n]$에 대하여 앞서 살펴본 <span style="color: skyblue">부분집합</span> $I_i$가 존재한다고 해보자.
$I_i$가 있다는 것은, $x_i$를 복원할 때 필요한 $r$개의 서로 다른 심볼들의 위치가 존재함을 의미한다.

$I_i$로 인덱스를 제한한 코드워드들이 <span style="color: gold">*다른 집합*</span> $\mathcal{C}(i, a')$과 겹치는 부분이 없을 때(disjoint),
코드 $\mathcal{C}$는 <span style="color: tomato">*locality*</span> $r$ 을 갖는다고 할 수 있다. 즉, 아래 식을 만족함을 의미한다.

$$
C_{I_i}(i, a) \medspace \cap \medspace C_{I_i}(i, a') = \text{\O}, \quad a \neq a'
$$

결과적으로, 코드 $\mathcal{C}_{I_i \cup \lbrace i \rbrace}$를 코드 $\mathcal{C}$의 <span style="color: tomato">***local code***</span>라 부른다.
LRC 코드의 구성에서는 $(n, k, r)$ LRC 코드가 보통 $(r+1, r)$의 <span style="color: gold">*로컬 MDS 코드*</span>로 나뉘어서 심볼의 <span style="color: skyblue">*recovering set*</span> 역할을 한다.

## 📌 Properties
LRC 코드의 성질은 크게 두 가지로, *minimum distance*와 *rate*의 bound에 관한 것이다.

LRC 코드 $\mathcal{C}$의 <span style="color: tomato">rate</span> 는 아래의 upper bound를 갖는다.

$$
\frac{k}{n} \le \frac{r}{r+1}
$$

그리고 $\mathcal{C}$의 <span style="color: tomato">minimum distance</span>는 아래의 upper bound를 갖는다.

$$
d \le n - k - \lceil\frac{k}{r}\rceil + 2
$$

그리고, <span style="color: tomato">distance</span>에서 $d = n - k - \lceil\frac{k}{r}\rceil + 2$를 만족하는 코드가 <span style="color: gold">*Optimal LRC code*</span> 이다.

기본적으로 `(n, k) linear code` 이므로, $k$ 개의 심볼로 어느 코드워드 심볼이든 복구 할 수 있음이 명확하다.
따라서, 자연스레 $r = k$를 만족하고 $1 \le r \le k$ 범위로 <span style="color: tomato">*locality*</span>를 가짐을 알 수 있다.

1. $r=k$

> 만약 $r=k$인 경우를 생각해보면, <span style="color: tomato">distance</span>는 아래와 같이 계산된다.
> $$
> \begin{aligned}
> d &\le n - k - \lceil\frac{k}{k}\rceil + 2 \newline
> \therefore d &\le n - k + 1
> \end{aligned}
> $$
> 따라서, RS코드와 같은 MDS코드의 distance 로 귀결됨을 알 수 있다.

2. $r=1$

> 반면에 $r=1$인 경우를 생각해보면, <span style="color: tomato">distance</span>가 아래와 같이 계산된다.
> $$
> \begin{aligned}
> d &\le n - k - \lceil\frac{k}{1}\rceil +2 \newline
> \therefore d &\le 2 \lparen \frac{n}{2} - k + 1 \rparen
> \end{aligned}
> $$
> 따라서, $(n/2, k)$ MDS 코드의 심볼을 두 배로 복제한 코드워드와 같음을 알 수 있다.

# 3. Code Construction
이 장에서는 <span style="color: tomato">***optimal linear $(n, k, r)$ LRC code***</span>을 구성한다.
코드는 유한체 $\mathbb{F}_q$ 상에서 정의되며, $q$는 소수의 거듭제곱 또는 $n$이다.

## 3.1 General Construction

