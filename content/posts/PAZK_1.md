+++
title = "Reed-solomon Fingerprinting"
description = "An example of the efficiency of randomness"
date = "2025-05-23"

[taxonomies]
categories = ["ThalerBook"]
tags = ["Proof System"]

[extra]
blog_categorized = true # posts can be categorized
math = true
+++

# 2.1 Reed-Solomon Fingerprinting

  증명 시스템을 본격적으로 다루기에 앞서, `무작위성(randomness)`이 어떻게 특정 알고리즘의 효율을 극대화시킬 수 있는지를 살펴본다. 여기서 다루는 증명 과정의 참여자들은 모두 신뢰할만하고, 계산 능력도 충분하다고 가정한다. 즉, 아래 사항을 전제로 한다.
- *Alice*, *Bob* 이 서로를 신뢰(trust)한다.
- 특정한 연산 $f$ 을 수행할 충분한 능력이 있다.

## 2.1.1 Setting
  `Alice` 와 `Bob`이 서로 멀리 떨어져 지낸다고 가정해본다. 그들 각각은 $n$ 개의 글자로 이뤄진 **크기가 큰 파일**을 가지고 있다. 각 글자를 ASCII 코드로 생각하면 총 $m=128$ 가지의 글자가 올 수 있다.
`Alice` 와 `Bob`의 파일을 각각 아래와 같이 나타내면,
$$
\begin{cases}(a_1, a_2, \cdots, a_n) \\ (b_1, b_2, \cdots, b_n)\end{cases}
$$
이들의 <font color="tomato">목표</font>는 다음과 같다.
$$
\text{Pr}[a_i = b_i: \forall i \in [n]] = 1
$$
  파일의 크기가 크기 때문에, 이들의 실제 목표는 *통신 비용 (communication cost)* 을 최대한 낮추는 것이 된다. 
  <font color="tomato">목표</font>를 달성하는 가장 직관적인 방법은 `Alice` 가 모든 파일을 `Bob`에게 넘기는 것이다. 하지만, 파일의 크기가 너무 크기 때문에 이러한 전송은 현실적이지 않다. 1997년에 작성된 Eyal Kushilevitz와 Noam Nisan 의 논문 ***Communication Complexity*** 에서는 이런 직관적인 방법보다 더 낮은 정보를 전송하는 *deterministic* procedure는 없음을 증명하기도 했다. (즉, 파일을 그대로 주는게 결정론적 절차에서는 가장 효율적임을 의미한다.)
  하지만, `Alice`와 `Bob`이 아주 작은 확률로 오답을 내놓는 *randomized* procedure 를 수행할 수 있다고 가정할 때, 훨씬 적은 communication 으로 <font color="tomato">목표</font>를 달성할 수 있음을 확인할 수 있다.
## 2.1.2 Communication Protocol
### High-Level Idea
개략적인 아이디어는 `Alice`가 아래 두 값을 주는 것이다.
1. (small) family of hash functions $\mathcal{H}(\cdot)$ (특정한 구조나 성질을 만족하는 해시 함수들의 집합 정도로 이해할 수 있다)
2. 파일 $\textbf{a}=(a_1, a_2, \dots, a_n)$ 에 대한 해시값 $h(\textbf{a}) \text{ where } h \in \mathcal{H}$
이 때, 해시값 $\mathcal{H}(\textbf{a})$ 를 일종의 <span style="color: tomato">***Fingerprint***</span> 로 생각할 수 있다. 만약 서로 다른 $x, y$ 에 대해 아래 식이 성립한다면, 우리는 ***Fingerprint*** 를 특정 데이터에 대한 "거의 고유한 식별자"로 활용할 수 있게 된다.

$$
\forall x \neq y, \underset{h \in \mathcal{H}}{\text{Pr}}[h(x) = h(y)] \le \epsilon
$$
따라서, `Alice`가 $\textbf{a}, h(\textbf{a})$ 를 전송하면 `Bob`은 $h(a) \overset{?}{=} h(b)$ 를 진행한다.
- $h(a) \neq h(b)$ 이면, `Bob`은 $a \neq b$ 임을 알게 된다.
- $h(a) = h(b)$ 이면, `Bob`은 $a=b$ 임을 **높은 확률로 확신**할 수 있게 된다.

### Details
위 아이디어를 더 구체적으로 만들기 위해 소수 $p \ge \max\{m, n^2\}$ 와 $\mathbb{F}_p$ 를 정한다.
소수 $p$ 의 lower-bound에 대해서 아래와 같이 설명할 수 있다.
1. $p \ge n^2$ 인 이유
   
   우선 <span style="color: red">에러</span>가 발생하는 경우는, 서로 다른 데이터 $a, b$ 에 대해 같은 해시 값이 나오는 경우이다. 즉, 아래와 같이 확률을 계산할 수 있다.
   $$
   \text{Pr}[h(a) = h(b), \text{ where } a \neq b] = \frac{1}{p}
   $$
   이 때, 각 데이터가 길이 $n$인 벡터이므로 <span style="color: skyblue">데이터가 서로 다르다</span>라는 의미는 최대 $n$ 개의 character가 다르다는 의미와 같다. 즉, 아래와 같이 기술할 수 있다.
   $$
   \text{Pr}[h(a) = h(b), \text{ where } a \neq b, |a| = |b| = n] \le \frac{n}{p}
   $$
   ?????