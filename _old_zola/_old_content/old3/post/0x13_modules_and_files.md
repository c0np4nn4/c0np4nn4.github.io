+++
title = "0x13. Modules and Files"
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
# 📌 File Tree
- Package 에 포함된 코드는 아래와 같이 구성되어 있습니다.
```md
.
├── src/
│   ├── main.rs
│   ├── lib.rs
│   ├── aaaa.rs
│   └── aaaa/
│       ├── aaaa_child_1.rs
│       └── aaaa_child_2.rs
└── Cargo.toml
```

---
## 📍 Cargo.toml
- `package`의 정보를 명시한 파일입니다.
```toml
# Cargo.toml
[package]
name = "my_new_package"
version = "0.1.0"
edition = "2021"

[dependencies]
```

---
## 📍 lib.rs
```rust
// lib.rs
mod aaaa;

pub use aaaa::aaaa_child_1;
pub use aaaa::aaaa_child_2;
```

- `aaaa.rs` 모듈을 하위 모듈로 갖습니다.
- `aaaa` 모듈 아래의 `aaaa_child_1`, `aaaa_child_2`를 `pub use`로 <txtylw>Re-exporting</txtylw> 합니다.
    - `main.rs` 에서 이를 이용하여 path 를 간단히 명시할 수 있습니다.

---
## 📍 aaaa.rs
```rust
// aaaa.rs
pub mod aaaa_child_1;
pub mod aaaa_child_2;
```

- `aaaa_child_1`, `aaaa_child_2`를 하위 모듈로 갖습니다.
- 두 모듈은 `pub mod`로 외부에서도 접근 가능합니다.

---
## 📍 aaaa_child_*.rs
```rust
// aaaa_child_1.rs
pub fn aaaa_function_1() {
    println!("aaaa function 1111");
}

// aaaa_child_2.rs
pub fn aaaa_function_2() {
    println!("aaaa function 2222");
}
```

- `pub` 키워드로 외부에서도 접근 가능한 함수를 <txtylw>export</txtylw> 합니다.

---
## 📍 main.rs
- `main.rs` 는 `lib.rs`의 코드를 실행해 보는 역할입니다.
```rust
// main.rs
use my_new_package::aaaa_child_1::aaaa_function_1; 
use my_new_package::aaaa_child_2::aaaa_function_2;

fn main() {
    aaaa_function_1();
    aaaa_function_2();
}
```

---
# 📌 Styles
- 위 예제에서는 `<module_name>.rs`로 모듈을 명시했지만, 아래와 같은 방식으로도 구성할 수 있습니다.
```
src/aaaa.rs
src/aaaa/mod.rs (older style)
```

- 가끔 github 의 코드를 보다보면 아래와 같이 `mod.rs`를 사용하는 경우도 많습니다.
