+++
title = "Ethernaut Introduction"
date = "2023-08-15"
+++

# Ethernaut
`Ethernaut`은 `EVM` 에서 발생할 수 있는 `Solidity 스마트 컨트랙트`의 취약점을 공부할 수 있는 wargame 입니다.
원활한 공부를 위해 로컬에서 환경을 구축하고 `hardhat`을 이용해 문제를 풀 수 있습니다.

# 환경 구축
우선 [Ethernaut Github](https://github.com/OpenZeppelin/ethernaut) 레포를 clone 해옵니다.
```bash
git clone https://github.com/OpenZeppelin/ethernaut.git
```

clone 해왔다면, `yarn`을 입력해서 필요한 모듈들을 준비합니다.
```bash
yarn
```

이후 `client > src > constants.js`에 가서 `ACTIVE_NETWORK` 를 `NETWORKS.LOCAL` 로 수정합니다.
<img alt="constants.js" src="https://github.com/c0np4nn4/c0np4nn4.github.io/assets/49471288/a49975c3-2098-4e03-aca8-f9fb8c653d11" />

터미널을 하나 더 실행해서 아래 명령어들을 차례로 실행하면 됩니다.
```bash
# 터미널 1
yarn network # 로컬 이더리움 노드를 하나 실행

# 터미널 2
yarn deploy:contracts # 모든 Solidity 스마트 컨트랙트를 컴파일하고 로컬 네트워크에 배포
yarn start:ethernaut # Ethernaut 웹 페이지
```

<img alt="network" src="https://github.com/c0np4nn4/c0np4nn4.github.io/assets/49471288/a367f442-02b3-4d37-9a13-fbc1fc9aa880" />

로컬 노드에서 제공하는 비밀키 중 한 개를 Metamask 에 추가하고, 로컬 네트워크 정보도 추가하면 됩니다.

이후 첫 단계부터 시작하면 됩니다.

---

# 팁
간혹, 로컬에서 Tx가 fail 되는 경우가 있습니다.
이 때는 지갑의 Nonce 정보를 초기화해주면 됩니다.

---
