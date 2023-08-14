+++
title = "0x1B. Lifetime"
description = "Rust"
date = 2023-08-06
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
# 📌 Lifetime
- `Lifetime`은 <txtylw>Reference</txtylw>가 유효한 범위를 의미합니다.

---
## 📍 Preventing Dangling Reference
- `Dangling Reference`를 방지하는데 활용됩니다.

```rs
fn main() {
    let r;                // ---------+-- 'a
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |
        r = &x;           //  |       |
    }                     // -+       |
                          //          |
    println!("r: {}", r); //          |
}                         // ---------+
```

---
## 📍 Generic Lifetimes
- 함수를 정의할 때, `generic lifetime`을 명시할 수 있습니다.
```rs
fn concat_str<'a>(x: &'a str, y: &'a str) -> &'a str {
    //
}
```
- 함수를 정의할 시점엔, 전달받는 인자의 `lifetime` 등을 확정할 수 없기 때문에 `generic` 과 비슷하게 추상적으로 정의해둡니다.

---
## 📍 Lifetime Annotation Syntax
- `Lifetime`을 명시함으로써 각각의 `Reference`들에 대한 scope 를 좀 더 명확히 할 수 있다는 장점이 있습니다.
```rs
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

---
## 📍 Lifetime Annotation Syntax
- 다시 `concat_str()`함수를 살펴보겠습니다.
```rs
fn concat_str<'a>(x: &'a str, y: &'a str) -> &'a str {
    //
}
```

- 여기서 `&'a`의 의미는 반환값도 인자인 `x`, `y`의 `lifetime`을 갖게 됨을 의미합니다.
- 만약 `x`와 `y`의 `lifetime`이 상이할경우, 둘 중 <txtred>***더 좁은***</txtred> 영역에 해당하는 `lifetime`을 갖습니다.
- 즉, 아래와 같은 경우 컴파일 에러가 일어납니다.

```rs
fn main() {
    let string1 = String::from("Hello, ");

    let result;
    {   //---------------------------------------------------------------+       
        let string2 = String::from("World!\n");                   //     +
                                                                  //     + &'a
        result = concat_str(string1.as_str(), string2.as_str());  //     +              
    }   //---------------------------------------------------------------+ 
    println!("Hello, World! == ", result);                    
}
```

- `string2`의 `lifetime`이 `&'a`가 되기 때문에, `result`는 `println!()`매크로에서 참조할 수 없습니다.

---
## 📍 Thinking about terms of Lifetimes
- `Reference`를 반환할 때 `Dangling Reference`가 되지 않도록 `Lifetime`을 잘 활용할 수 있습니다.
- 일반적으로, `함수 인자`의 `Lifetime`과 연관되도록 함수를 정의하면 됩니다.

---
## 📍 Lifetimes in Structs
- `Struct`를 정의할 때도 `Reference`를 field 값으로 갖도록 할 수 있습니다.
```rs
struct MyStruct<'a> {
    data: &'a str,
}
```

---
## 📍 Lifetimes Elision
- `Lifetime`을 명시하지 않아도 되는 상황에 관한 내용입니다.
- `Rust`에 관한 짤막한 역사와 `Elision`에 대한 세 가지 규칙의 설명이 있습니다...
    - <txtylw>Rule 1</txtylw>: 각 인자에 대한 `lifetime`이 있다고 가정한다. (`&'a, &'b, ...`)
    - <txtylw>Rule 2</txtylw>: 오직 하나의 인자만 받는다면, 반환 인자는 모두 동일한 `lifetime`을 갖는다.
    - <txtylw>Rule 3</txtylw>: 만약 입력 인자에 `&self`, `&mut self`가 있다면 `self`의 `lifetime`을 따른다.

---
## 📍 Lifetime in Method
```rs
impl<'a> MyStruct<'a> {
    fn getThree(&self) -> i32 {
        3
    }
}
```
- `Rule 1`에 따라 `&self`는 `&'b` 등의 개별 `lifetime`을 갖습니다.
- `&'a`는 `MyStruct` 구조체의 `lifetime`으로, `field` 값등이 유효한 범위를 의미합니다.

```rs
impl<'a> MyStruct<'a> {
    fn getData(&self, msg: &str) -> &str {
        println!("Msg: {}",msg);
        self.data
    }
}
```
- `Rule 1`에 따라 `&self`는 `&'b`, `msg`는 `&'c`의 개별적인 `lifetime`을 갖습니다.
- 이후 `Rule 3`에 따라 `&self`의 `lifetime`인 `$'b`를, 반환값에도 적용합니다. `&'b str`

---
## 📍 Static Lifetime
- 프로그램 실행 전체 범위를 `lifetime`으로 갖는 `'static` 이 있습니다.
