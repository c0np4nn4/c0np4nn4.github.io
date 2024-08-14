+++
title = "Ethernaut Local Environment"
description = "Ethernaut: Local Env Setting"
date = 2023-03-29
toc = true

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "ethernaut"]

[extra]
math=true
+++

---

# Ethernaut on testnet...
- Goeril testnet 을 이용하는 [[Ethernaut 사이트](https://ethernaut.openzeppelin.com/)]는 바로 접속해서 풀이를 할 수 있다는 점이 편리하기는 하지만, Goeril Eth 가 항상 부족한 문제가 있었다..
- 문제를 다 푼 것 같아도 돈이 없어서 Tx 를 못 보내는 경우가 생겼고, 의욕이 뚝뚝 떨어졌다.
- 다행히도 [[Github](https://github.com/OpenZeppelin/ethernaut)] 을 통해 Local 환경을 구축할 수 있음을 발견했다!!!

---

# Nodejs
- 처음엔 <mark>Nodejs 18.xx.xx</mark> 을 사용하고 있었는데, 이유는 잘 모르겠지만 이 때문에 React 가 제대로 돌아가지 않았다..
- `nvm` 을 이용해서 <mark>16.18.0</mark> 버전으로 바꾸고 나니 잘 돌아갔다.

---

# 후기
- 일단 돈이 부족한게 없다. `10,000ETH` 를 갖고 시작한다.
- `Block` 생성 속도도 엄청 빠르다. 하하
- 스크립트를 뜯어보니 역시나 `Hardhat` 을 이용하고 있었다.

---
