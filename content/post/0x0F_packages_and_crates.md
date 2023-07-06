+++
title = "0x0F. Packages and Crates"
description = "Rust"
date = 2023-07-06
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
# 📌 Crates
- `Crate`라는 단어는 사전적으로 `나무 상자`를 의미합니다.

<img src="https://cdn.pixabay.com/photo/2020/04/05/01/09/crates-5004274_1280.png" width="400rem"/>

- `Rust` 에서는 `Crate`를 아래와 같이 <txtylw>정의</txtylw>합니다.
> <txtred>Compiler</txtred>가 한번에 고려하는 코드의 최소량
- `Crate`는 아래 두 종류로 나타날 수 있습니다.
    - `binary crate`
        - 실행할 수 있는 프로그램으로 컴파일되는 코드
        - 코드에 항상 `main` 함수를 포함해야 함
    - `library crate`
        - 코드에 `main` 함수가 없음
        - 일반적으로 `crate`를 부르는 종류
- `Crate Root`는 <txtred>Compiler</txtred>가 `Crate`의 `Root Module`을 구성하는 소스 파일입니다.

---
# 📌 Package
- `Package`는 <txtylw>*하나 이상의 `Crate`로 이루어진 묶음*</txtylw> 을 의미합니다.
- `Package`는 하나의 `Cargo.toml` 파일을 하나 갖습니다.
    - 내부에 `Package` 내의 `Crate`를 <txtylw>빌드하는 방법을 명시</txtylw>합니다.

---
## 📍 cargo new project
- 아래와 같이 입력하면 새로운 `binary crate` 를 생성할 수 있습니다.
```bash
cargo new project
```

- 이후 `/project` 디렉토리의 파일들을 살펴보면 아래와 같습니다.

```bash
➜  project git:(master) ls -l
total 8
-rw-r--r-- 1 user user  176 Jul  6 19:25 Cargo.toml
drwxr-xr-x 2 user user 4096 Jul  6 19:25 src
➜  project git:(master)
```

- `Cargo.toml`을 살펴보면, `Package`에 대한 정보를 확인할 수 있습니다.

```bash
➜  project git:(master) cat Cargo.toml
[package]
name = "project"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
➜  project git:(master)
```

- 잘 보면, `src/main.rs`와 같이 `main`함수가 정의된 코드를 명시하지 않고 있습니다.
- 이는 `Rust`의 `Cargo`가 `src/main.rs` 를 <txtylw>Crate Root</txtylw>로 생각한다는 일종의 ***convention*** 을 따르기 때문입니다.
- 이와 마찬가지로, 만약 `src/lib.rs` 가 있다면, `Cargo`는 `src/lib.rs`를 <txtylw>Crate Root</txtylw>로 간주합니다.
    - `Cargo`는 이러한 <txtylw>Crate Root</txtylw>를 `rustc` <txtred>Compiler</txtred>로 보내서 <txtylw>빌드</txtylw>를 진행합니다.

<br />

- `src/bin` 아래에 <txtylw>다수의 binary crate</txtylw>를 두는 것도 가능합니다.
