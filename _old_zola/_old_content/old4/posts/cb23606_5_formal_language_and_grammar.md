+++
title = "CB23606, 5, Formal Language and Grammar" 
date = "2023-10-22"
+++

# TL;DR
- ***BNF*** and ***Derivation***
    - `BNF`: the syntax of a language with a set of (production)rules.
    - `Production Rule`: `<variables> + "constants"`
    - `Derivation`: *application* of the production rules
- ***Grammar*** and ***Language***
    - `Grammar` $\stackrel{\text{defines}}\longrightarrow$ `Language`
    - `4 Types of grammar`: introduced by *Noam Chomsky*
    - `Grammar rules` $\stackrel{\text{produce}}\longrightarrow$ ***Strings*** $\in$ `Language`
- ***Context-Free Grammar***
    - `Type 2` Grammar
    - Usually used for defining the ***syntax*** of a programming language
---

# Syntax and Semantics

- ***Syntax***

    `Programming Language`의 관점에서 어떤 것이 ***Well-formed*** 인지를 정의합니다.

    **문자열 집합** 으로 정의되는 `Formal Language`로 기술됩니다.

- ***Semantics***

    프로그램의 실행 결과를 기술하는 부분입니다.

    `Natural Language`로 기술됩니다.

---

# Formal Language
> - `Alphabet`:  `Symbol`의 집합
> - `String`: `Alphabet`의 연속(sequence)
> - `(Formal)Language`: `String`의 집합

정리하면, `Language`란 `symbol`의 집합인 `alphabet`이 연속되어 나타난 `문자열`들의 집합입니다.

또한,  ***Grammar***에 의해 `Language`는 표현될 수 있습니다.

---

# BNF
`BNF(Backus-Naur Form)`은 프로그래밍 언어의 `Syntax`를 나타내는 방법으로, ***Context-Free Grammer***의 표기법입니다.

```md
<expression>    ::= <number> + <number>
                  | <number> - <number>
<number>        ::= "0" | "1"
```

"extended" BNF 인 `EBNF`가 존재하며, `BNF`와 같은 표현력을 같습니다.

---

# Derivation
`BNF`가 명시하는 규칙들로 상수를 생성할 수 있으므로, 이를 `Production Rule`이라 부르기도 합니다.

`Derivation`은 $\Rightarrow$ 로 표기하며, 이러한 ***Rule*** 의 application 입니다.

---

# Grammar
`Grammar`는 `Language`를 생성하기 위한 ***규칙의 집합*** 입니다.

언어학자이자 철학자인 `Noam Chomsky`가 네 가지 Type으로 묶은 언어에 따라,  네 가지 Type 의 Grammar 를  생각해볼 수 있습니다.

## Four Types of Grammars
- Unrestricted Grammer  [Type 0]
- Context-Sensitive Grammars    [Type 1]
- Context-Free Grammars [Type 2]
- Regular Grammars  [Type 3]

<center>

<img alt="hierarchy" src="https://files.codingninjas.in/article_images/chomsky-hierarchy-in-theory-of-computation-toc-1-1680363175.jpg" />

</center>

## Context-Free Grammar
$G=(N, T, P, S)$ 로 정의됩니다.
- $N$: `non-terminal`
- $T$: `terminal`
- $P$: `Production Rules`
- $S$: `Start Symbol`

***Grammar*** $G$로 정의되는 ***Language*** $L$은 다음과 같이 정의됩니다.
$$L(G) = \\{w | S \stackrel{+}\Rightarrow w \\}$$

- ***Context-Free Grammar*** 
    - 모든 $P$가 $A \rightarrow x$의 형태
        - $A$: $N$
        - $x$: $(N \cup \Sigma)$인 *string*

