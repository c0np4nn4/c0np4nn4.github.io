+++
title = "📌 1. ZK-SNARKOP"
date = 2023-02-03

[taxonomies]
tags = ["kr", "Cryptography", "ZKP", "Zero-Knowledge Proof"]
+++

<!-- Math rendering -->
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML' async></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']]
  },
  TeX: {
    extensions: ["AMSmath.js"],
  }
});
</script>
<!-- Math rendering -->

# Description
- 임의의 다항식 $P$ 에 대하여,
  - *Prover* 는 자신이 $P$ 를 알고 있음을 주장한다.
  - *Verifier* 는 이를 검증한다.

---

# Succinct Non-interactive ARgument of Knowledge of Polynomial
- *Prover* 가 아래와 같이 주장한다고 하자.
  - "나는 degree가 $d$ 이고 $t(x)$ 를 인자로 갖는 다항식 $p(x)$ 를 알고 있다."
  - 즉, $p(x) = c_d \cdot x^d + \cdots = t(x) \cdot h(x)$ 인 $p(x)$ 를 알고 있다고 주장한다.
- 아래의 `Setup`, `Prove`, `Verify` 과정을 통해 이를 증명할 수 있다.

---

## Setup
- **secret parameter** $s, \alpha$ 를 정한다.
- `Homomorphic Encryption` $E(x)$ 를 다음과 같이 정의한다. 
$$E(x) = g^x \bmod n$$
- $E(x)$ 를 이용해 아래의 값들을 계산한다.
$$E(\alpha), {\{E(s^i)\}}, {\{E(\alpha \cdot s^i)\}} \text{ where } (i \in \{ 0, \dots, d \} ) $$
- 계산된 값들을 이용하여, `Proving Key`와 `Verifying Key`를 생성한다.
  - `Proving Key`: $(E(s), E(\alpha \cdot s))$
  - `Verifying Key`: $(E(\alpha), E(t(s)))$

---

## Proving
- `다항식을 알고 있다`는 것은 `Coefficient`를 알고 있다는 것과 동치이다.
  - 즉, ${c^i}_{i \in \{ 0, \dots, d \} }$ 를 이용해 아래와 같이 다항식을 만들 수 있다.
  - $p(x) = c_d \cdot x^d + \cdots + c_1 \cdot x^1 + c_0 \cdot x^0$
- 다항식 $h(x)$ 를 계산한다.
  - $h(x) = \frac{p(x)}{t(x)}$
- `Proving Key` 를 이용해 아래의 값들을 계산한다.
  - `encrypted evaluation`: $E(p(s)), E(h(s))$
  - `encrypted evaluation with alpha shift`: $E(\alpha \cdot p(s))$
- `Zero-Knowledge`를 위해 임의의 값 $\delta$ 를 정한다.
- 아래와 같이 주장에 대한 `Proof` $\pi$ 를 완성한다.
  - $\pi = (E(\delta p(s)), E(\delta h(s)), E(\delta \alpha p(s)))$

---

## Verification
- `Proof` $\pi$ 를 아래와 같이 파싱한다.
  - $\pi = (\mathfrak{p}, \mathfrak{h}, \mathfrak{p}')$
- `Verification` 에서는 `Homomorphic Encrypted Data` 에 대한 곱셈 연산을 위해 `Cryptographic Pairing` 을 이용한다.
- `Cryptographic Pairing` 을 이용한 곱셈은 아래와 같이 이해할 수 있다.
  - $e(E(a), 1) = e(1, E(a))$
  - $e(E(a), E(b)) = e(E(ab), 1)$
  - $e(E(a), E(b)) = e(1, 1)^{ab}$
- `Verify` 는 크게 두 가지 과정을 거친다.
  - `polynomial restriction check` 를 아래와 같이 진행한다.
    - $e(\mathfrak{p'}, 1) = e(\mathfrak{p}, E(\alpha))$
  - `polynomial cofactor check` 를 아래와 같이 진행한다.
    - $e(\mathfrak{p}, 1) = e(\mathfrak{h}, E(t))$

