+++
title = "0x09. Struct"
description = "Rust"
date = 2023-07-01
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---
# 🤔 Struct
- `Struct` 는 `Tuple`과 비슷하게, <txtylw>여러 데이터를 하나의 타입</txtylw>으로 관리합니다.
- 둘은 <txtylw>다양한 자료형</txtylw>을 가질 수 있다는 <u>공통점</u>이 있습니다.
- <u>차이점</u>은 `Struct`에서는 각각의 데이터를 <txtylw>이름</txtylw>으로 binding 해야 한다는 점입니다.
    - 이를 통해, <txtylw>데이터 순서</txtylw>에 대해 신경쓰지 않아도 되며 더욱 ***유연한*** 프로그래밍을 가능하게 합니다.

---
## 📌 Struct Define
- `Struct`를 정의하기 위해서는 <txtylw>struct</txtylw> 키워드를 사용하면 됩니다.
- `Struct`의 내부 데이터를 <txtylw>*field*</txtylw>라 부릅니다.
```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

---
## 📌 Struct Instance
- 정의된 `Struct`를 사용하기 위해선 <txtylw>*Instance*</txtylw> 를 생성해야 합니다.
- <txtylw>*Instance*</txtylw>는 <txtylw>Struct name</txtylw>와 중괄호 안에 *key : value* 형식으로 <txtylw>field</txtylw> 값을 명시하면 됩니다.
    - 단, <txtylw>field</txtylw>의 순서를 지킬 필요는 없습니다.
    - <u>*key* 는 <txtylw>field name</txtylw></u>이며, <u>*value* 에는 <txtylw>field type</txtylw>에 맞는 <txtylw>데이터</txtylw></u>를 명시하면 됩니다.

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    let user2 = User {
        active: false,
        email: String::from("someone2@example.com"), // email before username
        username: String::from("someusername123123"),
        sign_in_count: 1,
    };
}
```

---
## 📌 Struct Reference
- `Struct`로부터 특정 값을 참조하기 위해서는 `.` 을 이용하면 됩니다.
- 그리고, 만약 `Struct`가 <txtylw>mutable</txtylw>로 선언되었다면 <txtylw>field update</txtylw>도 가능합니다.

```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```

---
## 📌 Field Init Shorthand
- 만약 `Struct`의 <txtylw>field name</txtylw>과 동일한 <txtylw>변수명</txtylw>으로 `Instance`를 생성하려는 경우, 아래와 같이 *간략하게* 코딩할 수 있습니다.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

---
## 📌 New Instance from old one
- 기존의 `Struct`가 갖고 있던 <txtylw>field data</txtylw>를 이용해서 새로운 `Instance`를 만드는 경우가 종종 있습니다.
- 이런 경우, *struct update syntax* 라는 이름의 방법으로 `..` 키워드를 이용해서 아래와 같이 코딩할 수 있습니다.
```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

- 주목할 점은, `=` 를 사용하게 되면, <txtred>Move</txtred>가 일어날 수 있다는 점입니다.
- 위 코드에서는 <txtylw>user1</txtylw>의 <txtylw>field</txtylw> 중 *username* 을 그대로 쓰게 되고, 이후 <txtylw>user1</txtylw>는 해당 <txtylw>field</txtylw>의 소유권을 <txtylw>user2</txtylw>로 <txtred>Move</txtred>한 것과 같기 때문에, 참조하려고 할 시 <txtred>Compile Error</txtred>가 일어납니다.
```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        active: true,
        username: String::from("john"),
        email: String::from("another@example.com"),
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
    
    println!("user1 name: {}", user1.username); // Compile Error!
}
```

---
# 🤔 Tuple Struct
- `Struct` 처럼 <txtylw>이름이 붙은 `Tuple`</txtylw> 입니다.
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```
- 같은 자료형들을 갖는 `Tuple`에 서로 다른 <txtylw>이름</txtylw>을 부여하여, 프로그래밍을 할 때 유용하게 사용할 수 있습니다.
- `Tuple`과 동일하게 `.`로 <txtylw>내부 데이터를 참조</txtylw>할 수 있습니다.

---
# 🤔 Unit-like Struct
- <txtylw>field</txtylw>가 아예 없는 `Struct`입니다.
- `Struct` 내부에 <txtylw>field</txtylw>를 구현할 필요는 없고, <txtred>trait</txtred>만 구현하고 싶을 때 사용할 수 있습니다.

---
# 🎇 Ownership of Struct Data
- 앞선 예제들에서는 `Struct`가 <txtylw>String</txtylw> 데이터를 온전히 <txtylw>소유</txtylw>하고 있는 코드들을 살펴보았습니다.
- 물론, `&str`타입과 같이 <txtylw>reference</txtylw>로 데이터를 가질 수도 있습니다.
- 하지만, `Rust`에서는 *메모리 안전성* 등 다양한 이유로 <txtred>***lifetime***</txtred> 을 명시하도록 <txtred>Compile</txtred> 단계에서 이를 강제합니다.
