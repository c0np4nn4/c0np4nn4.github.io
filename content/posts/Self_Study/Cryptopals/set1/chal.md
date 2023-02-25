+++
title = "🔐 Set 1"
date = 2023-01-09
+++

---

### Challenge 1 [[#](https://cryptopals.com/sets/1/challenges/1)]

#### Description
- **<u>Convert hex to base64</u>**

- Base64 encoding, decoding 을 구현하는 문제입니다.

#### Solution
- `8-bit`의 ASCII 문자를 `6-bit`로 변환하고, 적절한 *padding* 을 고려해서 구현하면 됩니다.

- [[#](@/posts/Self_Study/Cryptopals/set1/base64.md)] Base64에 대한 정리

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch1.rs)]

---

### Challenge 2 [[#](https://cryptopals.com/sets/1/challenges/2)]

#### Description

- **<u>Fixed XOR</u>**

- 같은 길이의 두 hex string에 대한 `XOR` 연산을 구현하는 문제입니다.

#### Solution

- hex string 을 `Vec<u8>`로 변환하는 식으로 구현했습니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch2.rs)]

---

### Challenge 3 [[#](https://cryptopals.com/sets/1/challenges/3)]

#### Description

- **<u>Single-byte XOR cipher</u>**

- `1-Byte` 값을 Key로 이용하여, `XOR`로 암호화된 암호문을 푸는 문제입니다.

#### Solution

- $1$byte 는 $8$bits 이므로, $2^8 = 256$가지 값을 갖습니다.

- 따라서, `Brute-Force` 로 충분히 값을 찾아낼 수 있습니다.

- 문제에서는 `scoring` 을 통해, $256$개의 복호화된 결과물 중 정답을 찾는 방법을 제시합니다.

- [[Wiki](https://en.wikipedia.org/wiki/Frequency_analysis)]에서 설명하는 `빈도 분석`을 토대로, 
`single_char_key_attack`라는 이름의 [함수](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/utils/src/attack/mod.rs#L83-L143)를 만들어 문제를 해결했습니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch3.rs)]

---

### Challenge 4 [[#](https://cryptopals.com/sets/1/challenges/4)]

#### Description

- **<u>Detect single-character XOR</u>**

- 길이가 60 인 암호문들로 구성된 문제파일이 주어집니다.

- `Challenge 3` 에서 사용한 방법을 응용하여 아래와 같이 해결했습니다.

#### Solution

- 우선 각각의 암호문들에 대해 `single_char_key_attack` 을 행합니다.

- 공격의 결과값인 `Score` 중 최대값을 갖는 평문을 찾습니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch4.rs)]

---

### Challenge 5 [[#](https://cryptopals.com/sets/1/challenges/5)]

#### Description

- **<u>Implement repeating-key XOR</u>**

- 평문과 Key 의 길이가 상이할 때, Key 확장을 구현하는 문제입니다.

#### Solution

- 문자열의 길이를 맞춘 후, `Challenge 2` 에서 구현한 `Fixed XOR` 을 이용하면 됩니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch5.rs)]

---

### Challenge 6 [[#](https://cryptopals.com/sets/1/challenges/6)]

#### Description

- **<u>Break repeating-key XOR</u>**

- *임의의* 키를 이용해 **Repeating-Key XOR**(`Challenge 5`) 로 암호화된 암호문을 해독하는 문제입니다.

- 암호문과 알고리즘만 알고 있으므로, *Cipher-text Only Attack* 으로 분류할 수 있습니다.

#### Solution

- 문제를 해결하는 순서는 아래와 같습니다.
> 1. 적절한 `KEY_SIZE` 를 찾습니다.
> 
> 2. 적절한 `KEY` 를 복원합니다.
> 
> 3. `KEY`를 이용해 복호화 한 뒤, 결과를 확인합니다.

##### 1. Finding `KEY_SIZE`
- `KEY_SIZE` 를 2 ~ 40 으로 가정하고, 아래의 과정을 진행합니다.
> 1. 전체 암호문 `CT`를 `KEY_SIZE` 단위의 chunk로 나눕니다.
> 
> 2. chunk들을 한 쌍씩 묶어 *hamming_distance* 를 구합니다.
> 
> 3. *hamming_distance* 값을 `KEY_SIZE` 로 나눕니다.
>       - 즉, 단위 `SIZE`에 대한 *hamming_distance* 값을 구합니다.
>   
> 4. 이 값이 가장 작을 때의 `SIZE`가 실제 `KEY_SIZE`일 확률이 높습니다.

- 값이 작을 때, `KEY_SIZE` 일 확률이 높은 이유는 아래와 같습니다. 

- [crypto.stackexchange site](https://crypto.stackexchange.com/questions/8115/repeating-key-xor-and-hamming-distance) 를 참고하였습니다.

> - *hamming_distance* 값을 구하는 함수 $hd$를 아래와 같이 정의합니다.
> $$hd(x, y) = r$$
> - $1$byte 값 $X, Y$에 대하여
>     - 만약 $X, Y$ 가 무작위 $1$byte 값이라면, $hd(x, y) = 4$ 일 가능성이 높다.
>     - 만약 $X, Y$ 가 문자열 값이라면, $hd(x, y) = 2, 3$ 일 가능성이 높다.
> 
> - 문자열은 **ASCII**로 표현되기 때문에 일종의 패턴을 갖습니다.
> - $\therefore$ *hamming_distance* 에 대해 무작위 값들과 차이가 있음을 이용한 방법입니다.

##### 2. Recover `KEY`
- `KEY_SIZE`를 구한 뒤에는 아래와 같은 아이디어로 `KEY`를 복원할 수 있다. 
> - `KEY_SIZE` 크기의 chunk 들에 대해, 특정 index 의 값은 동일하다.
> - 즉, chunk[0], chunk[1], chunk[2], ... 에 대하여
> - chunk[0][k], chunk[1][k], chunk[2][k], ... 는 모두 같은 `KEY` byte 를 사용한다.

##### 2. Decrypt `Ciphertext` w/ `KEY`

---

### Challenge 7 [[#](https://cryptopals.com/sets/1/challenges/7)]

#### Description

#### Solution

---

### Challenge 8 [[#](https://cryptopals.com/sets/1/challenges/8)]

#### Description

#### Solution

---


<!-- Math rendering -->
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML' async></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']]
  },
  TeX: {
    extensions: ["AMSmath.js"],
  }
});
</script>
<!-- Math rendering -->
