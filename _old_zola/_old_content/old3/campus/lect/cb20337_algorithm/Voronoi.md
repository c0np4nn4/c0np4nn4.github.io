+++
title = "계산기하학_18MAY2023"
description = "Geometry Algorithm"
date = 2023-05-18
toc = true

[taxonomies]
categories = ["Algorithm", "Lect"]
tags = ["Algorithm", "Lect", "Geometry Algorithm"]

[extra]
math=true
+++

---

# Voronoi
`Voronoi Diagram`에 point 를 추가하는 방법에 관해 논함

- `Randomized Incremental Voronoi Algorithm`
  - 무작위로 <txtylw>임의의 point</txtylw>가 추가되면, `Voronoi algorithm` 을 이용해서 <txtylw>point</txtylw> 를 포함하는 영역을 계산함
- `Sweep-line Voronoi Algorithm`
  - <txtylw>point</txtylw> 를 <txtred>먼저</txtred> `x` 축을 기준으로 정렬을 함
  - 그리고, `Voronoi Algorithm` 을 이용해서 영역을 계산함

---

# Delaunay Trinagulation
*[ref](https://en.wikipedia.org/wiki/Delaunay_triangulation)*
*[ref2](https://gwlucastrig.github.io/TinfourDocs/DelaunayIntro/index.html)
- `Voronoi` 와 연관된 개념인듯..
- mesh 와도 연관 있는듯..

---

# 영역 검색 문제
> *Range Search*

## 1D 영역 탐색
- `영역 탐색`: $k_1, k_2$ 사이의 모든 $key$ 찾기
- `영역 카운트`: $k_1, k_2$ 사이의 $key$ 개수
<br />
<br />
- 이를 `List` 상에서 생각해보면,
  - `unordered`: 삽입은 빠름, 탐색은 느림
  - `ordered`: 삽입은 느림, 탐색은 빠름
- `영역 카운트`
  - BST 로 구현할 수 있음

---

# 겹치는 문제 (Intersect)

## Sweep-Line Algorithm
- `y`축 을 기준(1D-array)으로 선분의 시작과 끝을 tracking 하면서, 교차하는 부분이 발생하는지 sweep-line 알고리즘으로 판별

---

# k-d Tree
- 앞서 살펴본 `1-D` 의 경우, BST 를 이용함
- 이를 `다차원`으로 확장한 개념

## orthogonal range search
***Idea***

> $M$ by $M$ grid 로 영역을 분할하여, search space 를 줄일 수 있음 

- QuadTree?
  - 평면을 사분면으로 나눔

## 2d Tree 구축
- 좌표 값을 `x`, `y` 를 번갈아가며 기준으로 삼고 BST 를 구축함
- 이 때, `자식 노드`가 <txtred>비어 있으면</txtred> 삽입함


---

