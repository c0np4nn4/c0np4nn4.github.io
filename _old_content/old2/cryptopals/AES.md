+++
title = "AES"
description = "AES core"
date = 2023-03-03
draft = false

[taxonomies]
categories = ["Cryptopals"]
tags = ["Cryptopals","Rust", "AES"]

[extra]
toc = true
keywords = "Cryptography, Cryptopals, Rust, AES"
+++

# AES
- <mark>AES</mark>는 <mark>DES</mark>를 보완하기 위해 고안된 대칭키 암호 알고리즘입니다.

- <mark>AES</mark>는 ***Byte-oriented*** 시스템으로, 바이트를 단위로 하여 연산을 진행합니다.

- <mark>AES</mark> 는 [SPN구조](https://en.wikipedia.org/wiki/Substitution%E2%80%93permutation_network) 의 대칭키 암호 알고리즘입니다.

---

## Block
- Block Size는 <kbd>128</kbd>-bit 입니다.

- 한 Block 은 Round 가 진행될 때, <mark>State</mark> 라는 이름으로 다뤄집니다.

- <mark>State</mark> 는 아래 그림과 같이 Matrix 형태로 연산에 이용됩니다. 

<center>
<img src="/cryptopals/img/AES_state.png" alt="Merge Sort Image" />
</center>

- 즉, <kbd>128</kbd>-bit 의 Block을 <kbd>16</kbd>-byte 의 State로 하여 암/복호화를 진행합니다.

---

## Key
- <mark>Key</mark> 는 <kbd>128</kbd>-bit, <kbd>192</kbd>-bit, <kbd>256</kbd>-bit 의 크기를 갖습니다.

- <mark>Key</mark> 는 <kbd>Key Expansion</kbd> 알고리즘에 의해 확장되어 이용됩니다.

- <mark>Key</mark> 의 크기에 따라, 전체 Round 의 수가 달라집니다.

<center>
<img src="/cryptopals/img/AES_overview.png" alt="Merge Sort Image" w=800 h=600) />
</center>

### Key Expansion

- 입력받은 <mark>Key</mark> 를 토대로, 각 Round 에서 사용할 **Round Key**를 생성하는 작업입니다.

---

## Round
- Round(Encryption) 는 크게 아래 네 과정으로 분류됩니다.
  - <mark>Byte Substitution</mark>
  - <mark>Shift Row</mark>
  - <mark>Mix Column</mark>
  - <mark>Add Round Key</mark>

