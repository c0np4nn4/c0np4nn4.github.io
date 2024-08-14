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

예를 들어, Ethereum Wallet 에 본인이 '사람'임에 대한 정보를 등록해두었다고 해봅시다.
자신이 어느 '사람'인지 드러내지 않으면서 '사람'임을 증명하려 할 때, 
아래와 같이 수학적으로 기술할 수 있습니다.

> - `private input` $(w)$
>   - 지갑 주소인 $A$ 와 지갑 주소에 관한 `priv_key` $k$
> - `public input` $(x)$
>   - '사람'임에 대한 정보가 등록되어 **인증된** 모든 지갑 주소 모음 $\{H_1, H_2, \dots, H_n\}$
> - `verification function` $f(x, w)$
>   - 두 입력값 $w, x$ 를 각각 $(A, k)$ 와 $\{H_1, \dots, H_n\}$ 으로 해석합니다.
>   - $A$ 가 $\\{H_1, \dots, H_n\\}$ 의 원소임을 **증명**합니다.
>   - $\text{priv_to_addr} (k) = A$ 임을 **증명**합니다.
>   - 위 두 **증명**을 통과하면 $True$ 를 반환합니다.

조금 더 자세하게 풀어서 설명하겠습니다.

자신이 '등록된 사람'임을 증명하고자 하는 주체인 `Prover` 는 아래 과정을 거칩니다.

1. `private input` $w = (A, k)$ 생성
    - $A$: 자신의 지갑 주소
    - $k$: 지갑에 연관된 비밀키
2. `public input` $x = \\{H_1, H_2, \dots, H_n\\}$ 생성
    - $\\{H_1, H_2, \dots, H_n\\}$: blockchain 에 등록된 ***사람임이 인증된*** 지갑 주소
    - `prover`는 어느 `block` 에서 이 정보를 가져왔는지 기억해둡니다.
3. $w, x$ 값을 이용해 `Proof`인 $\pi$ 생성
    -  두`input` 값 $w, x$와 `ZK-SNARK` 알고리즘을 이용해 증명(`Proof`)를 생성합니다.
4. `Verifier`에게 $\pi$와 $x$ 또는 block number 전달

이후 `Verifier`는 전달받은 $(\pi, x)$ 를 토대로 verification 을 진행하면 됩니다.

## Making the proof-of-membership more efficient
위에서 소개한 *Proof of Membership* 의 한 가지 약점은, `Verifier`가 전체 멤버($\\{H_1, H_2, \dots, H_n\\}$
)에 대한 정보를 다룬다는 점입니다. 만약 멤버의 수가 꽤 많을 경우, verification 과정에도 $O(n)$ 이 잡아먹는 오버헤드가 커짐이 자명합니다.

이를 해결할 수 있는 방법은 `Prover`에게 이 작업을 전가하는 것입니다.
즉, `private input` 에 하나를 더 추가하여 `Prover`의 지갑 주소가 `Merkle Root` 를 만들 수 있음을 증명하는 `Merkle Proof` $M$을 추가합니다.
이렇게 되면, **전체** 멤버를 다룰 필요 없이 `Merkle Root`를 생성하는데 필요한 멤버만 다루어도 되므로 $\log_2(N)$ 만큼 크기가 줄어듭니다.

