+++
title = "Ethernaut 로컬환경"
description = "Ethernaut: Local Env Setting"
date = 2023-05-09
toc = true

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "ethernaut", "Ethereum"]

[extra]
math=true
+++

[이전 포스트](@/post/ethernaut_local_env.md)에 Ethernaut 로컬환경 구축에 관해 작성해두긴 했었는데, 정리가 잘 되어있지 않은것 같아서 누구든 보고 따라하기만 하면 구축할 수 있을 튜토리얼 느낌으로 다시 작성해두려 합니다.

우선 저는 현재 **OS**로 `Ubuntu 22.04.02 LTS` 를 사용하고 있으며, 이를 기준으로 작성해두려 함을 미리 말씀드립니다.

---

# 깃헙 레포
우선은 [[깃헙 레포](https://github.com/OpenZeppelin/ethernaut)]로 향합니다.

<img src="../../../images/post/ethernaut/ethernaut_local_1.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

`Code` 버튼을 누른 뒤, `SSH` 탭에서 아래 내용을 복사합니다.

<img src="../../../images/post/ethernaut/ethernaut_local_2.png" width="300rem" alt="adsf" style="border: 2px solid white"/>

터미널 상에서 아래와 같이 입력하여 레포를 클론해옵니다.

```bash
git clone git@github.com:OpenZeppelin/ethernaut.git
```

아래와 같이 클론이 잘 된 것을 확인할 수 있습니다.

<img src="../../../images/post/ethernaut/ethernaut_local_3.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

---

# 빌드
로컬환경을 구축하는 순서는 아래와 같습니다.

> - `yarn install`
>   - 필요한 *dependecy* 를 설치합니다.

> - `yarn network`
>   - 로컬넷을 구동합니다.
>   - `hardhat` 을 이용하며, 20개의 가상계좌를 이용할 수 있습니다.

> - `yarn compile:contracts`
>   - `ethernaut` 에서 제공하는 스마트 컨트랙트를 컴파일합니다.

> - `yarn deploy:contracts`
>   - 로컬넷에 `ethernaut` 컨트랙트들을 배포합니다.

> - `yarn start:ethernaut`
>   - `ethernaut` 웹페이지를 띄웁니다.

---


일단은 `nvm`을 이용해 `node.js` 버전을 *16.18.0* 으로 지정해 두었습니다. 정확한 이유는 모르겠으나, 원래 적용된 18 이상의 버전으로 진행하다보니 막히는 부분이 있었습니다..;;

이어서 `yarn install` 로 dependency 들을 설치합니다.

```bash
nvm use 16.18.0
yarn install
```

<img src="../../../images/post/ethernaut/ethernaut_local_4.png" width="400rem" alt="adsf" style="border: 2px solid white"/>

---

다음으로는 `Ethernaut` 에서 제공하는 스마트 컨트랙트를 컴파일합니다.

```bash
yarn compile:contracts
```

<img src="../../../images/post/ethernaut/ethernaut_local_5.png" width="500rem" alt="adsf" style="border: 2px solid white"/>

---

이제 로컬넷을 띄울 차례입니다. 

```bash
yarn network
```

<img src="../../../images/post/ethernaut/ethernaut_local_6.png" width="600rem" alt="adsf" style="border: 2px solid white"/>

---

가상계좌 중 하나(첫번째 계정)의 ***Private Key*** 를 이용해서 `Metamask` 에 추가해둡니다. 자세한 내용은 [[metamask: how to import an account](https://support.metamask.io/hc/en-us/articles/360015489331-How-to-import-an-account)] 를 참고하시면 되겠습니다.

컴파일된 스마트 컨트랙트를 로컬넷에 배포하기 위해 `yarn deploy:contracts`를 합니다.

```bash
yarn deploy:contracts
```

<img src="../../../images/post/ethernaut/ethernaut_local_7.png" width="800rem" alt="adsf" style="border: 2px solid white"/>

---

이제 새로운 터미널을 열어서 `yarn start:ethernaut` 을 입력하면 *react* 로 만들어진 웹페이지를 확인할 수 있습니다.

```bash
yarn start:ethernaut
```

<img src="../../../images/post/ethernaut/ethernaut_local_8.png" width="800rem" alt="adsf" style="border: 2px solid white"/>

---

로컬에서 돌아가는 네트워크로 트랜잭션을 보내기 때문에 쾌적환 환경에서 `Ethernaut` 문제를 풀 수 있습니다!
