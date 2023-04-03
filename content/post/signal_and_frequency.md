+++
title = "Signal and Frequency"
description = "Signal and Frequency"
date = 2023-04-04

[taxonomies]
categories = ["Lect"]
tags = ["Data communication", "Network", "CB24149"]

[extra]
math=true
+++

---

# Signal
- 전자기적인 `Signal` 이라 함은, 정보를 전달하는 어떠한 <mark>수단</mark> 이라 정의할 수 있습니다.
- `Signal` 을 <u>시간에 대한 함수</u> $s(t)$ 로 나타내어 보면 다음과 같이 분류될 수 있습니다.
  - <mark>Discrete</mark> / <mark>Continuous</mark>
  - <mark>Analog</mark> / <mark>Digital</mark>
    - <mark>Analog</mark>
      - 끊임없이 변화하는 전자기파의 형태입니다.
      - 유선, 광섬유, 공기 중 등 ***다양한 매질*** 을 통해 전파될 수 있습니다.
      - 신호의 감쇠 정도가 <u>낮습니다</u>.
    - <mark>Digital</mark>
      - 전압 펄스의 연속 형태입니다. (010101...)
      - 오직 유선 매체에서만 전달 가능합니다.
      - 저렴하고, 노이즈에 강합니다.
      - 신호의 감쇠 정도가 <u>높습니다</u>.
  - <mark>Periodic</mark> / <mark>Aperiodic</mark>
    - 주기함수의 형태인 경우 아래와 같이 식을 적을 수 있습니다.
    $$s(t + P) = s(t)$$

---

# Period, Frequency
- 주기가 $T$인 주기함수 $s(t)$ 는 아래와 같이 쓸 수 있습니다.
$$s(t + T) = s(t)$$
- <mark>주기</mark>: $T$
- <mark>진동수/주파수</mark>: $f = {1}/{T}$

## Trigonometric Function
- `삼각함수`는 대표적인 주기함수입니다.
- $\cos(\omega x)$
  - <mark>주기</mark>: $T=2 \pi / \omega$
  - <mark>진동수</mark>: $f = {\omega}/{2 \pi}$
- $\cos(2 \pi f x)$
  - <mark>주기</mark>: $T=2 \pi / 2 \pi f = 1 / f$
  - <mark>진동수</mark>: $f$
- 따라서,$\cos(2 \pi \cdot 100 \cdot x)$ 함수의 <mark>진동수</mark>는 $100$ 입니다.

---

# Domain: Time to Frequency
- 시간에 대한 함수 $x(t)$ 로 <mark>주파수</mark>가 각각 $f_1, f_2, f_3$ 인 신호들의 선형결합인 신호를 표현하면 아래와 같습니다.
$$x(t) = c_1 \cdot \cos(2 \pi \cdot f_1 \cdot t) + c_2 \cdot \cos(2 \pi \cdot f_2 \cdot t) +c_3 \cdot \cos(2 \pi \cdot f_3 \cdot t)$$
- 사실 위 식을 그래프로 나타내면, 한 눈에 `세 종류의 신호가 선형결합된 형태` 임을 파악하기는 **불가능**합니다.
- 대신, domain 을 시간 $t$ 에서 주파수 $f$ 로 변환해주면 **가능**합니다.
- 이는 본질적으로, `어떤 신호는 다른 주기 신호함수들의 선형결합` 이기 때문에 가능합니다.
