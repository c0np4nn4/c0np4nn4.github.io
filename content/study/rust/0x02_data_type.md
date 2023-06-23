+++
title = "0x02. 자료형"
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

# 🤔 자료형
- `Rust`에서의 자료형은 크게 두 범주로 나누어 볼 수 있습니다.
> - Scalar
> - Compound
- `Rust`는 <txtylw>*Statically Typed* Language</txtylw> 입니다.
    - 따라서, <txtred>컴파일 시점</txtred>에 모든 변수의 타입을 알고 있어야 합니다.
    - `Rust`의 <txtred>컴파일러</txtred>는 값을 유추(infer)하는 것이 가능합니다.

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

- 만약 위 코드에서 `u32` 자료형을 명시하지 않는다면, <txtred>컴파일 에러</txtred>가 발생합니다.

---

## 1️⃣ Scalar Type
- `Scalar Type` 단일 값을 갖는 타입입니다.

---

### 📌 Integer
- <txtylw>Two's complement</txtylw> 로 구현되어 있으며, 아래 타입을 가집니다.

<center>

| 길이 | Signed | Unsigned |
| :--- | :----: | :------: |
| 8-bit |  i8     | u8       |
| 16-bit | i16     | u16       |
| 32-bit | i32     | u32       |
| 64-bit | i64     | u64       |
| 12-bit | i128     | u128       |
| architecture | isize     | usize       |

</center>

- `Rust` 에서는 `Integer`를 `100u8` 과 같이도 적을 수 있습니다.
- 각 *진법*으로 `Integer`를 적는 방법은 아래와 같습니다.

<center>

| Number Literals | Example |
| :--- | :---- |
| Decimal |  1_000_000 |
| Hex| 0xcafe|
| Octal| 0o123 |
| Binary | 0b1101_1101 |
| Byte (`u8`)| b'A'    |

</center>

> - `Integer Overflow` 를 처리하는 방법은 아래와 같이 나뉩니다.
> - <txtylw>Debug</txtylw> 모드로 컴파일: <txtred>Overflow</txtred> 발생 시 <txtred>Panic</txtred>
> - <txtylw>Release</txtylw>모드로 컴파일:  <txtred>Wrapping</txtred>이라는 방법으로 처리 (256 -> 0, 257 -> 1)

---

### 📌 Floating-point
- <txtylw>IEEE-754</txtylw> 표준을 기반으로 부동소수점을 표현합니다.
- 기본 타입은 `f64` 이고, `f32` 도 사용할 수 있습니다.

---

### 💫 Numeric Operations
- `Rust`는 기본적인 수학 연산들을 제공합니다.
- 연산자에 관한 보다 자세한 내용은 [Operators, Rust](https://doc.rust-lang.org/book/appendix-02-operators.html) 를 참고하면 됩니다.

---

### 📌 Boolean
- `1-Byte` 크기를 갖습니다.
- `true`, `false` 두 종류의 값을 가질 수 있습니다.

---

### 📌 Character
- <txtylw>*String Literals*</txtylw> 와 달리, `Character` 는 `single quote` 로 값을 나타냅니다.
- `4-Byte` 크기를 갖기 때문에, 일반적인 `ASCII` 보다 훨씬 많은 문자를 표현할 수 있습니다.
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';

    println!("{}", c);
    println!("{}", z);
    println!("{}", heart_eyed_cat);
}
```
- 위 코드의 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_04.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

---

## 2️⃣ Compound Type

- `Compound Type`은 <u>여러 값을 하나의 타입으로 그룹화</u>한 타입입니다.

---

### 📌 Tuple
- `고정 길이`를 갖습니다.
- 각 값의 `타입`이 다를 수 있습니다.
```rust
fn main() {
    let tup: (u8, f32, char) = (100, 2.2, '3');
}
```

- <txtylw>Destructure</txtylw> 도 가능합니다.
```rust
fn main() {
    let tup: (u8, f32, char) = (100, 2.2, '3');

    let (a, b, c) = tup;

    println!("a: {}", a);
    println!("b: {}", b);
    println!("c: {}", c);
}
```

<img src="../../../images/study/rust/rust_01_05.png" width="300rem" alt="adsf" style="border: 2px solid white"/>

- 또, *index* 를 이용해 참조하는 것도 가능합니다.
```rust
fn main() {
    let tup: (u8, f32, char) = (100, 2.2, '3');

    // let (a, b, c) = tup;

    println!("a: {}", tup.0);
    println!("b: {}", tup.1);
    println!("c: {}", tup.2);
}
```
- 마지막으로, 내용 없이 비어있는 `Tuple`을 ***Unit*** 이라 부릅니다.

---

### 📌 Array
- `고정 길이`를 갖습니다.
- 각 값의 `타입`은 같아야 합니다.
- 값을 `스택`에 저장해두고 싶을 때 유용합니다.
```rust
let months = [
    "Jan", "Feb", "Mar", "Apr", "May", "Jun", //
    "Jul", "Aug", "Sep", "Oct", "Nov", "Dec",
];

let seq: [u32; 5] = [1, 2, 3, 4, 5];

let nums = [7; 3]; // == [7, 7, 7];
```
- 위 코드와 같이 `배열`을 선언할 수 있습니다.

---

- `Rust`는 배열의 원소를 참조할 때, *index* 값이 배열의 크기를 넘어서는 값인지를 검사합니다.
- 즉, `arr: [u32; 10]` 인 배열 `arr`에 대해 `arr[10]` 을 시도하면, <txtred>Panic</txtred>이 일어납니다.
- 이는 `Rust` 가 갖는 <txtylw>*Memory Safety*</txtylw> 의 한 예시이기도 합니다.
    - `C` 언어에서의 `BOF`가 일어날 수 없습니다.
---
