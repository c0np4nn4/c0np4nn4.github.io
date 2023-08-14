+++
title = "0x11. Path in Module Tree"
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
# 📌 Path
- `Module Tree`로부터 특정 `함수`나 `타입`을 찾기 위해서는 적절한 `Path`를 지정해줘야 합니다.
- `Path`는 크게 두 가지 종류가 있습니다.
    - `Absolute Path`: `crate` 키워드를 사용하여, `Crate Root`부터 시작해서 모든 경로를 명시
    - `Relative Path`: `self`, `super` 등의 키워드를 사용하여, 간략히 명시

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

- 위 코드는 **절대경로**와 **상대경로**의 사용 예시입니다.
- 그러나, 위 코드와 같이 단순히 `front_of_house::hosting::~`하면 제대로 <txtred>Compile</txtred>되지 않습니다.
- 왜냐하면, <txtylw>*child*</txtylw> 에 해당하는 `hosting` module 이 <txtred>*private*</txtred> 로 default 설정 되어있기 때문입니다.
> - <txtred>Parent</txtred>는 <txtylw>(private) child</txtylw>의 내부 코드를 볼 수 없습니다.
> - 반면, <txtylw>Child</txtylw>는 <txtred>Parent</txtred>의 모든 코드를 볼 수 있습니다.

---
## 📍 Exposing with 'pub' keyword
- `pub` 키워드를 사용하면 <txtred>*private*</txtred> 을 <txtred>*public*</txtred> 으로 바꿀 수 있습니다.
- `pub`을 적절히 사용하여 <txtylw>API</txtylw>를 구성하는 방법은 [Rust API Guide](https://rust-lang.github.io/api-guidelines/)에서 보다 자세히 확인할 수 있습니다.

> - `src/main.rs` 와 `src/lib.rs` 를 모두 갖고 있는 Pacakge의 경우 아래와 같이 코드가 구성됩니다.
>     - `lib.rs`에 module tree 를 명시
>     - `main.rs`는 마치 *외부 rust binary* 처럼 `lib.rs`의 코드를 사용
> - 따라서, 이러한 구성은 `API`를 설계할 때 유리합니다.

---
## 📍 Relative Path: 'super' keyword
- 상위 모듈에 접근할 때 사용하는 키워드 입니다.
```rust
fn foo() {}

mod myModule {
    fn foo2() {
        bar();
        super::foo();
    }

    fn bar() {}
}
```

---
## 📍 'pub' keyword w/ Struct and Enum
- `Struct`는 `pub`을 각 <txtylw>field</txtylw>에 대해 지정할 수 있습니다.
```rust
pub struct myStruct {
    field_1: u8,
    pub field_2: u8,
}
```

- 반면, `Enum`은 `pub`을 하면 모든 타입이 `pub`이 됩니다.
```rust
pub enum myEnum {
    type_1,
    type_2,
}
```
