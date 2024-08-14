+++
title = "0x0C. Enum"
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
> - `Enum`과 `Struct`를 활용해서 깔끔하게 코딩할 수 있습니다.
> - `Option` 을 이용하면 *Null* 에 관한 문제를 해결할 수 있습니다.

---
# 📌 Enum
- `Enum` 은 `Struct`와 비슷하게 <txtylw>데이터 타입</txtylw>을 좀 더 이해하기 쉽게 만들어주지만, `Struct`와는 다른 방식으로 유용하게 쓰일 수 있습니다.
- `Enum` 은 <txtylw>***하나의 데이터 타입이 여러 형태의 값***</txtylw> 을 가질 때 유용하게 쓸 수 있습니다.
- 예를 들어, <txtylw>*대학생 학년*</txtylw> 이라는 <txtylw>자료형</txtylw>이 있다고 할 때, 아래와 같이 `Enum` 을 활용할 수 있습니다.

```rust
enum CollegeYear {
    Freshman,
    Sophomore,
    Junior,
    Senior,
}
```

---
## 📍 Enum Variable
- 위에서 정의한 <txtylw>*대학생 학년*</txtylw> 을 응용해서 아래와 같이 코딩할 수 있습니다.

```rust
#[derive(Debug)]
enum CollegeYear {
    Freshman,
    Sophomore,
    Junior,
    Senior,
}

#[derive(Debug)]
struct CollegeStudent {
    year: CollegeYear,
    name: String,
}

fn main() {
    let student_1 = CollegeStudent {
        year: CollegeYear::Freshman,
        name: "John".to_string(),
    };

    println!("Student: {:#?}", student_1);
}

```

<img src="../../../images/study/rust/rust_04_01.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

- 또, `Enum`내에 <txtylw>특정 타입</txtylw>의 데이터를 갖도록 할 수도 있습니다.

```rust
#[derive(Debug)]
enum CollegeYearWithID {
    Freshman(u32),
    Sophomore(u32),
    Junior(u32),
    Senior(u32),
}

#[derive(Debug)]
struct CollegeStudent {
    year: CollegeYear,
    name: String,
}

fn main() {
    let student_1 = CollegeStudent {
        year: CollegeYearWithID::Freshman(202312345),
        name: "John".to_string(),
    };

    println!("Student: {:#?}", student_1);
}
```

- 만약 각각의 <txtylw>학년</txtylw>을 `Struct`로 구현해야 한다면, 코드가 굉장히 지저분해질 수 있습니다.
- 즉, `Enum`을 활용하면 조금 더 깔끔하게 코딩할 수 있습니다.

---
## 📍 Option
- <txtylw>***Standard Library***</txtylw> 에 정의된 `Enum` 중, 정말 유용하게 쓰일 수 있는 `Option` 타입에 대해 알아봅니다.
- `Option` 은 어떤 값이 `Something` 또는 `Nothing` 임을 표현할 수 있습니다.

<br />

- `Rust`는 <txtred>***Null***</txtred> feature를 구현하지 않습니다.
-  <txtred>***Null***</txtred> 이 일으킬 수 있는 여러 문제들이 있지만, 사실 있으면 프로그래밍에 굉장히 유용합니다.
- 따라서, `Rust`에서는 이를 직접 구현하는 대신 `Enum`의 `Option` 을 제공함으로써 기능을 사용할 수 있도록 합니다.
- `Enum` 은 아래와 같이 정의됩니다.
```rust
enum Option<T> {
    None,
    Some(T),
}
```

- `<T>`는 generic 에 관한 것으로, 차후 다루도록 합니다.
- 주목할 점은, <txtylw>*Something or Nothing*</txtylw> 을 의미하는 자료형이라는 사실입니다.
- 값이 있는지 없는지를 <txtylw>자료형</txtylw> 단계에서 처리하고, 값이 있는 상태(`something`)에만 연산을 진행하는 방식을 통해 <txtred>***Null***</txtred>과 관련한 문제를 방지할 수 있습니다.

<br />

- 다만, *something* 의 경우 아래의 문제를 해결해야 합니다.

```rust
{
    let a = Some(53);
    let b = 10;

    c = a + b;  // compile error!
}
```

- `Option<u32>`타입과 `u32`타입이 <txtylw>*서로 다르기 때문*</txtylw> 에 발생하는 문제입니다.
