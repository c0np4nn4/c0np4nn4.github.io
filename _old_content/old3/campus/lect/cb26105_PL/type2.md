+++
title = "type2"
description = "PL"
date = 2023-05-16
toc = true

[taxonomies]
categories = ["Lect", "PL"]
tags = ["PL"]

[extra]
math=true
+++

---

## 기억을 잘 하는 법
- `잘 아는 것`에 `잘 모르는 것`을 연결해서 기억하면 좋음

---

## Review

<details>
<summary>지난 시간까지의 내용 복습</summary>
$\text{변수} \in \text{이름}$ 과 <txtylw>속성</txtylw> 을 연결하는 것을 `바인딩`이라 부름
이 `바인딩`을 언제 하느냐가 `binding time`

> - binding time
>   - static: <txtred>실행 전</txtred>, `주류`
>   - dynamic: <txtred>실행 후</txtred>

<txtylw>속성</txtylw>의 종류는 아래와 같음
> 이름
> 타입
> 값
> 주소
> 지속시간
> (가시)영역----참조환경(볼 수 있는 이름의 집합)
- `지속시간`과 `영역`은 밀접한 관계가 있지만 같지는 않음

이 중 <txtred>타입</txtred> 에 대해 살펴봄

---

| | Fundamental<br/><center>기본</center>| Derived<br/><center>유도</center>|
|---|---|---|
| Atomic (Scalar)<br/><center>단순</center>| `int`: <txtred>2's 보수</txtred>, <br />`float`<txtred>exceed 127</txtred>| `enum`, `subrange`|
| Composite<br/><center>복합</center>| `string`: <txtred>3가지 구현 방법</txtred>, `complex` | `array`:<txtred>size 가 type의 일부인지?, size 값을 함께 제공해야 하는지</txtred>, `record`|

---
- `언어` 에 의해 나뉘는 타입의 종류
  - <txtylw>primitive (built-in)</txtylw>, `원시 타입`
  - <txtylw>user defined (ordinal type)</txtylw>, `사용자 정의 타입`
     - `c++11` 에서는 ***enum class*** 를 이용할 수 있음

</details>

---

# Types 2
- `C` -> `C++` -> `Java` 로 이어질 때, `goto`, `union` 은 물려받지 않음.
  - ***없어도 가능하기 때문***
---
> - `associative array`
>   - java, c++: map
>   - python: dict
>   - per: hash

- `pointer` 로 인한 문제 (<txtred>기억해야함</txtred>)
  - `garbage(space leak)`: <u>***분실 객체***</u>, <del style="color: #6f6f6f">쓰레기라고 하지 말자</del>
  - `dangling reference`: <u>***허상 참조***</u>

---

## Open Array
- `range` 가 따로 전달 안되는 `Array`
  - 전달받은 것 안에서 따로 `size` 에 관한 함수를 이용함
```
a: Array of 3..7
a[3], a[4], a[5], a[6], a[7] 참조 가능

만약 function(Row: array) 의 인자로 넘기면
a[1], a[2], a[3], a[4], a[5] 참조 가능 (High 함수를 통해 범위를 파악)
```

- *Delphi (aka. Visual Pascal)*

---

## Array Operation
- `Array` 를 한 단위(unit) 으로 보는 연산들 (원소를 하나 빼는 등은 해당 안됨)
  - `Inner Product`, `Transpose`, etc.
- `Array`에 특화된 언어
  - <txtylw>Fortran</txtylw><txtred>90</txtred>
    - `Intrinsic Function`
  - <txtylw>APL</txtylw>

---

## Slice
- <txtylw>Fortran</txtylw><txtred>90</txtred>
```
INTEGER VECTOR(1:10)
1~10 (포함)
```
- <txtylw>Ada</txtylw>
- python 의 그 slice 를 생각하면 됨

---

### Impl. of Array
- `Compile-Time Descriptor`
  - Lower bound, Upper bound 등
  - index 로 참조한다든지 할 때, `검사`를 위한 값들이므로
  - $\langle$Lower Bound, Upper Bound, etc.$\rangle$ 를***Dope Vector*** 라고도 부름
- `Access Function`
  - 첨자 ----------> 주소<br />
    index   Acc.F  Address

