+++
title = "Nodejs"
description = "Backend study"
date = 2023-08-02

[taxonomies]
categories = ["Nodejs", "Backend"]
tags = ["Dev"]

[extra]
math=true
+++

# Nodejs
- `Node.js`는 [공식 사이트](https://nodejs.org/ko/)에서 소개하듯, 아래의 특징들을 갖습니다.
> - 크롬 v8 엔진으로 빌드된 <txtred>Javascript Runtime</txtred>
> - <txtylw>Event-driven</txtylw>, <txtylw>Non-blocking I/O Model</txtylw>
> - <txtylw>npm</txtylw> 을 통해 오픈 소스 라이브러리 생태계에 참여할 수 있음

---

## Server
- `Server`란 <txtylw>Request</txtylw> 에 대한 <txtylw>Response</txtylw>를 제공하는 컴퓨팅을 의미합니다.

<img src="../../../images/nodejs_server.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

- 필요한 경우, `Server`가 다른 `Server`로 추가적인 <txtylw>Request</txtylw>를 보낼 수도 있습니다.

---

## Javascript Runtime
- <txtred>Runtime</txtred>은 아래와 같이 정의할 수 있습니다.
> ***특정 언어로 만든 프로그램을 실행할 수 있는 환경***
- 기존의 `Javascript`는 인터넷 브라우저 위에서만 실행할 수 있었습니다.
- 하지만, 구글이 <txtylw>V8</txtylw>엔진을 사용한 ***크롬*** 을 2008년 출시하고 이를 오픈소스로 공개한 이후, `Ryan Dahl`이 <u><txtylw>V8</txtylw> 엔진과 <txtylw>libuv</txtylw>라는 라이브러리를 기반으로 진행한 프로젝트</u>가 바로 <txtred>Node.js</txtred>입니다.
- <txtylw>V8</txtylw>과 <txtylw>libuv</txtylw>는 `C`와 `C++`로 구현되어 있습니다.
> - <txtylw>V8</txtylw>엔진
>     - `Javascript` 프로그램의 <u><txtylw>실행 속도</txtylw></u>를 매우 높임
> - <txtylw>libuv</txtylw>라이브러리
>     - <txtred>Node.js</txtred>의 특징인 <txtylw>Event-driven</txtylw>, <txtylw>Non-blocking I/O Model</txtylw>을 구현

<img src="../../../images/nodejs_inner_structure.png" width="500rem" alt="adsf" style="border: 2px solid white"/>

- 위 그림과 같이, `C`, `C++`로 작성된 코드와 `Node.js`의 코어 라이브러리 사이에 적절한 Binding 을 두는 식으로 내부 구조가 구성되어있습니다.

---

## Event-driven
- <txtylw>Event-driven</txtylw>이란 아래와 같이 정의할 수 있습니다.
> <txtylw>Event</txtylw>가 발생할 때, <u>*미리 지정해 둔 <txtred>작업</txtred>*</u> 을 수행
- 위에서의 <txtred>작업</txtred>을 <txtylw>Callback</txtylw>함수라 부릅니다.

<img src="../../../images/nodejs_callback.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

- 위 그림의 과정을 간략히 설명하면 아래와 같습니다.
> 1. <txtylw>Event Listener</txtylw>에 <txtylw>Callback</txtylw>함수를 등록한다.
> 2. <txtylw>Event</txtylw>가 발생한다.
> 3. 등록된 <txtylw>Callback</txtylw>를 호출한다.

### Call Stack

- 일반적으로 아래와 같이 함수를 호출했을 때의 <txtylw>Call Stack</txtylw>은 아래와 같습니다.
```js
function one() {
    two();
}
function two() {
    three();
}
function three() {
    console.log("Hello World!");
}

one();
```

<img src="../../../images/nodejs_callstack.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

---
### Event Loop
- 그런데 만약 아래와 같이 `setTimeout`을 사용하는 경우는 <txtylw>Call stack</txtylw>만으로는 프로그램을 설명하기가 어렵습니다.
- 이는, <u>setTimeout 의 콜백함수가 언제 <txtylw>Call Stack</txtylw>으로 들어가는지 알기 어렵기 때문</u>입니다.
```js
function foo() {
    console.log("Hello World");
}
console.log("1111");
setTimeout(foo, 3000);
console.log("2222");
```

- 실행 결과는 아래와 같습니다.
```
1111
2222
Hello World
```

- 동작을 이해하기 위해 다음 세 개념을 이해할 필요가 있습니다.
> - <txtylw>Event Loop</txtylw>
>     - <txtylw>Call back</txtylw>함수 관리
>     - <txtylw>Call back</txtylw>함수 실행 순서 관리
> - <txtylw>Task Queue</txtylw>
>     - 실행 될 <txtylw>Call back</txtylw>함수들이 대기하는 큐
> - <txtylw>Background</txtylw>
>     - Timer, I/O 또는 Event Listener 들이 대기하는 곳

