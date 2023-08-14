+++
title = "Longest Common Subsequence"
description = "Dynamic Programming"
date = 2023-04-04
toc = true

[taxonomies]
categories = ["Algorithm", "Lect"]
tags = ["Dynamic Programming", "Algorithm"]

[extra]
math=true
+++

# LCS
## Concept
## Brute force Solution
## LCS Algorithm
- Solutions of `sub-problem` are parts of the `final solution`
###Find Length
$$c\[i, j\] = \begin{cases}c\[i-1, j-1\] + 1 \quad \text{if} x\[i\] = y\[j\] \newline \text{max}(c\[i, j-1\], c\[i-1, j\]) \quad \text{otherwise} \end{cases}$$
  ```rust
  fn LCS_Length(X, Y) {
    // get the len(X)
    m = len(X)

    // get the len(Y)
    n = len(Y)

    // init c[i, 0] = 0
    for i = 1 to m c[i, 0] = 0

    // init c[0, j] = 0
    for j = 1 to m c[0, j] = 0

    // find length in O(n^2)
    for i = 1 to m
      for j = 1 to n
        if (X[i] == Y[j])
          c[i, j] = c[i - 1, j - 1] + 1
        else
          c[i, j] = max(c[i - 1, j], c[i, j - 1])
  }
  ```
#### Running Time
- $O(m \times n)$

### Find Actual LCS
- c[i, j] = c[i - 1, j - 1] + 1 일 때의 문자열을 `기억` 해두는 방식으로 구현할 수 있음

---
# Sequence Alignment (rel. Bio)
- 두 종(species) 간의 관련성을 찾는 문제
## LCS to Alignment
- <mark>Scoring System</mark> 을 도입함
  - 유전학적 특성을 반영한 결과라고 함
## Pairwise-alignment
- `Needleman-Wunsch Algorithm`
  - LCS 와 동일하지만, <mark>Scoring System</mark> 으로 값을 구함
- score(x, y) = max(s[x, y-1] - penalty, s[x - 1, y] - penalty, s[x-1, y-1] + score(x,y))

### Global vs Local
- Global: 주어진 염기서열 전체에 대한 alignment 
- Local: 염기서열의 특정 부분에 대한 alignment

## Local alignment
- max() 함수에 $0$ 을 추가함.
- 음수 결과를 제함으로써, 염기서열이 같은 부분은 높은 점수를 가질 수 있도록 함 (mismatch 가 일어날 때마다 0으로 수렴함)
