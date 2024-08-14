+++
title = "ALGO 30MAY2023"
description = "Algorithm"
date = 2023-05-30
toc = true

[taxonomies]
categories = ["Algorithm"]
tags = ["Quick Note"]

[extra]
math=true
+++

---

# Time Complexity & NP Problems

- `Polynomial-Time Algorithm`
- `다루기 힘든 정도(Intractability)`
- 문제의 종류
  - `P`, `NP`, `NP-Complete`, `NP-Hard`

---
- `P` 문제
  - <txtred>다항시간 내에 해결</txtred> 할 수 있는 문제
- `NP` 문제
  - <txtred>다항시간 내에 답의 존재여부</txtred> 를 알 수 있는 문제
- 결정형 문제: 'yes' or 'no'
- 결정형 알고리즘: `deterministic algorithm`, 다음 단계가 유일하게 결정되는 알고리즘
- 비결정형 알고리즘: `non-deterministic algorithm`, 다음 단계가 유일하게 결정되는 알고리즘
---
- `비결정형 알고리즘`
  - 추측 단게 (비결정)
    - 임의의 $s$ 를 *추측한 해답*으로 생성함
  - 검증 단계 (결정)
---
- `다차시간 비결정 알고리즘(polynomial-time nondeterministic algorithm)`
  - <u>검증 단계가 다차시간 알고리즘</u>인 비결정 알고리즘
- `NP`: `다차시간 비결정 알고리즘`으로 풀 수 있는, <txtylw>모든 진위 판별 문제의 집합</txtylw>
---
- `NP-Hard`: `NP`만큼 어려운 문제
- `NP-Complete`

---

# Interactable
- 문제의 분류
  - polynomial time algorithm 이 발견된 문제 <txtred>P 문제</txtred>
  - 다루기 힘듦이 증명된 문제 <txtred>P도 NP도 아님</txtred>
    - `Halting Problem`: ***결정 불가능*** ([ref](https://johngrib.github.io/wiki/halting-problem/))
  - 다루기 힘듦도, poly-time algrotihm 도 발견되지 않은 문제 <txtred>NP</txtred>

---

- `최적화` vs `결정`
  - `최적화`: <txtylw>가장 좋은 답</txtylw>을 찾는 것
  - `결정`: boundary 에 대해, 답이 존재하는지를 결정
>   - `TSP`
>   - `0/1 Knapsack`
>   - `Graph Coloring`
