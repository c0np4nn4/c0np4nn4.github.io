+++
title = "Linear Regression"
description = "ML (1)"
date = 2023-03-29

[taxonomies]
categories = ["Lect", "Machine Learning"]
tags = ["Machine Learning", "ML" ]

[extra]
math=true
+++

---

# Simple Linear Regression
- 변수 $X$ 에 대한 값 $Y$ 를 예측하는 간단한 모델입니다.
$$Y \approx \beta_0 + \beta_1 X$$
- 위 식에서 <mark>$\beta_0$</mark>, <mark>$\beta_1$</mark> 은 모델의 **Coefficient** 또는 **parameter** 라 부릅니다.
>  - <mark>$\beta_0$</mark> 는 *intercept* 라고도 불리며, **$\bf{y}$ 절편**을 나타냅니다.
>  - <mark>$\beta_1$</mark> 은 *slope* 라 불리며, 선형 모델에서의 **기울기**를 의미합니다.
- 일단 *training data* 를 이용해 모델을 생성하면, 이 두 **Coefficient** 를 계산해낼 수 있습니다.
- 생성된 모델이 예측한 값은 *추정치* 이므로, 수식의 모든 <u>*추정치*</u> 에 **$\hat{ }$** 을 씌워줍니다.
- 즉, 선형 회귀 모델은 아래와 같은 수식을 같습니다.
$$\hat{y} = \hat{\beta_0} + \hat{\beta_1}x$$

---

- 모델을 구성하는 *추정* **Coefficient** 를 구하는 방법에 대해 살펴봅니다.
- 학습할 때 사용한 데이터 $(x_1, y_1), (x_2, y_2), \dots (x_n, y_n)$ 에 대하여, $\hat{y} = \hat{\beta_0} + \hat{\beta_1} x$ 가 모든 점에 가장 가깝게 지나갈 수 있도록 $\hat{\beta_0}, \hat{\beta_1}$ 값을 정하면 됩니다.
- 이 때 사용되는 방법이 `Least Squares` 입니다.

---

- <mark>i</mark> 번째 $x$ 값에 대한 예측인 $y$ 값을 계산하는 모델을 아래와 같이 정의했다고 합시다.
$$\hat{y} = \hat{\beta_0} + \hat{\beta_1}x$$
- 이 때, **실제값** $y_i$ 와 **예측된 값** $\hat{y_i}$ 값의 차이를 `Residual` 이라 부르고, 아래와 같이 정의합니다. 
$$e_i = y_i - \hat{y_i}$$
- 따라서, `Residual Sum of Squares, RSS` 은 아래와 같이 정리할 수 있습니다.
$$\text{RSS} = e_1^2 + e_2^2 + \cdots + e_n^2$$
- $e_i = y_i - \hat{y_i}$ 이므로,
$$\text{RSS} = (y_1 - \hat{\beta_0} - \hat{\beta_1}x_1)^2 + \cdots (y_n - \hat{\beta_0} - \hat{\beta_1}x_n)^2$$
- 직관적으로, $\text{RSS}$ 의 값이 최소가 되어야 `예측 모델이 실제 값에 근접한 결과를 낼 수 있음`을 알 수 있습니다.
- 약간의 계산을 하면 최솟값일 때의 $\hat{\beta_0}, \hat{\beta_1}$ 값은 아래와 같이 정리할 수 있습니다.
$$
\begin{align}
  \hat{\beta_1} & = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n} (x_i - \bar{x})^2} \newline
  \hat{\beta_0} & = \bar{y} - \hat{\beta_1}\bar{x} \newline
\end{align}
$$
- 이 때, $\bar{x} = \frac{1}{n} \sum_{i=1}^n x_i$, $\bar{y} = \frac{1}{n} \sum_{i=1}^n y_i$ 로, `샘플 데이터의 평균` 을 의미합니다.

---

# Multiple Linear Regression
- Real World 에서는 `단일 변수`에 의해 현상을 예측하는 일은 적습니다.
- 따라서, `다양한 변수`를 이용해 `예측 모델`을 만드는 방법에 대해 고민할 필요가 있습니다.
- 일반적으로 아래와 같이 식을 세울 수 있습니다.
$$Y = \beta_0 + \beta_1 X_1 + \cdots + \beta_p X_p + \epsilon$$
- 각각의 변수는 다음을 의미합니다.
  - $Y$: 예측 결과를 의미합니다.
  - $X_i$: $i$ 번째 변수를 의미합니다.
  - $\beta_i$: $i$ 번째 변수와 결과간의 관계를 나타냅니다.
---
- `Simple Linear Regression` 에서 한 것처럼, 추정치를 구해야 합니다.
- 즉, 위 식에서 $\beta_j$ 값들을 계산해야 하며, `예측 모델`은 아래와 같이 표현됩니다.
$$\hat{y} = \hat{\beta_0} + \hat{\beta_1}x_1 + \cdots + \hat{\beta_p}x_p$$
- 그리고 여기서도 `RSS` 값이 최소가 되는 $\hat{\beta_0}, \hat{\beta_1}, \dots, \hat{\beta_p}$ 를 구하는 것이 목표입니다.

---

# Correlation Analysis
- `상관분석` 이란, `상관계수` 를 이용해 <mark>두 변수 간에 어떤 선형적 관계를 가지는지</mark> 분석하는 기법입니다.
- `상관계수` $r$ 은 아래와 같이 계산할 수 있습니다.
$$r = \frac{\sum_{i=1}^{n} \[(x^{(i)} - \mu_x)(y^{(i)} - \mu_y)\]}{ \sqrt{\sum_{i=1}^{n} (x^{(i)} - \mu_x)} \sqrt{\sum_{i=1}^{n} (y^{(i)} - \mu_y)}}=\frac{\sigma_{xy}}{\sigma_x \sigma_y}$$
- 이 때, 각각의 기호는 다음을 의미합니다.
  - $\mu_j$: $j$ 의 평균
  - $\sigma_k$: $k$ 의 표준 편차
  - $\sigma_{mn}$: $m$, $n$ 의 공분산
- `상관계수` $r$ 은 $-1 \le r \le 1$ 의 범위를 갖고, 부호에 따라 다음과 같이 해석됩니다.
  - $r < 0$: 음의 선형관계
  - $r = 0$: 선형관계 없음
  - $r > 0$: 양의 선형관계
- `상관분석` 을 통해, 변수 간의 관계를 고려하여 분석하는 것이 가능합니다.

---
