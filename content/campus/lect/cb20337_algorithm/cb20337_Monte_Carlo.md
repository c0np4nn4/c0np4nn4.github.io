+++
title = "Monte Carlo Method"
description = "Backtracking Algorithm"
date = 2023-04-25
toc = true

[taxonomies]
categories = ["Algorithm", "Lect"]
tags = ["Algorithm", "Lect", "backtracking"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB20337 (Professor Lee)*

# Estimating the Efficiency of Backtracking algorithm w/ Monte Carlo Algorithm
- [N-Queen Problem](/campus/lect/cb20337_algorithm/cb20337-n-queen/) 에서 살펴봤던 것처럼, 어떤 문제에 대해 `State Space Tree` 를 그려서 접근하면 Nodes 의 수가 굉장히 많아짐을 알 수 있습니다.
- $n$ 이라는 동일한 값과 <txtylw>*두 instance*</txtylw> 가 주어졌을 때, 어떤 경우에는 <u>***굉장히 적은 수의 node 만을 체크***</u>하고, 또 어떤 경우에는 <u>***전체 트리를 다 체크***</u>해야 할 수도 있습니다.
- 만약 주어진 <txtred>***Backtracking Algorithm***</txtred> 이 주어진 <txtylw>*instance*</txtylw> 에 대하여 얼마나 효율적인지 추정치를 구할 수 있다면 알고리즘의 적용을 고려하는데 도움이 될 수 있습니다.
- 이러한 추정치를 <txtred>***Monte Carlo Algorithm***</txtred> 으로 구할 수 있습니다.
---
# Monte Carlo Algorithm
- `Monte Carlo Algorithm`은 `Probabilistic Algorithm` 입니다.

> - `Probabilistic Algorithm`
>   - 다음에 실행될 명령이 `확률분포`에 근거해 random 으로 결정되는 알고리즘입니다.
> - `Deterministic Algorithm`
>   - 위와 같은 일이 발생하지 않는 알고리즘 입니다.

- `Monte Carlo` 는 *Sample Space* 에 정의된 값으로 기대치를 추정합니다.
- 이 ***추정치*** 가 실제 값과 같다는 <u>보장</u>은 없지만, 알고리즘에 들어간 시간이 길어질수록 확률이 증가한다고 합니다.
> - 이는 <txtylw>*확률적*</txtylw>으로 구한 값들을 모아서 평균을 구한다든지 하는 방식으로 해소할 수 있습니다.

---

# Probabilistic Algorithm
- `Monte Carlo` 는 <txtred>*확률적 알고리즘*</txtred> 입니다.
  - <txtred>*확률적 알고리즘*</txtred> 이란, 다음의 *instruction* 이 확률적으로 수행되는 알고리즘을 의미합니다.
  - 반대되는 개념으로 <txtred>*deterministic algorithm*</txtred> 이 있습니다.
- `Monte Carlo` 알고리즘은 변수들에 대한 값을 추정합니다.
- <txtylw>추정치</txtylw> 가 실제 기대값에 가깝다는 보장은 없지만, 알고리즘에 사용할 수 있는 시간이 증가함에 따라 두 값 사이의 오차가 적을 확률이 증가합니다. (???)
- <txtylw>일반적인 경로</txtylw> 를 생성한 뒤, 해당 경로의 <txtylw>노드 수</txtylw>를 체크합니다.

---

# Monte Carlo Algrotihm
- `Monte Carlo` 아래 두 조건을 만족시키는 분석 알고리즘을 필요로 합니다.
  - <txtylw> Same Promising Function </txtylw> on <txtylw>Same level</txtylw>
  - <txtylw> Same # of children </txtylw> on <txtylw>Same level</txtylw>
- `Monte Carlo` 알고리즘의 과정을 나타내면 아래와 같습니다.
> - Level $0$ 에서 임의의 노드 $m_0$ 을 선택합니다.
> - 다음 Level $1$ 에서 임의의 `Promising Node` $m_1$ 를 선택합니다.
> - 이를 반복적으로 행하여 level $k$ 에서 `Promising Node` $m_k$ 를 선택합니다.
- 같은 *Level* 에서의 노드들은 같은 수의 *children* 을 갖고 있으므로, $m_i$ 는 Level $i$ 에서의 <txtylw>***Average # of Promising Node***</txtylw> 임을 알 수 있습니다.
- 또, *Level* $i$ 에서의 한 노드에 대한 children 의 개수를 $t_i$ 라고 하면 아래와 같은 수식을 완성할 수 있습니다.
$$1 + t_0 + m_0 t_1 + m_1 t_2 + m_2 t_3 + \cdots + m_{i-1} t_i + \cdots$$
- 이를 말로 풀어보면, <mark>*Promising Node 갯수* $\times$ *children node 갯수* 들의 총합</mark> 입니다.

# 참고
- [Monte Carlo](https://www.kancloud.cn/leavor/cplusplus/630541)
- [강원대 수업자료](https://cs.kangwon.ac.kr/~ysmoon/courses/2010_1/alg/08.pdf)
- [Monte Carlo, blog](https://nolzaheo.tistory.com/20)
