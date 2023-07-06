+++
title = "0x0A. Struct Example"
description = "Rust"
date = 2023-07-04
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---
# <txtred>**TL;DR**</txtred>
> - `Struct`를 <txtylw>Type</txtylw>처럼 활용할 수 있습니다.
> - <txtylw>Derived Traits</txtylw>를 이용할 수 있습니다.
>     - `Display`: 화면 출력과 관련한 trait
>     - `Debug`: 디버깅을 위한 trait

---
# ✔ Struct example
- `Struct`를 이용해서 <txtylw>직사각형</txtylw>에 관한 코드를 작성하면 아래와 같습니다.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    lect rect = Rectangle {
        width: 30,
        height: 40,
    };

    println!(
        "Area: {}",
        area(&rect)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

---
# 📌 Derived Traits
- `Rust`의 <txtred>prinln!()</txtred> <txtylw>매크로</txtylw>는 각 자료형의 `Display` 에 따라서 `{}`에 대한 출력값을 결정합니다.
    - <txtylw>*Primitive Type*</txtylw> 들은 기본적으로 `Display`가 구현되어 있으며, 간단한 값들을 출력하기 때문에 크게 걱정할 것이 없습니다.
- 하지만, `Struct`의 경우에는 <txtylw>값을 어떻게 출력할 것인지</txtylw>를 명시해주어야 합니다.
- 이를 직접 구현하기에 앞서, <txtylw>println!()</txtylw>은 출력하려는 데이터에 `Debug`라는 <txtylw>trait</txtylw>이 구현되어 있으면, `{:?}`로 해당 데이터를 ***디버깅*** 하듯이 단순하게 보여줄 수 있습니다.

```rust
{
    println!("Debug: {:?}", any_struct_instance);
}
```
- `Debug` <txtylw>trait</txtylw>을 추가하는 방법으로 아래와 같이 코딩할 수 있습니다.

```rust
#[derive(Debug)]
struct AnyStruct {
    data: &str,
}
```

- 이를 응용하여 아래와 같이 <txtylw>*Rectangle*</txtylw> instance 가 갖는 값을 확인할 수 있습니다.
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    lect rect = Rectangle {
        width: 30,
        height: 40,
    };

    println!("Rectangle: {:?}", rect);
    println!("Rectangle: {:#?}", rect); // larger
}
```
---
# 📌 dbg! macro
> - <txtred>println!</txtred>은 사실 <txtred>*stdout*</txtred> 로 결과를 출력합니다.
> - 이와 달리 <txtred>dbg!</txtred> 라는 <txtylw>매크로</txtylw>는 <txtred>*stderr*</txtred> 로 결과를 출력합니다.

- 또, <txtred>dbg!</txtred>는 <u><txtylw>expression</txtylw>의 <txtylw>**Ownership**</txtylw></u>를 가져간 후, <u>*몇 번 줄에서 호출되었는지, <txtylw>expression</txtylw>의 결과값은 무엇인지*</u> 를 출력하고, <u>결과값에 대한 <txtylw>**Ownership**</txtylw></u>을 다시 반환하는 동작을 합니다.
    - <txtred>println!</txtred>는 단순히 <txtylw>reference</txtylw>를 빌렸습니다.
- 따라서, 아래와 같이 코딩할 수 있습니다.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        width: dbg!(30 * scale),
        height: 50,
    };

    dbg!(&rect1);
}
```

- 코드의 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_03_01.png" width="700rem" alt="adsf" style="border: 2px solid white"/>

- `Debug` 외에도 여러 <txtylw>trait</txtylw>들이 <txtylw>Derive</txtylw>를 이용해서 구현될 수 있습니다.
