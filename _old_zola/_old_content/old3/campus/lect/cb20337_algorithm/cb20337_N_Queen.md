+++
title = "N-Queen Problem"
description = "N-Queen Problem"
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

# N-Queen Problem
- `Backtracking` 문제는 아래의 과정으로 해결할 수 있습니다.
  - `Promising` check 를 위한 *criterion* 을 설정
    - 같은 열에 있는가 (col[i] == col[k])
    - 같은 대각에 있는가 (abs(col[i] - col[k]) == i - k)
- 이를 코드로 표현하면 아래와 같습니다.
```c
// index i 는 `i-번째 행`을 의미합니다.
void queens(index i)
{
  index j;

  if (promising(i))
  {
    // promising node 이면서 leaf 이기 때문에, solution 을 의미합니다. 
    if (i == n) 
      cout << col[1] through col[n];
    else
      for (j = 1; j <= n; j++)
      {
        // 일단 다음의 `j-번째 열` 에 퀸을 두어 봅니다.
        // queens(i + 1) 내부에서 pruning 여부를 결정하면 됩니다.
        col[i + 1] = j;
        queens(i + 1);
      }
  }
}

// `i-번째 행`의 `j-번째 열`에 퀸을 두었을 때의 체크입니다.
bool promising (index i)
{
  index k;
  bool switch;

  k = 1; // 임시 `행` 정보입니다.
  switch = true;

  while (k < i && switch)
  {
    // 같은 행에 기물이 놓였거나, 대각에 위치한다면 swtich 를 false 로 바꿉니다.
    // 만약 false 가 됐다면, 그 다음 queens(i + 1); 호출로 (다른 자식 노드) 시도해봅니다.
    if (col[i] == col[k] || abs(col[i] - col[k]) == i - k)
      switch = false;
    k++;
  }

  return switch;
}
```

# Algorithm Analysis
- `pruned state space tree` 의 ***Upper Bound*** 를 구하기 위해, 우선 `전체 state space tree` 의 노드를 세어봅니다.
- Level 0 에는 Root node 하나만 있습니다.
- Level 1 에는 가로 길이인 N 개 만큼의 경우의 수를 갖습니다.
- Level 2 에 또 n 개만큼의 경우의 수가 있기 때문에, 경우의 수는 $n^2$ 가 됩니다.

$$1 + n + n^2 + \cdots + n^n = \frac{n^{n+1} - 1}{n - 1}$$

- 만약 $n=8$ 이라면, $19,173,961$ 라는 값을 얻습니다.
- 이 값은 큰 의미가 없는데, 왜냐하면 *말도 안되는 path* 들도 모조리 포함한 값임이 자명하기 때문입니다.
---
- 만약, ***같은 행에 놓일 수 없다*** 는 정보를 포함해서 ***Upper Bound*** 를 구해보려 한다면 아래와 같은 식을 세울 수 있습니다.

$$1 + 8 + 8 \times 7 + \cdots + 8!$$

- 이제 값은 $109, 601$ 로 꽤 많이 줄었지만, 여전히 아래의 문제점이 있습니다.
  - ***같은열, 같은 대각선*** 에 관한 정보를 제외하지 않았기에, 아직도 ***큰 값*** 으로 볼 수 있음
  - 그러한 `Non-promising` 의 수가 꽤 많을 것이기 때문에, 값을 줄일 필요가 있음
---
- 프로그램을 직접 돌려서 `Promising` 의 수를 세어보는 방법도 있습니다.
- 하지만, 이런 접근은 실제 `분석`이 될 수는 없고, 효율적인지 성능을 평가하는 정도로만 사용할 수 있습니다.
---
- 마지막으로, `promising()` 함수를 최적화하는 방법에 관해서도 고민할 필요가 있습니다
- `promising()` 함수에서 걸리는 시간을 최적화하는 것이 더 적은 수의 노드를 체크하는 것보다 좀 더 주목해야할 점입니다.
