+++
title = "0x0E. If Let 구문"
description = "Rust"
date = 2023-07-06
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
# 📌 if let
- 아래 `match` 구문에서 `_ => ()` 부분은 <txtylw>*사실 없어도 상관없는 코드*</txtylw> 입니다.
```rust
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (),
    }
```
- `Rust`에서는 이를 `if let` 구문으로 간단히 적을 수 있습니다.
```rust
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }
```
- 즉, `if let Some(max)`로, `max`변수에 값을 저장하고, 블록 내에서 `println` 의 인자로 출력할 수 있습니다.
- 이렇게 코딩하면, 아래와 같은 장단점을 가질 수 있습니다.
> - <txtred>***장점***</txtred>: <txtylw>*훨씬 간결하게*</txtylw> 코딩할 수 있다.
> - <txtred>***단점***</txtred>: <txtylw>*모든 경우를 <u>명시적으로</u> 체크*</txtylw> 할 수 없다.
- 따라서, 프로그래머는 이 둘의 `trade-off`를 잘 생각해서 코딩해야 합니다.

---
## 📍 'else' as '_'
- `if let` 블럭 뒤에 `else` 블럭을 붙일 수 있습니다.
- 따라서, 아래 두 코드는 동일하게 동작합니다.

```rust
    let mut count = 0;
    match coin {
        Coin::Quarter(state) => println!("State quarter from {:?}!", state),
        _ => count += 1,
    }
```

```rust
    let mut count = 0;
    if let Coin::Quarter(state) = coin {
        println!("State quarter from {:?}!", state);
    } else {
        count += 1;
    }
```
