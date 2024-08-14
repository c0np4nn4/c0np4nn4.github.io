+++
title = "CB23606, 2, Phases"
date = "2023-09-10"
+++

# TL;DR
- ***Implementation***
    - `Compiler`: accepts *source*, produces *target*
    - `Interpreter`: accepts *source*, *executes*
    - `Hybrid`: adopts **both** of them
- ***Analysis-Synthesis Model*** 
    - `Analysis` + `Synthesis`
- ***Phases*** and ***Passes***
    - `Phase`: Steps of transformation of the program
    - `Pass`: Whole scan of the program

---

# Overview

<center>
<img alt="phase overview" src="../../Class/CB23606_Compiler/2_1.png" />
</center>

위 그림과 같이 `전단부`와 `후반부`와 구성된 컴파일러는 가운데에 **모래시계**와 같은 형태의 단계가 있으며, 트리구조로 표현되는 ***중간 표현(IR)*** 이 있음을 이해하면 됩니다.

`전단부`에서는 `소스 코드`를 **분석(Analysis)** 하여 여러 의미 있는 정보를 수집합니다.
그리고, `후반부`에서는 중간표현인 *Tree* 에 대한 정보를 토대로 의미를 **생성(합성, 종합)** 합니다. 

컴파일러의 전체 과정을 좀 더 자세히 나타내면 아래와 같습니다.

<center>
<img alt="compilation overview" src="../../Class/CB23606_Compiler/2_2.png" />
</center>

---

# Compilers and Interpreter
## Compilation

`소스코드`로 작성된 프로그램을 *target langugage*로 작성된 `의미적으로 동일한 프로그램`으로 변환하는 작업입니다.

<center>
<img alt="Compilation" src="../../Class/CB23606_Compiler/2_3.png" />
</center>

## Interpretation

`소스코드`에 작성된 **연산**을 수행하는 작업입니다. 개념적으로는 ***Input Value***와 ***Source Code***를 동시에 입력받고 ***Output***을 생성합니다.

<center>
<img alt="Interpretation" src="../../Class/CB23606_Compiler/2_4.png" />
</center>

## Hybrid

`Compilation`의 단계를 일부 까지만 수행하여 적당한 ***portable byte-code***를 생성하고, **Virtual Machine** 등에서 `Interpretation` 작업을 수행하는 형태입니다.

<center>
<img alt="Hybrid" src="../../Class/CB23606_Compiler/2_5.png" />
</center>

---

# Analysis-Synthesis Model
## Analysis
**트리 구조**에 기록된 `소스 코드`가 암시하는 **연산**을 결정합니다.

## Synthesis
**트리 구조**를 가져와서 그 안에 있는 **연산**을 `대상 프로그램`으로 변환합니다.

## Example
> Source: `1 + 2 * 3`
> 
>  ↓
> 
> Tree: `(ADD 1 (MUL 2 3))`
> 
>  ↓ 
> 
> Code: `PUSH 1; PUSH 2; PUSH 3; MUL; ADD;`

---

# Friends of a Compiler
- `Preprocessor`
- `Compiler`
- `Assembler`
- `Linker`

---

# Phases of a Compiler

<center>

| Phase | Output |
| :--- | :--- |
| Programmer | 소스코드(코딩의  결과물)|
| ***Scanner*** (lexical analysis, 어휘 분석) | `Token String` |
| ***Praser*** (syntax analysis, 구문 분석) | `Parse Tree` <br /> or `Abstract Syntax Tree` |
| ***Semantic Analyser***, 의미 분석 | `Annotated Parse Tree` <br /> or  `Abstrat Syntax Tree` |
| ***Intermediate Code Generator***, 중간 코드 생성 | `Three-address Code`, `Quads`, or `RTL` |
| ***Code Generator***, 코드 생성 | `Assembly Code` |

</center>

---

# Grouping of Phases
컴파일러의 Frontend 와 Backend 가 각각 아래와 같이 나뉩니다.
- **FrontEnd**: `Analysis` (Machine Independent)
- **BackEnd**: `Synthesis` (Machine Dependent)

또한, 전체 `소스코드`를 스캔하는 것을 `Pass`라 부르며 아래와 같이 분류할 수 있습니다.
- **Single Pass**: 소스코드를 딱 한 번만 스캔합니다.
- **Multiple Pass**: 소스코드를 여러  번 스캔합니다.  예를 들어, 첫 번째에는 '선언'을 읽고 다음에 실제 코드를 생성하고 최적화 하는 방식입니다.
