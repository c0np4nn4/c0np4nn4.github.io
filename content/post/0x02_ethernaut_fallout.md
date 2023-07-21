+++
title = "0x02. Fallout"
description = "Ethernaut"
date = 2023-07-18
toc = true

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "Ethereum"]

[extra]
math=true
+++

---

# 💣 취약점
- 문제가 되는 부분은 `constructor`로 주석이 달린 부분입니다.

```solidity
/* constructor */
function Fal1out() public payable {
    owner = payable(msg.sender); // Type issues must be payable address
    allocations[owner] = msg.value;
}
```

- `constructor` 가 아니라, 단순한 `public function`이므로 누구나 호출하고 `owner`가 될 수 있습니다.

---

- [Solidity Docs, Constructor](https://docs.soliditylang.org/en/v0.8.13/contracts.html#constructors) 에서 확인할 수 있듯이, `0.5.0` 이후에는 아래와 같이 <txtylw>constructor</txtylw>를 별도의 키워드로 생성해야 합니다.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

abstract contract A {
    uint public a;

    constructor(uint a_) {
        a = a_;
    }
}

contract B is A(1) {
    constructor() {}
}
```
---
