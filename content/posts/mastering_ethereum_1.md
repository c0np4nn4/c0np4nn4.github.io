+++
title = "Mastering Ethereum 1"
date = "2023-08-15"
+++

---

<img src="https://images.unsplash.com/photo-1620321023374-d1a68fbc720d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1497&q=80" alt="Eth" />

> 본 포스팅은 [Matering Ethereum](https://github.com/ethereumbook/ethereumbook)을 읽고 작성하였습니다.

# 이더리움이란 무엇인가?
이더리움은 ***스마트 컨트랙트*** 프로그램을 실행하는, 오픈 소스에 기반을 둔, 전 세계에 걸쳐 탈중앙화된 computing infrastructure 라고 정의할 수 있습니다.

# 비트코인과의 비교
이더리움과 [비트코인](https://en.wikipedia.org/wiki/Bitcoin)이 갖는 가장 큰 차이점은 `Utility` 라 할 수 있습니다.
이더리움은 `Ethereum Virtual Machine(EVM)` 을 구현하고 있는데, 이 위에 Solidity와 같은 Turing Complete 언어로 프로그램을 만들고 실행할 수 있습니다.
즉, 이더리움은 단순히 `디지털 화폐`로서 활용되는 비트코인과 달리, 프로그램 실행을 위해 플랫폼 사용료로써 화폐인 `ETH`를 지출하도록 합니다.

# 블록체인의 구성요소
일반적으로 `Public Blockchain` 은 아래의 구성요소들을 가집니다.
- `P2P Network`
- `Transaction`
- `Consensus`
- `VM`
- `Ledger`
- `Incentive Mechanism`
위의 기술들과 생략된 세부적인 기술들을 모두 묶어서 하나의 `Client`로 개발하면 **블록체인 코어**로써 기술 구현이 됩니다.
이더리움에서는 [Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)를 통해 수학적 기술로 구현을 소개하기도 합니다.

# 이더리움의 탄생
이더리움 탄생하기 전, 사람들은 비트코인 모델을 기반으로 한 `Application(응용 프로그램)` 개발에 대해 관심을 갖기 시작했습니다.
이는 두 가지 방법으로 시도해볼 수 있는데, 각각은 <u>비트코인 위에서 개발</u>을 하거나 <u>완전히 새로운 프로토콜을 만들어내는 방법</u>입니다.
`비트코인 위에서 개발한다는 것`은 다시 말해 비트코인 규격(트랜잭션 타입, 데이터 타입, 데이터 스토리지 크기 등) 내에서 제한적으로 개발한다는 의미입니다.
이는 곧 여러가지 `off-chain` 계층을 필요로 했으며, *Public Blockchain을 사용하는 장점* 을 약화시키는 결과로 이어졌습니다.
따라서, 두 번째 선택지인 `새로운 프로토콜을 만드는 것`을 선택해야 하는 이유가 되었습니다.
하지만 이는 P2P를 위한 `네트워크`, Ledger를 위한 `DB`, 프로그램 실행을 위한 `VM` 등 모든 인프라 기술들을 구현하고 모두 잘 동작하도록 검증하는 등 상당히 난이도가 높은 작업입니다.

2013년 말을 시작으로, [비탈릭 부테린](https://vitalik.ca/)이 새로운 프로토콜인 **이더리움**에 대한 아이디어와 개발을 시작하고 [개빈 우드](https://github.com/gavofyork)의 합류로 결국 **이더리움**이 탄생하게 됩니다.
2015년 7월 30일에 **첫 번째 이더리움 블록**이 채굴되었고, 탈중앙화 블록체인 응용 프로그램들을 위한 Public Blockchain 이 그 시작을 알렸습니다.

# 이더리움 개발 4단계
이더리움의 개발 `네 단계`로 진행되었으며, 중간 중간 주요 변경사항이 있을 때 `하드 포크`를 통해 이전 버전과 호환되지 않는 업데이트를 해왔습니다.
현재까지의 `개발 단계` 및 중간 `하드 포크`는 아래와 같습니다.
- 블록 `#0`
    - **프론티어(Frontier)**: *2015년 7월 30일* 부터 *2016년 3월* 까지 지속된 이더리움의 `초기 단계`
- 블록 `#200,000`
    - **아이스 에이지(Ice Age)**: `난이도`가 기하급수적으로 증가하게끔 수정하여 `PoS`로의 전환에 동기 부여를 한 `하드 포크`
- 블록 `#1,150,000`
    - **홈스테드(Homestead)**: *2016년 3월* 에 시작된 이더리움의 `두 번째 단계`
- 블록 `#1,192,000`
    - **DAO**: DAO 사건의 피해자들에게 보상금을 지급하고, `이더리움 클래식`으로 분기된 `하드 포크`
- 블록 `#2,463,000`
    - **탠저린 휘슬(Tangerine Whistle)**: 특정 I/O가 많은 작업에 대한 가스 계산을 변경하고, 해당 작업의 가스 비용이 낮은 서비스 거부(DoS)공격으로부터 축적된 상태를 지우는 `하드 포크`
- 블록 `#2,675,000`
    - **스퓨리어스 드래곤(Spurious Dragon)**: 더 많은 DoS 공격 벡터를 처리하고 다른 상태를 지우는 `하드포크`
- 블록 `#4,370,000`
    - **매트로폴리스 비잔티움(Metropolis Byzantium)**: `메트로폴리스`는 이더리움의 `세 번째 단계`로, *2017년 10월* 에 `하드 포크`. `비잔티움`은 `메트로폴리스`를 위해 계획된 2개의 `하드 포크` 중 `첫 번째`

# 범용 블록체인
비트코인의 블록체인은 `비트코인 단위`와 `소유 상태`를 추적 할수 있는 `State Machine`이라 할 수 있습니다.
비트코인의 `트랜잭션(Transaction)`들은 이러한 `State Machine`에 `State Transition`을 일으키는 데이터라 할 수 있습니다.
이더리움의 경우, 이러한 Transaction이 비단 **화폐(ETH)** 에 관한 State 뿐만 아니라 보다 범용적인 관점에서의 **키-밸류 튜플**에 대해서도 State Transition 을 일으킵니다.
즉, 이더리움은 `범용 데이터 저장소`를 갖고 있으며 이는 일반적인 현대 컴퓨터의 `RAM`과 비슷하게 생각할 수 있습니다.
다만 블록체인이 갖는 두 가지 특성인 *State Transition에 화폐가 소비된다* 와 *모든 client 들이 동일한 데이터를 전파 받고 관리한다* 는 차이점이 있습니다.

---

# 이더리움의 구성요소 
## P2P Network
이더리움은 TCP 포트 30303으로 접속 가능한 이더리움 메인 네트워크(Ethereum Main Network)에서 실행되며, `DEVP2P`라는 프로토콜을 실행합니다.
## Consensus Rule
이더리움의 `합의 규칙(Consensus Rule)`는 [Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)에 기술된 대로 구현됩니다.
## Consensus Algorithm
현재는 `PoS`기반으로 동작하는 *가중 투표 시스템* 인 [Casper](https://academy.binance.com/en/articles/ethereum-casper-explained)로 전환된 것으로 알려졌습니다.
## Transaction
이더리움의 `트랜잭션(Transaction, Tx)`은 *보낸 사람*, *받는 사람*, *값*, *데이터 페이로드* 등이 포함된 `네트워크 메세지`입니다.
## State Machine
이더리움의 `State Transition`은 `EVM`이 `bytecode`를 처리하면서 일어납니다.
`Solidity`라는 High Level Language 로 작성된 소스 코드를 컴파일하면 `bytecode`가 만들어집니다.
## Data Structure
이더리움의 `State`는 `Merkle Patricia Tree`라 불리는 자료 구조로 관리되며, 각 노드의 로컬 DB 에 저장됩니다.
## Economic Security
`PoS`를 통해 보안성을 갖습니다.

---

# 이더리움과 튜링 완전
영국의 수학자 [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing)은 **보편적 계산 가능성(Universal Computability)** 에 대한 수학적 기초에 대해 고안했습니다.
보편적 계산 가능성을 쉽게 설명하자면 **모든 문제를 해결할 수 있다** 는 의미입니다.
튜링은 `정지 문제` 가 해결되지 않는 문제임을 증명했고, 이러한 문제를 해결하는 장치를 [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine) 이라 정의했습니다.

[튜링 완전](https://en.wikipedia.org/wiki/Turing_completeness)이란 튜링이 고안한 [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine)과 동일한 계산 능력으로 문제를 풀 수 있음을 의미합니다.
즉, 여건(저장 메모리 크기, 전력 문제 등)만 된다면 **무한히 연산할 수 있는**장치임을 의미합니다.

그러나 [튜링 완전](https://en.wikipedia.org/wiki/Turing_completeness)은 개방형 시스템에서는 쉽게 공격을 받을 수 있고, 만약 `정지 문제`를 일으키는 공격을 받을 경우 이를 복구하는 것이 중요해집니다.
블록체인의 경우 공격에 대해 단순히 노드를 **껐다가 다시 키는 작업**을 할 수 없으므로, 이에 잘 대비하는 것이 중요합니다.

# 튜링 완전의 함축적 의미
[Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing)은 또한, 컴퓨터에서 프로그램을 시뮬레이션할 때 종료 여부를 정확히 예측할 수 없음을 증명했습니다.
다시 말해 프로그램을 실행한 후 무슨 일들이 일어나는지, 언제쯤 프로세스가 종료되는지는 실제로 실행하지 않고서는 경로를 예측할 수 없습니다.
따라서, 스마트 컨트랙트도 소위 **무한 루프**에 빠질 수 있는 위험성이 존재합니다.

이더리움은 `가스(gas)`라는 **과금 메커니즘**을 도입하여 이를 해결했습니다.
`EVM`이 스마트 컨트랙트를 실행하게 되면, `bytecode`의 각 명령어에 대해 `EVM`이 비용을 계산합니다.
`EVM`으로 보내진 `Tx`에는 **최대 가스량**에 대한 정보가 들어있어야 하며, 이를 초과하는 프로그램이 온 경우 실행을 중지하게 됩니다.

# Dapp
다양한 용도로 프로그래밍할 수 있도록 만들어진 `범용 블록체인`인 이더리움 상에서 가장 활발히 개발된 응용프로그램은 단연 [Dapp](https://www.coindesk.com/learn/what-is-a-dapp-decentralized-apps-explained/) 입니다.
`Dapp`은 웹 프론트엔드로 개발된 `UI`와 블록체인 `스마트 컨트랙트`를 최소 구성 요소로 갖습니다.

# Web3
개빈 우드가 제안한 용어로, `탈중앙화 P2P Network`라는 기존의 웹 애플리케이션과 다른 개념을 '버전'으로써 일컫고자 사용하는 용어입니다.

# 이더리움의 개발 문화
'빨리 움직이고 파괴하라' 라는 한 문장으로 많은 것이 설명되는 것 같습니다.
스마트 컨트랙트는 '업그레이드'라는 개념이 존재하지 않습니다.
새로운 기술들이 등장했을 때, 기존의 것에 얽매이지 않고 빠르게 나아가는 자세로 개발에 임하는 것이 중요한 것 같습니다.

---

# TL;DR
이번 장에서는 원론적인 이야기와 이더리움 전반에 관한 내용을 간략히 다루었습니다.
이더리움의 구체적인 스펙에 관해서는 다음 장부터 소개되는 것으로 보입니다.

