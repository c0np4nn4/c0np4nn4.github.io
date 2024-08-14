+++
title = "0x19. Generic"
description = "Rust"
date = 2023-08-05
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---
### <txtred>**TL;DR**</txtred>

---
# 📌 Generic
- `Generic Type`을 이용해 코딩하는 방법에 대해 알아봅니다.

---
## 📍 Function Definition
```rust
fn print_out<T>(data: &T) {
    println!("[print_out]: {:?}", T);
}
```
---
## 📍 Struct Definition
```rust
struct Point<T, U> {
    x: T,
    y: U,
}
```

---
## 📍 Enum Definition
```rs
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

---
## 📍 Method Definition
```rs
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
```
## 📍 Performance
- `Generic` 을 이용하더라도, 퍼포먼스에는 큰 영향이 없습니다.
- 왜냐하면, <txtred>Compiler</txtred>가 내부적으로 <txtylw>*Monomorphization*</txtylw>이라는 개념을 바탕으로, 각 타입에 대한 새로운 함수들을 생성해두기 때문입니다.
- 예를 들어, `i32`, `u64`에 대한 각각의 함수를 다시 생성합니다.

