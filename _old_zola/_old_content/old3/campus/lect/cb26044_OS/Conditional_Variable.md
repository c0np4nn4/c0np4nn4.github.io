+++
title = "OS: Conditional Variable"
description = "Lock"
date = 2023-06-10
toc = true

[taxonomies]
categories = ["OS"]
tags = ["OS", "Concurrency", "Lock"]

[extra]
math=true
+++

*2023 Spring, PNU, CB26044 (Professor Ahn)*

---

# Conditional Variable
- `thread`를 실행하기 전 일종의 `조건`을 검사하는 방법입니다.
- 예를 들어, <txtylw>parent thread</txtylw>가 <txtylw>child thread</txtylw>의 생성 여부를 확인한 뒤 작업을 이어나가는 상황을 가정해봅니다.

```cpp
void *child(void *arg) {
  printf("child\n");
  return NULL:
}

int main(int argc, char *argv[]) {
  printf("parent: begin\n");

  pthread_t c;
  Pthread_create(&c, NULL, child, NULL);

  printf("parent: end\n");
  return 0;
}
```

- 위 코드를 실행시켰을 때, 기대하는 결과는 아래와 같습니다.

```
parent: begin
child
parent: end
```

---
## Simple Approach: Spin-based
- 정말 단순한 방법은 `spinning`으로 기다리는 것입니다.

```cpp
volatile int done = 0;

void *child(void *arg) {
  printf("child\n");

  // 완료여부를 표시합니다.
  done = 1; 

  return NULL:
}

int main(int argc, char *argv[]) {
  printf("parent: begin\n");

  pthread_t c;
  Pthread_create(&c, NULL, child, NULL);

  // spinning
  while (done == 0);

  printf("parent: end\n");
  return 0;
}
```

- 직관적으로도, 이러한 코드는 <txtred>매우 비효율적</txtred>입니다.

---
## Waiting, Signaling
- 위 코드에서 알 수 있듯이 아래 두 기능이 필요합니다.
- `Waiting`
  - 특정 작업이 수행되지 않기를 바랄 때, 해당 thread 를 잠시 저장해둘 <txtylw>Queue</txtylw>가 필요합니다.
- `Signaling`
  - 만약 변화가 발생해서 <txtylw>Queue</txtylw>에서 기다리던 thread들을 깨울 필요가 있을 때, 이를 적절히 알려주는 방법이 필요합니다.

---
## Definition and Routines
- `conditional variable`은 아래와 같이 선언합니다.
```cpp
pthread_cond_t c;
```
- `operation` 은 `wait`, `signal` 두 가지가 있으며 아래와 같이 사용할 수 있습니다.
```cpp
// wait(), `mutex`를 인자로 갖는 이유는 내부에서 잠시 release 했다가 깨어날 때 다시 acquire 하기 위함
pthread_cond_wait(pthread_cond_t *c, pthread_mutex_t *m); 

// signal()
pthread_cond_signal(pthread_cond_t *c);
```

- 아래 코드는 `spin`이 아닌, `conditional variable`을 이용해 구현한 방법입니다.
```cpp
int done = 0;

pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t c = PTHREAD_COND_INITALIZER;

void thr_exit() {
  Pthread_mutex_lock(&m);

  done = 1;
  Pthread_cond_signal(&c);

  Pthread_mutex_unlock(&m);
}

void *child(void *arg) {
  printf("child\n");

  thr_exit();

  return NULL;
}

void thr_join() {
  Pthread_mutex_lock(&m);

  while (done == 0)
    Pthread_cond_wait(&c, &m);

  Pthread_mutex_unlock(&m);
}

int main(int argc, char *argv[]) {
  printf("parent: begin\n");

  pthread_t p;
  Pthread_create(&p, NULL, child, NULL);

  thr_join();

  printf("parent: end\n");
  return 0;
}
```

- 코드의 흐름은 아래와 같습니다.
> - `Parent`:
>   - `Pthread_creat()`로 *child* 까지 생성합니다.
>   - `thr_join()`을 호출해 *child thread*가 완료될 때까지 기다립니다.
>     - 내부적으로 `Pthread_cond_wait()`를 호출해 기다립니다.
> - `Child`:
>   - "child" 문자열을 출력합니다.
>   - `thr_exit()`를 호출합니다.
> - `Parent`:
>   - `done` 이므로, while 을 탈출합니다.
>   - 정상적으로 종료합니다.

- 만약 `Parent thread`에서 `thr_join()`을 호출하기 직전에 ***Context Switch*** 가 일어나더라도, 전혀 문제가 생기지 않습니다.
  - 이는 `done`이라는 <txtred>상태 변수</txtred>를 잘 두었기 때문입니다.
  - 만약 `done`이 없다면, 특정 `thread`가 <txtred>*영원히 깨어나지 못하는 상황*</txtred>이 발생할 수 있습니다.

