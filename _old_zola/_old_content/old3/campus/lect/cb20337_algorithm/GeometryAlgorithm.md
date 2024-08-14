+++
title = "계산기하학_16MAY2023"
description = "Geometry Algorithm"
date = 2023-05-16
toc = true

[taxonomies]
categories = ["Algorithm", "Lect"]
tags = ["Algorithm", "Lect", "Geometry Algorithm"]

[extra]
math=true
+++

---
# Geometric Primitives
- Point, Line, width, height, etc.
## Primitive Operations
  - `Point` 가 `Polygon` 내부에 있는지
  - 두 `Line` 이 교차하는지
  - 여러 `Point` 의 방향성이 `CW` 인지, `CCW` 인지

<details>
문제를 풀 때, 너무 큰 그림부터 보려고 하지 말고, `Primitive` 부터 접근하는 방법에 관해 들음
</details>

---
# CCW 
> ([참고용 블로그](https://snowfleur.tistory.com/98))
- ***쉬워 보이지만, 구현하려고 하면 어려움***
- `외적` 을 이용해서 <txtylw>*CW*</txtylw>, <txtylw>*CCW*</txtylw> 를 구분할 수 있음
  - `외적`은 `면적`으로 이해할 수 있음
- 임의의 점 $P_1, P_2$ 에 대하여,
  - $P_1 \times P_2 > 0$ 이면, $P_1$ 이 $P_2$ 에 대해 <txtylw>*CW*</txtylw> 에 있음
  - $P_1 \times P_2 < 0$ 이면, $P_1$ 이 $P_2$ 에 대해 <txtylw>*CCW*</txtylw> 에 있음
  - <txtred>어느 점을 기준으로 갖는지 주의해야함</txtred>


$$
p_1 \times p_2 = 
  \begin{vmatrix}
  x_1 & y_1 \newline
  x_2 & y_2 \newline
  \end{vmatrix}
= x_1y_2 - x_2y_1
$$

$$
p_2 \times p_1 = 
  \begin{vmatrix}
  x_2 & y_2 \newline
  x_1 & y_1 \newline
  \end{vmatrix}
= x_2y_1 - x_1y_2 = -(x_1y_2 - x_2y_1)
$$

---

## Line Segment Properties
임의의 점 $p_1 = (x_1, y_1), p_2 = (x_2, y_2)$ 가 있을 때, 선분 $\overline{p_1p_2}$ 상의 점 $p_3 = (x_3, y_3)$ 을 $p_1, p_2$ 의 선형 결합으로 표현할 수 있음

$$ p_3 = a * p_1 + (1-a) * p_2$$

---

# Intersection of 2 Line Segments
위에서 살펴본 `Line Segment` 성질을 이용해서 임의의 선분 $\overline{P_1P_2}$ 와 $\overline{P_3P_4}$ 각각의 임의의 선분 상의 점 $A, B$ 가 일치하는 점이 있다면 `교차한다(Intersect)`라고 말할 수 있음

## Cases
- `교차`
- `교차 x`
- 한 선분의 끝점이 다른 선분 상에 존재
- 두 선분이 linear 하게 연결 (한 선분의 끝점이 다른 선분의 또 다른 끝점)
- 두 선분이 linear 관계에 있지만, `교차 x`

## 판별
- 두 선분 중 한 선분의 한 점 $P$ 를 기준으로 하여, 나머지 선분의 두 끝점에 대해 `cross product` 시행한 후 `부호`에 따라 <txtylw>Left</txtylw>, <txtylw>Right</txtylw> turn 을 판별하고 이를 기준으로 `Intersection` 여부를 결정할 수 있음
- `Cases` 에 따른 연산 결과들을 볼 필요가 있음

<img src="../../images/post/cb20337/line_seg.png" alt="invalid src" width="400rem" />

- 위 (a), (b) 는 각 점의 위치에 따라 판별하면 됨

## Summary
*그림 추가할 예정임*

<txtred>각 case 에서의 수식에 대해 이해할 필요가 있음</txtred>

---

## Bounding Box
- 선분에 대한 `Box` 를 만들어서, 교차 여부를 한 단계 위에서 판단할 수 있는 방법

---

# Convex Hull
- 모든 점을 포함하는 가장 작은 `Convex Set`
- `Pacakage Wrapping`
  - 임의의 점에서 시작하여, 가장자리의 점들을 계속해서 찾아가면 됨
- `Graham Scan` ([ref](https://kbw1101.tistory.com/50))
  - 모든 정점에 대해 `닫힌 경로`를 만듦
  - `CCW` 가 아닌 점을 경로에서 제거함 (반복)
  - ***Linear Time*** ($O(n\log n)$)에 가능

---

# Voronoi Diagram
- Nearest Neightbor Problem 과 이어지는 개념
- `Voronoi Region`: 주어진 점에서 가장 가까운 점들의 집합 (경계선을 만듦)
