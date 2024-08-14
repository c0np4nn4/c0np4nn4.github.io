+++
title = "Re-entrancy"
date = "2023-08-30"
+++


---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> `Re-entrancy` 문제입니다.

<center>
<img alt="main image" 
src="https://images.unsplash.com/photo-1552819401-700b5e342b9d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2670&q=80" />
</center>

---

# Introduction
`Re-entrancy` 는 [The DAO](https://hackingdistributed.com/2016/06/18/analysis-of-the-dao-exploit/) 사건으로 알 수 있듯이, 정말 크리티컬한 취약점입니다.
이번 문제에서는 `Re-entrancy`가 일어나는 코드와 그 과정에 관해 살펴보겠습니다.

---

# Problem Detail

## Contract
우선 컨트랙트 코드를 살펴보겠습니다.

<center>
<img alt="contract" src="../../ethernaut_img/10_reentrance_1.png" />
</center>

*payable* 인 `donate()`가 있고, *view* 인 `balanceOf()` 함수가 있습니다.
하지만, 이 문제에서 가장 중요한 함수는 `withdraw()` 입니다.

`withdraw()`함수는 우선 `balances[msg.sender] >= _amount`로, <u>출금하려는 금액</u>이 <u>잔고에 남아있는 금액</u> 을 **초과**하는지 검사합니다.
이 단계에서 별 이상이 없다면, 바로 `msg.sender.call{value: _amount}("")`로 <u>**msg.sender**에게 돈을 송금</u>합니다. 바로 이 때, 문제가 발생합니다.

앞선 문제 [King](@/posts/ethernaut_9.md) 에서 살펴보았듯이, <u>***외부 컨트랙트가 어떤 receive() 함수를 구현했을지 모른다***</u> 는 문제가 있습니다.
여기서 <u>외부 컨트랙트</u>는 **msg.sender** 입니다.
**msg.sender**가 구현하고 있을 `receive()` 함수는 임의의 동작을 수행할 수 있고, 심지어는 `withdraw()` 함수를 한 번 더 호출할 수도 있습니다!

이것을 `Re-entrance` 취약점이라 부릅니다.


## Goal

<center>
<img alt="goal" src="../../ethernaut_img/10_reentrance_2.png" />
</center>

`Reentrance` 컨트랙트의 잔고를 `0`으로 만들면 됨을 확인할 수 있습니다.

---

# Exploit
## Strategy
<center>
<img alt="overview" src="../../ethernaut_img/10_reentrance_3.png" />
</center>

앞서 설명한대로, `Attacker` 계정에서 `receive()` 함수 내에 `withdraw()`를 호출하도록 정의한뒤,  `withdraw()`를 호출하여 ***Recursive call*** 을 일으킵니다.

## Code
### Attacker

<center>
<img alt="attakcer" src="../../ethernaut_img/10_reentrance_4.png" />
</center>

`receive()` 함수를 보면, 두 종류의 공격으로 나뉘어 놓은 것을 확인할 수 있습니다.

첫 번째 공격은 **recursive call** 만으로, 모든 돈을 빼앗는 방법입니다.

두 번째 공격은 우선 <u>소량의 돈을 출금하면서, `balances`라는 상태 변수에  ***underflow*** 를 일으킵니다</u>. 이후, 조작된 `balances` 값을 이용해 모든 돈을 안전하게 출금합니다.

두 방법 모두 **recursive call** 을 이용하지만, 두 번째 방법의 경우는 *소량의 돈을 출금하는 작업을 반복* 하다보니 gasLimit 이나 call stack limit 등 다양한 요인으로 인해 모든 돈을 출금하지는 못하는 것으로 확인되었습니다.

## Result

<center>
<img alt="result" src="../../ethernaut_img/10_reentrance_5.png" />
</center>

여담으로, 이번 문제를 풀면서 로깅이 너무 힘들다는 것을 깨달았습니다...

그래서, 좀 더 로깅을 잘 찍어주는 것으로 보이는 `foundry` 를 이용해 푸는 것을 생각 중입니다.



---

# Conclusion

<script src="https://utteranc.es/client.js"
        repo="c0np4nn4/utterance_repo"
        issue-term="pathname"
        label="utterances"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