- Decryption 에서의 ARK를 제외한 Round 연산은 모두 Inverse 연산입니다.

  ---

  ### <mark>Byte Substitution</mark>
    - Byte $A$ 를 <u>일정한 규칙</u>에 따라, Byte $B$ 로 바꾸는 작업입니다.
    - <u>일정한 규칙</u>으로써 아래 그림과 같은 Table 을 이용할 수 있습니다.

      <center>
      <img src="/cryptopals/img/AES_SboxTable.png" alt="Merge Sort Image" w=600 h=400) />
      </center>

    - 즉, Byte 의 값이 <mark>f4</mark> 라면, 행과 열의 교차점에 있는 <mark>bf</mark> 로 값을 바꾸는 방식입니다.

      <center>
      <img src="/cryptopals/img/AES_ByteSub.png" alt="Merge Sort Image" w=600  h=400) />
      </center>

  ---

  ### <mark>Shift Row</mark>
    - <mark>State</mark> 를 기준으로 설명하면 이해하기 쉽습니다.
    - 아래 그림처럼 각 행을 1, 2, 3 만큼 Left shift 해주는 작업입니다.

      <center>
      <img src="/cryptopals/img/AES_ShiftRow.png" alt="Merge Sort Image" w=400 h=200) />
      </center>

  ---

  ### <mark>Mix Column</mark>
    - ref: [wiki](https://en.wikipedia.org/wiki/Rijndael_MixColumns)
    - <mark>State</mark> 의 각 Column 을 $\text{GF}(2^8)$ 의 값을 갖는  아래 형태의 Polynomial 로 간주합니다.
    - 이 때, Coefficients 값은 $\text{GF}(2)$ 의 값입니다.

    $$b(x) = b_3x^3 + b_2x^2 + b_1x + b_0$$

    - 각각의 Column 은 아래 Polynomial 과 곱해지게 됩니다.

    $$
    \begin{align*}
    a(x) &= 3x^3 + x^2 + x + 2 \newline 
    &= a_3x^3 + a_2x^2 + a_1x + a_0
    \end{align*}
    $$

    - 이를 전개하면 아래와 같습니다.

    $$
    \begin{align*}
    a(x) \boldsymbol{\cdot} b(x) &= c(x) \newline
    &= (a_3x^3 + a_2x^2 + a_1x + a_0) \boldsymbol{\cdot} (b_3x^3 + b_2x^2 + b_1x + b_0) \newline
    &= c_6x^6 + c_5x^5 + c_4x^4 + c_3x^3 + c_2x^2 + c_1x + c_0
    \end{align*}
    $$

    - $c_k$ 를 정리하면 아래와 같습니다.

    $$
    \begin{align*}
    &c_0 = a_0 \cdot b_0 \newline
    &c_1 = a_1 \cdot b_0 \oplus a_0 \cdot b_1\newline
    &c_2 = a_2 \cdot b_0 \oplus a_1 \cdot b_1 \oplus a_0 \cdot b_2\newline
    &c_3 = a_3 \cdot b_0 \oplus a_2 \cdot b_1 \oplus a_1 \cdot b_2 \oplus a_0 \cdot b_3\newline
    &c_4 = a_3 \cdot b_1 \oplus a_2 \cdot b_2 \oplus a_1 \cdot b_3\newline
    &c_5 = a_3 \cdot b_2 \oplus a_2 \cdot b_3\newline
    &c_6 = a_3 \cdot b_3 \newline
    \end{align*}
    $$

    - 곱셈의 결과인 $c(x)$ 는 7개의 항으로 이루어져있습니다.

    - 따라서, 이를 4개 항, 즉 $4$-byte로 변환할 필요가 있습니다.

    - 이를 위해 *modular reduction* 을 수행합니다.

    $$
    \begin{align*}
    x^6 \bmod (x^4 + 1) &= -x^2 = x^2 \\ \\ \text{over} \\ \\ \text{GF}(2^8) \newline
    x^5 \bmod (x^4 + 1) &= -x = x \\ \\ \text{over} \\ \\ \text{GF}(2^8) \newline
    x^4 \bmod (x^4 + 1) &= -1 = 1 \\ \\ \text{over} \\ \\ \text{GF}(2^8) \newline
    \end{align*}
    $$

    - 위 결과를 이용하여 아래와 같이 정리할 수 있습니다.

    $$
    \begin{align*}
    a(x) \otimes b(x) &= c(x) \bmod (x^4 + 1) \newline
    &= (c_6x^6 + c_5x^5 + c_4x^4 + c_3x^3 + c_2x^2 + c_1x + c_0) \bmod (x^4 + 1) \newline
    &= c_6x^2 + c_5x^1 + c_4 + c_3x^3 + c_2x^2 + c_1x + c_0 \newline
    &= c_3x^3 + (c_6 \oplus c_2)x^2 + (c_5 \oplus c_1)x + (c_4 \oplus c_0) \newline
    &= d_3x^3 + d_2x^2 + d_1x + d_0 \newline
    \end{align*}
    $$

    - $d(x) = d_3x^3 + d_2x^2 + d_1x + d_0$ 를 결괏값이라고 두고, 아래와 같이 정리해볼 수 있습니다.

    $$
    \begin{align*}
    &d_0 = c_0 \oplus c_4 \newline
    &d_1 = c_1 \oplus c_5 \newline
    &d_2 = c_2 \oplus c_6 \newline
    &d_3 = c_0  \newline
    \end{align*}
    $$

    - 앞서, $c(x)$를 계산했으므로 아래와 같이 전개할 수 있습니다.

    $$
    \begin{align*}
    d_0 &= a_0 \cdot b_0 \oplus a_3 \cdot b_1 \oplus a_2 \cdot b_2 \oplus a_1 \cdot b_3 \newline
    d_1 &= a_1 \cdot b_0 \oplus a_0 \cdot b_1 \oplus a_3 \cdot b_2 \oplus a_2 \cdot b_3 \newline 
    d_2 &= a_2 \cdot b_0 \oplus a_1 \cdot b_1 \oplus a_0 \cdot b_2 \oplus a_3 \cdot b_3 \newline 
    d_3 &= a_3 \cdot b_0 \oplus a_2 \cdot b_1 \oplus a_1 \cdot b_2 \oplus a_0 \cdot b_3 \newline 
    \end{align*}
    $$

    - 또, $a(x)$ 를 알고 있으므로, 아래와 같이 다시 적을 수 있습니다.

    $$
    \begin{align*}
    d_0 &= 2 \cdot b_0 \oplus 3 \cdot b_1 \oplus 1 \cdot b_2 \oplus 1 \cdot b_3 \newline
    d_1 &= 1 \cdot b_0 \oplus 2 \cdot b_1 \oplus 3 \cdot b_2 \oplus 1 \cdot b_3 \newline 
    d_2 &= 1 \cdot b_0 \oplus 1 \cdot b_1 \oplus 2 \cdot b_2 \oplus 3 \cdot b_3 \newline 
    d_3 &= 3 \cdot b_0 \oplus 1 \cdot b_1 \oplus 1 \cdot b_2 \oplus 2 \cdot b_3 \newline 
    \end{align*}
    $$

    - 마지막으로 이를 행렬로 표기하면 아래와 같습니다.

    $$
    \begin{bmatrix}
      d_{0} \newline 
      d_{1} \newline 
      d_{2} \newline 
      d_{3} \newline
    \end{bmatrix} =
    \begin{bmatrix}
      2&3&1&1 \newline 
      1&2&3&1 \newline 
      1&1&2&3 \newline 
      3&1&1&2 
    \end{bmatrix}
    \begin{bmatrix}
      b_{0} \newline 
      b_{1} \newline 
      b_{2} \newline
      b_{3}
    \end{bmatrix}
    $$

  ---
  ### <mark>Add Round Key</mark>
  - <kbd>Round Key</kbd> 를 <mark>State</mark>에 XOR 연산으로 적용하는 단계입니다.

