+++
title = "Challenge 7"
description = "AES in ECB mode"
date = 2023-03-03
draft = false

[taxonomies]
categories = ["Cryptopals"]
tags = ["Cryptopals","Rust", "AES"]

[extra]
toc = true
keywords = "Cryptography, Cryptopals, Rust, AES, ECB"
+++

---

> - 전체 코드는 [[Github Link](https://github.com/c0np4nn4/cryptopals/tree/main/cryptopals/challenges/src/set1)] 에서 확인할 수 있습니다.

---

# Challenge 7 [[#](https://cryptopals.com/sets/1/challenges/7)]

> <u>**AES in ECB mode**</u>

- <mark>AES in ECB</mark> 로 암호화된 값을 복호화하는 문제입니다.

- 문제에서는 <mark>OpenSSL</mark>을 이용해 <kbd>CLI</kbd> 환경에서 문제를 풀기보다, 직접 코드를 작성할 것을 권하고 있습니다.

- 학습 목적으로 <mark>AES</mark> 알고리즘을 [[구현](https://github.com/c0np4nn4/cryptopals/tree/main/cryptopals/utils/src/crypto/aes/core)] 해보았습니다.

- 위 코드를 작성하기 위해 학습한 내용을 [[AES 정리 글](@/cryptopals/AES.md)] 에 기록해 두었습니다.

