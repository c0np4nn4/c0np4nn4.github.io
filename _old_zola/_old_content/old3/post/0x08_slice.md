+++
title = "0x08. Slice"
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

# 🤔 Slice
- `Slice` 는 <txttylw>Reference</txtylw>의 일종으로, <u>데이터 전체에 대한 <txtylw>ref</txtylw></u>대신, <u>연속된 요소 시퀀스에 대한 <txtylw>ref</txtylw></u>를 참조할 수 있게 해줍니다.
- 만약 `Slice`없이, 시퀀스에 대한 참조를 하면 어떤 일이 발생하는지 알아보겠습니다.

---
## 📌 연속적인 데이터를 참조하는 법
- 가장 직관적으로 떠올릴 수 있는 방법은 <txtylw>Index</txtylw>를 통한 참조 입니다.
- 임의의 문장이 있을 때, <txtylw>첫 번째 공백의 위치</txtylw>를 반환하는 함수를 구현해보겠습니다.

```rust
fn main() {
    let mut s = String::from("Hello World");

    let first_space = get_first_space(&s);

    println("first space: {first_space}");
}

fn get_first_space(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &it) in bytes.iter().enumerate() {
        if it == b' '{
            return i;
        }
    }
    
    s.len() // string itself is a word
}
```

- `get_first_space()` 함수는 <txtylw>String reference</txtylw>를 함수인자로 받고, 첫 번째 공백의 <txtylw>Index</txtylw>를 반환하는 함수입니다.
- 위 코드를 다시 한번 살펴보면, `s`가 <txtylw>mutable</txtylw>로 선언되있음을 알 수 있습니다.

---
- 만약, 아래와 같이 `main()` 함수가 수정된다면 <txtred>문제</txtred>가 발생합니다.
```rust
fn main() {
    let mut s = String::from("Hello World");

    let first_space = get_first_space(&s);

    s.clear();
    s.push_str("A B C");

    // answer is 1, but it says 5
    println("first space: {first_space}"); 
}
```

- 이는 <txtylw>문자열</txtylw>과 <txtylw>문자열에 관한 정보</txtylw>가 개념적으로만 연관되어 있고, 실제로는 별개의 값으로 관리되기 때문이라 볼 수 있습니다.
- `Rust`에서는 문자열에 관해 <txtylw>String Slice</txtylw>를 지원함으로써 이런 문제를 해결합니다.

---
## 📌 String Slice
- <txtylw>String Slice</txtylw>는 아래와 같이 <txtylw>문자열의 일부</txtylw>를 가리키는 <txtylw>Reference</txtylw>로 선언하고 활용할 수 있습니다.
```rust
let s = String::from("Hello World");

let hello = &s[0..5]; // start..end
let world = &s[6..11];

let he = &hello[..2]; // omit start
let ld = &world[3..]; // omit end

let hello_world = &s[..];
```

- <txtylw>String Slice</txtylw>는 내부적으로 아래와 같은 값들을 관리하며 데이터를 참조합니다.

<center>

|name|value|
|----|-----|
|ptr| 데이터의 시작 주소|
|len| `end` - `start`|

</center>

- `Slice` 는 <txtylw>immutable ref</txtylw>이기 때문에, 이제 앞서 발생했던 <txtred>문제</txtred>는 발생하지 않게 됩니다.
```rust
fn main() {
    let mut s = String::from("Hello World");

    let first_word = get_first_word(&s);

    s.clear(); // Compile Error!

    println("first word: {first_word}"); 
}

fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```
- 이번에는 <txtylw>첫 번째 단어</txtylw>를 <txtylw>*String Slice*</txtylw>로 반환하는 함수가 있다고 해보겠습니다.
- <txtylw>String Slice</txtylw>가 <txtylw>immutable ref</txtylw>로 존재할 때, `s.clear()` method 로 데이터에 대한 수정을 가하면 <txtred>Compile Error</txtred>가 나리라 예상할 수 있습니다.
- 실제로, `s.clear()`는 <txtylw>Mutable ref</txtylw>를 활용하는데, [지난 글](@/post/0x07_reference.md)에 정리한 내용에 따르면 아래의 규칙을 어기게 됩니다.
> - ***Immutable Ref*** 와 ***Mutable Ref*** 의 Scope 는 서로 분리되어야 한다.
- 따라서, `Rust`의 특성으로 <txtred>버그</txtred>를 애초에 방지할 수 있습니다.

---
## 📌 String Literals as Slice
- <txtylw>String Literals</txtylw>는 <u>*고정 길이*</u> 이며, 상수와 같이 처음에 정해진 문자열로써 <u>*Immutable*</u> 입니다.
- 즉, <txtylw>String Literals</txtylw>를 아래와 같이 생각할 수 있습니다.
> - `binary`상에 저장된 <txtylw>문자열</txtylw>을 가리키는 <txtylw>Reference</txtylw>
- `&str` 타입을 갖습니다.

---
## 📌 String Literals as Parameters
- <txtylw>String Literals</txtylw>를 <txtylw>함수 인자</txtylw>로 사용하는 것이 조금 더 유연한 프로그래밍에 도움이 됩니다.
- 왜냐하면, <txtylw>String Slice</txtylw> 자체도 `&str` 이기 때문입니다.

---
## 📌 다른 Slice 타입
- <txtylw>String</txtylw>에 대한 Slice 가 가능했던 것처럼, 아래와 같은 Slice 도 사용할 수 있습니다.
```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

- 위에서 변수 `slice`의 자료형은 `&[i32]` 입니다.
