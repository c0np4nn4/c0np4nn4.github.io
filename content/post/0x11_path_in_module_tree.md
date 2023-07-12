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
