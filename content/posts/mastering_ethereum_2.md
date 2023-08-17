+++
title = "Mastering Ethereum 2"
date = "2023-08-17"
+++

---

<img src="https://images.unsplash.com/photo-1620321023374-d1a68fbc720d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1497&q=80" alt="Eth" />

> 본 포스팅은 [Matering Ethereum](https://github.com/ethereumbook/ethereumbook)을 읽고 작성하였습니다.

# ETH 화폐 단위
이더리움의 화폐 단위는 `ETH`라 적고, 이더(ether)라 부릅니다. 화폐 단위에 관해 아래 표와 같이 다양한 단위를 사용할 수도 있습니다.

<center>

| 값(웨이) | 지수 표현 | 이름 |
|--:|:-:|---|
| 1  | $10^0$  | 웨이(wei)  |
| 1,000  | $10^3$  | 배비지(Babbage)  |
| 1,000,000  | $10^6$  | 러브레이스(lovelace)  |
| 1,000,000,000  | $10^9$  | 섀넌(Shannon)  |
| 1,000,000,000,000  | $10^{12}$  | 사보(szabo)  |
| 1,000,000,000,000,000  | $10^{15}$  | 피니(finney)  |
| 1,000,000,000,000,000,000  | $10^{18}$  | 이더(ether)  |
| 1,000,000,000,000,000,000,000  | $10^{21}$  | 그랜드(grand)  |

</center>

# 지갑
이더리움은 하나의 `State Machine` 입니다. 전 세계의 모든 이더리움 노드가 `단 하나의 State`를 가지게 됩니다.
이러한 `State`에 변화를 주는 방법은 [Transaction](https://ethereum.org/en/developers/docs/transactions/)을 통해 송금, 컨트랙트 호출 등을 수행하는 것입니다.
[Transaction](https://ethereum.org/en/developers/docs/transactions/)을 `이더리움 노드`에 성공적으로 전달하기 위해서는 *서명*, *gas 비용 지불* 등의 여러 부가적인 작업이 필요합니다.
이더리움의 여러 [지갑](https://ethereum.org/en/wallets/)은 이러한 작업을 간단하게 처리할 수 있도록 도와주는 일종의 툴입니다.
앞서 언급한 작업들 외에도 이더리움의 여러 [네트워크](https://ethereum.org/en/developers/docs/networks/)를 선택하거나, 사용자의 이더리움 [계정](https://ethereum.org/en/developers/docs/accounts/)정보를 보관해주는 등의 역할을 하기도 합니다.

---

# 월드 컴퓨터
[EVM](https://ethereum.org/en/developers/docs/evm/)은 컴퓨터 상에서 돌아가는 에뮬레이트된 컴퓨터이며, EVM 상에서 실행되는 프로그램이 [스마트 컨트랙트](https://ethereum.org/en/developers/docs/smart-contracts/)입니다.
앞서 살펴본 `ETH`는 마치 기존의 컴퓨터를 사용하기 위해 전기세를 지불하듯, `EVM`의 상태를 변화시키기 위해 활용되는 요금으로 활용될 수 있습니다.

## EOA 및 컨트랙트
이더리움에서 [계정](https://ethereum.org/en/developers/docs/accounts/)이란 두 종류로 구분됩니다.
- 외부 소유 계정 (Externally Owned Account, EOA)
    - `개인키 (private key)`를 가짐
- 컨트랙트 계정 (Contract Account)
    - `개인키 (private key)`가 없음
    - 스마트 컨트랙트 코드를 가짐
    - 코드로 제어됨

컨트랙트도 `계정`이기 때문에 주소(공개키)를 갖습니다.
따라서, 컨트랙트를 `목적지`로 설정하여 트랜잭션을 보내는 것이 가능합니다.
EVM 은 이러한 트랜잭션을 일종의 `입력값`으로 사용하여 컨트랙트를 **실행**합니다.
트랜잭션에는 화폐인 `ETH`외에도 여러 `데이터`값을 함께 담을 수 있습니다.
즉, 컨트랙트 내의 특정 함수를 지정하여 호출할 수 있습니다.

또한, 컨트랙트 계정은 개인키를 갖지 않기 때문에 트랜잭션에 서명할 수 없습니다.
즉, 트랜잭션을 시작(initiate)하는 것이 불가능합니다.
하지만, 다른 컨트랙트 계정을 호출하는 것은 가능하기에 ***컨트랙트 A*** 가 ***컨트랙트 B*** 를 호출하는 형태로 Dapp 을 만들 수도 있습니다.

---

# TODO
`Hardhat`을 이용해 간단한 `Faucet` 컨트랙트를 로컬 네트워크에 배포하고 호출하는 예제를 진행해보려 합니다.
