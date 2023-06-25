+++
title = "0x03. 함수"
description = "Rust"
date = 2023-06-22
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

# 🤔 함수
- `Rust`의 <txtylw>함수</txtylw>는 <txtylw>snake case</txtylw> 컨벤션을 따릅니다.

```rust
fn main() {
    println!("Main function");
    
    another_function();
}

fn another_function() {
    println!("Another function");
}
```

- 위 코드에서 알 수 있듯이, `Rust`는 <txtylw>*같은 scope 내에 정의되어 있다면*</txtylw> 순서에 상관없이 함수 호출이 가능하다는 특징이 있습니다.

---

## 📌 Parameter
- 함수에 적절한 `인자`를 정의할 수 있습니다.
```rust
fn main() {
    print_num(32);
}

fn print_num(x: i32) {
    println!("Num: {}", x);
}
```
- 코드 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_06.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

- `Rust`는 함수를 정의할 때 <txtred>Type</txtred>을 반드시 명시하도록 설계되었습니다.
    - <txtylw>***함수 정의***</txtylw> 에서 <txtred>Type</txtred>을 명시해두면, <txtylw>컴파일러</txtylw>는 코드의 다른 부분에서 이를 찾지 않아도 됩니다.
    - 이는, <txtylw>컴파일러</txtylw>로 하여금 더 유용한 에러 메세지를 띄울 수 있게 돕기도 합니다.

---
## 📌 Statements and Expressions
- `Rust`는 <txtylw><u>*Expression-Based Language*</u></txtylw> 입니다.
- 따라서, `Statements`와 `Expression`을 구분해서 이해할 필요가 있습니다.
> - `Statement`: 일련의 작업(명령)을 수행하고, 값을 반환하지 않음
> - `Expression`: 결과값을 연산함

---
### 📍 Statements 

```rust
fn main() {
    let num:u32 = 100; // statement
    
    let a = (let b = 3); // error!
}
```

- 위 코드는 `Statement`의 예시 입니다.
- 두 번째 문장은 에러가 발생합니다. `let b = 3`은 `Statement`이므로 아무런 값도 반환하지 않습니다. 따라서, 변수 `a`는 binding 할 값이 없어 <txtred>에러</txtred>가 발생합니다.

---
### 📍 Expressions

- 수식 `5 + 6`은 `11` 이라는 값으로 연산됩니다.
- 이처럼 어떠한 결과값을 내는 것을 `Expression`이라 생각하면 됩니다.
- `Expression`은 `Statement`의 일부분이 될 수도 있습니다.
- `Expression`의 예시들은 아래와 같습니다.
> - `let y = 6;` 에서 `6`
> - 함수 호출
> - 매크로 호출
> - `{}`로 정의되는 새로운 *block scope* 생성

```rust
fn main() {
    let num = {
        let a = 10;
        a + 90
    };

    println!("Num: {num}");
}
```

- 코드 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_07.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

```rust
{
    let a = 10;
    a + 90
}
```
- 위 블록의 결과값이 `100`으로 연산되기 때문에, `num` 변수에 대해 `let Statement`로 binding 될 수 있습니다.
<br />
<br />
- 위 코드에서의 특징은 마지막 `a + 90` 끝에 세미콜론(`;`)이 없다는 것입니다.
- 만약 세미콜론을 붙이게 되면 `Expression`을 `Statement`로 바꾸게 됩니다.

---
## 📌 Functions with Return Values
- `Rust` 에서는 <txtylw>함수 반환값</txtylw>을 명시할 때, <txtylw>화살표</txtylw> (`->`) 키워드 오른편에 <txtred>Type</txtred>을 표기합니다.
- 또, `return` 키워드를 이용할 수도 있지만, 보통의 경우에는 함수 맨 마지막에 `Expression`으로 반환값을 표시합니다.

```rust
fn hundred() -> i32 {
    100
}

fn main() {
    let num = hundred();

    prinltn!("Num: {num}");
}
```

- 코드 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_08.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

---
### 📍 Function with parameter
- 이번에는 <txtylw>함수 인자</txtylw>를 받는 예를 살펴보겠습니다.

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}

fn main() {
    let num = plus_one(100i32);

    println!("Num: {num}");
}
```

- 코드 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_09.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

---
### 📍 Ends with semicolon
- 만약 **plus_one** 함수에서 `x + 1` 끝에 세미콜론(`;`)을 넣게 된다면 어떻게 될지 살펴보겠습니다.

- 함수를 아래와 같이 수정합니다.
```rust
fn plus_one(x: i32) -> i32 {
    x + 1; // semicolon!
}
```

- 코드 실행 결과는 아래와 같습니다.

<img src="../../../images/study/rust/rust_01_10.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

- <txtred>에러 메세지</txtred>에서 볼 수 있듯이 `Implicitly returns ()` 임을 알 수 있습니다.
