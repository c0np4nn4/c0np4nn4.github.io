+++
title = "Tromino Tiling"
description = "divide_and_conquer"
date = 2023-04-10
toc = true

[taxonomies]
categories = ["Algorithm", "Divide and Conquer"]
tags = ["Alogirhtm", "Divide and Conquer"]

[extra]
math=true
+++

# Code
```cpp
#include <cmath>
#include <iostream>

using namespace std;

int m;
int board[1024][1024];

enum Type { RD, RU, LU, LD };
void put_tromino(int x, int y, Type t, int depth);
void print_board();

void put_tromino(int x, int y, Type t, int depth) {
  int a = depth > 1 ? pow(2, depth - 1) : 1;
  int b = pow(2, depth);

  if (depth == 0) {
    board[x][y] = 1;
    return;
  }

  if (t == RD) {
    put_tromino(x + 0, y + 0, RD, depth - 1);
    put_tromino(x + a, y + 0, LD, depth - 1);
    put_tromino(x + 0, y + a, RU, depth - 1);
    put_tromino(x + a, y + a, RD, depth - 1);

    board[x + a - 1][y + a - 1] = 1;
    board[x + a + 0][y + a - 1] = 1;
    board[x + a - 1][y + a + 0] = 1;

    board[x + b - 1][y + b - 1] = 0;
  }

  if (t == RU) {
    put_tromino(x + 0, y + 0, RD, depth - 1);
    put_tromino(x + a, y + 0, RU, depth - 1);
    put_tromino(x + 0, y + a, RU, depth - 1);
    put_tromino(x + a, y + a, LU, depth - 1);

    board[x + a - 1][y + a - 1] = 1;
    board[x + a - 1][y + a + 0] = 1;
    board[x + a + 0][y + a + 0] = 1;

    board[x + b - 1][y] = 0;
  }

  if (t == LD) {
    put_tromino(x + 0, y + 0, RD, depth - 1);
    put_tromino(x + a, y + 0, LD, depth - 1);
    put_tromino(x + 0, y + a, LD, depth - 1);
    put_tromino(x + a, y + a, LU, depth - 1);

    board[x + a - 1][y + a - 1] = 1;
    board[x + a + 0][y + a - 1] = 1;
    board[x + a + 0][y + a + 0] = 1;

    board[x][y + b - 1] = 0;
  }

  if (t == LU) {
    put_tromino(x + 0, y + 0, LU, depth - 1);
    put_tromino(x + a, y + 0, LD, depth - 1);
    put_tromino(x + 0, y + a, RU, depth - 1);
    put_tromino(x + a, y + a, LU, depth - 1);

    board[x + a + 0][y + a - 1] = 1;
    board[x + a + 0][y + a + 0] = 1;
    board[x + a - 1][y + a + 0] = 1;

    board[x][y] = 0;
  }

  print_board();
}

void print_board() {
  cout << "\n===========================" << endl;
  for (int i = 0; i < pow(2, m); i++) {
    for (int j = 0; j < pow(2, m); j++) {
      cout << board[i][j] << ' ';
    }
    cout << endl;
  }
  cout << "===========================" << endl;
}

int main() {
  cin >> m;

  put_tromino(0, 0, Type::RD, m);

  print_board();

  return 0;
}
```

# Result
```
$ ./3_tromino
2

===========================
1 1 0 0
1 0 0 0
0 0 0 0
0 0 0 0
===========================

===========================
1 1 0 0
1 0 0 0
1 0 0 0
1 1 0 0
===========================

===========================
1 1 1 1
1 0 0 1
1 0 0 0
1 1 0 0
===========================

===========================
1 1 1 1
1 0 0 1
1 0 1 1
1 1 1 0
===========================

===========================
1 1 1 1
1 1 1 1
1 1 1 1
1 1 1 0
===========================

===========================
1 1 1 1
1 1 1 1
1 1 1 1
1 1 1 0
===========================

```