> `ZK-proving` Membership 에 관해서 `Merkle proof`를 다루는 효율적인 대체 방법으로 [Caulk](https://eprint.iacr.org/2022/621)라는 방법도 있다고 합니다.
> 비탈릭은 이를 꽤 긍정적으로 평가하고 있습니다.

---

# ZK-SNARKs for coins
[Zcash](https://z.cash/)나 `Tornado.cash` 와 같이 *privacy-preserving currency* 라는 슬로건으로 `ZKP`를 응용한 프로젝트들도 있습니다.
앞서 살펴본 *ZK proof-of-humanity* 에서 *humanity profile* 에 대한 접근을 **증명**했다면, 여기서는 *coin* 에 대한 접근을 **증명**합니다.

`Coin`과 관련해서 `ZKP`를 적용하려면 한 가지 문제를 더 해결해야 합니다.
바로 ***Double Spending*** 에 대한 것입니다.
우선 `Coin`을 갖고 있는 누구나 `private secret` 값으로 $s$ 를 가집니다.
로컬에서 `coin Merkle Tree`의 `Leaf` 값으로 $L = hash(s, 1)$ 를 계산하고 on-chain 에 등록합니다. 
그리고, `Nullifier` 값으로 $N=hash(s, 2)$ 를 계산합니다.

> - `public input` $(N, R, L')$
>   - $N$: `coin` 의 주인이 $s$ 값으로 생성한 `Nullifier` 입니다.
>   - $R$: `coin` 에 대한 정보가 존재하는 시점의 `Merkle Root` 값입니다.
>   - $L'$: 새로운 `coin` 에 대한 `private secret` $s'$ 로 생성한 값입니다.
> - `private input` $(s, L, M)$
>   - $s$: `coin` 에 대한 `private secret` 정보입니다.
>   - $L$: `coin` 에 대한 Merkle Tree의 `Leaf` 값입니다.
>   - $M$: `Merkle Branch` 값입니다.
> - `verification function`
>   - $M$ 값을 이용해 `Merkle Root`가 $R$ 일 때, `Leaf` 값으로 $L$을 가짐을 **증명**합니다.
>   - $hash(s, 1) = L$ 임을 증명합니다. 
>   - $hash(s, 2) = N$ 임을 증명합니다. 

$L'$ 값에 대한 **증명**은 하지 않지만, `이전 coin` 에 대한 증명을 할 때 함께 기록해둡니다.
이를 통해, 제 3자가 임의로 정보를 조작하는 것을 방지하는 것 같습니다.

위 방식의 핵심은 `coin` 에 대한 `private secret` $s$ 를 숨김으로써 $L$ 과 $M$ 사이의 링크를 깨는 것입니다.
각각의 값이 의미하는 바를 정리하면 아래와 같습니다.
- $L$: `coin` 이 **생성**되었을 때 on-chain 에 사실 기록
- $N$: `coin` 이 **소비**되었을 때 on-chain 에 사실 기록

또한, $s$ 에서 $s'$ 로 넘어가는 일종의 `소비` 행위를 할 때 `Nullifier` 가 드러나고 on-chain 에 기록됩니다.
이를 이용해 on-chain 에 `Nullifier` $N$이 있는지 검색하는 방식으로 ***Double Spending*** 을 막을 수도 있습니다.

이러한 방식을 편의상 ***"privately spend only once"*** gadget 이라 칭해놓겠습니다.
이후 소개될 다른 두 방식에서도 이와 유사한 scheme 이 응용된다고 합니다.

## Coins with arbitrary balances
위 방식은 각 `coin`에 임의의 "금액"(500원, 100원 처럼..)을 붙이는 방식으로 확장될 수 있습니다.
말 그대로, `coin` 에 'balance' 라는 필드를 붙여주는 식으로 구현할 수 있습니다.
직관적인 방법은 `coin` 에 대한 `Leaf`값 $L$과 `encrypted balance`정보를 묶어서 저장해주는 것입니다.

**소비**하는 방식은, 두 개의 `old_coin` 에 대해 두 개의 `new_coin` 을 생성하면서 `old sum`과 `new sum`의 값이 `spent` 만큼 차이가 나는지와 `balance`가 음수가 아닌지 등을 검사하게 됩니다.

---

# Zk anti-denial-of-service
예를 들어, on-chain 상에 non-trivial 한 `identity` 를 만들어 뒀다고 해봅시다.
peer-to-peer network 상에서 메세지를 보내는 주체가 이러한 `identity`를 갖고 있는지에 대한 `Proof`를 이용하면 anti-DoS 를 구현할 수 있다는 개념입니다.

단순히 생각해보면, `identity list`를 두고 전달받은 `identity`가 포함되는지 검사하는 것은 `Proof-of-Membership`에서 다룬 내용입니다. 
`DoS`의 특성을 생각해보면 ***특정 주체로부터의 과도한 메세지 전송을 막는다*** 라는 기능이 추가로 포함되어야 합니다.

이를 위해 아래와 같이 `Protocol`을 하나 설계해볼 수 있습니다. 

## The Protocol
***과도한 메세지 전송*** 이라는 말에는 **특정 단위 시간 동안** 이라는 말이 숨어 있습니다.
따라서, 우선 '한 시간'을 $1000$ 개의 `epoch`로 나눠서 $epoch = 3.6 secs$ 로 두겠습니다.

우선 `Nullifier` 개념을 활용해볼 수 있습니다.
$$N=hash(k, e)$$

$k$ 는 특정 주체의 `key` 값이고, $e$는 `epoch number` 입니다.
메세지를 전송할 때, `message` $m$ 과 함께 $N$ 을 보낸다고 생각하면 됩니다.
여기서 `ZK-SNARK`를 적용해서 $hash(m)$을 한번 더 섞어주면 `message` 에 대한 아무런 정보를 드러내지 않을 수 있습니다.

`Nullifier`가 `epoch number`에 의해 유일하게 결정되므로, 서로 다른 `message`에 대해 같은 `Nullifier`를 사용할 수 없음을 쉽게 알 수 있습니다.

여기서 조금 더 나아가면, ***악의적인 행동을 할 경우 penalty를 주는 프로토콜***을 설계할 수도 있습니다.
예를 들어, `Protocol`에 아래와 같은 방정식을 하나 세워둡니다.
$$L_e(x) = hash(k, e) * x + k$$
즉, 각 계정의 unique  한 $k$값을 y축 절편으로 하고 `Nullifier`를 기울기로 하는 직선을 하나 만들어 둡니다.
인자 $x$에는 `message`에 대한 정보 $hash(m)$ 을 넣도록 설계했다고 해보겠습니다.

우선, `Protocol`에 대한 `ZK-SNARK` 과정을 한번 정리해보면 아래와 같습니다.

> - `public input`
>   - $\\{A_1, \dots, A_n \\}$: `message` 전송이 가능한 지갑 주소 전체 모음
>   - $m$: `message`
>   - $e$: `epoch number`
>   - $y$: 방정식 $L_e(x)$ 의 연산 결과 
> - `private input`
>   - $k$: 지갑의 `private key`
> - `verification function`
>   - $\text{priv_to_addr} (k) \in \\{A_1, \dots, A_n \\}$ 임을 **증명**합니다.
>   - $y = hash(k, e) \times hash(m) + k$ 임을 **증명**합니다.

만약 임의의 주체가 같은 `epoch number`로 서로 다른 `message` $m_1, m_2$ 를 보내려고 한다고 하면
아래와 같이 $k$ 에 대한 수식을 세울 수 있게 됩니다.

$$
\begin{align}
y_1 &= hash(k, e) \times hash(m_1) + k \newline
y_2 &= hash(k, e) \times hash(m_2) + k \newline
\newline
y_1 - y_2 &= hash(k, e) \times (hash(m_1) - hash(m_2)) \newline
(\therefore) \\ hash(k, e) &= \frac{y_1 - y_2}{hash(m_1) - hash(m_2)} \newline
\newline
(\rightarrow) \\ k &= y_1 - hash(k, e) \times hash(m_1) \newline
k &= y_1 - \frac{y_1 - y_2}{hash(m_1) - hash(m_2)} \times hash(m_1) \newline
\end{align}
$$

즉, $k$ 를 연산하는데 필요한 값($m, y$) 가 모두 `public input`이므로 누구나 비밀키 $k$를 복원할 수 있게 됩니다.
이러한 ***Penalty*** 를 부여하는 것이 가능합니다.
이와 관련하여 [RLN(rate limiting nullifier)](https://medium.com/privacy-scaling-explorations/rate-limiting-nullifier-a-spam-protection-mechanism-for-anonymous-environments-bbe4006a57d)가 있다고 합니다.

---

# ZK negative reputation
***완벽한 익명성*** 을 보장하는 온라인 커뮤니티를 만든다고 가정해보겠습니다. 
이 커뮤니티에 `Reputation System`을 추가한다고 하면, 조금 구현 난이도가 올라갑니다.

## Chaining posts: the basics
누구나 on-chain 에 `post` 메세지를 날림으로써 글을 쓸 수 있고, `ZK-SNARK`를 이용해 아래 두 가지를 증명합니다.
- `Proof-of-humanity` **증명**
- 이전에 어떤 `post`를 작성했는지 **증명**

`ZK-SNARK`를 좀 더 상세히 정리하면 아래와 같습니다.
> - `public input`
>   - $N$: `Nullifier`
>   - $R$: 최근의 blockchain state root 
>   - `post contents`: `post`와 binding 하기 위해 받음
> - `private input`
>   - $k$: 지갑의 `private key`
>   - $N_{prev}$ 또는 `external identity`: 이전 `post`에 대한 정보를 위해 받음
>   - $M$: $N_{prev}$ 또는 `external identity`가 on-chain 정보인지 확인하기 위해 받음
>   - $i$: 작성했던 `post`의 갯수
> - `verification function`
>   - $M$을 이용해 $N_{prev}$ 또는 `external identity`가 on-chain 정보인지 **증명**합니다.
>   - $N = enc(i, k)$ 임을 **증명**합니다.
>   - $i = 0$ 이면, 처음 글을 쓰는 것이므로 $A = \text{priv_to_addr}(k)$ 만 **증명**합니다.
>   - $i \neq 0$ 이면, $N_{prev} = enc(i - 1, k)$ 임을 **증명**합니다.

앞서 살펴본 ***privacy-preserving coin*** 방식과 비슷하지만, 몇 가지 차이가 있습니다.
- `Nullifier`를 $s$ 같은 `일회성 비밀키`가 아닌 지갑과 연관된 $k$를 이용해 생성합니다.
- $hash$ 대신 $enc$ 를 사용하여, ***복호화*** 가 가능하게 했습니다.

## Adding Reputation
`Smart Contract` 를 이용해서 `addReputation` 과 같은 method로 `reputation system`을 구현합니다.
`addReputation`은 인자로 아래 두 가지를 받습니다.
- `Nullifier`: `post`와 연관된 정보 입니다.
- `score`: `add` 하거나 `subtract`할 평판 점수입니다.

또한, `post` 당 on-chain 에 저장할 정보를 아래와 같이 확장합니다.
- $N$: `Nullifier`로, `post` 마다의 고유한 정보입니다.
- $\bar{h} = hash(h, r)$: $h$ 는 `proof` $\pi$ 를 생성할 때의 `state root`의 ***block height*** 입니다.
- $\bar{u} = hash(u, r)$: $u$ 는 계정의 `reputation` 값 입니다.
$r$ 은 단순한 랜덤 값으로, $h, u$ 값을 brute-force 공격으로 알아낼 수 없도록 도와주는 값입니다.

정리하면, 각 `post`는 아래의 값과 연관됩니다. (어딘가에 저장해두기도 합니다)
- $R$: `State Root` 값
- $\\{N, \bar{h}, \bar{u}\\}$

이를 응용해서, `User`가 이때까지 쌓아온 `reputation` 값 $u$ 에 대한 정책을 세울 수도 있습니다.

---

(wip)
