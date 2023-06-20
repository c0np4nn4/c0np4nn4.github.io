+++
title = "0x01. 변수와 Mutability"
description = "Rust"
date = 2023-06-20
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---

# 🤔 변수
- 기본적으로 `Rust`에서의 변수들은 모두 <txtred>Immutable</txtred> 입니다.

```rust
fn main() {
    let x = 5; 
    println!("1, Value x: {}", x);

    x = 10; 
    println!("2, Value x: {}", x);
}
```

- 위 코드를 실행한 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_01.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

- 이렇게 <txtred>Immutable</txtred> 을 기본으로 설정하면, *변수값의 변경을 추적하는 등* 신경 쓸 요소가 줄어들기 때문에, 보다 <txtylw>안전</txtylw>하게 코딩할 수 있습니다.

---

- 하지만, `Rust` 에서는 <txtred>Mutable</txtred> 한 변수도 만들 수 있도록 `mut` 키워드를 제공합니다.
- `mut` 를 이용해 위 코드를 수정하면 아래와 같습니다.

```rust
fn main() {
    let mut x = 5; 
    println!("1, Value x: {}", x);

    x = 10; 
    println!("2, Value x: {}", x);
}
```

- 위 코드를 실행한 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_02.png" width="300rem" alt="adsf" style="border: 2px solid white"/>

- 성공적으로 변수의 값이 변경됨을 확인할 수 있습니다.

---

# 🤔 상수
- `상수`는 <txtred>Immutable</txtred> 변수와 비슷하지만, 차이점이 있습니다.
    - `mut` 를 사용할 수 없으며 항상 불변
    - 프로그램 전역에서 참조 가능
    - 런타임 계산값으로 설정 불가능, 상수 표현식으로만 설정 가능
- `상수`에 대한 *Name Convention* 은 `대문자, _` 입니다.

```rust
const TWO_HOURS_IN_SECONDS: u32 = 2 * 60 * 60;
```

- 위에서 `2 * 60 * 60` 은 <txtylw>컴파일러</txtylw> 단에서 연산되어 저장됩니다.
- `상수`는 프로그램 실행 내내 유효한 scope 에서 참조 가능합니다.

---

# 🤔 Shadowing
- `Rust` 에서는 <txtylw><u>*같은 이름의 변수를 사용*</u></txtylw> 하는 것이 가능하며, 이를 <txtylw>Shadowing</txtylw> 이라 부릅니다.

```rust
fn main() {
    let x = 5;
    println!("1, value: {}", x);

    let x = x + 1;
    println!("2, value: {}", x);

    {
        let x = x * 2;
        println!("3, value: {}", x);
    }

    println!("4, value: {}", x);
}
```

- 위 코드를 실행한 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_03.png" width="450rem" alt="adsf" style="border: 2px solid white"/>

- <txtylw>Shadowing</txtylw> 과 <txtred>Mutable</txtred> 변수의 차이는 아래와 같습니다.
    - <txtylw>Shadowing</txtylw> 은 변수의 `자료형`을 바꿀 수 있음
    - <txtylw>Shadowing</txtylw> 된 변수 값을 변경(mutate)하면 <txtred>컴파일 에러</txtred>가 일어남

---
