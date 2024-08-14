+++
title = "note, compiler"
date = "2023-12-05"
+++

# LR Parsing
- `input`: LL과 같음
- `output`: 우파스(right parse)
- `stack`: 상태열(state) <-> LL: 문법 기호열
- `table` 
    - *Action Table* (row: 상태, col: T U {$})
    - *Goto Table* (row: 상태, col: N)
## 구성방법
- **Table** 크기에 따라
    - SLR: 간결한 표
        - SLR(0)
        - SLR(1)
    - LALR: SLR 보다는 정교한 표(크기는 같음), ***YACC***, ***BISON*** 이 하는 것
    - CLR: 엄청 큰 크기의 표

---
## LR Parser Structure
- `input`
- `output`
- `stack`: 스택 top 에는 항상 상태(**State**)가 있음
- `table` 

## LR Parser Configuration
- (`Stack`, `Input`) 값을 정해놔야함

## LR Parser Action
- **Shift**
    - `shift S` 로, 스택에 push 하는 것과 비슷함
- **Reduce**
    - `Stack`에 위치한 Action ($X_mS_m$)을 $\beta$로 바꿈 ($A \rightarrow \beta$)

## LR Parsing Algorithm
- **Accept**

## LR Example
- *Action Table*
    - $s_i$: shift, $i$: 상태 번호
    - $r_j$: reduce, $j$: Production Rule 번호
    - empty space: error
    - *acc*: accept
- 우파스: Actions 중 production rule을 순서대로 나열

---

- $S' \rightarrow S$: accept와 관련

## LR Items
- `Dot`: '어디까지 읽었나?'
> - A -> XY.Z : 'XY까지 읽었고, Z는 아직 안읽었다.'

## Closure
- `Dot` 다음의 symbol: `mark symbol` (표시 기호)
- `Initial Item`: Augmented 시작 전의 item
- `Reduce Item`: `Dot`을 오른쪽에 두는 symbol
- `Kernel Item` 
- `Closure Item`

## Closure Example
- Closure는 언젠간 끝남
$$
\begin{align}
Closure(I) &= \newline
[ [E' \rightarrow .E], [E \rightarrow .E + T], [E' \rightarrow .T] \newline
 [T \rightarrow .T * F], [T \rightarrow .F],  \newline
 [F \rightarrow .(E)], [F \rightarrow .\text{id}] ]
\end{align}
$$

---

## Goto(I, X)

---

## Example of $C_0$
$$
\begin{align}

\end{align}
$$

---
> SLR(0), SLR(1) , ... SLR(i), i: # of LA
