+++
title = "Near Dapp 연습기 (1)"
description = "near dapp"
date = 2023-05-17
toc = true

[taxonomies]
categories = ["Near", "Dapp"]
tags = ["Blockchain", "Rust"]

[extra]
math=true
+++

*Testnet*

```bash
npx create-near-app@latest
```

위 코드를 통해 아주 간단한 니어 Dapp 을 살펴볼 수 있습니다.

---

# Dapp 구조
크게 `frontend` 와 `contract` 로 나누어 살펴보면 됩니다.

---

## Contract
- `Contract` 는 크게 네 부분으로 나누어 이해할 수 있습니다.
> - `Struct 정의`
>   - 컨트랙트에서 사용할 `struct` 를 정의합니다.
>   <br/>
>   <br/>
> - `Impl 정의`
>   - 컨트랙트에서 사용할 `struct` 에 대한 `impl` 들을 정의합니다.
>   <br/>
>   <br/>
> - `Impl(Default) 정의`
>   - 컨트랙트에서 사용할 `struct` 에 대한 `default impl` 들을 정의합니다.
>   <br/>
>   <br/>
> - `test case`
>   - 컨트랙트에 대한 `Test Case` 들을 작성해둡니다.