> (<txtred>시험에 잘 나옴</txtred>)
> - `row major` ([ref](https://en.wikipedia.org/wiki/Row-_and_column-major_order))
>   - 한 행을 기준으로 메모리에 로드함
>   - `matrix[i][j]` --> 자연스럽게 `row major`가 됨
>     - `i == k` 로 정해지면, `행`이 정해진 것으로 보면 됨
> - <txtylw>Fortran</txtylw> 은 ***col major*** 임
>   - (addr of matrix[1, 1]) + ((j-1) * m + (i-1))

---

### Associative Arrays
- `key` 로 `index` 를 획득함
  - `key` 는 문자열 등 **임의의 데이터**
  - `key` 로 `index` 를 생성하는 함수를 ***Hash Function*** 이라 함
    - 메모리 값이 싸져서 `Hash Table` 자료구조가 강세일 수 있음

> `python`: Dict
> `Lua`: Table(일부)

---

## Record
- `Array` 는 원소의 type 이 모두 같음
  - $\mathbb{homogeneous}$
- 반면, `Record` 는...
  - $\mathbb{heterogeneous}$
```
struct {int x, floaty} b;
```

- `COBOL`의 `fixed-point number`
  - $99\text{V}99 = 99.99$
  - `floating-point number` 보다 ***정확하다*** 는 장점이 있음

---

### Record Field References
- `COBOL` 의 `OF` (영어 구문을 사용)
- 다른 언어에서는 `.` 을 씀

---

### Record Operations
- `COBOL` 의 `MOVE CORRESPONDING`
  - 다른 `Record` 사이의 대입(복사)이 가능함
- ??????????

---

## Union
- 합집합, 이것저것 다 저장할 수 있음

> Record 는 $T_1 \neq T_2$ 에 대해 $T_1 \times T_2$
> 
> Union 은 $T_1 \neq T_2$ 에 대해 $T_1 \cup T_2$
> 
> Array 은 $T_1 = T_2$ 

- 메모리 사용을 절약할 수 있음
- `가변 레코드`라 부르기도 함

---

### Union in Algol 68
- `Conformity Clauses`
  - **값**을 활용하는 코드를 따로 지정해줄 수 있음

---

### Union in Pascal
- `Variant Record` 라 불림
  - `case tag`
- `Type Checking` 문제가 있음
  - `Tag`가 별도로 들어가기 때문에, *ival*, *rval* 부분에서 문제가 생김

---

### Union in Ada
- `Discriminated Union` 이라 불림
- 할당할 때는 <u>Tag</u> 값을 꼭 포함해야 함.

---

## Pointer Type
> - `nil`: 명사
> - `null`: 형용사
- `nil` 값을 가질 수 있음
- `nil` 은 무슨 타입일까?
---
- `Pointer` 는..
  - 동적 메모리 할당
  - 함수 전달할 때도 사용할 수 있음
---
- `pointer` 와 `reference` 차이
  - `reference` 는 **암시적** 포인터라 생각하면 됨
- `C`: pointer, `Java`: reference, `C++`: pointer, reference

---

### Pointer Operations
- C 코드를 통해 쉽게 이해할 수 있음

---

### Problems with Pointer
- <txtred>Dangling Pointers</txtred> (허상 포인터)
  - <txtred>큰 문제</txtred>
- <txtred>Lost Heap-Dynamic Variables</txtred> (분실 객체(변수))
  - 크게 문제가 되지는 않음 (<txtred>낭비</txtred>)

---

### Lang. Examples of Pointer
- `Pascal`: Dangling
- `C/C++`: Dangling
- `Ada`: Dangling (UNCHECKED_DEALLOCATION)

---

### Handling Dangling Pointers
- `Tombstone` 방식
  - 포인터가 `메모리`를 직접 가리키지 않고, `비석`을 가리킴
  - 만약 메모리 공간이 해제되면, `비석`에 해제되었음을 기록
- `Locks-and-Keys` 방식
  - 포인터, 메모리에 `key` 가 존재
  - 참조 직전에 두 `key` 를 비교함
  - 만약 해제하면 메모리의 `key` 도 해제되며, 자연스레 `key` 불일치가 일어남
> 두 방식 다 `검사` 과정이 있기 때문에 효율성은 많이 떨어짐
