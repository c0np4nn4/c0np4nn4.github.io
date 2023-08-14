+++
title = "0x0B. Method"
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
# <txtred>**TL;DR**</txtred>
- `impl` 블록의 함수는 모두 *Associated Function* 이다.
- `Self` 는 `impl` 뒤에 오는 자료형의 <txtred>***Alias***</txtred> 이다.

---
# 📌 Method
- `Method`란, `Struct` 내부에서 정의된 <txtylw>함수</txtylw>를 의미합니다.
- <txtylw>함수 인자</txtylw>의 첫 번째는 항상 <txtred>self</txtred>로, 자신의 `Struct`를 가리킵니다.

---
## 📍 Defining Method
- [*[0x0A_struct_usage](@/post/0x0A_struct_usage.md)*] 에서의 코드를 `Method` 로 다시 작성하면 아래와 같습니다.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

// fn area(rectangle: &Rectangle) -> u32 {
//     rectangle.width * rectangle.height
// }

fn main() {
    lect rect = Rectangle {
        width: 30,
        height: 40,
    };

    println!(
        "Area: {}",
        // area(&rect)
        rect.area()
    );
}
```

- `impl` 키워드를 사용하여 특정 `struct`에 대한 `method`를 구현할 수 있습니다.

<br />

- `Method` 에서 사용하는 <txtylw>&self</txtylw>는 <txtylw>*self: &Self*</txtylw> 의 짧은 표기법입니다.
- 이 때, `Self`는 `impl` 블록이 가리키는 `struct`를 의미합니다.
- 프로그래머는 이를 경우에 따라 `self`, `&self`, `&mut self` 등 다양하게 활용할 수 있습니다.
    - *소유권 획득, 단순 참조, 참조 후 내부 field 값 수정*

> - `C++`에서는 `method call`에 `->` 연산자를 따로 두었지만, `Rust` 에서는 이를 <txtylw>*automatic referencing and dereferencing*</txtylw> 라는 방법으로 해결했다고 합니다.
> - 즉, `Rust`는 아래와 같이 코드에 <txtylw>*자동으로*</txtylw> `&`, `&mut`, `*` 등을 붙여줍니다.
> ```rust
> object.something()
> (&object).something()
> ```
> - 사실 이렇게 자동으로 붙여주는 것이 가능한 이유는 `Method` 정의 부에서 <txtylw>*self*</txtylw> 를 적절히 정의했기 때문이기도 합니다.
>     - <txtylw>*&self*</txtylw> : <txtred>*Reading*</txtred>
>     - <txtylw>*&mut self*</txtylw> : <txtred>*Mutating*</txtred>
>     - <txtylw>*self*</txtylw> : <txtred>*Consuming*</txtred>
---
## 📍 Method with parameters
- 함수 인자를 추가하여 아래와 같이 코딩할 수 있습니다.
```rust
#[derive(Debug)]
struct ID {
    name: String,
    age: u32
}

impl ID {
    // Self 는 ID 의 alias
    // 즉, return type: &ID
    fn get_info(&self) -> &Self {
        return &self;
    }

    fn update_age(&mut self, new_age: u32) {
        self.age = new_age;
    }
}

fn main() {
    let mut id_01 = ID {
        name: String::from("John"),
        age: 24,
    };

    println!("[before] Info Id 01: {:?}", id_01.get_info());

    id_01.update_age(25);

    println!("[after]  Info Id 01: {:?}", id_01.get_info());
}
```

<img src="../../../images/study/rust/rust_03_02.png" width="500rem" alt="adsf" style="border: 2px solid white"/>

---
## 📍 Associated Functions
- `impl` 블록의 모든 함수는 *Associated Function* 입니다.
- *"특정 자료형과 관련있는 함수"* 이기 때문입니다.
- 경우에 따라서는 <txtylw>*self*</txtylw>를 인자로 받지 않는 함수를 정의할 수도 있습니다.
    - 이 경우 `Method` 는 아닙니다.
```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

---
## 📍 Multiple impl Blocks
- 하나의 `Struct` 에 대해 여러 `impl` 을 작성하는 것이 가능합니다.
- 나중에 *여러 파일을 관리* 하게 되면 이러한 방법이 유용하게 사용될 수 있습니다.
