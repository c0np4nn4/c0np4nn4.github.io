+++
title = "0x14. Vector"
description = "Rust"
date = 2023-07-12
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
# 📌 Vector
- 동일한 `type`의 데이터를 저장할 때 사용할 수 있는 `collection` 입니다.
- <txtylw>*Generic*</txtylw>으로 구현되었기 때문에, `타입`을 명시해줘야 합니다.

```rust
let v: Vec<i32> = Vec::new();
```

- 아래와 같이 `vec!` 매크로를 이용해서도 선언할 수 있습니다.

```rust
let v = vec![1, 2, 3];
```

- 아래와 같이 값을 추가할 수 있습니다.
- 추가되는 값을 `i32`로 추론할 수 있기 때문에, 따로 명시할 필요는 없습니다.

```rust
let mut v = Vec::new();

v.push(100);
v.push(200);
```
---
## 📍 Reading value
- `Vector` 의 값을 읽는 방법은 두 가지 방법이 있습니다.
    - *index* 를 이용
    - *.get()* 메서드를 이용
```rust
let v = vec![1, 2, 3, 4];

// (1) index
println!("v[2]: {}", v[2]);

// (2) .get()
println!("v[2]: {}", v.get(2).or_else(|| { Some(&-1) }).unwrap());
```

- *.get()*의 인자로 유효하지 않은 `index` 를 줄 때, -1 값을 반환하는 코드입니다.

---
## 📍 Borrowing value
- 아래 코드는 <txtred>컴파일 에러</txtred>를 일으키는 코드입니다.

```rust
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);

println!("The first element is: {first}");
```

- 첫 번째 값을 <txtylw>빌려오는</txtylw> 것이 전체 `Vector`에 <u>어떤 영향을 주는지</u> 직관적으로 이해하기 힘들 수 있습니다.
- 이러한 <txtred>컴파일 에러</txtred>는 사실 `Vector`의 동작 방식 때문에 발생하게 됩니다.
- 메모리에 나란히 값을 저장하고 있는 `Vector`가 새로운 값을 추가할 때, <u>충분한 공간을 확보할 수 없다</u>고 판단하면, <u>새로이 적절한 메모리 공간을 할당</u>받고 그 곳으로 값을 복사하는 경우가 발생할 수 있다고 합니다.
- 이런 경우, `Vector`의 첫 번째 값에 대한 `Reference`가 <txtred>*Dangling Pointer*</txtred> 가 될 가능성이 있습니다.
- 따라서, `Rust`에서는 `immutable borrow` 이후 `mutable borrow` 시도를 막음으로써 이를 방지합니다.

---
## 📍 Iterating
- `Vector` 의 값을 순회하며 참조하는 방법입니다.
```rust
let v = vec![1, 2, 3];

for i in v {
    println!("{i}");
}
```

---
## 📍 with Enum
- `Enum` 가 <txtylw>같은 자료형에 다른 이름</txtylw>을 사용할 수 있는 방법임을 상기하면, `Vector`를 좀 더 유용하게 사용할 수 있습니다.

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

---
## 📍 Drop
- `Vector`가 `Drop` 되면, 내부에 있던 값들도 모두 `Drop` 됩니다.
```rust
{
    let v = vec![1, 2, 3];
} // <- v is freed
```
