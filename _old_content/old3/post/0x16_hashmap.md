+++
title = "0x16. Hashmap"
description = "Rust"
date = 2023-07-19
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
# 📌 Hashmap
- `Key`, `Value` 로 값을 저장할 수 있는 자료구조입니다.

---
## 📍 Create
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
```

- `Hashmap`의 데이터는 모두 <txtylw>Heap</txtylw>에 저장됩니다.
- 또, `Vector`와 마찬가지로 모든 내부 값이 <txtylw>동일한 형태</txtylw>로 저장되어야 합니다.

---
## 📍 Access
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);
```

- `.get()` method 를 사용하여 값을 불러올 수 있습니다.
- 이 때 가져오는 값은 `Option<&T>`의 타입을 갖습니다.
- 따라서, `.copied()`로 값을 하나 복사한 뒤 `.unwrap_or()` method 로 `Some(T)`의 값을 가져옵니다.

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    for (key, value) in &scores {
        println!("{key}: {value}");
    }
```

- 위 코드처럼 iterate 하며 값을 참조할 수도 있습니다.

---
## 📍 Ownership

```rust
    use std::collections::HashMap;

    let field_name = String::from("Favorite color");
    let field_value = String::from("Blue");

    let mut map = HashMap::new();
    map.insert(field_name, field_value);
```
- 위와 같이 코딩하면, `field_name`, `field_value` 값의 소유권이 `Hashmap` 으로 이동하게 됩니다.

---
## 📍 Updating

### Overwriting
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25);

    println!("{:?}", scores);
```

### Adding a (K, V)
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);

    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50);

    println!("{:?}", scores);
```

- `Blue` 라는 키 값이 없을 때만 새로 추가할 수 있습니다.
- 이를 응용하면, 아래와 같이 단어 수를 세는 코드를 짤 수 있습니다.

```rust
    use std::collections::HashMap;

    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0);
        *count += 1;
    }

    println!("{:?}", map);
```

---
`Hashmap`은 `SipHash`라 불리는 해시 함수로 구현되었습니다. 이는 DoS 공격에 저항성이 있다고 알려져 있습니다. 비록 ***가장 빠른 알고리즘***은 아니지만, 안전성을 위해 채택되었다는 점에서 `Rust`스러움을 확인할 수 있습니다...


