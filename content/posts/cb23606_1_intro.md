+++
title = "CB23606, 1, Introduction"
date = "2023-09-10"
+++

# What is a Compiler
- `Com`: 함께, `Pile`: 쌓다.

컴파일러를 한 문장으로 정의하면 "*프로그래밍 언어* 로 작성된 코드를 **다른** *프로그래밍 언어* 로 변환(transform) 시켜주는 것"입니다.

<center>
<img alt="compiler and interpreter" src="../../Class/CB23606_Compiler/1_1.png" />
</center>

# Program and Language
**Program**이란 *"Sequence of Instructions"* 라 할 수 있습니다. 자연스레 **Process**란 *"Sequence of Instruction in **Execution**"* 라 할 수 있습니다.

# Two Big Phases of Compilataion
`Compiler`는 크게 **전단부(FE)** 와 **후단부(BE)** 로 나누어 살펴볼 수 있습니다.

**전단부(FE)** 에서는 소스코드를 읽고 분석하며 여러 정보를 수집합니다.

**후단부(BE)** 에서는 목적코드를 생성합니다. 앞서 수집한 정보를 이용하여 의미있는 코드를 생성하는 역할을 합니다.

# Course Objectives
본 수업을 통해서 컴파일러에 관한 *원리와 이론* , *구성*, AST 등과 관련된 *자료 구조*, 그리고 어휘 및 구문을 분석하는 *툴에 대한 이해* 를 익힐 예정입니다.

# Expectation
기본적인 *원리와 이론* 을 이해하고, 새로운 언어를 *설계* 할 수 있으며, 사용할 수 있을 정도의 언어를 *구현* 하게 됩니다.
또한, 관련 툴들을 사용할 수 있고 컴파일러를 작성하기 위해 필요한 기초적인 기술들을 이해하게 됩니다.
