+++
title = "Hello Ethernaut"
date = 2023-03-01

[taxonomies]
categories = ["Ethernaut"]
tags = ["Metamask","Ethereum"]

[extra]
toc = true
keywords = "Ethernaut, Metamask, Solidity, Ethereum"
+++

---

- <mark>Ethernaut</mark> 을 본격적으로 시작하기 위한 튜토리얼 단계입니다.

# 1. Set up Metamask

- <mark>Ethernaut</mark> 을 시작하기 위해서는 지갑 앱인 <kbd>MetaMask</kbd>를 이용해야 합니다.

- Browser Extension 으로도 제공되는 <kbd>MetaMask</kbd>를 추가하고, 간단히 지갑을 생성하면 아래와 같은 화면을 확인할 수 있습니다.

<center>
{{ img(src="/blockchain/ethernaut/img/metamask_on_browser.png" alt="Metamask" w=300 h=200) }}
</center>

- 여우 모양의 아이콘이 <mark>MetaMask</mark> 입니다.

<center>
{{ img(src="/blockchain/ethernaut/img/switch_to_testnet.png" alt="Testnet" w=300 h=200) }}
</center>

- 아이콘을 클릭한 후, <kbd>Goeril</kbd> 이란 이름의 **Testnet** 으로 네트워크를 변경합니다.


# 2. Open the Browser's Console

- <kbd>F12</kbd> 를 눌러서 `개발자도구` 창을 보면 아래와 같은 메세지를 확인할 수 있습니다.

<center>
{{ img(src="/blockchain/ethernaut/img/hello_ethernaut.png" alt="hello_message" w=400 h=200) }}
</center>

- 메세지들 중 <mark>Player's Address</mark> 를 나타내는 값이 있을텐데, 문제를 푸는 동안 이를 아는 것이 중요하다고 합니다.

- Console에 <kbd>player</kbd>를 입력해서 다시 확인할 수 있다고 합니다.

# 3. Use the console helpers

- 또한, <mark>get_balance(player)</mark> 를 입력하면 현재 이더 잔액을 조회할 수도 있다고 합니다.

- 이러한 여러 기능들은 <mark>help()</mark> 를 입력해서 확인할 수 있으니, 문제를 푸는 동안 요긴하게 사용하면 될 것 같습니다.

# 4. The ethernaut contract

- Console에 <mark>ethernaut</mark> 을 입력하면 각 문제의 <kbd>Smart Contract</kbd> 정보를 확인할 수 있습니다.

<center>
{{ img(src="/blockchain/ethernaut/img/ethernaut_contract.png" alt="contract" w=300 h=200) }}
</center>

# 5. Interact with ABI

- Console 에서 확인한 <mark>ethernaut</mark> 는 블록체인 상에 배포(deploy)된 <mark>Ethernaut.sol</mark> 의 <kbd>TruffleContract</kbd> object 라고 합니다.

- <mark>ethernaut</mark> 의 여러 값들 중 <mark>ABI</mark> 는 <mark>Ethernaut.sol</mark> 의 모든 **public method** 를 이용할 수 있게 해줍니다.

- 따라서, 아래와 같이 Console 에 입력하여 Contract 주인의 주소를 확인하는 것이 가능합니다.

```js
await ethernaut.owner()
```

# 6. Get test ether

- 문제들을 풀기 위해서는 Contract 와 interact 해야하고, 이더리움 상에서는 이를 위해 Ether 를 사용해야 합니다.

- **Mainnet** 에서는 이 Ether 가 실제 돈이지만, **Testnet** 에서는 무료입니다.

- 무료로 돈을 받을 수 있도록 각 **Testnet** 마다 적법한 *faucet*이 있습니다.

- 적당히 ethernet goeril faucet 라 구글링해보면 나오는 곳을 통해 Ether를 확보하면 됩니다.

# 7. Getting a level instance

- 문제를 풀기 위해서 <mark>Smart Contract</mark> 와 직접 interact 할 필요는 없습니다.

- 대신에, <mark>Ethernaut</mark> 에서 제공하는 <kbd>Level Instance</kbd> 기능을 이용할 수 있습니다.

- <mark>Contract</mark> 를 새로 배포하는 작업이기 때문에, 시간은 조금 소요된다고 합니다.

# 8. Inspecting the contract

- Console 에 `contract` 를 입력하여 ABI 를 확인할 수 있습니다.

# 9. Interact with the contract to complete the level

- 아래와 같이 Console 에 입력하여 문제 정보를 확인할 수 있습니다.
```js
await contract.info()
```

- 또, 문제를 다 풀었다고 생각하면 페이지 하단의 <mark>Submit Instance</mark> 를 클릭하면 됩니다.

# Done

- 문제를 풀고 Submit 하면 아래와 같은 문구를 확인할 수 있습니다.

<center>
{{ img(src="/blockchain/ethernaut/img/hello_ethernaut_done.png" alt="Metamask" w=600 h=200) }}
</center>
