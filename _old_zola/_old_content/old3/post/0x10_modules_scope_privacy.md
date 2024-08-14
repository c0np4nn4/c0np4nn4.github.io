+++
title = "0x10. Modules, Scope and Privacy"
description = "Rust"
date = 2023-07-07
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
# 📌 How module works
- <txtred>Compiler</txtred>가 어떻게 <txtylw>*Module*</txtylw> 을 포함한 `binary crate`를 컴파일하는지 살펴봅니다.
> 1. `Crate Root`
>     - 우선 <txtred>Compiler</txtred>는 `Crate Root` (`src/lib.rs` 또는 `src/main.rs`)를 찾고 컴파일을 시작합니다.

> 2. `Module` 선언
> ```rust
> // src/graden.rs
> mod garden;
> ```
> - `crate root` 파일에 <txtylw>*module*</txtylw> 을 위와 같이 선언해 둘 수 있습니다.
> - <txtred>Compiler</txtred>는 아래 세 경로를 통해 해당 <txtylw>*module*</txtylw> 의 코드를 찾습니다.
> ```
> `src/garden.rs` 파일
> `src/garden/mod.rs` 파일
> `inline`, 선언 뒤에 바로 정의
> ```

> 3. `Submodule` 선언
> ```rust
> // src/graden/vegetables.rs
> mod vegetables;
> ```
> - `crate root` 파일을 제외한 다른 파일 내에서 `Submodule`을 선언할 수도 있습니다.
>     - <txtred>Compiler</txtred>는 <txtylw>*parent module*</txtylw> 의 이름을 가진 디렉토리 내에서 `submodule`의 코드를 찾습니다.
> ```
> `src/garden/vegetables.rs` 파일
> `src/garden/vegetables/mod.rs` 파일
> `inline`, 선언 뒤에 바로 정의
> ```

> 4. `Module`로 향하는 `Path`
> - `crate` 내에 어떤 <txtylw>*module*</txtylw> 이 추가되면, <txtylw>같은 Crate 내</txtylw>의 다른 코드에서 <txtylw>privacy</txtylw>문제만 없다면 모두 접근할 수 있습니다.
> - 예를 들어, `src/garden/vegetables.rs` 내의 `Asparagus` 자료형은 아래와 같이 참조될 수 있습니다.
> ```rust
> crate::garden::vegetables::Asparagus
> ```

> 5. `Privacy`
> - <txtylw>*module*</txtylw> 내의 코드는 <txtred>*Prviate*</txtred> 가 default 입니다.
> - 따라서, `parent` module 에서 이를 참조하기 위해서는 `pub` 키워드로 <txtred>*Public*</txtred> 임을 명시해야 합니다.

> 6. `use` 키워드
> - `use` 키워드로 한번 참조에 대한 선언을 적어두면, 훨씬 *간략하게* 코딩하는 것이 가능합니다.

---
## 📍 (e.g.) backyard (binary) crate 
- 위에서 언급한 `garden`, `vegetable` 예시를 토대로 아래와 같이 `backyard` crate 를 구성할 수 있습니다.

```
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

- <txtylw>*binary crate*</txtylw> 의 `Crate Root` 파일은 `src/main.rs` 이며, 아래와 같이 코딩되어있다고 가정합니다.

```rust
// file: src/main.rs
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {:?}!", plant);
}
```

- `pub mod garden;` 은 <txtred>Compiler</txtred>에게 `src/garden.rs` 에서 코드를 찾은 뒤 `src/main.rs` 에 추가할 것을 명시한 부분입니다.
- `src/garden.rs`의 코드는 아래와 같습니다.

```rust
// file: src/garden.rs
pub mod vegetables;
```

- 마지막으로, `src/garden/vegetables.rs`의 코드는 아래와 같습니다.

```rust
// file: src/garden/vegetables.rs
#[derive(Debug)]
pub struct Asparagus {}
```

---
# 📌 관련있는 코드를 Module 로 묶기
- `Module`은 코드를 <txtylw>*읽기 쉽고, 재사용하기 편리하게*</txtylw> 만들어 줍니다.
- 또한, <txtred>*Private*</txtred> 이 default 이기 때문에, 코드에 대한 ***privacy*** 를 제어할 수도 있습니다.
- 예를 들어, 레스토랑에서 일어나는 일들 중 손님과 관련한 부분을 `front_of_house` 라 부르기로 하고, 아래와 같이 `library crate`를 만들어 볼 수 있습니다.
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}
        fn serve_order() {}
        fn take_payment() {}
    }
}
```

- 앞서 `src/lib.rs`와 `src/main.rs`를 `Crate Root`라는 이름으로 불렀습니다.
- 이는, 두 파일이 `Crate` 라는 <txtred>*최상위 모듈*</txtred> 이 되기 때문입니다.
- 위 레스토랑 예시에 관해 `module tree` 를 그려보면 아래와 같습니다.

```
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```
