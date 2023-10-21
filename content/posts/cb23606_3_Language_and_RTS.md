+++
title = "CB23606, 3, Language and RTS"
date = "2023-10-21"
+++

# TL;DR
- ***Language Design Issue***
    - Types and Declaration
    - Programming Paradigms
- ***Run-Time System***
    - The Running environment of a program.
    - Most of the RTS supports `Control Stack` and `Heap`
- ***Stack Frame***
    - *BP* and *SP* used for managing `Stack Frame`
    - ***Prologue*** + ***Epilogue***

---

# Language Design Matrix
<center>

|                  | Procedural | Object-Oriented | Functional  |
|------------------|:----------:|:---------------:|:-----------:|
| Typed            |            |                 |             |
| Static Checking  |     C      |      Java       |   Haskell   |
| Static Inference |   Pascal   |      Scala      |    OCaml    |
| Typeless         |            |                 |             |
| Dynamic Checking |  Assembly  |     Python      |     Lisp    |

</center>

## Types and Declaration
- ***Typed***

    모든 값(value)과 표현식(expression)들이 `Type`을 갖는 형태입니다. 대부분의 `Typed Language`는 *Declaration(선언)* 을 필요로 합니다.

    > C/C++, Java, Pascal, Scala, Haskell, ML

- ***Typeless***

    모든 `Language`들은 기본적으로 *Type*을 갖긴 합니다. 하지만, *identifier*에 대해 조금은 느슨한 연관을 허용할 때 `Typeless Language`라 할 수 있습니다.

    > Python, Javascript, LISP, Scheme

## Declaration Before Use
대부분의 `Typed Language`는 ***Declaration-Before-Use*** 규칙을 따르며, <u>*One-Pass Compilation*</u> 을 가능하게 해줍니다.

또한,  모든 `Language`는 ***Type Checking*** 을 통해,  `type-safety`를 지원합니다.
`strongly-typed language`는 모든 `type-error`를 감지해냅니다.

## Strongly-Typed and Static-Typing
- ***Strongly-Typed***: `language property`

    `Strongly-Typed Language`는 모든 `type-error`를 감지하며,  *compilation time* 또는 *run-time*에 오류를 감지할 수 있습니다.

- ***Static Typing***: `implementation property`

    `Static Typing`에서는 *Compilation Time(Translation Time)* 에 오류를 감지합니다.

    중요한 점은, **구현**에서의 속성이지 **언어의 설계(Language Design)** 에서의 속성이 아니라는 점입니다.

---

# Programming Paradigm
`Programming Paradigm`은 프로그래머가 문제를 해결하고 소프트웨어를 설계하는 데 사용하는 <u>특정한 접근 방식</u>이나 <u>관점</u>을 말합니다.

잘 알려진 종류는 아래와 같습니다.

- `Imperative`: C, C++, Java, Fortran
- `Procedural`: C, Pascal, Fortran, BASIC
- `Functional`: Haskell, Lisp, Scala, Erlang
- `Logic`: Prolog, Mercury
- `Object-Oriented`: Java, Python, Ruby

---

# Run-Time System
`코드를 실행하는 환경`을 의미합니다. *subprogram* 들에 대한 ***Control Stack*** 과 ***Heap*** 을 지원하기도 합니다.

## Garbage collector
메모리를 알아서 관리해주는 별도의 프로세스로,  대부분의 `VM-based Implementatoin`에서 채택하고 있는 방식입니다.

---

# Stack Machine
`Abstract Stack Machine`이라는 이름으로, 언어에 대한 `Interpreter`, `Compiler`에서 가상의 **Stack**에 임시 변수의 값을 저장하거나 할 수 있습니다.

실제 machine 이 갖고 있는 *Register*를 이용하여 `Stack Machine`에 대한 정보(*sp*, *bp* 등)을 관리하도록 구현할 수 있습니다.
