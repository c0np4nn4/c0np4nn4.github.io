+++
title = "King"
date = "2023-08-27"
+++


---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> `King-ship`을 통해 소유권(*ownership*)을 다룰 때 주의할 점을 살펴보는 문제입니다.

<center>
<img alt="main image" 
src="https://images.unsplash.com/photo-1578925518470-4def7a0f08bb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2071&q=80" />
</center>

---

# Introduction
앞서 살펴본 `fallback`, `receive` 등의 개념과 스마트 컨트랙트에서 자주 사용하는 `Ownership`의 개념이 모두 적용된 문제입니다.

[이더리움의 계정](https://ethereum.org/en/developers/docs/accounts/#types-of-account)은 크게 두 가지로 나뉩니다.
하나는 `EOA(Externally Owned Account)`로 *priavte Key* 를 갖는 계정이고, 다른 하나는 `Contract Account` 입니다.
앞선 문제([fallback](@/posts/ethernaut_1.md))에서 살펴봤듯이 `Contract Account`는 `payable fallback` 또는 `receive`의 함수를 정의해야 `ETH`를 전달받을 수 있습니다.
그렇다면, `EOA` 는 어떻게 `ETH`를 받을까요? 
`EOA`는 `Contract Account`와 달리 코드를 실행할 수 없습니다.
거래를 할 때 어떤 동작도 트리거하지 않습니다.
따라서, 항상 이더를 받을 준비가 되어 있습니다.
이는 마치 `Contract Account`가 `payable fallback`함수를 구현하고 있는 것과 같습니다.

스마트 컨트랙트에서 ***Ownership*** 은 중요한 개념입니다.
가장 직관적으로는 `withdraw()`와 같은 함수를 이용하여, 오직 컨트랙트 소유자만이 자금을 회수하도록 할 수 있기 때문입니다.
그러므로, ***Ownership*** 과 관련된 부분, 특히 `상태 변수`에서의 변경은 보안적으로 견고하게 설계되어야 합니다.

---

# Problem Detail

## Goal
이번 문제의 목표는 조금 유심히 봐야합니다.

<center>
<img alt="goal" src="../../ethernaut_img/9_king_1.png" />
</center>

우선 `result`가 **false** 를 가져야 합니다.
그러기 위해선, `King` 컨트랙트로의 `ETH 전송`이 제대로 이뤄지지 않아야 합니다.

또, `King` 컨트랙트 내부의 `king`이라는 상태 변수는 처음 배포될 때 `Factory`의 주소로 지정([#](https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/King.sol#L12))되었습니다.
위 코드에서 `address(this)` 는 `Factory`의 주소를 의미합니다.
그러므로, `king`값에 변경이 있었어야 함을 알 수 있습니다.

정리하면, 아래 두 목표를 달성해야 합니다.
> (1) `ETH` 전송에 실패
>
> (2) `king`을 한번 변경해야함 (*king-ship*)

## Contract
`King` 컨트랙트는 아래와 같습니다.

<center>
<img alt="king contract" src="../../ethernaut_img/9_king_2.png" />
</center>

우선 `constructor()`에서 상태 변수(*king*, *prize*, *owner*) 값을 모두 초기화 하고 있습니다.

그 다음의 `receive()` 함수를 주목해야 합니다.
우선 *require()* 로 아래 두 가지를 체크합니다.
> 현재 보낸 `ETH` (*msg.value*)가 이전에 저장한 값 (*prize*) 보다 크거나 같은가
> 
> 처음 `King`을 배포했던 계정(`Factory`)으로 `receive`를 호출한 것인가

이후 `king`을 `msg.sender`로 변경합니다.
즉, *king-ship* 의 변경이 일어납니다.
`prize`에는 이번 호출에 전달받은 `ETH`양을 기록합니다.

`King` 컨트랙트가 의도한 사용 Flow 는 아마 아래와 같을 것으로 보입니다.

<center>
<img alt="king contract overview" src="../../ethernaut_img/9_king_3.png" />
</center>

*Alice* 계정은 `0.1 ETH` 를 냄으로써 `king` 권한을 획득합니다.
이후, *Bob* 계정이 `0.1 ETH`보다 큰 `0.2 ETH`를 내면, *Alice* 계정에게 `0.2 ETH`가 보내지고 `king` 권한은 *Bob* 이 획득하게 됩니다.


## King-ship
흔히 `Ownership`이라 하여, OpenZeppelin 에서 제공하는 [Ownable.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol)를 이용해
컨트랙트의 소유권을 명시하는 경우가 많습니다.
이렇게 하는 이유는 `Owner`라는 소유자를 명시할 경우 **관리 및 제어**가 용이하기 때문입니다.
예를 들어, *NFT* 프로젝트의 경우 *NFT* 를 발행하고 판매 수익을 가져가는 주체가 바로 `Owner` 일 것입니다.

이번 문제에서는 이러한 소유권에 관한 정보를 `king` 이라는 상태 변수로 관리하는 것을 보여줍니다.
`receive()` 함수에서 확인할 수 있듯이, 새로 들어온 돈은 `king` 에게 전달됩니다.
그리고, `king` 이 변경되면 새로운 주체가 다시 돈을 받을 수 있는 **권한**을 갖게 되는 것입니다.

---

# Exploit
## Strategy
앞서 살펴본 ***Goal*** 을 다시 해석해보면 아래와 같습니다.
> 이전에 `king`이 한번 변경되었어야 하고, `.transfer()`를 통한 `ETH` 전송에 실패해야 한다.

문제 풀이를 위해 조금 더 의미를 부여해서 적어보면 아래와 같습니다.
> 1. 우선 임의의 `Contract`인 `MaliciousKing` 으로 `king`을 변경한다.
>
> 2. `MaliciousKing`은 `payable fallback` 또는 `receive` 함수 호출 시, 무조건 ***revert()*** 를 일으켜서 `ETH` 전송을 실패하도록 강제한다.

이렇게 생각해볼 수 있는 이유는 앞서 살펴본대로, `EOA` 계정이 항상 `ETH` 전송을 받는데 성공하기 때문입니다.
따라서, 임의의 `Contract`를 배포하여 해당 `Contract`를 `king`으로 만들어주면 그 부분을 공략할 수 있습니다.
그러므로, 문제 풀이를 위한 단계를 아래와 같이 세워 볼 수 있습니다.
> 1. `MaliciousKing` 컨트랙트를 배포합니다.
>
> 2. `MaliciousKing`이 `king` 권한을 획득합니다.
>
> 3. 임의의 계정(`EOA` 또는 `Contract Account`)이 `King` 컨트랙트에 `ETH`를 전송합니다.
>
> 4. `MaliciousKing`에게 `payable(king).trasfer(msg.value)`로 `ETH`를 전송하는 것에 실패합니다.
>
> 5. `ETH` 전송 직후에 있는 `king = msg.sender`를 수행할 수 없으므로, 계속해서 `MaliciousKing`이 `king`인 상태가 유지됩니다.

이를 도식화하면 아래와 같습니다.

<center>
<img alt="strategy overview" src="../../ethernaut_img/9_king_4.png" />
</center>


## Code
### MaliciousKing
우선 문제 풀이를 위해 작성되어 배포된 `MaliciousKing` 컨트랙트 입니다.

<center>
<img alt="malicious king" src="../../ethernaut_img/9_king_5.png" />
</center>

컨트랙트가 배포된 후, `attack()` 함수를 통해 `king` 권한을 획득합니다.

### exploit.js
문제 해결을 위해 작성된 Javascript 코드 입니다.

<center>
<img alt="exploit" src="../../ethernaut_img/9_king_6.png" />
</center>

## Result
적절히 로그와 함께 확인한 실행 화면은 아래와 같습니다.

<center>
<img alt="result" src="../../ethernaut_img/9_king_7.png" />
</center>

---

# Conclusion
`Ownership`은 컨트랙트 작성 시 매우 주의해야 하는 부분임을 확인했습니다.
특히, `Ownership`의 변경이 일어나는 부분은 잘 설계해야 합니다.
`King`문제에서 일어나는 문제는 **버그**로 인하여 `King-ship`을 누군가 독점하는 것이 가능하다는 점입니다.

또한, ***다른 컨트랙트로 `ETH`를 송금*** 할 경우, <u>전적으로</u> 상대의 `fallback` 함수에 의해 성패가 달려있음을 주지시켜주는 문제이기도 합니다.
따라서, 이를 해결하기 위한 방법으로 아래와 같은 방법을 생각해볼 수 있습니다.
> ***환불*** 매커니즘을 구현합니다. (*[KingRefund.sol](https://github.com/c0np4nn4/EtherStudy/blob/main/ethernaut_solution/contracts/prob/King/KingRefund.sol)*)
> 
> - `king` 권한을 잃은 계정은 컨트랙트로부터 돈을 환불받는 방식입니다. 
> - *Tx 전송 실패* 등이 일어나지 않도록하는 구조입니다.

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
