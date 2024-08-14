+++
title = "Info Sec 시험 전 정리"
date = "2024-04-24"
+++

# 1. 고전 암호
## 1-1. Shift Cipher
- Caesar Cipher
### Attack Model
- 1. COA
- 2. KPA
- 3. CPA
- 4. CCA
## 1-2. Substitution Cipher
- 단순한 치환
    - *Key Space* 와 *Security Level* 이 같지 않음
    - **빈도 분석 공격**
        - COA
- Multi-Letter
    - `Playfair`
        - 여전히 **빈도 분석 공격** 가능
    - `Vigenere`

## 1-3. Transposition Cipher
- *Rail Fence* Cipher
- *Row Transposition* Cipher

## 1-4. Enigma
- **Rotor Machine**

---

# 2. 블록 암호
## 2-1. 스트림 암호
- 암/복호화가 전부 `XOR`
### 2-1-1. Synchronous, Asynchronous Stream Cipher
- *Synchronous*
    - Key 만 가지고 `Key Stream` 생성
- *Asynchronous*
    - Key + Cipher text 가지고 `Key Stream` 생성
    - `Feed back` 이 존재함
### 2-1-2. XOR 사용 이유
- `Key Stream` 이 "랜덤"을 보장한다면, `XOR` 연산 결과도 "랜덤"을 보장함 (50%확률로 0, 1)
### 2-1-3. LFSR
- 아래와 같은 수식으로 만들 수 있음

$$
x_{m+3} = x_{m+1} \oplus x_m
$$

- Key 길이가 $n$ 일 때, Sequence 길이는 $2^n - 1$ 이하
- **Linear** 이므로, 쉽게 공격할 수 있음
    - KPA
        - 평문, 암호문 쌍을 $2n$ 길이 만큼만 알아도 전부 복원 가능
- 따라서, **Non-Linear** 요소를 추가해야함

### 2-1-4. A5/1
- `Majority` 개념을 추가하여, 3개의 *LFSR* 중 어느 것을 선택할지에 관해 *non-linear* 를 적용함

### 2-1-5. Trivium
- *Init*, *Warm-Up*, *Encryption* 단계로 동작

## 2-2. 블록 암호
### 2-2-1. S-P Network
- Claude Shannon
    - **S**ubstitution: Confusion
    - **P**ermutation: Diffusion
### 2-2-2. Feistel Cipher
- `S-P Network` 의 구현체
- `(L, R)`, `F function`
### 2-2-3. DES
- Structure
- Encryption
    - IP, round, 32-bit swap, $\text{IP}^{-1}$
    - Key Schedule
- `Round` 함수의 `F`가 **non-linear** 임
- `Key Schedule`
    - *PC1*, *PC2*
    - 56-bit -> 28-bit -> 24-bit -> 48-bit 과정
### 2-2-4. DES Cryptanalysis
- `Key Size`
    - $2^{56}$ 인데, 1999년에 22시간 만에 뚫린 사례가 나옴
- `Timing Attack`
    - 부채널 공격
- `Differential Cryptanalysis`
    - (딥하게 안함)
    - *encryption* 의 두 결과를 바탕으로, '차이'를 분석함
    - 확률적인 방법
- `Linear Cryptanalysis`
    - 선형적인 근사를 찾는 방법
    - $2^{47}$ 까지 줄일 수 있는데, 여전히 큰 숫자임
## 2-3. Modes of Operation
### 2-3-1. ECB
- 쓰면 안됨
### 2-3-2. CBC
- `Ct`가 다음 블록에 연결된 구조
### 2-3-3. CFB
- `IV`, `CT` 값으로 **Encryption** 하고, 그 결과를 `PT`와 XOR 하는 방식
- 다음 블록과 연결되는 것이 `CT` 임
- DES Encryption 만을 사용함
- 오류 전파
- 암호화는 순차적, 복호화는 병렬 가능

### 2-3-4. OFB
- `CFB` 랑 유사함
- 다음 블록과 연결되는 것은 **Encryption** 결과 (output) 임
- DES Encryption 만을 사용함
- 오류 전파 없음
- 암/복호화 모두 병렬로 처리 가능

### 2-3-5. CTR
- Counter 값을 암호화 하고, 그 결과를 `PT` 와 XOR 함
- 병렬 가능
- key, counter 를 재사용하면 안됨

### 2-3-6. GCM-CTR
- *Encryption*
    - CTR Mode 로 진행
- *Authentication*
    - GCM 으로 진행
    - `Tag` 하나 만드는 과정

---

# 3. Finite Fields
## 3-1. Group, Ring, Field
### 3-1-1. Group
- `수의 집합` + 아래 성질을 만족하는 `연산`
    - Closure
    - Associative Law (결합 법칙)
    - has Identity
    - has Inverse
- 만약 *commutative* (교환 법칙) 도 성립한다면
    - **Abelian Group**
- `Permutation Group`
    - *Permutation* 여러 번은 한 번과도 같음
    - 따라서, 여러 번 해도 "security"가 올라가지 않음
#### 3-1-1-1. Subgroup
- Group $G$ 의 원소 중 일부를 갖고 $G$ 의 연산자도 적용될 때, 해당하는 $H$ 를 *Subgroup* 이라 부른다 함
- 아래는 아님

