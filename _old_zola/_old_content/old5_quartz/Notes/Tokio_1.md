---
tags:
  - Rust
  - Tokio
---
# Overview

`Tokio`는 Rust 언어를 위한 *asynchronous runtime* 입니다.
네트워킹 프로그래밍에 활용될 수 있으며, 아래와 같은 주요 component를 제공합니다.
- *asynchronous code* 를 실행하기 위한 multi-threaded runtime
- Standard library의 *asynchronous* 버전

## Project 에서 Tokio의 역할
*asynchronous* 방식으로 코드를 작성하면 동시에 여러 작업을 할 때 드는 cost를 많이 줄일 수 있습니다. *asynchronous code*는 `Tokio` 라이브러리의 *asynchronous runtime* 으로 실행할 수 있습니다. 많은 Standard library들은 "blocking"으로 인해 이러한 비동기 환경에서 실행할 수 없는 경우가 있습니다. 이를 위해 `Tokio`는 비동기 버전의 라이브러리를 제공합니다.

## Tokio의 장점
`Tokio`를 사용했을 때의 장점을 아래와 같이 볼 수 있습니다.
- <mark style="background: #BBFABBA6;">Fast</mark>
	- Rust 언어 자체가 속도가 빠릅니다.
	- Rust 언어에서 제공하는 `async/await`로 concurrent task를 돌릴 수 있으므로, *Scalable* 합니다.
- <mark style="background: #BBFABBA6;">Reliable</mark>
	- Rust 언어의 메모리 관련 버그 방지에 기반합니다.
- <mark style="background: #BBFABBA6;">Easy</mark>
	- Rust 언어의 `async/await`으로 비동기 프로그래밍하기가 쉽습니다.
	- 또, Standard Library와 같은 naming convention을 따르는 *asynchronous* 버전의 library를 제공하기 때문에 개발이 용이합니다.
- <mark style="background: #BBFABBA6;">Flexible</mark>
	- 단순히 `multi-threaded runtime`만을 제공하는 것이 아닌 경량화를 위한 `work-stealing runtime`이나 `single-threaded runtime` 등도 제공합니다.

## Tokio를 사용하지 않아야 할 때
- Parallell programming 이 더 중요할 때
	- `Tokio`는 IO-bound application을 개발할 때 좀 더 적합합니다.
	- 단순히 병렬 프로그래밍이 중요할 때에는 `raryon`을 사용하는 것이 좋습니다.
	- 당연히 이 둘을 적절히 섞어가며 사용해도 좋습니다.
- 단순히 **File Read**작업이 많은 경우에, `Tokio`를 사용하는 것이 그다지 효용이 없습니다.
- *single web request* 를 다루는 경우에도 큰 이점은 없습니다. 이러한 경우는 blocking API를 사용해도 됩니다.

---