+++
title = "Logistic Regression"
description = "ML (2)"
date = 2023-04-04

[taxonomies]
categories = ["Lect", "Machine Learning"]
tags = ["Machine Learning", "ML"]

[extra]
math=true
+++

---

# Classification
- 각 데이터가 어떤 클래스에 속하는지 판별하는 목적으로 사용됩니다.

---

# Logistic Regression
- <u>임의의 샘플</u>이 <u>특정 클래스</u>에 속할 확률을 $P$ 라고 하겠습니다.

---

## Odds Ratio
- `Odds Ratio` 는 확률 $P$ 와 $1-P$ 의 비를 나타냅니다.
$$Odds Ratio = \frac{P}{1-P}$$

---

## Logit Function
- `Odds Ratio`에 <u>자연 로그</u>를 취한 것을 `Logit Function` 이라 합니다.
$$logit(P) = \ln \frac{P}{1-P}$$
- `Logistic Regression` 에서는 이 `logit function` 를 변수 $X$의 선형결합으로 둡니다.
- 즉,
$$\ln \frac{P}{1-P} = \beta_0 + \beta_1 X_1 + \cdots + \beta_p X_p = \beta^{T} \mathbb{X}$$

---

## Sigmoid Function
- `Logit Function` 의 값을 $z$ 라 두었을 때, 아래와 같이 식을 유도할 수 있습니다.
$$
\begin{align}
\ln \frac{P}{1-P} &= z \newline
\frac{P}{1-P} &= e^z \newline
P &= e^z (1-P) \newline
P &= e^z - e^z P \newline
P (1 + e^z) &= e^z\newline
\therefore P &= \frac{e^z}{1+e^z} = \frac{1}{1+e^{-z}} \newline
\end{align}
$$
- 이를 $\phi(z)$ 라 정의하면, $0 \le \phi(z) \le 1$ 임을 알 수 있습니다($P$ 가 확률이기 때문).
- $\phi(z)$ 를 `Sigmoid Function` 이라 부르고, 확률과 같이 사용하게 됩니다.
- 조금 더 생각해보면, <u>인자</u>인 $z$ 는 `Logit Function` 입니다.
- 즉, `Sigmoid Function` 을  <u>특정 샘플이 해당 클래스에 속할 확률</u>로 활용할 수 있음을 알 수 있습니다.
- 예를 들어, Threshold Function 을 통해 아래와 같이 `Classification` 을 할 수도 있습니다.
$$\hat{y} = \begin{cases}1 \quad \phi(z) \ge \tau \newline 0 \quad otherwise\end{cases}$$

---

## Maximum Likelihood Estimation (MLE)
- `Logit Function`이 변수 $X$의 선형결합으로 나타난다고 했습니다.
- `MLE`는 변수 $X_i$의 가중치(계수) $\beta_i$ 의 값을 추정하는 방법입니다.
- <mark>Binary Classification</mark> 의 경우는 `Positive Class` 의 확률과 `Negative Class`의 확률을 곱한 값이 최대화되는 가중치를 찾습니다.
- <mark>Multinomial Classification</mark> 의 경우는 각각의 클래스에 대한 확률을 구하고 이를 모두 곱해서 최대화되는 가중치를 찾습니다.

---

# Multinomial Logistic Regression
- `Class` 가 $K \\ (K>2)$일 경우는 $K - 1$ 개의 `Logistic Regression` 이 필요합니다.
- 즉, <u>특정 샘플이 각각의 클래스에 속할 확률</u> 을 구해야 하는데, `확률` 을 구하는 것이므로 $K$ 번째 확률은 앞에서 구한 $K-1$ 개의 총합을 $1$에서 빼면 됨을 알 수 있습니다.