$$
<Z_{10}, +> \quad \nsubseteq \quad <Z_{12}, +>
$$

#### 3-1-1-2. Cyclic Subgroup
- 원소 $a$ 를 갖고 생성할 수 있는 `Subgroup`
- $a^0=e$ 임

#### 3-1-1-3. Cyclic Group
- 원소 $g$ 를 갖고 group $G$ 를 생성할 수 있을 때의 **Group** $G$

#### 3-1-1-4. Abelian Group
- *commutative* 있으면 Abelian 임

## 3-1-2. Ring
- 연산 2개:  덧셈, 곱셈
    - `덧셈`
        - closure
        - associative
        - identity
        - inverse
        - commutative
    - `곱셈`
        - closure
        - associative
        - distributive over addition

### 3-1-2-1. Ring+
- **Commutative Ring**
    - `곱셈`이 *commutative* 만족할 때
- **Integral domain**
    - `곱셈`이 *identity* 를 가질 때
    - `곱셈`이 *zero-divisor* 가 없을 때

### 3-1-3. Field
- 덧셈, 곱셈 둘 다 **Abelian Group** 임
- 곱셈은 여전히 *distributive over addition* 만족
- 덧셈
    - $\\{F, +\\}$
- 곱셈
    - $\\{F - \\{0\\}, *\\}$

## 3-2. Modular Arithmetic
$$
a = qn + r
$$
- $0 \le r < n$
- $q=\lfloor a / n \rfloor$
- **나머지 연산** 으로 보면 됨
- *Divisor*
    - $2 \mid 24$
    - $12 \mid 24$
    - *나누어 떨어진다*
- `합동`(congruence)
$$
a \equiv b \bmod n \quad \text{iff} \quad n \mid (a-b)
$$

- `Commutative Ring` 을 형성함
    - 곱셈에 대해서 *Inverse* 가 있다고 장담할 수 없음
    - 아래와 같이 $N$ 에 대해 서로소인 경우만 고려할 때도 있음

$$
\begin{align}
(ab) \equiv (ac) \bmod n \newline
\Rightarrow b \equiv c \bmod n
\end{align}
$$

- 위 식은 $a$ 와 $n$ 이 `서로소`일 때만 성립함

## 3-3. Euclidian Algorithm
- `최대공약수` 구하는 알고리즘

$$
\gcd(a, b) = \gcd(b, a \bmod b)
$$

- 증명하면 됨

## 3-4. Finite Field of the form $GF(p)$
- 유한체 중 *order* 가 $p^n$ 인 것들을 `Galois Field` (GF) 라 부름
- $n=1$: prime field, $GF(p)$
- $n>1 \text{and} p=2$: binary field, $GF(2^n)$

### 3-4-1. GF(p)
- $Z_p = \\{0, 1, \dots, p-1\\}$
- $p$ 가 $Z_p$ 의 원소들과 모두 `서로소`이므로, "Finite Field" 임

### 3-4-2. GCD & EEA
- 베주 다항식으로 $gcd(r_0, r_1)=1$ 이면 둘 중 하나를 moduli 로 사용해서 역원을 찾아내는게 가능하다는 식의 설명

## 3-5. Polynomial Arithmetic
- 대충 coefficient 를 binary 값으로 쓸 수 있다는 설명
- *Polynomial GCD*

## 3-6. Finite Field of the form $GF(2^n)$
- moduli 가 `irreducible polynomial`
    - 정수의 `소수` 같은 느낌
- "나머지" 는 알아서 계산하면 됨
- 컴퓨터 상에서의 연산을 위해, 약간의 트릭이 있음

# 4. AES
- `Feistel` 이 아니라 `Iterative` 방식으로 설계됨
## 4-1. Round 구성
- **BS**
- **SR**
- **MC**
- **ARK**
- **BS** 에서만 *Substitution* 을 통한 *non-linear* 적용
    - byte-by-byte substitution
    - DC, LC 에 대한 방지
- **SR**
    - byte-by-byte permutation
- **MC**
    - $GF(2^8)$ 에 대한 연산으로 substitution 함
    - **SR** 과 비슷한 목적
- ***ARK***
    - Key XOR 하는 과정
- **Invert 함수** 를 구할 수 있음
- **SR, MC** 는 diffusion-layer
- **MC** 에서 하는 연산이 전부 $GF(2^8)$ 에서의 연산
## 4-2. Key Schedule
- 128-bit 기준
- `g`함수
    - RotWord
    - SubWord
    - RConstant
## 4-3. S-box
- $GF(2^8)$ 상에서의 연산으로 S-Box 만들 수 있음
- Affine * Inverse of x + Constant = S-box result
## 4-4. Decryption
- 전부 **Invertible Function**
- 순서 그대로 하다보니 좀 밀림

| | 1 | 2 | 3 | 4 |
|---|:-:|:-:|:-:|:-:|
|Encrypt |BS|SR|MC|ARK|
|Decrypt |ISR|IBS|ARK|IMC|

- ISR, IBS 는 순서 바꿔도 상관 없다고 함
- ARK 가 아니라 `IARK` 로 바꾸는 수식이 있음

