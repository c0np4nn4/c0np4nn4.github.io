+++
title = "Vault"
date = "2023-08-26"
+++


---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> ***Storage*** 에 대한 이해를 묻는 문제입니다.
>
> 또한, Ethereum 의 ***투명성*** 에 대해 확인할 수 있는 문제입니다.

<center>
<img alt="main image" src="https://images.unsplash.com/photo-1615279113483-82e6693dab8f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2062&q=80" />
</center>

---

# Introduction
Solidity에는 총 4가지의 [Visibility](https://solidity-by-example.org/visibility/) 가 존재합니다.
> public, private, internal, external
이렇게 하면, 외부 계정의 접근을 적절히 제한할 수 있습니다.

그런데, 잘 알려졌다시피 `Ethereum`은 **투명성**이라는 특징을 갖습니다.
당장 [Etherscan](https://etherscan.io/)만 가더라도, 블록과 트랜잭션의 정보들을 확인할 수 있습니다.
그렇다면 `private` 키워드가 정말 **privacy** 를 보장해줄 수 있을까요?

이번 문제에서는 **Storage** 의 구조와 **State Variable** 을 참조하는 방법에 대해 알아봅니다.

---

# Problem Detail

## Goal
문제의 목표는 아래와 같이 `Vault` 컨트랙트의 `lock`을 풀어주는 것입니다.

<center>
<img alt="goal" src="../../ethernaut_img/8_vault_1.png" />
</center>

## Contract
`Vault` 컨트랙트는 아래와 같습니다.

<center>
<img alt="vault contracvault contract" src="../../ethernaut_img/8_vault_2.png" />
</center>

State Variable 은 총 `2`개입니다.
이 중, **password**가 `private`로 선언된 것을 확인할 수 있습니다.

`Vault` 컨트랙트에서 `unlock()` 함수를 이용해 `lock`을 풀어주면 됨을 확인할 수 있습니다.
다만, 정확한 `_password` 값을 입력해야 함을 알 수 있습니다.


## Storage
[State Variables in Storage](https://docs.soliditylang.org/en/latest/internals/layout_in_storage.html) 를 보면,
Contract Storage 는 `32bytes` 크기의 `slot`들로 이루어져있으며, 복잡한 데이터 타입이 아니면 순차적으로 저장됨을 알 수 있습니다.

<center>
<img alt="contract storage" src="../../ethernaut_img/8_vault_3.png" />
</center>

---

# Exploit
## Strategy
비교적 간단합니다.
배포된 `Vault` 컨트랙트에서 `password`가 저장된 슬롯 (`slot 1`)값을 가져와서 `_password`로 사용하면 됩니다.


## Code
문제 풀이에 사용된 Javascript 코드입니다.

<center>
<img alt="exploit" src="../../ethernaut_img/8_vault_4.png" />
</center>

`ethers.provider.getStorageAt` 메서드를 이용해서 간단하게 `slot`의 값을 가져올 수 있습니다.

## Result
적절히 로그와 함께 실행 화면은 아래와 같습니다.

<center>
<img alt="result" src="../../ethernaut_img/8_vault_5.png" />
</center>

---

# Conclusion
이더리움은 투명하게 정보를 읽을 수 있다는 점을 다시 한번 확인할 수 있었습니다.

그렇다면 조금 더 안전하게 `password`를 다루는 방법은 어떤게 있을까요?
직관적으로 드는 방법은 `keccak256`으로 얻은 해시값을 저장해두고, 해시를 생성하기 위해 사용한 `seed`값을 인자로 받는 방법이 있겠습니다.
암호학적 해시함수는 *역상저항성* 이라 일컫는 단방향성을 갖기 때문에, `해시값`을 공개하더라도 역연산으로 `seed`를 알아내기 **무지하게** 힘들기 때문입니다.

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
