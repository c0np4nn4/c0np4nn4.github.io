+++
title = "Multi-Layer Perceptron"
description = "MLP"
date = 2023-06-12
toc = true

[taxonomies]
categories = ["Machine Learning"]
tags = ["Deep Learning", "Machine Learning"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB35268 (Professor Park)*

# Perceptron
- 인간의 `뉴런`에 기반해서 제시된 개념
> - ***`Binary`*** 출력
> - <txtylw>*다수의 입력*</txtylw> 을 받아서 <txtylw>*하나의 결과*</txtylw> 를 출력함
> - 합쳐진 신호가 일정 <txtred>***역치***</txtred> 를 넘으면 출력 신호로 전달됨
- `Binary` 출력은 다음의 종류를 가짐
  - Positive: +1
  - Negative: -1
- 출력의 종류를 결정하는 함수를 <u>***결정 함수***</u> 라 부름
  - <txtylw>결정 함수</txtylw>는 $\phi (z)$ 로 표기함
  - $z$ 는 입력값으로, <txtylw>입력 벡터</txtylw> $\mathbb{x}$ 와 <txtylw>가중치</txtylw> $\mathbb{w}$ 의 선형조합임
  - $\phi (z) = \phi (\mathbb{w}^T \mathbb{x})$
- $z$ 값이 <txtylw>역치</txtylw>에 해당하는 값 $\tau$보다 크고 작은지에 따라 종류(+1/-1)가 결정됨

---

## 학습 규칙
- 학습의 <txtred>**목표**</txtred>는 $\mathbb{w}$ 가 적절한 수렴값을 갖도록 하는 것입니다.
- 이를 위해 아래의 작업을 진행합니다.
> - 1. $\mathbb{w}$를 $0$ 또는 작은 값으로 초기화
> - 2. 각 샘플 $\mathbb{x}$에 대한 <txtylw>출력 값</txtylw> $\hat{y}$ 를 계산
> - 3. <u>실제 $y$ 값</u>과 <u>계산된 값$\hat{y}$</u> 의 차이를 기반으로 $\mathbb{w}$를 업데이트
> - 4. $\mathbb{w}$ 가 적절히 수렴할 때까지 ***2~3 을 반복***
- 가중치 $\mathbb{w}_k$에 대한 업데이트는 다음과 같이 적을 수 있습니다.

$$\Delta \mathbb{w}_k = \alpha (y - \hat{y}) \mathbb{x}_k$$

- 예를 들어, $\hat{y} = -1$, $y = 1$, $\alpha = 1$, $\mathbb{x}_k = 0.5$ 라 해봅시다.
$$\Delta \mathbb{w}_k = 1 \times (1 - (-1)) \times 0.5 = 1$$
- 따라서, $\mathbb{w}_k$ 의 값이 커지게 되므로, $z = \mathbb{w}_k \mathbb{x}_k$ 의 값도 커지게 됩니다.
- 기존의 $\hat{y} = -1$ 는 $z$ 값이 $\tau$ 보다 작기 때문에 얻은 값이었습니다.
- 따라서, $z$ 값이 커지면 $\hat{y} = +1$ 이 될 가능성이 높아지고 <txtylw>*가중치가 특정 값으로 수렴*</txtylw> 하게 됩니다.

---

## 전체 흐름

<img src="../../../images/post/cb35268/perceptron_01.png" width="500rem" alt="not yet" style="border: 2px solid #b3deef"/>

- 앞서 설명한 내용을 그림으로 표현하면 위와 같습니다.
- 이러한 <txtylw>***Perceptron***</txtylw> 은 <u>*선형적으로 구분 가능한 데이터*</u> 에 대해 사용할 수 있습니다.

---

## Adaptive Linear Neuron

<img src="../../../images/post/cb35268/perceptron_02.png" width="500rem" alt="not yet" style="border: 2px solid #b3deef"/>

- <txtylw>***Perceptron***</txtylw> 에 대한 후속 연구들 중, ***Step Function*** 이 아닌 ***Linear Activation Function*** 을 이용해서 <txtylw>가중치 업데이트</txtylw> 를 하는 방식이 등장했습니다.
- 즉, <txtylw>***Linearlity***</txtylw> 가 추가되어 `미분`이 가능하게 되는 등의 이점이 발생합니다.

---

# Multi-Layer Perceptron (MLP)
- 다수의 **뉴런**을 하나의 **Layer** 를 이룬다고 해봅시다.
- 이러한 **Layer** 를 여러 개 쌓은, <txtylw>다층 신경망</txtylw>을 구축할 수 있습니다.

<img src="../../../images/post/cb35268/mlp_01.png" width="500rem" alt="not yet" style="border: 2px solid #b3deef"/>

- 위 그림에서와 같이 `input`, `output` 레이어가 아닌 그 사이의 `hidden` 레이어가 하나 이상 있을 경우 <txtylw>심층 인공 신경망(Deep Artificial Neural Network)</txtylw>라 부르기도 합니다.
- 각 노드로 이어지는 <txtred>edge</txtred> 가 굉장히 많아져서 복잡해보이지만, `Perceptron` 에서 살펴본 구조를 떠올리면 이해가 어렵지 않습니다.
  - multi-input, single-output

---

## Forward Pass
- 위 그림에서 $\hat{y}$ 값을 구하는 과정입니다.
- 첫 번째 레이어에서 두 번째 레이어로의 과정은 `Perceptron` 에서의 연산을 여러번 한 것과 같습니다.
  - 즉, 행렬로 이를 계산면 용이함을 쉽게 알 수 있습니다.
  - 그리고, 이러한 연산을 모든 레이어에 대해 반복합니다.
- 결국 최종 목표는 $\hat{y}$ 값인 <txtred>정답 추정값</txred>를 계산하는 것입니다.

---

## Backward Pass
- 정답과 추정값이 모두 있으므로, 다음으로 <txtylw>가중치 업데이트</txtylw>를 위한 방법을 살펴봅니다.
- Backward 에서는 `Backpropagation` 이라는 잘 알려진 방식을 사용합니다.
  - `Backpropagation`의 핵심은 <txtylw>가중치</txtylw>와 <txtylw>Bias</txtylw>의 <txtred>변화</txtred>가 어떻게 `Cost Function` 에 변화를 주는지 입니다.

---

...
