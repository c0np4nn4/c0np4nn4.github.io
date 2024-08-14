+++
title = "0x1A. Trait"
description = "Rust"
date = 2023-08-05
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
# 📌 Trait
- `Trait` 은 <txtylw>Type</txtylw>에 대한 기능들을 정의하는 방법입니다.
- 다른 언어들의 `Interface`와 흡사합니다.

---
## 📍 Define a Trait
- `Trait`을 정의하는 방법은 아래와 같습니다.
```rs
pub trait Comment {
    fn comment(&self) -> String;
}
```
---
## 📍 Implement a Trait
- `Trait`이 정의되었다면, 구현할 차례입니다.

```rs
struct MyType {
    // skip
}

impl Comment for MyType {
    fn comment(&self) -> String {
        format!("This is my custom type...");
    }
}
```
---
## 📍 Default Implementation
- `Trait`을 정의할 때, default impl 을 정해둘 수도 있습니다.

```rs
pub trait Comment {
    fn comment(&self) -> String {
        println!("(default comment string)...");
    };
}
```

---
## 📍 Trait as a parameter
- `Trait`을 함수 인자로 넘기는 경우, 의미는 아래와 같습니다.
    - 해당 `Trait`을 구현한 타입이면 함수 인자로 받을 수 있습니다.

```rs
pub fn emergency(item: &impl Comment) {
    println!("Emergency! {}", item.comment());
}
```
---
## 📍 Trait bound syntax
- `impl Trait`을 이용할 때 좀 더 깔끔하게 적을 수 있는 방법입니다.
```rs
pub fn emergency<T: Comment>(item: T) {
    println!("Emergency! {}", item.comment());
}
```
- 만약 둘 이상의 `Trait`을 위와 같이 쓰고 싶다면 `+` 를 중간에 추가하면 됩니다.
```rs
pub fn emergency<T: Comment + Display>(item: T) {
    println!("Emergency! {}", item.comment());
}
```


---
## 📍 where Clauses
- `where` 키워드를 사용하면 더욱 깔끔하게 `trait bound syntax`를 활용할 수 있습니다.

```rs
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {}
```
- 아래와 같이 정리할 수 있습니다.
```rs
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
}

```

---
## 📍 Return using Trait
- `impl Trait`을 return type 에 대해서도 적용할 수 있습니다.
- 즉, `Trait`을 구현한 type 이면 반환하도록 함수를 정의할 수 있습니다.
- 하지만, <txtred>*한 번에 하나의 타입*</txtred>만 반환할 수 있다는 점에 유의해야 합니다.
```rs
fn returns_commentable() -> impl Comment{
    Post {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        like_count: 0,
    }
}
```

---
## 📍 Trait bounds to implement method
- 특정 `Trait`을 구현하고 있는 자료형에 대해 `method`를 구현할 수 있습니다.

```rs
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

- 위 코드에서, `Pair<T>`는 `PartialOrd`가 구현되었을 때에만 `cmp_display`를 구현합니다.


