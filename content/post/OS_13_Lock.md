+++
title = "OS: Lock"
description = "Lock"
date = 2023-05-31
toc = true

[taxonomies]
categories = ["OS"]
tags = ["OS", "Concurrency", "Lock"]

[extra]
math=true
+++

*2023 Spring, PNU, CB26044 (Professor Ahn)*

---

# Classic withdraw example
아래와 같은 `출금` 코드가 있다고 가정해봅니다.

```c
int withdraw (account, amount) {
  // 계정 정보를 토대로 잔금을 확인합니다.
  balance = get_balance(account);

  // 잔금에서 찾으려는 금액만큼 차감합니다.
  balance = balance - amount;

  // 남은 잔금을 계정에 저장합니다.
  put_balance (account, balance);

  // 잔금을 반환합니다.
  return balance;
}
```

얼핏보면 잔금 계산도 잘 되고, 계정에 정보도 잘 업데이트 해주는 것처럼 보입니다.

하지만, 만약 동시다발적으로 한 계정에 대해 `출금`하려고 하면 문제가 발생할 수 있습니다. 아래 그림은 중간에 Context Switch 가 일어나는 경우의 상황을 보여줍니다.

<img src="../../../images/post/cb26044/lock_01.png" width="400rem" alt="not yet" style="border: 2px solid #b3deef"/>

`파랑 박스`의 작업이 일어나던 도중, `빨강 박스`의 작업으로 ***Context Switch*** 가 일어난 상황입니다.

`파랑 박스`가 계정에 <txtred>잔금 정보를 업데이트하기 전</txtred>에 `빨강 박스`가 계정 정보를 획득하여 `출금`이 일어나는 것을 확인할 수 있습니다.

> (+) 블록체인 상의 Smart contract 는 이러한 `withdraw` 함수를 구현해서 실제 '돈'을 다룹니다. 추후 시간이 되면 solidity 코드들에서 발생하는 문제들을 정리해볼까 합니다.

---

## In Program
위에서 살펴본 문제를 실제 c 코드에서의 예시로 한번 더 살펴봅니다.
```c
extern int g;
void inc() { g++; }
```

위 코드를 Assembly 로 표현하면 아래와 같습니다.
```asm
movl 0x1000, %eax
addl $1, %eax
movl %eax, 0x1000
```

`withdraw()` 에서와 마찬가지로 아래 같은 상황이 발생할 수 있음을 쉽게 유추할 수 있습니다.

<img src="../../../images/post/cb26044/lock_02.png" width="340rem" alt="not yet" style="border: 2px solid #b3deef"/>

즉, 두 `Thread` 를 통해 `g++` 가 두 번 일어났으므로 <u>기대하는 결과값</u>은 <txtylw>***2***</txtylw> 이지만, <u>실제로</u>는 <txtred> ***1*** </txtred> 을 얻게 됩니다.

---

# Sharing Memory
- `Thread`는 하나의 `Process` 상에서 ***Address Space*** 를 공유합니다.
- 따라서, 일정 <txtylw>메모리 공간</txtylw>을 <txtylw>공유</txtylw>하고 있습니다.
- 각 `Thread`가 갖는 `Stack`을 제외하면 `Heap`, `Data`, `Text` 등 대부분의 메모리 공간을 모두 접근할 수 있습니다.

---

## Synchronization Problem
- 앞선 `withdraw()` 예제에서도 확인했듯이, `Thread`가 `Sharing Memory` 영역에 접근하면 <txtred>Non-deterministic</txtred> 문제가 발생합니다.
- 이는 <txtred>Race Condition</txtred>으로도 부를 수 있습니다.
- 이를 해결하기 위해 적절한 <txtylw>Synchronization</txtylw> 기법이 필요함을 알 수 있습니다.

---

# Critical Section
- <txtred>**Critical Section**</txtred> 이란, `Shared Memory`에 접근하는 코드 조각을 의미합니다.
- `Critical Section`에 대해서는 아래의 조치들이 필요합니다.
> - 한 번에 하나의 `Thread`만 이를 실행할 수 있습니다.
> - 다른 모든 `Thread`는 강제로 자신의 차례를 기다려야 합니다.

---

# Locks (Basic)
- `Critical Section`이 마치 <txtylw>*하나의 코드인 것처럼*</txtylw> 수행되도록 보장해주는 장치입니다.
- `Lock` 변수는 아래 두 상태를 가질 수 있습니다.
  - <txtylw>*Available (unlock, free)*</txtylw>
    - 현재 Lock 을 획득한 `Thread`가 없습니다.
  - <txtylw>*Acquired (locked, held)*</txtylw>
    - 현재 Lock 을 획득한 `Thread`가 하나 있고, `Critical Section`을 수행 중이라 예상됩니다.

---

## lock() 함수
- `lock()`
  - `Lock` 을 얻기 위해 시도해봅니다.
  - 만약 다른 `Thread` 가 `Lock`을 갖고 있다면, 얻지 못하거나 기다려야 합니다.
  - `Lock`을 얻었다면 해당 `Thread`가 <txtylw>주인</txtylw>이 됩니다.
