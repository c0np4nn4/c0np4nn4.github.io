+++
title = "Lesson 1"
description = "Making the Zombie Factory"
date = 2023-03-11

[taxonomies]
categories = ["Cryptozombie"]
tags = ["Solidity","Ethereum", "Cryptozombie"]

[extra]
toc = true
keywords = "Ethernaut, Solidity, Ethereum, Cryptozombie"
+++

---

# 1. Contracts
- Solidity 코드는 항상 아래 두 구조를 갖습니다.
  - 첫 줄에 <mark>compiler 버전</mark>
  - <mark>contract 키워드</mark>로 묶인 body

```sol
prgama solidity  // 버전 명시

contract {
  // 컨트랙트 내용 작성
}
```

---

# 2. State Variables
- Solidity 에서 선언한 변수는 Contract Storage 에 <mark>영구히 기록</mark>되는 특징이 있습니다.

```sol
pragma solidity >=0.8.0;

contract ZombieFactory {
  uint dnaDigits = 16;
}
```

- <kbd>uint</kbd> 는 `unsinged integer 256-bit` 를 의미합니다.

- <kbd>uint8</kbd>, <kbd>uint16</kbd>, <kbd>uint32</kbd> 등도 있습니다.

- 참고로 Solidity 에서는 크게 <kbd>Value Type</kbd>, <kbd>Ref Type</kbd>, <kbd>Mapping Type</kbd> 으로 Type 종류를 구분합니다.
>  - <kbd>Value Type</kbd> 은 `call by value` 로 다뤄지는 Type 입니다.
>    - *primitive types*: <kbd>boolean</kbd>, <kbd>integer(unsigned, signed)</kbd>, <kbd>address</kbd>
>    - *string literals*
>    - *enums*
>    - *function types*
>  - <kbd>Ref Type</kbd> 은 `call by reference` 와 같은 맥락에서 이해하면 됩니다.
>    - *Array*
>    - *Array Slice*
>    - *Struct*
>  - <kbd>Mapping Type</kbd> 은 일종의 `key-value` 구조의 type 으로 이해할 수 있습니다.

