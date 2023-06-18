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

- `lock()` 함수에서 `turn = other;` 부분은 <txtred>필수적</txtred>으로 있어야 합니다.
  - `flag[self] = 1;` 직후 ***Context Switch*** 가 일어나는 상황을 가정해봅니다.
  - 그렇게 되면, 모든 `Thread`가 <txtred>*기다리는*</txtred>상태가 됩니다.

---
## H/W Supports
- 즉, `Lock`을 fetch 하는 과정은 크게 두 부분으로 나눌 수 있습니다.
> - `Lock`이 획득 가능한지 ***TEST***
> - `Lock`을 획득해오는 ***SET***
- 이를 `H/W`의 도움을 받아 <txtylw>*Atomic Instruction*</txtylw>으로 구현하면 훨씬 유리합니다.

---

### Test-And-Set (Atomic Exchange)
- 간단한 `Lock` 을 만드는 코드입니다.
```c
int TestAndSet(int *ptr, int new) {
  int old = *ptr;
  *ptr = new;
  return old;
}
```

- 위 코드를 아래와 같이 `while` 문 속에서 활용할 수 있습니다.
```c
while(TestAndSet(&lock->flag, 1) == 1);
```

- 처음 TestAndSet 을 호출할 때는 <txtylw>false</txtylw> 이지만, 그 이후에는 <txtylw>true</txtylw> 가 되어 `Critical Section`으로 진입하게 됩니다.

---

- `Test-And-Set`을 평가하면 아래와 같습니다.
- ***Correctness***: <txtylw>**Yes**</txtylw>
  - 하나의 `Thread`만 `Critical Section`으로 들어갈 수 있게 합니다.
- ***Fairness***: <txtred>**No**</txtred>
  - 극단적인 예로, 어떤 `Thread` 는 평생 *Spin* 할 수도 있습니다..
  - 이는 <u>*`Process` 의 도착순서와 무관하게 `Lock`을 획득하기 때문*</u> 입니다.
- ***Performance***
  - `Single-Cpu` 일 때는 오버헤드가 큽니다.
  - 하지만, `CPU`의 수만큼 `Thread`가 사용될 때는 꽤 잘 동작한다고 합니다.

---

### Compare-And-Swap
- `Test-And-Set` 에서는 단순히 `Lock`의 소유 여부만을 검사했습니다.
- 이번에는 `expected` 값을 추가하여, 예상되는 값인지 <txtred>비교</txtred>하는 부분을 추가합니다.

```cpp
int CompareAndSwap(int *ptr, int expected, int new) {
  int actual = *ptr;
  if (actual == expected)
    *ptr = new;
  return actual;
}

void lock(lock_t *lock) {
  while (CompareAndSwap(&lock->flag, 0, 1) == 1)
    ; // spin
}
```

---

### Load-Linked and Store-Conditional
- 간략히 설명하면, <txtylw>*Link 위치의 값(*ptr) 을 미리 로드*</txtylw>해둔 뒤, <txtylw>*해당 Link의 값이 변경되었는지 (conditional)*</txtylw> 검사하여 값(Lock)을 저장하는 방식입니다.
- 따라서, `Lock` 값을 획득한 직후 만일 <txtred>Context Switch</txtred>가 발생해서 미리 로드했던 `Lock` 값에 대한 변화를 감지할 수 있게 됩니다.

---
### Fetch-and-Add
- `Fetch and Add` 는 이전의 값을 반환함과 동시에 값을 증가시키는 코드입니다.
```cpp
int FetchAndAdd(int *ptr) {
  int old = *ptr;
  *ptr = old + 1;
  return old;
}
```

### Ticket Lock
- `Ticket Lock`은 `FetchAndAdd`를 기반으로 구현할 수 있는 `Lock` 입니다.
- 각 프로세스가 `Ticket` 을 받고 자기 차례를 기다림으로써, <txtylw>*Fairness*</txtylw>를 충족시킵니다.

```cpp
typedef strut __lock_t {
  int ticket;
  int turn;
} lock_t;

void lock_init(lock_t* lock) {
  lock->ticket = 0;
  lock->turn = 0;
}

void lock(lock_t *lock) {
  int myturn = FetchAndAdd(&lock->ticket);
  while (lock->turn != myturn)
    ; //spin
}

void unlock(lock_t* lock) {
  FetchAndAdd(&lock->turn);
}
```

