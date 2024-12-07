---
tags:
  - Ethernaut
  - Write-up
---
문제에 나와있듯이 ownership 을 획득하면 되는 문제입니다.

사실 코드를 좀 살펴보면, `***Factory.sol` 에 풀이 완료 조건이 있긴 합니다.

![[Pasted image 20241004044800.png]]

<mark style="background: #BBFABBA6;">instance.owner() 와 현재 player 가 동일</mark>하다면 통과하는 로직입니다.

`owner` 를 바꾸는 로직은 문제에서 `/*constructor*/`로 주석을 남긴 함수가 바로 보입니다.
심지어 `public`으로 되어있으니 그냥 호출하면 되겠죠?

솔리디티 공부했는지 검사하는 문제였습니다.

좀 더 정확히는 constructor 함수에 대한 내용, public, external 등 지시자에 대한 내용을 살펴보게 만드는 문제라 생각합니다~

![[Pasted image 20241004045438.png]]

---

## 여담

foundry 에서 solc 버전이 다를 경우 어떻게 해결을 해야 하나.. 좀 고민이었는데, 큰 문제는 없었습니다.
Fallout 문제가 사용하는 0.6.x 버전의 SafeMath 경우에도 그냥 레포에 가져왔더니 쉽게 해결됐습니다 ^_^;;

![[Pasted image 20241004045545.png]]

ParseError 가 많이 신경쓰이긴 하지만 뭐.. 좀 참거나 다른 방법을 찾아야 할 것 같습니다 -_-;;