+++
title = "0x18. Result"
description = "Rust"
date = 2023-07-21
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
# 📌 Result
- <txtylw>Panic</txtylw>은 문제가 발생했을 때, <u>*프로그램을 종료*</u> 시켜버렸습니다.
- 사실 문제가 발생해도, 굳이 프로그램을 종료시켜버릴 필요는 없습니다.
- <txtylw>Result/txtylw>는 <txtylw>Option</txtylw>과 마찬가지로 `enum`으로 구현되었습니다.
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

- `T`는 결과가 성공적일 때의 타입이고, `E`는 에러가 발생했을 떄의 타입입니다.
- 즉, 프로그램 수행의 결과에 따라 ***서로 다른 자료형*** 을 다룰 수 있습니다.

---
## 📍 Shortcuts for Panic
- `unwrap` 또는 `expect` method 를 활용하면 아래와 같이 코딩할 수 있습니다.
    - `unwrap`: <txtred>Error</txtred> 일 경우, <txtylw>Panic</txtylw>을 일으킴
    - `expect`: <txtred>Error</txtred> 일 경우, <u>인자로 넣은 문자열</u>과 함께 <txtylw>Panic</txtylw>을 일으킴

```rust
type Score = u8;

fn solve(flag: String) -> Result<Score, String> {
    if flag == "FLAG{Hello}" {
        Ok(100)
    } else {
        Err(String::from("Wrong Flag!"))
    }
}
fn main() {
    let try_1 = solve("whoami".to_string()).unwrap();

    println!("result: {:?}", try_1);
}
```

```rust
type Score = u8;

fn solve(flag: String) -> Result<Score, String> {
    if flag == "FLAG{Hello}" {
        Ok(100)
    } else {
        Err(String::from("Wrong Flag!"))
    }
}
fn main() {
    let try_1 = solve("whoami".to_string()).expect("haha");

    println!("result: {:?}", try_1);
}
```

---
## 📍 Propagating Error
- **꽤 중요한** 개념이고, 실제로 코딩할 때 잘 써먹은 개념입니다.
- <txtred>Error</txtred>가 발생했을 때, 해당 함수에서 처리하고 끝나는 것이 아니라 <txtylw>*Error를 Return 함으로써 다양하게 활용할 수 있는*</txtylw> 방법입니다.
- 앞서 봤던 `unwrap`, `expect`와는 다르게 아래와 같이 `Err(e)`로 return 하는 식으로 코딩하면 됩니다.
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");

    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();

    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

- 그리고 `?` 연산자를 사용하면 위 코드에서의 `match`문과 같은 동작을 하도록 할 수 있습니다.
---
- 단, `?`연산자를 사용한 *propagating*은 <u>`Result`, `Option`, 또는 `FromResidual`를 구현한 타입을 반환하는 함수</u> 내에서만 사용할 수 있습니다.
- `Result` 와 `Option` 은 서로 자동으로 변환되지 않기 때문에, 만약 둘을 한꺼번에 쓰는 코드가 있다면 `?`를 잘 사용해야 합니다...
---
- `main`함수도 다행히 `Result`를 반환하도록 명시할 수 있습니다.
```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```
- `Box<dyn Error>`는 *trait object* 라 불리는 개념인데, 이는 이후 살펴볼 예정입니다.
- `main`함수는 원래 `()`를 반환하며 끝냈지만, 만약 위와 같이 코딩된다면 아래와 같이 값을 반환합니다.
    - `Ok(())`: 0을 반환함
    - `Err(E)`: 0이 아닌 수를 반환함
