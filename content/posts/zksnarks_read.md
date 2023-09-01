+++
title = "[Read] Some ways to use ZK-SNARKs for Privacy"
date = "2023-09-01"
+++

> 비탈릭 부테린의 [Some ways to use ZK-SNARKS for Privacy](https://vitalik.ca/general/2022/06/15/using_snarks.html)를 읽고 작성하였습니다.

# Intro
`ZK-SNARKs`는 아주 강력한 암호학 도구이며, 블록체인과 그 너머 이상의 개발을 하는 사람들에게 중요해지는 분야 입니다.
하지만, <u>*어떻게 동작하는지*</u> 인 `원리`에 대한 부분과
<u>*어떻게 사용할 수 있는지*</u> 인 `응용`에 대한 두 부분이 모두 복잡하다는 한계가 있습니다.

비탈릭 부테린은 첫 번째 의문인 <u>*어떻게 동작하는지*</u> 에 관한 [설명 글](https://vitalik.ca/general/2021/01/26/snarks.html)을 남겨 둔 바가 있습니다.
그는 글 속에서 `ZK-SNARKs` 속의 *수학적 지식* 을 충분히 이해될만한 수준으로 설명하고자 시도했습니다만, 여전히 **이론적으로 복잡**한 부분이 많습니다.

이번 글은 두 번째 의문인 <u>*어떻게 사용할 수 있는지*</u> 에 대해 다룹니다.
- 기존 프로젝트에 어떻게 `ZK-SNARKs`를 적용할 수 있을지
- `ZK-SNARKs`가 어떠한 것을 할 수 있고, 할 수 없는지
- `ZK-SNARKs`를 적용할 수 있을지의 가능성에 대한 <u>일반적인 가이드라인</u>이란 어떤 것인지

**그리고 특히, 이 글에서는 <u>Preserving Privacy</u>라는 `ZK-SNARKs`의 응용에 대해 집중해서 다룹니다.**

---

# What does a ZK-SNARK do?
> `ZK-SNARK`가 무엇을 하나요?

예를 들어, *`public input`* 값인 $x$, *`private input`* 값인 $w$, 
그리고 임의의 함수 $f(x, w) \rightarrow \\{ True, False \\}$ 가 존재한다고 해봅시다.
즉, 함수 $f(x, w)$ 는 *input 에 대한 `verification`* 을 하는 함수로 볼 수 있습니다.

`ZK-SNARK`를 이용하면 *`비밀 값`* 인 `w` 를 드러내지 않고도, *`공개된 정보`* 인 $x$ 와 $f(x, w)$ 를 이용해서 <u>*`w` 를 알고 있음을 증명*</u> 할 수 있습니다.
게다가, 만약 *`비밀 값`* 을 공개하더라도 `검증자(verifier)`는 ***증명(proof)*** 에 대하여, $f(x, w)$ 를 직접 계산하는 것보다 훨씬 **빠르게** <u>*`w` 알고 있음을 증명*</u> 할 수 있습니다.

위 문장을 통해`ZK-SNARK`가 ***privacy*** 와 ***scalability*** 라는 두 특성을 가짐을 알 수 있습니다.

하지만, 이 글에서는 ***privacy*** 에 집중해서 `ZK-SNARK`를 살펴봅니다.

---

# Proof of Membership
> 구성원임을 증명

예를 들어, Ethereum Wallet 을 가진 사람이 자신의 정보를 드러내지 않으면서 `proof-of-humanity`를 증명하고자 한다고 가정해봅시다.
수학적으로 아래와 같이 기술할 수 있습니다.

> - `private input` $(w)$
>   - 지갑 주소인 $A$ 와 지갑 주소에 대한 *key* $k$

