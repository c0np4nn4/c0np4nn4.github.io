+++
title = "0x17. Panic"
description = "Rust"
date = 2023-07-21
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
# 📌 Panic
- <txtylw>Panic</txtylw>을 일으키는 방법은 크게 두 가지가 있습니다.
    - 프로그램 실행 중 일어나는 사고
    - 프로그래머가 명시적으로 `panic!` 매크로를 사용한 경우
- 일단 <txtylw>Panic</txtylw>이 발생하면, `Rust`는 기존의 사용하던 `Stack`공간을 전부 정리합니다.
- 만약 이러한 작업을 `Rust`가 아니라 `OS`가 하도록 내버려 두고 싶으면 아래와 같이 `Cargo.toml`을 수정할 수 있습니다.

```toml
[profile.release]
panic = 'abort'
```

---
- `panic!` 매크로를 이용하면 <txtylw>Panic</txtylw>을 명시적으로 일으킬 수 있다는 점을 기억하면 됩니다.
- 또한, 프로그램 실행 시 `RUST_TRACEBACK` 옵션으로 backtrace 할 수 있음을 기억하면 됩니다.
