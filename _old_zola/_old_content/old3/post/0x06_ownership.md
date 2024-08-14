+++
title = "0x06. 소유권"
description = "Rust"
date = 2023-06-27
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---
# TL;DR
> - `Rust`는 <txtylw>Compile</txtylw> 단계에서 메모리에 관한 검사를 함
> - <txtylw>소유권</txtylw>으로 메모리 관리를 할 수 있음
> - <txtylw>Move</txtylw>, <txtylw>Clone</txtylw>, <txtylw>Copy</txtylw> 의 소유권 이동에 관한 개념이 있음

---

# 🤔 소유권
- 프로그래밍에서 <txtylw>*메모리 관리*</txtylw> 는 일반적으로 아래 두 방식으로 이뤄집니다.
    - `Garbage Collect`
      - 주기적으로 <u>*사용하지 않는 메모리*</u> 를 검사하는 `garbage collector`가 동작
    - `By Programmer`
      - 명시적으로 메모리를 <txtylw>할당</txtylw>하거나, <txtylw>해제</txtylw>
    - 두 방식은 모두 <txtred>Runtime</txtred>에 이루어집니다.
---
- `Rust` 에서의 `Ownership`은 이와 달리, <txtred>Compile</txtred> 단계에서 이루어집니다.
- <txtred>*Compiler*</txtred> 는 정해진 규칙(`ownership`)에 따라 코드를 검사하고, 규칙에 어긋날 시 <u>*컴파일을 하지 않습니다.*</u>
- 따라서, <u><txtred>Runtime</txtred> 중의 *성능 저하*</u> 등의 문제는 없습니다.
---
- 여기서는 `String` 자료구조를 통해 `Ownership`에 대해 설명합니다.

---

## 📌 소유권 규칙
> - `Rust`에서 모든 <txtred>값</txtred>은 <txtred>주인</txtred>이 있음
> - 항상 <txtred>단 한 명</txtred>의 <txtylw>주인</txtylw>만 존재할 수 있음
> - <txtylw>주인</txtylw>이 scope 밖으로 벗어나게 되면, <txtylw>값</txtylw>은 <txtred>Drop</txtred>됨

---
## 📌 변수 Scope
- `변수 Scope`란, 프로그램 내에서 `변수`가 유효한 범위를 의미합니다.
- 일반적으로 통용되는 개념이므로, 자세한 설명은 생략합니다.

---
## 📌 String Type
- 앞서 살펴본 [자료형](@/post/0x02_data_type.md)의 변수들은 모두 <txtred>Stack</txtred> 에 저장되는 <txtylw>고정 크기</txtylw> 자료형입니다.
- 이와 달리, `String`은 <txtred>Heap</txtred> 에 저장되는 자료형입니다.
```rust
let msg_1 = "This is a message, 1";
let msg_2 = String::from("This is a message, 2");
```
- 따라서, 아래와 같이 Runtime 중에 문자열의 크기가 변하는 것이 가능합니다.

```rust
let mut msg = String::from("Hello");
msg.push_str(", world!");

println!("msg: {:?}", msg);
```

### 메모리 할당과 해제
- `String` 자료형을 위해 Heap 메모리에 두 종류의 작업을 해야 합니다.
> - 메모리 할당
> - 메모리 해제
- <txtylw>*메모리 할당*</txtylw>은 `String::from("blah blah")`로 `String` 변수를 만드는 것을 확인했습니다.
- <txtylw>*메모리 해제*</txtylw>는 ***변수가 Scope를 벗어나는 순간 <txtylw>자동으로</txtylw>*** 해제 됩니다.
    - 이는 `String` 자료형이 <txtylw>drop</txtylw> 이라는 특수한 함수를 갖고 있고, 괄호가 닫힐 때 `Rust`가 이를 자동으로 호출하기 때문입니다.
    - 이러한 메모리 해제 패턴은 `C++`에서 `RAII`로 불리는 것과 유사합니다.

---

### Move
```rust
let old_var = String::From("Test");
let new_var = old_var;

println!("old_var: {}", old_var);
```

- 위 코드의 동작을 아래와 같이 이해할 수 있습니다.

```rust
// old_var 는 "Test" 문자열이 저장된 Heap 공간을 가리킵니다.
let old_var = String::From("Test");

// new_var 는 old_var 가 가리키던 Heap 공간을 가리킵니다.
// 이는 `shallow copy`라고도 불리는 방법인데,
// 같은 메모리 공간을 둘 이상의 변수가 가리키고 있는 상태입니다.

// 만약 이 상태로 현재 scope 를 벗어나게 되면,
// Rust 는 자동으로 drop 을 호출해서 메모리 공간을 해제합니다.

// 이렇게 되면 double free bug 가 발생할 수 있기 때문에,
// Rust 에서는 `shallow copy` 가 일어난 후, 
기존의 변수가 더 이상 메모리 공간을 가리키지 않는 것으로 처리합니다.

// 이를 `Move` 라 부릅니다.
let new_var = old_var;


// old_var 는 binding 된 데이터가 없는 것으로 간주되기 때문에,
// Compile Error 가 발생합니다.
println!("old_var: {}", old_var);
```

---

### Clone
- `Deep Copy` 를 하고 싶다면, <txtylw>Clone</txtylw> method 를 호출하면 됩니다.
```rust
let old_var = String::From("Test");
let new_var = old_var.clone();

println!("old_var: {}", old_var);
```

- 위 코드는 `old_var`, `new_var` 가 각각의 Heap 공간을 가리키며, 두 공간에는 동일한 데이터가 존재합니다.
- 그러나, `Deep copy`의 특성상 <txtylw>비싼 연산</txtylw>이라는 점을 알아야 합니다.

---

### Copy
- <txtylw>Stack</txtylw> 상의 자료형은 <txtylw>Compile Time</txtylw>에 크기가 고정되어 있기 때문에, `Deep copy`를 하더라도 비용이 그리 크지 않습니다.
- 따라서, <txtylw>Stack</txtylw>에 저장되는 자료형에 대해 `Copy` trait 을 구현하면, `Move` 대신 `Copy`가 일어납니다.
- 단, `Copy`와 `Drop` trait 을 둘 다 구현하면 <txtylw>Compile Error</txtylw>가 발생한다고 합니다.
- 아래와 같이 <u>*별도의 메모리 할당이 불필요</u>* 하거나 <u>*간단한 scalar 값*</u> 들에 대해 `Copy` trait 을 구현할 수 있다고 합니다.
> - `Integer`, `Boolean`, `floating-point`, `char`, `Tuples (원소가 모두 스택에 저장되는 류의 데이터일 경우)` 등등

---
## 📌 소유권과 함수 인자
- <txtylw>함수 인자</txtylw>로 값을 넘기는 경우에도 `Move` 또는 `Copy` 가 일어납니다.

```rust
fn main() {
    let msg = String::from("message");
    let val = 100;

    take_ownership(msg);
    make_copy(val);
}

fn take_ownership(m: String) {
    println("msg: {}", m);
}   // 만약 scope 를 벗어나게 되면, 
    // 변수 `m`이 가리키는 heap 공간은 `drop` 함수에 의해 자동으로 해제됩니다.


fn make_copy(v: u8) {
    println("val: {}", v);
}
```

---
## 📌 소유권과 함수 반환값
- <txtylw>함수의 return value</txtylw>에서도 `Move`가 일어납니다.
```rust
fn main() {
    let msg = give_ownership();

    let msg = take_and_give_back(msg);
}

fn give_ownership() -> String {
    String::from("message");
}

fn take_and_give_back(m: String) -> String {
   m 
}
```
