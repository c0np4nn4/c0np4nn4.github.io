+++
title = "Delegation"
date = "2023-08-24"
+++


---

> My Solutions: [github](https://github.com/c0np4nn4/EtherStudy/tree/main/ethernaut_solution)

---

# TL;DR
> ***Under/Overflow*** 문제입니다.

<center>
<img alt="main image" src="https://images.unsplash.com/photo-1568054491179-fbc8c8d40c8c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80" />
</center>

---

# Introduction

---

# Problem Detail
## Goal

## Contract

---

# Exploit
## Strategy

## Code


## Result
실행 화면은 아래와 같습니다.

<center>
<img alt="result" src="../../ethernaut_img/5_token_3.png" />
</center>

`player` 는 초기에 `20`만큼의 토큰을 갖고 있었고, `transfer()`함수를 이용해 `21`만큼의 토큰을 `owenr`에게 보내고자 했습니다.
***Underflow*** 가 발생하 여`require()` 문에 의한 검증도 우회하게 되었으며, `player`가 `uint`의 최대값을 가짐으로써 손쉽게 문제를 해결했습니다.

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
