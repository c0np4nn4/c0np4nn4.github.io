+++
title = "🔐 Cryptopals"
date = 2022-12-15

[taxonomies]
tags = ["kr", "cryptography", "cryptopals", "rust"]
+++

# Cryptopals

- [[Cryptopals](https://cryptopals.com/)] 의 문제들을 풀고 정리하는 글이다.

- 전체 소스코드는 [[github](https://github.com/c0np4nn4/cryptopals)] 에 있으며, `Rust` 언어를 사용했다.

---

# Set 1
- `Set 1` 에서는 본격적인 *Cryptography* 학습에 앞서, 간단한 예제들을 구현하고 학습한다.

- [[solutions](https://c0np4nn4.github.io/cryptopals/set1/challenges)]



<br/>


<!-- <details style="cursor:pointer"> -->
<!--   <summary style="font-weight:bold"> -->
<!--   Challenge 3: Single-byte XOR -->
<!--   </summary> -->
<!--   <p> -->


<!-- - Key가 1-byte 일 때, key recovery 를 하는 문제이다. -->

<!-- - 256 가지 경우이기 때문에 직접 찾아도 되지만, 문제에서는 알파벳의 빈도를 이용하는 방법을 제시하고 있다. -->

<!-- - 좀 더 정확히는, 1-byte brute-force attack 의 결과들에 대한 `점수`를 매겨서 최댓값일 때를 찾는 방식이다. -->

<!-- - 나는 Wiki의 [Frequency Attack](https://en.wikipedia.org/wiki/Frequency_analysis) 를 참고하여 테이블을 작성했다.  -->

<!-- - <u>[[코드 Github]](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/utils/src/attack/mod.rs#L79)</u> -->

<!--   </p> -->
<!-- </details> -->

<!-- <br/> -->

<!-- <details style="cursor:pointer"> -->
<!--   <summary style="font-weight:bold"> -->
<!--   Challenge 4: Detect single-character XOR  -->
<!--   </summary> -->
<!--   <p> -->



<!-- - **Challenge 3** 에서 사용한 방법을 그대로 사용하여, 여러 ciphertext 중 Key 가 1-byte character인 것을 찾는 문제이다.  -->

<!-- - 나는 전체 ciphertext에 대해 **Challenge 3**에서 사용한 key recovery를 적용해서 각각에 대한 최대 점수를 얻고, -->
<!-- 그 중 최대 점수를 얻은 `plaintext` 를 정답이라 생각했다. -->

<!-- - <u>[[코드 Github]](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/solutions/set1/ch4.rs)</u> -->

<!--   </p> -->
<!-- </details> -->

<!-- <br/> -->

<!-- <details style="cursor:pointer"> -->
<!--   <summary style="font-weight:bold"> -->
<!--   Challenge 5: Implement repeating-key XOR  -->
<!--   </summary> -->
<!--   <p> -->


<!--   - **Challenge 2** 에서의 *Fixed length XOR*과 비슷한 문제이다. -->

<!--   - 다만, `key` 가 single byte가 아니며 plaintext(ciphertext)의 길이에 맞춰 확장된다는 점만 유의하면 된다. -->

<!--   - <u>[[코드 Github]](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/utils/src/xor.rs#L31-L51)</u> -->

<!--   </p> -->
<!-- </details> -->

<!-- <br/> -->

<!-- <details style="cursor:pointer"> -->
<!--   <summary style="font-weight:bold"> -->
<!--   Challenge 6: <u>Break repeating-key XOR</u> -->
<!--   </summary> -->
<!--   <p> -->


<!--   - **Challenge 3** 에서와 같이 *Key Recovery*를 하는 문제이지만, 여기서는 ***key size*** 를 알려주지 않고 있다. -->

<!--   - [[홈페이지]](https://cryptopals.com/sets/1/challenges/6)에서 제시하는 방법은 아래와 같다. -->


<!--   1. `KEYSIZE`를 구한다. -->

<!--       1. `KEYSIZE` 를 2 ~ 40 정도로 추측한다. -->

<!--       2. 전체 Data를 `KEYSIZE` 크기의 chunk로 분할하고, <br /> chunk를 2개씩 짝지어 `hamming distance` 를 구한다. -->

<!--       3. 구한 `hamming distance` 들의 합을 모두 더한 뒤, `KEYSIZE`로 나누어 평균을 구한다. -->

<!--       4. 이 평균값 중 가장 작은 값을 갖는 `KEYSIZE` 가 실제 `KEYSIZE`일 확률이 높다. <br /> -->
<!--       혹시 모르니 <u>2, 3등까지도 후보군</u>으로 두거나 <u>chunk를 4개씩</u> 짝지어 구해볼 필요도 있다. -->

<!--   2. 전체 Data를 `KEYSIZE` 크기의 chunk로 분할하고, -->
<!--   각 chunk 의 index 에 대하여 <br /> **Challenge 3**에서 했던 것과 동일하게 *Single-byte Key Recovery*를 진행한다. -->

<!--       - <그림 추가 예정> -->


<!--   - 우선, `KEYSIZE`를 구할 때 *hamming distance*를 사용한 방법에 대한 이해는<br /> [[Stack Exchange]](https://crypto.stackexchange.com/questions/8115/repeating-key-xor-and-hamming-distance)를 참고하였다. -->

<!--   - 위 링크에 대한 설명을 간략히 정리하면, 아래와 같다.<br/> -->


<!--       - 임의의 1-byte X, Y에 대하여  -->
<!--       <br /> X, Y 가 정말로 랜덤한 8-bits 값이라면 `XOR` 연산의 결과가 평균적으로 <u>**4**</u>만큼 차이가 나야 한다. -->
<!--       <br /> 하지만 X, Y 가 문자라면 <u>**2~3**</u> 정도만 차이가 난다.  -->

<!--       - 적법한 `KEYSIZE`를 찾은 경우라면, (X ^ K) ^ (Y ^ K) == X ^ Y 이겠지만, <br /> -->
<!--       그렇지 않은 `KEYSIZE`라면 (X ^ <u>K_1</u>) ^ (Y ^ <u>K_2</u>) == <u>X</u> ^ <u>Y</u> ^ <u>K_1</u> ^ <u>K_2</u> 이므로, <br /> -->
<!--       임의의 1-byte 끼리 `XOR`한 결과와 다르지 않을 것이다. -->

<!--       - 따라서, 적법한 `KEYSIZE`를 찾은 경우에는 *hamming distnace*의 평균값이 **작을**  것이다. -->



<!--   - [[코드 Github]](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/challenges/src/solutions/set1/ch6.rs) -->


<!--   </p> -->
<!-- </details> -->

