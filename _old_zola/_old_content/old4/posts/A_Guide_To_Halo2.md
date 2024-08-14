+++
title = "A Guide To Halo2, KR"
date = "2024-02-29"
+++

# Circuit Structure
- 본격적으로 시작하기에 앞서 `Halo2`의 `circuit` 구조를 살펴봅니다.
- `Halo2`의 `circuit` 구조는 [Plonk](https://eprint.iacr.org/2019/953.pdf), [Groth16](https://eprint.iacr.org/2016/260.pdf) 과 차이가 있습니다.
- 간단히 말해, PLONK/Groth16 `constraint system`은 독립적으로 구성되며, 각 `constraint system`은 **해당 행**의 변수에만 영향을 미치고 곱셈 또는 덧셈과의 조합이라는 특정 연산만 지원합니다.
- 그러나, `Halo2`는 **본인 행**외에도 참조할 수 있습니다.

<center>
<img src="../../zkp/halo2_matrix_description.png" />
</center>

- `Halo2` 문서에 따라 개념을 정리하면 아래와 같습니다.
    - ***Statement*** 를 증명하는 것이 ***Proof System*** 입니다.
    > - ***public input*** 에 따라 ***Statement*** 는 달라집니다.
    > - ***private input*** 에 의해 ***Statement*** 가 유의미해집니다.
    - ***Releation*** 은 유효한 ***public/private input*** 조합을 특정하는 방법입니다.
        - ***Circuit*** 은 ***Releation*** 의 구현입니다.
    - ***arithmetization*** 은 ***Circuit*** 을 표현하기 위해 사용하는 언어(language)입니다.
    - `Halo2` 에서 사용하는 ***arithmetization*** 은 `PLONK`를 기반으로 한 `PLONKish arithmetization` 입니다.
- 위 그림의 *`PLONKish Circuits`* 은 이러한 `Halo2`의 ***arithmetization*** 을 기반으로 한 `Circuit`을 의미합니다.
- 그리고, **직사각형 행렬**로 표기됩니다.

---

- `Circuit` 은 크게 세 종류의 **Columns** 으로 구성됩니다.
    - `advice column`: *witness*
    - `instance column`: *public input* 또는 prover와 verifier 사이에 공유되는 모든 데이터
    - `fixed(selector) column`: gate를 on and off

---

# 용어
- `Chip`
    - `Circuit`을 구성하는 단위입니다.
- `Config`
    - `Columns` 에 대한 설정입니다.
- `Instructions`
    - 각 `Chip`과 함께 제공되는 것으로, `Instruction`을 호출함으로써 `Chip`을 `Circuit`에 포함할 수 있습니다(?)
- `Layouter`
    - `Layout`은 `Chip`을 `Circuit`에 포함하기 위해 사용합니다.
    - `Layouter`를 호출하여 `Layout`을 구현할 수 있습니다.
- `Assignment`
    - 어떤 **값**을 `circuit`에 할당하기 위해 사용됩니다.
