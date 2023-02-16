+++
title = "⛓️ [Near] Localnet"
date = 2023-01-09
+++

---

# Localnet

- [[Near Docs](https://docs.near.org/develop/testing/kurtosis-localnet)] 에서 로컬 개발환경 구축을 위한 방법을 찾을 수 있다.

- 문서에서는 [[Kurtosis](https://www.kurtosis.com/)] 에서 제공하는 `Kurtosis NEAR pacakge` 를 이용하는 방법을 설명하고 있다.

- `Kurtosis NEAR pacakge` 는 `Docker Container` 를 이용하여 아래 4 가지 서비스를 로컬 테스트용으로 제공한다.

> - Indexer for Explorer
>
> - NEAR Explorer
>
> - NEAR Wallet
>
> - Local RPC Endpoint

# Setup

- [[Setup](https://docs.near.org/develop/testing/kurtosis-localnet#setup)] 에 나온 과정을 따르면 된다.

- 우선

- `launch` 후 **ACTION** 섹션에서 환경변수 지정과 Alias 지정 등을 할 때는 <u>log 에 있는 것을 입력</u>하면 된다. 

# Testing

- [[Testing](https://docs.near.org/develop/testing/kurtosis-localnet#testing)] 에 나온대로 아래 명령을 입력했을 때 문제가 없으면 된다.

```bash
local_near state test.near
```

