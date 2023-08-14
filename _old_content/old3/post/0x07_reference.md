+++
title = "0x07. Reference"
description = "Rust"
date = 2023-06-30
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---

# 🤔 Reference
- <txtylw>Reference</txtylw> 는 <txtylw>Pointer</txtylw>와 비슷한 개념으로 이해할 수 있습니다.
- 다만, <txtred>소유권</txtred>은 그대로 있고, 데이터를 가리키는 <txtylw>주소</txtylw>만 <txtylw>빌려오는(borrow)</txtylw> 형태입니다.
- 따라서, `함수 인자`로 빌려오더라도 <txtylw>원래 소유권</txtylw>에는 아무 지장이 없습니다.

```rust
fn main() {
    let name = String::from("John");
    greeting(&name);
}

fn greeting(name: &String) {
    println!("Hello, {name}");
}
```

- 위 코드에서 처럼 <mark>&</mark>를 변수 앞에 붙여 <txtylw>reference</txtylw>를 사용할 수 있습니다.
- 또한, <txtylw>Reference</txtylw>의 <txtred>Scope</txtred>는 <u>선언부터 사용될 때까지</u>로 이해할 수 있습니다.

---
## 📌 Mutable Reference
- <txtylw>Reference</txtylw>로 빌려온 값을 <txtylw>수정</txtylw>하고 싶다면 아래와 같이 `mut` 키워드를 붙여주면 됩니다.

```rust
fn main() {
    let name = String::from("John");

    greeting(&mut name);  // & -> &mut
}

fn greeting(name: &mut String) {  // & -> &mut
    name.push_str("!!");
    println!("Hello, {name}");
}
```

- 단, <txtylw>Mutable Reference</txtylw>는 *데이터 변경* 이라는 <txtred>민감한 작업</txtred>을 할 수 있다는  점에 주목해야 합니다.
- `Rust`는 이로 인해 <txtylw>Race Condition</txtylw>과 같은 문제가 발생하는 것을 <txtylw>Compile Time</txtylw>에 방지합니다.
- 따라서, 아래와 같이 <txtred>*Mutable Reference를 다수의 변수가 할당받는 경우*</txtred> 에는 <txtylw>Compile Error</txtylw>를 일으킵니다.

```rust
{
    let s = String::from("data data");

    let s1 = &mut s;
    let s2 = &mut s;  // Compile Error !

    s2.push_str(" more data");
}
```

- 위 코드의 컴파일 에러를 해결하는 방법은 <txtylw>scope 를 분리</txtylw>하여, 아래와 같이 <txtylw>*한 번에 하나의 Mutable Reference만 이용*</txtylw> 하는 것입니다.
```rust
{
    let s = String::from("data data");

    {
        let s1 = &mut s;
    }

    let s2 = &mut s;
    s2.push_str(" more data");
}
```

- 또, <txtylw>immutable ref</txtylw>(편의상 ref로 적겠습니다)와 <txtylw>mutable ref</txtylw>를 동시에 사용하는 것도 불가능합니다.
- `Mutabtility` 가 혼재된 상황에서 여러 <txtred>버그</txtred>가 발생할 수 있기 때문입니다.
- 따라서, 둘을 함께 사용할 경우, <txtylw>Scope</txtylw>를 고려하여 <txtylw>잘 분리해서</txtylw> 사용해야 합니다.

```rust
{
    let mut s = String::from("data data");

    let r1 = &s;
    let r2 = &s;
    println!("{} and {}", r1, r2);

    let r3 = &mut s;
    println!("{}", r3);
}
```

---
## 📌 Dangling Reference
- <txtylw>허상 참조</txtylw>라고도 불리는 이 문제는, "사용하지 못하는" 메모리 공간을 가리키고 있을 때 발생합니다.
- 원래 사용하던 메모리 공간을 해제한 뒤에도 여전히 가리키고 있는 등의 경우를 생각할 수 있습니다.
- `Rust`에서는 이러한 문제가 발생하는 것을 <txtylw>Compile</txtylw>단게에서 방지합니다.

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```

- 위 코드는 <txtylw>Compile Error</txtylw>가 일어납니다.
- `dangle` 함수를 분석해보면 아래와 같습니다.

```rust
fn dangle() -> &String {
    // 변수 s 를 선언
    let s = String::from("hello");

    // s에 대한 reference 를 반환
    &s
}       // 함수 scope 가 끝나며, 변수 s 는 drop 됨
        // drop 된 값에 대한 reference 를 반환하고 있으므로, dangling reference !
        // 따라서, compile error 가 발생함
```

- 따라서, 이런 경우에는 그냥 <txtylw>소유권</txtylw>을 넘겨주는 식으로 수정하는 것이 좋습니다.
```rust,linenos
fn dangle() -> String {
    let s = String::from("hello");

    s
}
```
