+++
title = "0x0D. Match Control Flow"
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
# 📌 Match Control Flow
- `Match` 는 <txtylw>제어문</txtylw>에 관한 키워드로, `Pattern` 에 기반해서 코드를 실행할 수 있도록 합니다.
- <txtylw>**동전**</txtylw>을 예로 들어, 아래와 같이 코딩할 수 있습니다.

```rust
enum Coin {
    _500won,
    _100won,
    _50won,
    _10won,
}

fn main() {
    let coin = Coin::_500won;

    let coin_value = match coin {
        Coin::_500won => 500,
        Coin::_100won => 100,
        Coin::_50won => 50,
        Coin::_10won => 10,
    };

    println!("coin value: {}", coin_value);
}
```

- `match` 는 여러 개의 `arm` 으로 이루어져 있습니다.
- `arm`은 다시 `=>`연산자를 중심으로 아래와 같이 나뉘어집니다.
    - <txtylw>*왼편*</txtylw>: `Pattern`
    - <txtylw>*오른편*</txtylw>: 실행 코드 (*Expression*)
- 즉, 아래와 같이 코딩할 수도 있습니다.

```rust
// snip
    let coin_value = match coin {
        Coin::_500won => {
            println!("This is 500won!");
            500
        },
        Coin::_100won => 100,
        Coin::_50won => 50,
        Coin::_10won => 10,
    };
// snip
```

---
## 📍 Patterns bind to Value
- `Pattern`을 검사하며, 아래와 같이 데이터를 다룰 수도 있습니다.

```rust
enum Coin {
    _500won(u32),
    _100won(u32),
    _50won(u32),
    _10won(u32),
}

fn main() {
    let coin = Coin::_500won(2002);

    let coin_value = match coin {
        Coin::_500won(year) => {
            println!("Year: {}", year);
            500
        },
        Coin::_100won(year) => {
            println!("Year: {}", year);
            100
        },
        Coin::_50won(year) => {
            println!("Year: {}", year);
            50
        },
        Coin::_10won(year) => {
            println!("Year: {}", year);
            10
        },
    };

    println!("coin value: {}", coin_value);
}
```

---
## 📍 Matching with Option<T>
- 이제 `Option` 에 대해 `Match` 를 해보겠습니다.

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

fn main() {
    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
}
```
- 주의해야할 점은, <txtylw>*가능한 모든 Pattern에 대한 `arm`을 <txtred>명시</txtred>해 줘야 한다*</txtylw>는 점입니다.
- 즉, <txtylw>None</txtylw>에 대한 `arm` 을 생략하는 등의 코딩이 <txtred>불가능</txtred>합니다.

---
## 📍 Catch-All Pattern
- 그러나, 모든 *가능한 Pattern* 을 일일이 적어주는 것 대신 `Catch-All Pattern` 을 도입하여 간결하게 코딩하는 방법도 존재합니다.

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn reroll() {}

```

- `_` 패턴은 <txtylw>*그 밖의 모든 가능한 패턴*</txtylw> 을 의미합니다.
