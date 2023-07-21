+++
title = "0x03. CoinFlip"
description = "Ethernaut"
date = 2023-07-22
toc = true

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "Ethereum"]

[extra]
math=true
+++

---

# 💣 취약점
- `Solidity` 상에서 <txtylw>Randomness</txtylw>를 구현하는 것이 어려움을 보여주는 문제입니다.
- 일반적으로 프로그램들의 Random 은 ***Pseudo-Random*** 로 구현하게 되는데, 이는 <u>`input`값을 알아내기 힘들다</u>는 사실에 의존합니다.
- 즉, Blockchain 상에서는 이 `input`값을 알아내기가 <txtred>어렵지 않다</txtred>는 점 때문에 <txtylw>Random</txtylw>을 구현하기 힘듭니다.
---

# 📄 코드 분석
- 이번 문제는 아래 `flip` 함수를 잘 분석하면 풀 수 있습니다. 

```solidity
    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number.sub(1)));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue.div(FACTOR);
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
```
- 코드를 자세히 살펴보면 아래와 같습니다.

> - 우선 `blockValue`는 <txtylw>이전 블록</txtylw>의 <txtylw>hash 값</txtylw> 을 갖습니다.
> ```solidity
> uint256 blockValue = uint256(blockhash(block.number.sub(1)));
> ```

> - 그 다음에 오는 `if` 코드는, `flip` 함수 호출을 위한 transaction이 <u>다음 블록에 속해있어야 함</u>을 명시합니다.
> ```solidity
> if (lastHash == blockValue) {
>     revert();
> }
> ```

> - `lastHash` 값을 업데이트 해둡니다.
> ```solidity
> lastHash = blockValue;
> ```

> - `blockValue`의 값을 정해진 `FACTOR` 상수 값으로 나눈 몫은 **1** 또는 **0** 입니다.
> - 이 값을 `side` 변수에 <txtylw>bool</txtylw> 형태로 저장하는 코드입니다.
> ```solidity
> uint256 coinFlip = blockValue.div(FACTOR);
> bool side = coinFlip == 1 ? true : false;
> ```

> - 위에서 저장한 `side`값과, `flip()`함수 호출 시 전달한 인자 `_guess`값을 비교하여 `consecutiveWins` 값을 업데이트 합니다.
> ```solidity
> if (side == _guess) {
>     consecutiveWins++;
>     return true;
> } else {
>     consecutiveWins = 0;
>     return false;
> }
> ```

---
## 🎇 How to Solve
- 이번 문제를 해결하면서, `Interface` 를 이용한 방법을 익힐 수 있었습니다.
- 기존에는 `<prob>.t.sol` 파일을 작성하고, 오직 <txtylw>*문제 contract*</txtylw> 의 객체만 생성했었습니다.
- 이번에는 `<prob>Hack.sol` 파일을 작성하고, `<prob>.sol` contract 의 주소를 토대로 <txtylw>Interface</txtylw>를 이용했습니다.
    - 즉, `<prob>Hack`을 통해서 <txtylw>*임의로 작성한 프로그램 실행 후, `flip()`호출이 가능*</txtylw> 했습니다.
- 물론 `<prob>.t.sol` 내에서 해결할 수도 있지만, 이런 식으로 <txtylw>다른 contract 를 작성하여 해결하는 방식</txtylw>도 유용하게 쓰일 것 같습니다.

> - 아래는 `Level Attack`에 해당하는 코드입니다.
> ```solidity
> // block height 를 3으로 조정
> vm.roll(3);
> 
> // CoinFlipHack contract의 constructor는 CoinFlip contract의 주소를 인자로 받음
> // 해당 인자를 이용해 interface 연결
> CoinFlipHack coinFlipHack = new CoinFlipHack(levelAddress);
> 
> for (uint256 i = 0; i <= 10; i++) {
>     // block height 를 3 + i 로 조정
>     vm.roll(3 + i);
> 
>     // coinFlipHack 에 정의된 attack() 함수를 호출
>     // attack() 함수는 내부적으로 coinFlip contract의 flip()을 호출하도록 interface로 연결됨
>     coinFlipHack.attack();
> }
> ```
