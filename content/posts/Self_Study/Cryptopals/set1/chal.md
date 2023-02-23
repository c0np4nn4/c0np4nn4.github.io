+++
title = "🔐 Set 1"
date = 2023-01-09
+++

---

### Challenge 1 [[#](https://cryptopals.com/sets/1/challenges/1)]

- Base64 encoding, decoding 을 구현하는 문제입니다.

- `8-bit`의 ASCII 문자를 `6-bit`로 변환하고, 적절한 *padding* 을 고려해서 구현하면 됩니다.

- [[#](@/posts/Self_Study/Cryptopals/set1/base64.md)] Base64에 대한 정리

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch1.rs)]

### Challenge 2 [[#](https://cryptopals.com/sets/1/challenges/2)]

- 같은 길이의 두 hex string에 대한 `XOR` 연산을 구현하는 문제입니다.

- hex string 을 `Vec<u8>`로 변환하는 식으로 구현했습니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch2.rs)]

### Challenge 3 [[#](https://cryptopals.com/sets/1/challenges/3)]

- `1-Byte` 값을 Key로 이용하여, `XOR`로 암호화된 암호문을 푸는 문제입니다.

- $1$byte 는 $8$bits 이므로, $2^8 = 256$가지 값을 갖습니다.

- 따라서, `Brute-Force` 로 충분히 값을 찾아낼 수 있습니다.

- 문제에서는 `scoring` 을 통해, $256$개의 복호화된 결과물 중 정답을 찾는 방법을 제시합니다.

- 저는 [[Wiki](https://en.wikipedia.org/wiki/Frequency_analysis)]에서 설명하는 `빈도 분석`을 토대로, 
`single_char_key_attack`라는 이름의 함수를 만들어 문제를 해결했습니다.

- [[github](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/set1/ch3.rs)]


### Challenge 4 [[#](https://cryptopals.com/sets/1/challenges/4)]
### Challenge 5 [[#](https://cryptopals.com/sets/1/challenges/5)]
### Challenge 6 [[#](https://cryptopals.com/sets/1/challenges/6)]
### Challenge 7 [[#](https://cryptopals.com/sets/1/challenges/7)]
### Challenge 8 [[#](https://cryptopals.com/sets/1/challenges/8)]


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
