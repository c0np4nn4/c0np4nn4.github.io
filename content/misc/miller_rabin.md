+++
title = "(KOR) Miller-Rabin primality test"
date = 2026-04-16 
description = "Simple note on the Miller-Rabin Test"

[taxonomies]
tags = ["basic", "math"]

[extra]
math = true
+++

밀러-라빈 소수판별법은 입력으로 주어진 수가 소수인지 아닌지 확률적으로 판별하는 알고리즘이다.

# 1. 수학적 배경
{{ alert(type="note", title="페르마의 소정리", icon="tip", text="
소수 $p$와 $p$의 배수가 아닌 정수 $a$에 대하여 다음 식이 성립한다.

$$
a^{p-1} \equiv 1 \pmod{p}
$$
") }}

{{ alert(type="note", title="1의 자명하지 않은 제곱근 (non-trivial square roots of 1)", icon="tip", text="

소수 $p$에 대해 $x^2 \equiv 1 \pmod{p}$를 만족하는 $x$의 값은 
오직 $\lbrace 1, -1 \equiv p-1 \pmod{p} \rbrace$ 뿐이다.


") }}

# 2. 알고리즘 원리
임의의 정수 $n>2$에 대한 소수 판별은 다음과 같이 진행된다.

1. $n-1$ 분해하기  
    $n$이 홀수이므로 다음과 같이 적을 수 있다.
    $$
    n-1 = 2^s \cdot d
    $$
    이 때, $d$는 홀수이고, $s$는 1 이상의 정수다.


2. 밑 (base) 선택하기  
    $1 < a < n-1$ 범위의 임의의 정수 $a$를 선택한다.

3. 검사 진행  
    페르마의 소정리에 의해 $n$이 소수라면 $a^{n-1} \equiv 1 \pmod{n}$ 을 만족한다.  
    $n-1 = 2^s \cdot d$ 이므로, 아래와 같이 식을 정리할 수 있다.
    $$
    a^{n-1} = a^{2^s \cdot d} \equiv 1 \pmod{n}
    $$
    따라서, 임의의 정수 $n >2$이 소수가 되기 위해서는 아래 두 조건 중 하나라도 만족해야 한다.  
    1. $a^d \equiv 1 \pmod{n}$ 인 경우: $1$을 $s$번 제곱하는 결과
    2. $a^{2^r \cdot d} \equiv -1 \pmod{n}$ (where $0 \le r < s)$:  $-1$이 제곱되어 결국엔 $1$로 귀결되는 결과

# 3. 예외
그런데, Miller-Rabin Primality Test를 통과했다고 **무조건** 소수라 믿을 수 있는 것은 아니다.

예를 들어, base를 $a=2$로 두고 홀수인 정수 $2047$에 대한 MR test를 시도하면 아래와 같다.  

$$
2^{2047 - 1} = 2^{2046} = 2^{2 \times 1023} \equiv 1 \bmod 2047
$$

실제로 계산해보면 위와 같이 MR test를 통과한다.   
그런데, $2047 = 23 \times 89$인 합성수이므로 MR Test가 100%가 아님을 보여주는 반례이다.   

{{ alert(type="tip", text="

- 덧붙여 여러 Base에 대해 합성수이지만 MR Test를 통과하는 교묘한 수를 ***Strong Pseudoprime number***라 부른다. 
예를 들어, Base $2, 3, 5, 7$을 모두 통과하는 수는 3,215,031,751로 알려져 있다. 
이러한 pseudoprime에 대한 연구로는 `Monier-Rabin` theorem 
([MIT spring 2017 타원곡선 lecture note 12, Theorem 12.8](https://math.mit.edu/classes/18.783/2017/LectureNotes12.pdf))을 참고할 수 있다.

- 이러한 Strong pseudoprime number를 구하는 한 가지 방법으로는 
[RMPT1995](https://www.unilim.fr/pages_perso/francois.arnault/documents/papers/RMPT1995.pdf) 를 참고할 수 있다.
이 방법은 기본적으로 CRT(Chinese Remainder Theorem)와 카마이클 수(Carmichael number)를 활용한 방법이다.


") }}

