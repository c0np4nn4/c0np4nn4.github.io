+++
title = "Perceptron"
description = "Deep Learning (1)"
date = 2023-05-17

[taxonomies]
categories = ["Lect", "Deep Learning", "Quick Note"]
tags = ["ML", "Deep Learning"]

[extra]
math=true
+++

# Perceptron
- 특정 강도 이상의 신호가 오면 1, 아니면 0 으로 인식하는 단위

---

Perceptron 에서는 <txtylw>1</txtylw>, <txtylw>-1</txtylw> 로 분류함

결정함수는 입력벡터 $\mathbb{x}$ 와 가중치 $\mathbb{w}$ 의 선형 조합으로 표현함

$$\phi(z) = \phi(\mathbb{w}^T\mathbb{x})$$

---

Perceptron: 맞추면 유지, 틀리면 가중치를 *update*

- 정답을 <txtred>맞췄다면</txtred> $\Delta w_j = 0$
> $$y^{(i)} = -1, \hat{y}^{(i)} = -1, \quad \Delta{w_j} = \eta (-1 - (-1)) x_j^{(i)} = 0$$
> $$y^{(i)} = 1, \hat{y}^{(i)} = 1, \quad \Delta{w_j} = \eta (1 - (1)) x_j^{(i)} = 0$$

- 정답을 <txtred>틀렸다면</txtred> $\Delta w_j = 0$
> $$y^{(i)} = -1, \hat{y}^{(i)} = 1, \quad \Delta{w_j} = \eta (-1 - (1)) x_j^{(i)} = \eta(-2)x_j^{(i)}$$
> $$y^{(i)} = 1, \hat{y}^{(i)} = -1, \quad \Delta{w_j} = \eta (1 - (-1)) x_j^{(i)} = \eta(2)x_j^{(i)}$$

---

$\therefore$ <txtylw>맞는 방향</txtylw>을 향해서 $\phi(z)$ 값이 갱신됨

> 실제로 숫자를 넣어서 $\Delta w$ 가 어떻게 전체 $\phi(\mathbb{w}^T\mathbb{x})$ 에 영향을 미치는지 한번 더 이해하면 좋을듯

---

Perceptron 이 충분히 학습되기 위해서는 $\eta$ 가 충분히 작아야 함
- $\eta$ 가 충분히 작다면, 충분한 시간 후 예측이 가능함
- 단, <u>***선형적으로 구분 가능한 데이터***</u> 에서만 사용 가능함
  - 만약 선형 구분이 불가능하다면, 무한루프...

---

<img src="../../images/post/cb35268/clustering_1.png" alt="Invalid src" width="400rem"/>

위 그림에서 `threshold function` 로의 `input` 값에 대한 검증을 우선하는 방법을 추가할 수 있음

Error 판별에는 `linear` 한 함수를 사용
- <txtred>미분 가능</txtred> 특성이 생김
  - `학습`에 편리한 여러 기법들을 사용할 수 있게 됨(i.e. <txtylw>*back-propagation*</txtylw>)

---

결정경계를 좀 여유있게 가져가면 좋은 점?
- 새로운 데이터에 대한 예측을 잘 할 것임(일반화)

---
## MLP
- Perceptron 을 여러 층 쌓음
- 여러 층을 쌓으니 학습이 어려워짐...
  - `1986`년, ***Backpropagation*** 이 발표됨
- $N$ 개의 Layer 에 대하여, 첫번째 Layer 를 <txtylw>Input Layer</txtylw> 라 함
- 이후 $N-2$ 개의 Layer 를 <txtylw>Hidden Layer</txtylw> 라 함
- 마지막 $1$ 개의 Layer 를 <txtylw>Output Layer</txtylw> 라 함
---
- 각 Layer 를 앞에서 본 `Perceptron` 과 비교해서 생각
  - 입력에서 들어온 각각의 값들이 다음 Layer 각각의 값을 만듦
- 각 층 사이의 값을 만들어낼 때, 이전 Layer 의 결과값들(output)이 아주 ***Dense*** 하게 연결되어 있다. (Fully-connected Layer)
---

a: 행 벡터
w: 열 벡터
---> 새로운 행벡터
를 반복

---

BackPropagation

Error 에 W 가 기여한 정도만 아는 것

$\Delta W$ 에 대한 $\Delta \text{Error}$ 를 생각
--> 미분

$\Delta w = \frac{\delta E}{\delta w}$
- `의미`: w 가 증가하면, Error 가 증가, 따라서 <txtred>w를 감소하는 방향</txtred>으로 바꿔줘야 함

$\therefore w' = w - \alpha$
