+++
title = "Sum-of-Subsets Problem"
description = "Backtracking"
date = 2023-05-08

[taxonomies]
categories = ["Algorithm", "Backtracking"]
tags = ["Algorithm", "Backtracking"]

[extra]
math=true
+++

`Sum-of-subsets Problem`은 $n$ 개의 아이템 중 $k$개를 적절히 선택하여 목표값 $W$를 달성하는 문제입니다. `Knapsack Problem`을 예로 들면, 골라 담은 아이템의 총 무게가 **정확히** $W$ 가 되도록 하는 문제입니다.

이해를 돕기 위해, 아래와 같은 상황을 가정해보겠습니다.

> - 아이템 총 갯수: $n=3$
>
> - 목표 무게: $W=6$
>
> - 아이템 무게: $w_1=2, \quad w_2=4, \quad, w_3=5$

아이템을 선택하는 것에 대해, ***binary tree*** 를 그려 표현할 수 있습니다. 왼쪽 자식은 <txtred>선택했을 때</txtred>이고, 오른쪽 자식은 <txtred>선택하지 않았을 때</txtred>라고 하겠습니다.

<img src="../../../images/post/cb20337/sum_of_subsets_1.png" width="600rem" alt="adsf" />

노드 안에 선택된 아이템들의 <u>무게 총합</u>을 누적해가며 표시하면 아래와 같습니다.

<img src="../../../images/post/cb20337/sum_of_subsets_2.png" width="600rem" alt="adsf" />

전체 `Space State Tree` 에서 $W$ 와 <u>무게 총합</u> 이 같은 노드를 표시하면 아래와 같습니다.

<img src="../../../images/post/cb20337/sum_of_subsets_3.png" width="600rem" alt="adsf" />

---

`Backtracking` 을 이용하여 풀 수 있는 문제이며, `Promising` 조건을 잘 찾아내는 것이 중요합니다. 그런데 사실 조금만 생각해보면 `Non-promising` 의 조건을 찾는 것은 그리 어렵지 않습니다. 이 문제는 <u>***무게의 총합***</u> 값이 중요한데, 만약 총합이 ***절대로 $W$ 값이 되지 못할 상황***이라면 무조건 *Non-promising* 이 되는 것을 쉽게 이해할 수 있습니다. 이러한 상황은 크게 아래의 두 경우를 들 수 있습니다.
- 이때까지 선택한 아이템의 무게 총합이 $W$ 를 <txtred>넘었을</txtred> 경우
- 남은 아이템을 모두 선택해도 무게 총합이 $W$ 보다 <txtred>작을</txtred> 경우

위에서 그렸던 트리를 활용해서, `Non-promising` 인 노드를 표시하면 아래와 같습니다.

<img src="../../../images/post/cb20337/sum_of_subsets_4.png" width="600rem" alt="adsf" />

또한, $W=\sum{w_i}$ 인 경우에도 *더 이상 무게를 더할 필요가 없으므로* 바로 `backtrack` 할 수 있도록 해주어야 합니다.