- `unlock()`
  - `Lock` 을 갖고 있던 <txtylw>주인</txtylw>이 호출할 수 있습니다.
  - `Lock`을 기다리던 다른 `Thread`를 깨워줍니다.

---

## lock 활용
- `Lock` 은 맨 처음에는 <txtylw>*free*</txtylw> 상태입니다.
- `Critical Section` 전후로 `lock()`과 `unlock()` 을 호출합니다.
- `Lock` 의 <txtylw>주인</txtylw>이 `unlock()`을 호출하기 전까지는, 아무도 `Lock`을 획득하지 못합니다.
<br/>
<br/>
- `Lock`을 활용해서 처음의 `withdraw()` 예제를 수정하면 아래와 같습니다.


```c
int withdraw (account, amount) {

  lock(lock);                       // A
  balance = get_balance(account);   // cs_1
  balance = balance - amount;       // cs_2
  put_balance(account, balance);   // cs_3
  unlock(lock);                     // B

  return balance;
}
```

- `cs_1` ~ `cs_3` 는 `Critical Section` 을 의미합니다.
- 위 코드에 따라 실행 흐름의 그림을 다시 그려보면 아래와 같습니다.

<img src="../../../images/post/cb26044/lock_03.png" width="520rem" alt="not yet" style="border: 2px solid #b3deef"/>

- 즉, `Thread 1`이 `unlock()` 으로 `Lock`을 놓고, `Thread 2`가 `Lock`을 획득한 뒤에야 <txtred>Critical Section에 접근</txtred>할 수 있도록 수정되었습니다.

---

## Requirements for Locks
`Lock`은 크게 아래의 세 특성을 만족해야 합니다.
- <txtylw>***Correctness***</txtylw>
  - <txtred>Mutual Exclusion</txtred>: ***오직 하나의 `Thread`*** 만 `Lock` 을 획득할 수 있습니다.
  - <txtred>Progress</txtred>: `Critical Section`에 접근하고자 하는 `Thread`가 존재하면, 반드시 한 `Thread`가 접근할 수 있도록 해주어야 합니다. 이를 <txtred>*deadlock-free*</txtred>로도 부릅니다.
  - <txtred>Bounded Waiting</txtred>: `Lock` 획득을 기다리던 `Thread` 는, 언젠가는 `Lock`을 획득할 수 있도록 해줍니다. 이를 <txtred>*starvation-free*</txtred>로도 부릅니다.
- <txtylw>***Fairness***</txtylw>
  - 각 `Thread`는 `Lock`을 공평하게 획득할 수 있어야 합니다.
- <txtylw>***Performance***</txtylw>
  - `Lock` 메커니즘으로 인한 성능 저하가 얼마나 있을지 평가해야합니다.

---

# Implementing Locks
크게 아래의 세 방법으로 분류할 수 있습니다.
> - `Controlling Interrupts`

<br/>

> - `S/W-only Algorithms`
>   - `Dekker's Algorithm(1962)`
>   - `Peterson's Algorithm(1981)`
>   - `Lamports's Bakery Algorithm for more than two processors(1974)`

<br/>

> - `H/W Atomic Algorithms`
>   - `Test-And-Set`
>   - `Compare-And-Swap`
>   - `Load-Linked(LL) and Store-Conditional (SC)`
>   - `Fetch-And-Add`

---

## Controlling Interrupts
- 아주 초창기에 <txtred>Mutual Exclusion</txtred> 을 구현하기 위해 사용된 방법입니다.
- `Lock`과 관련된 구현은 없습니다.
- `Critical Section`에 진입하면, `Interrupts`가 발생하지 않도록 방지하여 ***Context Switch***를 막습니다.

```c
void lock() {
  DisableInterrupts();
}

void unlock() {
  EnableInterrupts();
}
```

- 간단하게 구현할 수 있고, *single-processor* 에서는 유용하게 쓰이는 <txtylw>장점</txtylw>이 있습니다.
- 그러나, *multi-processor* 에서는 작동하지 않고, 프로그램에 대한 신뢰에 많이 의존한다는 <txtred>문제</txtred>가 있습니다.

---

## Peterson's Algorithm
- 이번에는 두 `Thread`가 존재할 때, [스핀락](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%95%80%EB%9D%BD)인 상황을 구현해봅니다.

```c
int flag[2];    // 각 Thread 의 Lock 획득 여부를 저장할 변수입니다.
int turn;       // 어느 Thread 의 차례인지 체크할 변수입니다.

void init() {
  flag[0] = 0;
  flag[1] = 0;
  turn = 0;
}

void lock() {
  int other = 1 - self;   // 상대 Thread ID 를 계산합니다.
  flag[self] = 1;         // Lock 을 획득할 것임을 저장합니다.
  turn = other;           // 상대의 차례로 우선 지정해둡니다.
  while ((flag[other] == 1) && (turn == other));  // 상대가 Lock 을 갖고 있을 때까지 무한루프를 돌며 기다립니다.
}

void unlock() {
  flag[self] = 0;         // Lock 을 놓습니다.
}

```
