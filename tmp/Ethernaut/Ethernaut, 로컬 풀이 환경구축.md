---
tags:
  - Ethernaut
  - Write-up
---

Openzeppelin이 호스팅하고 있는 [Ethernaut](https://ethernaut.openzeppelin.com/)에서 문제를 직접 풀 수도 있겠지만, testnet인 sepolia를 이용해야 하므로 faucet을 통해 ETH를 받아와야 합니다.
Faucet을 통하는 과정이 번거롭기도 하고, testnet이다보니 블록 생성까지 기다려야 하는 등 아무래도 문제 풀이에 불편함이 많습니다.

Anvil을 이용해서 localnet에 똑같은 환경을 구축하면 이러한 불편함을 덜 수 있습니다.

상세하게 적을 필요는 없을 것 같고... 아래 순서대로 구축해두면 됩니다.

1. Anvil 로 localnet 환경을 만들어 둡니다.
2. [Ethernaut github](https://github.com/OpenZeppelin/ethernaut) Ethernaut 레포를 clone 해와서 [Running locally](https://github.com/OpenZeppelin/ethernaut) 를 참고합니다.
	- `package.json` 에서 적절한 script 를 찾아서 입력해주면 됩니다. 저는 아래와 같이 입력해두었습니다.
	  ```bash
		  yarn compile:contracts
		  yarn deploy:contracts
		  yarn start:ethernaut
		```

	- ![[Pasted image 20241004023856.png]]
	  ACTIVE_NETWORK를 위 그림과 같이 명시해두기도 했습니다. (그냥 주석 풀어주시면 됩니다...)

그럼 아래와 같이 localhost:3000 에서 ethernatu을 푸는 환경을 만들 수 있습니다.
![[Pasted image 20241004024429.png]]

저는 보통 localnet에서 기본적으로 제공하는 계정을 그냥 사용하는 편인데.. 다른 분의 블로그를 참조해보니 faucet으로 활용하고 본인 계정?에 가스비를 옮겨놓고 시작하는 방법도 꽤 괜찮은 것 같습니다.

https://velog.io/@oomia/%EB%A1%9C%EC%BB%AC%EC%97%90%EC%84%9C-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-Ethernaut

아무튼, 이제 한 문제씩 풀이를 정리해보겠습니다.