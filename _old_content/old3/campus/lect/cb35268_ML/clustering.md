+++
title = "Clustering"
description = "Machine Learning, Clustering"
date = 2023-04-24
toc = true

[taxonomies]
categories = ["Machine Learning", "Lect"]
tags = ["Machine Learning", "Lect"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB35268 (Professor Park)*

# Instance-Based Learning
- `Model` 은 크게 두 종류로 나눌 수 있습니다.
  - `Parametric Model`
    - 고정 길이의 parameter 를 이용해서 전체 데이터를 ***summarize*** 하는 방법입니다.
    - 모든 data가 model 로 부터 얻어진다는 ***가정*** 을 갖습니다.
    - e.g. <txtred>Linear Regression</txtred>
  - `Non-parametric Model`
    - 데이터의 분포가 너무 ***Non-Linear*** 해서, *bounded set of parameter* 로 표현할 수 없는 경우입니다.
    - *bounded set of parameter* 는 곧, ***특정 모델*** 을 의미합니다.
    - 따라서, data 분포가 모델을 결정하는데 영향을 많이 미치며 모집단의 크기가 클수록 모델을 잘 표현합니다.
    - e.g. <txtred>Instance-based (memory-based) learning</txtred>

---

## K Nearest Neighbors
- 어떤 데이터 분포가 있을 때, 데이터 $x_q$ 에 대하여, 가까운 $k$ 명의 이웃을 찾는 알고리즘입니다.

$$NN(k, x_q)$$

- 이를 이용하여 다음과 같이 활용할 수 있습니다.
  - `Classification`: 찾은 $k$ 명의 이웃들의 속성 중 다수를 대표하는 것으로 `Class` 를 나눌 수 있습니다.
  - `Regression`: $NN(k, x_q)$ 의 평균값을 구하거나, $NN(k, x_q)$ 를 데이터로 하여 *Linear Regression* 모델을 만들어 문제를 해결할 수 있습니다.
- 이 때, $k$ 값을 몇개로 해야 좋을지 결정해야 합니다.
- 이는 `cross-validation` 을 통해 얻은 모델을 기반으로 결정할 수 있습니다.

---

## Curse of Dimensionality
- ***차원의 저주*** 라 불리는 개념입니다.
- 앞서 살펴본 `kNN` 문제에서는 ***가장 가까운 이웃이 사실은 가깝게 느껴지지 않는*** 아이러니를 보여줍니다.
- 아래의 파라미터들이 있다고 해보겠습니다.
  - $k$: *이웃의 수*
  - $l$: *이웃과의 평균 측면 거리*
  - $l^n$: *이웃들이 이루는 hypercube 의 체적*
- 전체 데이터의 수가 $N$ 이라고 할 때, 아래와 같은 식을 세울 수 있습니다.

$$
\begin{align} 
\frac{\text{이웃들이 차지하는 부피}}{\text{전체 데이터가 차지하는 부피}} &= \frac{\text{이웃의 수}}{\text{전체 데이터 수}} \newline
\frac{l^n}{1^n} = l^n &= \frac{k}{N} \newline
\therefore l = &(\frac{k}{N})^{\frac{1}{n}}
\end{align}
$$

- 만약 $k=10$ 이고 $N=1,000,000$ 이라면
  - $n=3$ 일 때, $l=0.02$ 로, 상당히 reasonable 한 수치를 보입니다.
  - 그러나, $n=17$ 이 되면 $l=0.5$ 로, <txtred>훨씬 넓은 영역에 이웃들이 분포</txtred>하게 되는 것을 확인할 수 있습니다.
- 따라서, 향후 `차원 축소`에 관해서도 논해야 합니다.

---

## Distance Functions
- `가까움`의 기준을 위해, ***거리***를 구하는 공식을 소개합니다.
- 아래의 ***Minkowski distance*** 또는 $L^p$ norm 이라 불리는 공식을 사용합니다.

$$L^p(x_j, x_q) = (\sum_{i} {|x_{j, i}-x_{q, i}|}^p)^{\frac{1}{p}}$$

- $p=2$ 이면 ***Euclidean Distance*** 공식입니다.
- $p=1$ 이면 ***Manhattan Distance*** 공식입니다.
- $p=1$ 이고, Boolean 속성을 가지면 ***Hamming distance*** 공식이기도 합니다.
<br />
<br />
- 각각의 $\mathbb{x}$ 값은 벡터를 의미합니다.
- 그런데, 각 벡터의 dimension 마다 값을 표현하는 '단위'나 값 자체의 '크기'가 다르기 때문에 전체 결과에 미치는 영향이 서로 다를 수 있습니다.
- 이러한 현상은 `Normalization` 을 통해 방지할 수 있습니다.
$$x_{j, i} \rightarrow \frac{(x_{j, i) - \mu_i}}{\sigma_i}$$
- 여기서 $\mu_i$ 는 $i$번째 dimension 의 평균, $\sigma_i$ 는 $i$번째 dimension 의 표준편차를 의미합니다.
- 또는, 아래와 같은 식을 이용할 수도 있습니다.
$$a_i = \frac{v_i - \min v_i}{\max v_i - \min v_i}$$
- 만약 dimension 의 값의 범위가 $\[a, b\]$ 라면, 위 식의 결과로 $\[0, 1\]$ 가 됩니다.

---

# Clustering
- <txtred>정해진 정답</txtred>은없으며, `Data` 를 통해 모델의 형태를 알게 됩니다.
- 대표적인 예로 Mnist Digits 이 있습니다.
<img src="https://theanets.readthedocs.io/en/stable/_images/mnist-digits-small.png" alt="adsf" width="400rem"/>
- 위의 손글씨 그림을 토대로 clustering 한 결과가 아래와 같습니다.
<img src="http://nlml.github.io/images/tsne/tsne-mnist.png" alt="asdf" width="400rem" style="background: white"/>

---

- `Clustering`은 ***Unsupervised Learning*** 입니다.
  - 즉, `label` 이 정해진 것이 없습니다.
  - 데이터를 "*natural group*" 으로 나눕니다.
- `Clustering` 의 결과를 표현하는 방식은 아래와 같이 다양합니다.
  - Disjoint vs. Overlapping
  - Deterministic vs. Probabilistic
  - Hierarchical vs. Flat

---

- `Clustering`을 하는 방법은 여러가지가 있습니다.
  - `K-means Algorithm`: ***Disjoint*** cluster 로 데이터를 구분합니다.
  - `Hierarchical clustering`: 계층적 군집을 형성합니다.
  - `Statistical custering`: 다양한 확률분포 모델을 기반으로 하여 데이터를 각 cluster 에 확률적으로 할당합니다.

---

## K-means Clustering
- 아래의 단계를 따릅니다.
> - 일단 $k$ 개의 *random* 한 값들을 <u>cluster 의 중심</u>으로 가정하고 선택합니다.
> - 데이터들을 선택된 <u>cluster 의 중심</u> 을 기준으로 분류합니다.
> - 각 *cluster* 의 중심값을 계산합니다.
> - 새로이 구한 중심값을 <u>cluster 의 중심</u> 값으로 두고, 위의 과정을 반복합니다.
- cluster 의 중심으로부터의 거리 차가 최소가 되도록 반복합니다.
- 맨 처음에 선택하는 *random point* 의 값에 최종 cluster 가 크게 영향을 받습니다.


---
# 참고
- Cross Validation, [Medium Article](https://towardsdatascience.com/cross-validation-in-machine-learning-72924a69872f)
- k-NN, [tistory 블로그](https://lcyking.tistory.com/102)
- Cross Validation, [블로그](https://losskatsu.github.io/machine-learning/cross-validation/#%EC%B0%B8%EA%B3%A0%EB%A7%81%ED%81%AC)
- Instance-based Learning and Clustering, [CMU pdf](http://www.cs.cmu.edu/afs/cs.cmu.edu/academic/class/15381-s06/www/clustering.pdf)
- Hierarchical Clustering, [블로그](https://ratsgo.github.io/machine%20learning/2017/04/18/HC/)
