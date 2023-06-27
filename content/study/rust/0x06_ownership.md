+++
title = "0x06. 소유권"
description = "Rust"
date = 2023-06-27
toc = true

[taxonomies]
categories = ["Rust"]
tags = ["rust"]

[extra]
math=true
+++

---

# 🤔 소유권
- 프로그래밍에서 <txtylw>*메모리 관리*</txtylw> 는 일반적으로 아래 두 방식으로 이뤄집니다.
> - `Garbage Collect`
>   - 주기적으로 <u>*사용하지 않는 메모리*</u> 를 검사하는 `garbage collector`가 동작
> - `By Programmer`
>   - 명시적으로 메모리를 <txtylw>할당</txtylw>하거나, <txtylw>해제</txtylw>
- 두 방식은 모두 <txtred>Runtime</txtred>에 이루어집니다.
---
- `Rust` 에서의 `Ownership`은 이와 달리, <txtred>Compile</txtred> 단계에서 이루어집니다.
- <txtred>*Compiler*</txtred> 는 정해진 규칙(`ownership`)에 따라 코드를 검사하고, 규칙에 어긋날 시 <u>*컴파일을 하지 않습니다.*</u>
- 따라서, <u><txtred>Runtime</txtred> 중의 *성능 저하*</u> 등의 문제는 없습니다.
---
- 여기서는 `String` 자료구조를 통해 `Ownership`에 대해 설명합니다.

---

## 📌 소유권 규칙
> - `Rust`에서 모든 <txtred>값</txtred>은 <txtred>주인</txtred>이 있음
> - 항상 <txtred>단 한 명</txtred>의 <txtylw>주인</txtylw>만 존재할 수 있음
> - <txtylw>주인</txtylw>이 scope 밖으로 벗어나게 되면, <txtylw>값</txtylw>은 <txtred>Drop</txtred>됨

---
## 📌 변수 Scope
- `변수 Scope`란, 프로그램 내에서 `변수`가 유효한 범위를 의미합니다.
- 일반적으로 통용되는 개념이므로, 자세한 설명은 생략합니다.

---
## 📌 String Type
- 앞서 살펴본 [자료형](@/study/rust/0x02_data_type.md)의 변수들은 모두 <txtred>Stack</txtred> 에 저장되는 <txtylw>고정 크기</txtylw> 자료형입니다.
- 이와 달리, `String`은 <txtred>Heap</txtred> 에 저장되는 자료형입니다.
```rust
let msg_1 = "This is a message, 1";
let msg_2 = String::from("This is a message, 2");
```
- 