---
## Spinning Issue
- `H/W`의 도움을 받아 구현된 `spin lock`들은 매우 단순하고 잘 작동합니다.
- 하지만, thread가 spinning에 걸리면 <txtred>*전체 time slice를 아무것도 하지 않고 단순히 value checking*</txtred> 만 하게 된다는 점에서 <txtylw>비효율적</txtylw>입니다.
- 여기서 <txtylw>***OS***</txtylw>의 도움이 필요합니다.

---
### Simple Approach: Just yield
- `spin`에 빠질 것 같으면 일찌감치 `CPU`를 포기하는 방식입니다.
  - ***OS*** 가 해당 thread를 *running* 에서 *ready* state로 전환합니다.
  - 그러나, <txtylw>*Context Switch*</txtylw>와 <txtylw>*Starvation*</txtylw> 문제가 여전히 남아있습니다.
    - **포기**하는 순간 어떤 thread 가 cpu 를 선점해야하는지 정하지 않으면, fairness 문제가 발생하기 때문입니다.

---
### Using Queue: Sleeping
- `Queue`를 이용하면, 다음으로 `CPU`를 획득할 thread를 명시할 수 있습니다.
- 아래 두 함수를 구현합니다.
> - park(): 호출한 thread 를 `sleep` 시킵니다.
> - unpark(threadID): `threadID`로 지칭된 thread를 깨웁니다.
- 아래는 두 함수를 이용한 코드입니다.

```cpp
typedef struct __lock_t {
  int flag;
  int guard;
  queue_t *q;
} lock_t;

void lock_init(lock_t *m) {
  m->flag = 0;
  m->guard = 0;
  queue_init(m->q);
}

void lock(lock_t* m) {
  while (TestAndSet(&m->guard, 1) == 1)
    ; // spin 을 돌며 guard lock 을 획득합니다.

  if (m->flag == 0) {
    m->flag = 1;  // lock을 획득합니다.
    m->guard = 0;
  } else {
    queue_add(m->q, gettid()); // 우선 Queue 에 추가합니다.
    m->guard = 0;
    park(); // `sleep` 시킵니다.
  }
}

void unlock(lock_t *m) {
  while (TestAndSet(&m->guard, 1) == 1)
    ; // spin 을 돌며 guard lock 을 획득합니다.
  
  if (queue_empty(m->q))
    m->flag = 0;  // Queue에서 기다리는 thread가 없으므로, Lock 자체를 놓아버립니다.

  else
    unpark(queue_remove(m->q)); // Queue 의 가장 상단에서 기다리는 thread를 깨웁니다.

  m->guard = 0;
}
```

---
### Wakeup/Waiting Race
- 만약 `thread A`가 `Lock`을 갖고 있는 상황에서, `thread B`를 `park()`하려는 순간 ***Context Switch***가 일어났다고 가정해봅니다.
- `thread A`는 정상적으로 `unlock`을 하겠지만, `thread B`가 아직 `Queue`에 들어온 것은 아니기에 `Lock`을 <u>그냥 놓아버립니다</u>.
- 결과적으로, `thread B`는 영원히 잠들어 버릴 수도 있습니다...
- `Solaris OS`에서는 이러한 <txtylw>*곧 sleep이 되는 thread가 있음*</txtylw> 을 처리하기 위한 `setpark()` 시스템 콜을 갖고 있다고 합니다.

---
- `Linux`에서도`Futex`라는 이름으로 비슷한 개념을 구현하고 있습니다.
- 정말 간략히 적고 넘어가면, `futex_wait(address, expected)`함수로 '일단 sleep' 시키고, 두 인자 값이 서로 달라지는 순간 sleep 에서 깨는 구조입니다.
- 또, `futex_wake(address)` 라는 함수로 queue 에서 기다리고 있는 thread 를 지정해서 깨우기도 합니다.

---
### Two-Phase Locks
- `Two-Phase Lock`은 `spinning`을 잘 활용한 방법입니다.
> - *first phase*
>   - `lock`을 얻기까지 spinning 합니다.
>   - 여기서 `lock`을 얻지 못하면, 다음 phase로 넘어갑니다.
> - *second phase*
>   - 호출자가 `sleep`에 들어갑니다.
>   - `lock free`가 되는 순간 `sleep`에서 깹니다.
