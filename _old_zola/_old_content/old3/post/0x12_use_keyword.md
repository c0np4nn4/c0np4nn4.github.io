+++
title = "0x12. Use Keyword"
description = "Rust"
date = 2023-07-12
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
# 📌 use keyword
- `use` keyword 를 사용해서, 현재 `scope`로 `path`를 가져올 수 있습니다.

---
## 📍 Idiomiatic
- 아래와 같이 코딩하는 경우를 일컫습니다.
```rust
mod local_1 {
    pub mod local_2 {
        pub fn foobar() {}
    }
}

use crate::path_1::path_2::foobar;

pub fn eat_at_restaurant() {
    foobar();
```
}

---
## 📍 as keyword
- `use`로 가져온 데이터에 alias 를 붙이는 방법입니다.

```rust
use std::fmt::Result as FmtResult;
use std::io::Result as IoResult;
```

---
## 📍 'pub use'
- `use`로 가져온 데이터를 다시 `pub`으로 <txtylw>exporting</txtylw> 하는 방법입니다.
- 외부 코드는 명시된 path 부터 시작하여 `use`로 데이터를 가져올 수 있습니다.

```rust
// filename: my_code.rs
mod local {
    pub mod public_module {
        pub fn public_function() {}
    }
}

pub use crate::local::public_module;

pub fn local_function() {
    public_module::public_function();
}
```

- `pub use`에서 `pub`의 유무에 따른 결과는 아래와 같습니다.

```rust
// pub use
use myCode::public_module::public_function;

// use
use myCode::local::public_module::public_function;
```

---
## 📍 External Package
- 외부 Package는 `Cargo.toml`에 명시하여 사용할 수 있습니다.

- 예를 들어, 랜덤 값을 출력하는 프로그램을 만들고 싶다면 아래와 같이 코딩할 수 있습니다.

```toml
# Cargo.toml
[package]
name = "package_name"
version = "0.1.0"
edition = "2021"

[dependencies]
rand = "0.8.5"
```

```rust
// main.rs
use rand::Rng;

fn main() {
    let num = rand::thread_rng().gen::<u32>();

    println!("num: {:?}", num);
}
```

- 또한, standrad library 인 `std`도 외부 Package 입니다.

---
## 📍 Nested, Glob Operator
- 같은 path 에 존재하는 것들은 `{A, B, ...}` 와 같은 형태로 명시할 수 있습니다.
- 또, `module::*` 와 같이 모듈 내에 존재하는 모든 정보를 가져올 수도 있습니다.
