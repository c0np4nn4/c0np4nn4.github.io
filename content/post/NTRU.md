+++
title = "NTRU"
description = "Cryptography"
date = 2023-06-20
toc = true

[taxonomies]
categories = ["PQC", "Cryptography"]
tags = ["cryptography"]

[extra]
math=true
+++

---
> 본 글은 [***HPS98***](https://www.ntru.org/f/hps98.pdf) 를 토대로 작성되었습니다.

# 📌 Introduction
- <txtred>**NTRU**</txtred>는 `Public-Key Cryptosystem` 입니다.
- `Encryption`은 *polynomial algebra* 와 두 수 $p$, $q$에 대한 $\bmod$ `reduction` 연산에 기반을 둔 `mixing system` 을 사용합니다.
- `Decryption`은 *elementary probability theory* 에 유효성을 의존하고 있는 `unmixing system`을 사용합니다.

