+++
title = "0x15. String"
description = "Rust"
date = 2023-07-16
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
# 📌 String
- `Rust`에서 `String`은 <txtylw>collection of bytes</txtylw>로 구현되어 있습니다.
- `Rust`는 사실 <txtylw>*string slice*</txtylw> 를 의미하는 `str` 타입을 기본적으로 갖습니다.
    - 프로그램에 <u>***저장되어***</u> 있는 <u>문자열 조각</u>의 형태입니다.
- 이와 달리, `String` 타입은 <u>***변경가능***</u> 한 문자열이라 생각하면 됩니다.
- 일반적으로 `Rust`에서 <txtylw>문자열(String)</txtylw>을 얘기할 때, 이 둘 중 하나를 가리킨다고 보면 됩니다.

---
## 📍 Creating a new String
```rust
    let hello = String::from("السلام عليكم");
    let hello = String::from("Dobrý den");
    let hello = String::from("Hello");
    let hello = String::from("שָׁלוֹם");
    let hello = String::from("नमस्ते");
    let hello = String::from("こんにちは");
    let hello = String::from("안녕하세요");
    let hello = String::from("你好");
    let hello = String::from("Olá");
    let hello = String::from("Здравствуйте");
    let hello = String::from("Hola");
```
- `String`은 **UTF-8** 인코딩을 따르기 때문에, 왠만한 문자들을 모두 다룰 수 있습니다.

---
## 📍 Update, Concatenate
- `mut` 키워드를 붙이면, `Vector`와 마찬가지로 값을 더하는 것이 가능합니다.
```rust
let mut s1 = String::from("Hello, ");

let s2 = "world!";

s1.push_str(s2);

println!("{s1}");
```

- 또, `+` 연산자를 이용해서 아래와 같이 두 String 을 concatenate 할 수 있습니다.

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("World!");

let s3 = s1 + &s2;
```

- `+` 연산자는 아래의 `add` method 를 이용해서 두 문자열을 합칩니다.

```rust
fn add(self, s: &str) -> String  {
    //...
}
```

- `Rust`는 `&String`을 `&str`로 coerce 할 수 있습니다.
- 따라서, `s2`를 reference 로 빌려오면 내부에서는 `&str`로 이용할 수 있습니다.
- `s1`은 소유권을 아예 넘기고, `s2`는 reference 로 빌려옴으로써 단순히 <txtylw>*copy*</txtylw> 하는 것보다 효율적으로 두 문자열을 합칠 수 있습니다.

---

- `format!` 매크로를 이용해 아래와 같이 concatenate 하는 것도 가능합니다.

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("World!");

let s3 = format!("{}{}", s1, s2);
```

---
## 📍 Indexing
- `Rust`에서 `String`은 <txtylw>단순히 indexing 하는 것이 불가능</txtylw>합니다.
- 직관적인 이유는 `Rust`의 `String`이 <txtylw>UTF-8</txtylw>로 인코딩 되어있기 때문입니다.
```rust
let hello = "“नमस्ते";
let answer = &hello[0];
```
- 위 힌디어는 '나마스테'를 의미하는데, *18 bytes* 로 표현됩니다.
- *indexing* 을 통해 문자를 가리킨다면 아래와 같은 배열을 기대하게 됩니다.
```rust
["न", "म", "स्", "ते"]
```

- 내부적으로 `18 bytes`를 잘 해석해서 `4 글자`로 바꿔야 한다는 점에 주목하면 됩니다.

---
## 📍 Slicing
```rust
let hello = "Здравствуйте";

let s = &hello[0..4];
```

- 약간의 <txtylw>꼼수</txtylw>(?)를 부려서 `&hello[0..1]` 이렇게 코딩하면, <txtred>Compile Error</txtred>가 일어납니다.

---
## 📍 Iterating
- `String`을 iterating 하는 방법은 아래와 같습니다.
```rust
for c in "Зд".chars() {
    println!("{c}");
}
```

- 아니면, 아예 `bytes`단위로 다룰 수도 있습니다.
```rust
for b in "Зд".bytes() {
    println!("{b}");
}
```