---
## Another poor Impl. Issue
```cpp
void thr_exit() {
  done = 1;
  Pthread_cond_signal(&c);
}

void thr_join() {
  if (done == 0)
    Pthread_cond_wait(&c);
}
```

- 여기서는 <txtylw>***Race Condition***</txtylw> 이 발생할 수 있습니다.
- 만약 `Parent`가 `thr_join()`을 호출하고 `wait()`으로 `sleep`상태가 되기 직전에 ***Context Switch***가 발생했다고 해봅니다.
- `Child` 입장에서는 `thr_exit()`에서 `signal()`을 호출해도 깨울 상대가 없습니다.
- `Parent`는 결국 <txtred>영원히 잠들게 됩니다</txtred>..

---
# Producer/Consumer Problem
- `Multi-Threaded Web Server`에서 사용되는 `Producer / Consumer` 패턴이 있습니다.
- 또, 이렇게 데이터를 주고 받는 경우 `Bounded Buffer`라는 개념을 생각해볼 수 있습니다.

---
## Put and Get Routine (Ver.1)
- 아래와 같은 코드가 있다고 해보겠습니다.
```cpp
int buffer;
int count = 0;

void put(int value) {
  assert(count == 0);
  
  count = 1;

  buffer = value;
}

void get() {
  assert(count == 1);
  count = 0;
  return buffer;
}
```

---
## Producer/Consumer Threads(Ver.1)
- 위의 `put and get` 코드를 이용해 다음과 같은 `producer / consumer` 코드를 작성해볼 수 있습니다.
```cpp
void *producer(void *arg) {
  int i;
  int loops = (int) arg;
  for (i = 0; i < loops; i++) {
    put(i);
  }
}

void *consumer(void *arg) {
  int i;
  while (1) {
    int tmp = get();
    printf("%d\n", tmp);
  }
}
```
- `producer()`는 *shared buffer* 에 반복문 index 를 저장합니다.
- `consumer()`는 *shared buffer* 에서 값을 갖고 옵니다.

---
## Producer/Consumer Threads: Single CV
```cpp
cond_t cond;
mutex_t mutex;

void *producer(void *arg) {
  int i;
  for (i = 0; i < loops; i++) {
    Pthread_mutex_lock(&mutex);

    if (count == 1)
      Pthread_cond_wait(&cond, &mutex);

    put(i);

    Pthread_cond_signal(&cond);

    Pthread_mutex_unlock(&mutex);
  }
}

void *consumer(void *arg) {
  int i;
  for (i = 0; i < loops; i++) {
    Pthread_mutex_lock(&mutex);

    if (count == 0)
      Pthread_cond_wait(&cond, &mutex);

    int tmp = get();
    
    Pthread_cond_signal(&cond);

    Pthread_mutex_unlock(&mutex);

    printf("%d\n", tmp);
  }
}
```
- 위 코드는 `Producer` 와 `Consumer`가 각각 하나씩 있을 때는 정상적으로 동작합니다.
- 각자가 `wait()`으로 `sleep`에 빠지더라도, 상대의 `signal`로 깨어날 수 있습니다.

---
- 그러나, 만약 `Producer` 하나와 `Consumer` 둘이 있는 상황을 가정해보겠습니다.
> - <txtylw>Consumer 1</txtylw>은 현재 저장된 데이터가 없음을 확인하고, `sleep` 합니다.
> - <txtylw>Producer</txtylw>는 데이터를 하나 저장하고 `signal`로 <txtylw>Consumer 1</txtylw>을 깨웁니다.
> - 그런데, <txtred>Consumer 2</txtred>가 갑자기 `CPU`를 획득해서 저장된 데이터를 `get()`으로 가져가버립니다.
> - <txtylw>Consumer 1</txtylw>은 결국, `signal()`로 깨웠을 때와는 달리 취할 데이터가 없게 됩니다.

---
- 문제가 발생한 원인은 `Consumer 1`을 깨웠을 때의 상황이 그대로 유지되지 않았기 때문입니다.
- 가장 쉬운 해결 방법은, `if` 를 `while` 로 대체하여 <txtylw>한번 더 count 값을 체크하는 것</txtylw>입니다.

---
- 남은 문제는 `Consumer` 와 `Producer` 가 서로를 깨워야 한다는 부분입니다.
- 현재는 `cond` 라는 변수 하나로만 제어되고 있어, 이것이 불가능합니다.

---
- 따라서, `Producer` 는 `fill` 이라는 변수로 `signal`을 날리고
- `Consumer`는 `empty`라는 변수로 `signal`을 날리는 식의 구현이 가능합니다.
