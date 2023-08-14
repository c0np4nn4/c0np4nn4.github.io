+++
title = "0x05. 제어문"
description = "Rust"
date = 2023-06-24
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

# 🤔 제어문
- `Rust`에서 실행 흐름을 제어하는 방법으로는 `if` 와 `loop` 가 있습니다.

---

## 📌 if expression
- `if` 문 뒤의 <txtylw>*code block*</txtylw>을 `arm` 이라 부르기도 합니다.

```rust
fn main() {
    let n = 3;

    if n < 5 {
        println!("n < 5");
    } else {
        println!("n >= 5")
    }
}
```

- `if` 의 `condition`은 반드시 <txtred>bool Type</txtred>이어야 합니다.
- `else if` 도 사용 가능합니다.
- 또한, `if` 문이 `expression` 이기 때문에 아래와 같이 사용하는 것도 가능합니다.
```rust
fn main() {
    let cond = true;

    let val = if cond { 100 } else { 0 };

    println!("value: {val}");
}
```

- 단, 위와 같이 사용할 때는 `if` 문 전체의 `결과값`의 <txtred>Type</txtred>이 동일해야 합니다.

---

## 📌 loop
- `Rust` 에는 아래 세 종류의 반복문이 있습니다.
> - `loop`
> - `while`
> - `for`

---
### 📍 loop
- `loop` 내에서 `break`, `continue` 키워드를 사용할 수 있습니다.

- `break` 뒤에 `expression`을 두면 *return value* 로 활용할 수 있습니다.

```rust
fn main() {
    let mut cnt = 0;

    let res = loop {
        cnt += 2;

        if cnt == 10 {
            break cnt;
        }
    };
}
```

- 또, `loop`에 적절한 `label` 을 주어 구분할 수도 있습니다.
```rust
fn main() {
    let mut cnt = 0;
    let mut stop = false;

    'loop_a: loop {
        println!("loop_a");

        'loop_b: loop {
            println!("loop_b");

            if stop {
                break 'loop_a;
            }

            if cnt > 0 {
                break 'loop_b;
            }

            cnt += 1
        }

        stop = true;
    }
}
```

### 📍 while
---
- 반복문에서는 `조건`에 따라 `break`, `continue`를 주는 경우가 많습니다.
- 이를 `loop` 보다 `while` 로 구현하면 좀 더 용이합니다.

```rust
fn main() {
    let mut cnt = 1;

    while cnt < 10 {
        println!("count: {cnt}");

        cnt += 1;
    }
}
```

---
### 📍 for
- 또, ***Array***, ***Tuple*** 등과 같은 일종의 `collection`을 반복적으로 참조하거나 할 때는 `for`를 사용하면 편리합니다.

```rust
fn main() {
    let msg: String = "Hello, world!".to_string();

    // for 
    for (_, c) in msg.as_bytes().iter().enumerate() {
        if *c as char == ',' {
            break;
        }

        print!("{}", c.clone() as char);
    }

    println!("");
}
```