- 보다 자세한 내용은 ([docs](https://docs.soliditylang.org/en/v0.8.17/types.html#)) 를 참조하면 됩니다.


---

# 3. Operators
- Solidity 에서 사용하는 연산자는 ([docs](https://docs.soliditylang.org/en/v0.8.17/types.html#order)) 를 참고하면 됩니다.
- Solidity 에서는 대부분의 산술연산이 가능합니다.
  - <kbd>+</kbd>, <kbd>-</kbd>, <kbd>\/</kbd>, <kbd>*</kbd>, <kbd>%(mod)</kbd>, <kbd>**(exponentiation)</kbd>

---

# 4. Structs
- Solidity 에서 구조체는 아래와 같이 만들 수 있습니다.

```sol
struct Person {
  uint age;
  string name;
  // ...
}
```

---

# 5. Array
- Solidity 에서 배열은 아래와 같이 만들 수 있습니다.

```sol
// 길이가 2로 고정된 uint 배열:
uint[2] fixedArray;

// 길이가 5로 고정된 string 배열:
string[5] stringArray;

// 길이가 유동적인 uint dynamic 배열:
uint[] dynamicArray;
```

---

# 6. Function
- Solidity 에서 함수 선언 문법은 아래와 같습니다.

```sol
function eatHamburgers(string memory _name, uint _amount) {
  // eat hamburgers
}
```

- Solidity 에서는 param 앞에 밑줄 (_) 을 붙여주는 convention이 있다고 합니다.


---

# 7. Structs + Array
- 앞서 보았듯, <kbd>Struct</kbd> 도 <kbd>Ref Type</kbd> 의 한 종류입니다.

- 따라서, 이를 원소로 갖는 <mark>Array</mark> 를 선언하는 것이 가능합니다.

```sol
// Person 구조체를 정의
struct Person {
  uint age;
  string name;
}

// Person 을 원소로 갖는 dynamic Array 선언
Person[] public people;
```

---

# 8. Function (detail)
- Solidity 의 함수에 관해 좀더 살펴보겠습니다.

## Visibility
> - Solidity 의 함수는 아래 네 종류의 <mark>Visibility</mark> 를 가질 수 있습니다.
>   - <kbd>Public</kbd>
> 
>     : 현재 Contract 에서든, 외부에서든 모두 호출 가능합니다.
> 
>   - <kbd>Private</kbd>
> 
>     : 현재 Contract 에서만 호출 가능합니다.
> 
>   - <kbd>Internal</kbd>
> 
>     : 현재 Contract 와 이를 상속받은 Contract 들에서 호출 가능합니다.
> 
>   - <kbd>External</kbd>
> 
>     : 외부에서만 호출 가능합니다.
> 
>     : 단, ***external function f()***에 대하여 <u>아래와 같이 호출하는 것은 가능</u>하다고 합니다.
>       - ***this.f()*** 

## Return Value
- Solidity 의 함수는 아래와 같이 ***Return Value Type***을 지정할 수 있습니다.

```sol
function myFunction(uint _param1, uint _parma2) 
  // multiple value return 이 가능합니다.
  returns (uint res1, uint res2)  
{
    // ...
}
```

- 또, return value 의 name 은 생략 가능합니다.

- 함수에 관한 자세한 내용은 ([docs](https://docs.soliditylang.org/en/v0.8.17/contracts.html#functions)) 을 참고하면 됩니다.

## Mutability (view, pure)
- Solidity 함수가 *state variable* 을 변경하지 않는다면, ***view function*** 으로 선언할 수 있습니다.

```sol
function sayHello() 
  public 
  view 
  returns (string memory) 
{
  // print out the value of `memory`
}
```

- Solidity 함수가 심지어 *state variable*를 아예 참조하지 않을 때, ***pure*** 로 선언할 수 있습니다.
```sol
function sayWelcomeMessage() 
  public 
  pure
{
  // print out `Welcome`
}
```

- 다행히 <mark>Compiler</mark> 단에서 ***view***, ***pure*** 명시에 관해 알려준다고 합니다.


---

# 9. Random (Keccak256)
- Solidity 에서 ***Random*** 기능을 이용하기 위해 <mark>Keccak256</mark> 을 사용할 수 있습니다.

- <mark>Keccak256</mark> 은 SHA1, SHA2 의 안전성 문제를 보완하고자 개발된 ***해시 함수***입니다.

- Keccak에 대한 보다 자세한 내용은 [[official site](https://keccak.team/specifications.html)] 에서 확인할 수 있습니다.

- 즉, 임의의 input value $V_{i}$ 에 대하여, random output value $V_{o}$ 을 얻을 수 있습니다.

- 물론 엄밀하게 보자면 ***keccak256*** 으로 ***RANDOM*** 값을 만드는 것은 아닙니다.
  - 특정 값 $V$ 에 대한 결과값인 $F(V)$ 가 항상 예측이 되기 때문입니다.
  - 블록체인 상에서의 값들은 모두 투명하게 공개되어 있기에, 누구든 기록된 값을 이용할 수 있습니다.

---

# 10. Typecasting
- Solidity 에서는 아래와 같이 Typecasting 이 가능합니다.

```sol
uint8 a = 5;
uint b = 6;

// 1. a * b 가 uint 로 convert 
// 2. uint8 에 uint 값을 대입 -> 에러 발생
uint8 c = a * b;

// 1. uint 값 b 를 uint8로 convert
uint8 c = a * uint8(b);
```

---

# 11. Events
- Solidity 는 logging 을 위한 기능으로 <mark>Event</mark> 를 사용할 수 있습니다.

- 외부에서 Contract 내부의 Function call 을 했을 때, logging 을 쉽게 할 수 있도록 해줍니다.

- 자세한 내용은 [[docs](https://docs.soliditylang.org/en/v0.8.17/contracts.html#events)] 을 참고하면 됩니다.

---
