+++
title = "Why and how zk-SNARK works (2)"
date = "2024-04-21"
+++

> 그림 추가 예정입니다.

# 3. Non-interactive Zero-knowledge of Polynomial

## 3.1 다항식에 대한 ‘지식’을 증명하기
  현재까지 완성된 프로토콜은 사실 서로 간의 *신뢰* 를 바탕으로 동작합니다. `Prover` 가 다항식을 통해 값을 계산할 **강제**할 방법이 없습니다. 심지어는 계산 값인 $v’$ 의 범위가 그리 넓지 않다면 확률적으로 *찍어서* 맞추는 것도 가능합니다.
  본격적으로 수학적인 방법을 꺼내기 전에 “*다항식을 알고 있음*” 의 의미를 짚어보겠습니다. 예를 들어, 아래와 같은 다항식이 있다고 해보겠습니다.

$$
c_n x^n + \dots + c_1 x^1 + c_0 x^0
$$

  위와 같은 다항식을 **알고 있다** 는 것은, `계수(coefficient)` 들을 알고 있다는 말과 같습니다. 즉, 아래 값만 알고 있으면 됩니다.

$$
\\{c_n, \dots, c_1, c_0\\}
$$

`계수` 는 $0$ 을 포함한 값이므로 다항식의 `차수` $d$ 는 $n$ 임을 알 수 있습니다.

---
## 3.2 인수분해 (Factorization)
  대수학의 기본정리를 통해 임의의 다항식은 선형 다항식(i.e. 선으로 표현되는 1차 다항식)으로 인수분해가 됨을 알 수 있습니다. 결과적으로, 최고차항의 계수가 $1$인 임의의 다항식은 아래와 같이 표현될 수 있습니다.

$$
(X-a_0)(x-a_1)\cdots(x-a_n) = 0
$$

  인수에 해당하는 선형 방정식들 중 하나라도 $0$ 이 되면 등식이 성립하므로, $a_k, \quad (k = 0, 1, \dots , n)$ 가 유일한 해임을 알 수 있습니다.

  만약 `Prover` 가 아래와 같이 주장한다고 해보겠습니다.

> “*나는 $1, 2$ 를 해로 갖는 3차 다항식 $p(x)$을 알고 있다.*”
> 
> --> `Prover` 가 실제로 알고 있는 다항식은 $p'(x)$ 로 표기합니다.

  앞서 살펴본 내용을 토대로 아래와 같은 수식을 세워볼 수 있습니다.

$$
\begin{align}
p(x) &= (x-1)(x-2) \cdot h(x) \newline
&= t(x) \cdot h(x)
\end{align}
$$

  위 식에서의 $t(x)$ 를 ***target polynomial*** 라 부르겠습니다. `Prover` 가 정말 다항식 $p(x)$ 를 알고 있는 사람일 때만, *target polynomial* 을 이용해서 $h(x)$ 를 온전하게 구할 수 있습니다. 예를 들어, $p(x)$ 가 아래와 같다고 해보겠습니다.

$$
\begin{align}
p(x) &= x^3-6X^2+11x-6 \newline
&= (x-1)(x-2)(x-3)
\end{align}
$$

  이 때 `Prover` 가 실제로는 $p(x)$ 를 알지 못하고, 아래와 같은 $p’(x)$ 를 알고 있다고 가정해보겠습니다.

$$
\begin{align}
p’(x) &= x^3-7x^2+14x-8 \newline
&= (x-1)(x-2)(x-4)
\end{align}
$$

  그럼 `Prover` 가 구한 $h'(x)$ 값은 아래와 같습니다.

$$h’(x) = \frac{p’(x)}{t(x)} = \frac{(x-1)(x-2)(x-4)}{(x-1)(x-2)} = (x-4)$$ 

  이는 기대했던 값인 $h(x)$ 와 다릅니다.

$$h(x) = \frac{p(x)}{t(x)} = \frac{(x-1)(x-2)(x-3)}{(x-1)(x-2)} = (x-3)$$ 

이를 기반으로 아래와 같이 새로운 프로토콜을 만들 수 있습니다.

| | | 방향 | | 설명 |
| --- | --- | --- | --- | --- |
| 1 |  |  | `Verifier` | `Verifier` 는 임의의 값 $r$ 에 대하여, $t(r) = t$ 를 미리 계산합니다. |
| 2 | `Prover` | $\leftarrow$ | `Verifier` | `Verifier` 는 `Prover` 에게 값 $r$을 보냅니다. |
| 3 | `Prover` |  |  | `Prover` 는 $h'(x)=\frac{p'(x)}{t(x)}$ 를 계산합니다.  |
| 4 | `Prover` |  |  | `Prover` 는 $h'(x), p'(x)$ 와 `Verifier`로부터 받은 값 $r$을 이용해 $h'(r)=h', p'(r)=p'$을 계산합니다.  |
| 5 | `Prover` | $\rightarrow$ | `Verifier` | `Prover` 는 `Verifier` 에게 $h', p'$ 를 보냅니다. |
| 6 |  |  | `Verifier` | `Verifier` 는 $p' = h' \cdot t$ 임을 검사합니다. 만약 등식이 성립하면, $p'(x)$ 는 $t(x)$ 를 여인수(cofactor)로 가짐이 증명됩니다.|

  위 프로토콜도 문제점을 여전히 갖고 있습니다. `Prover` 가 $p(x) \ne p’(x)$ 인 $p’(x)$ 를 알고 있다고 해보겠습니다. 이런 상황에서도 `Prover` 는 아래 수식을 만족하기만 하면 $p(x)$ 를 알고 있는 것처럼 프로토콜을 속이는 것이 가능합니다. 

$$
p’ = t \cdot h’
$$

  이는 `Verifier` 가 아래 두 값이 **정수** 인지 검사하는 것으로 방지할 수 있습니다.

$$
h’(r) = \frac{p’(r)}{t(r)} = h’, \quad p’(r) = p’
$$

다시 말해, $h’(x) = \frac{p’(x)}{t(x)}$ 가 **나머지** 를 남기지 않을 때, $p’(x) = p(x)$ 라고 인정할 수 있습니다.
  다음 장에서부터는 ***암호학*** 을 활용해서 이러한 기능들을 추가해나갑니다.

## 3.3 Obscure Evaluation
  앞서 `Prover` 는 아래 수식을 통해 $p(x)$ 에 대한 지식을 거짓으로 증명할 수 있었습니다.

$$
p’ = t \cdot h’
$$

  이는 `Prover` 가 아래 두 값을 **그대로** 알고 있기 때문입니다.

$$
r, t(r)
$$

  따라서, 위 두 값을 “*암호화된 채로 사용할 수 있는*” 방법이 있다면 이를 방지할 수 있습니다. 이에 적합한 암호학 방법이 바로 `동형암호화 (Homomorphic Encryption)`입니다.

### 3.3.1 동형암호
`동형암호` 를 이용하면 특정 값 $v$ 를 암호화한 뒤 연산을 적용하는 것이 가능합니다. 여기서는 지수 연산으로 `동형암호`를 간단히 이해해보도록 하겠습니다.

예를 들어, $5$ 를 *base value* 로 두고 $3$ 을 “암호화” 하면 아래와 같습니다.

$$
Enc(3) = 5^3 = 125 = ct
$$

만약 암호화된 값($3$)을 $2$배 하고 싶으면, $ct$ 를 아래와 같이 제곱하면 됩니다.

$$
{ct}^2 = {(5^3)}^2 = 5^6
$$

만약 암호화된 값($3$)에 $2$ 를 더하고 싶으면, $ct$ 에 아래와 같이 암호화된 값을 곱하면 됩니다.

$$
ct \cdot 5^2 = (5^3) \cdot 5^2 = 5^{3+2} = 5^5
$$

그런데, *base value* 인 $5$ 가 공개되어 있기 때문에 암호문 ($ct$) 가 $1$이 될 때까지 나누면 암호화한 값($3$) 을 알아내는 것이 가능합니다. 따라서, **지수 연산** 과 관련한 수학 개념을 하나 더 도입하게 됩니다.

### 3.3.2 모듈로 연산
모듈로 연산은 정수 $N$ 에 대해, $\\{0, 1, \dots, n-1\\}$ 의 수만을 사용하는 모듈러스 공간 상에서 수행됩니다. 정말 간단히는 **나머지 연산** 으로 이해할 수 있습니다.

$$
\begin{align}
3 \times 5 = 15 = (6 * 2) + 3 \quad &(\bmod 6) \newline
5 + 2 = (6 + 1) = 1 \quad &(\bmod 6)
\end{align}
$$

또한, 연산의 순서가 중요하지 않습니다.

$$
\begin{align}
(2 \times 4 - 1) \times 3 \equiv 3 \quad &(\bmod 6) \newline
\newline
2 \times 4 \equiv 2  \quad &(\bmod 6) \newline
2 - 1 \equiv 1       \quad &(\bmod 6) \newline
1 \times 3 \equiv 3  \quad &(\bmod 6) \newline
\end{align}
$$

모듈로 연산이 유용한 이유는 아래 수식을 통해 바로 이해할 수 있습니다.

$$
\begin{align}
5 \times 4 \equiv 2 \quad &(\bmod 6) \newline
4 \times 2 \equiv 2 \quad &(\bmod 6) \newline
2 \times 1 \equiv 2 \quad &(\bmod 6) \newline
\end{align}
$$

위 수식은 모두 $2 \quad (\bmod 6)$ 로 계산되지만, 피연산자가 유일하지 않습니다. 따라서, **정보를 숨기면서 연산** 하는 것이 가능합니다.

### 3.3.3 Strong Homomorphic Encryption
이제 `동형암호` 와 `모듈로 연산` 을 모두 활용해 아래와 같은 수식을 생각해볼 수 있습니다.

$$
\begin{align}
5^5 &\equiv 3    \quad &(\bmod 7) \newline
5^{11} &\equiv 3 \quad &(\bmod 7) \newline
5^{17} &\equiv 3 \quad &(\bmod 7) \newline
\end{align}
$$

만약 위 식에서는 $7$인, *moduli* $N$ 값이 충분히 크면 암호화된 값(지수)을 알아내는 것이 거의 불가능합니다. 또한, 현대암호(*RSA*, *ECC* 등)들도 이러한 “어려운 문제” 를 기반으로 하고 있습니다.

`모듈로 연산` 과 `동형암호` 를 이용한 *암호화*, *곱셈*, *덧셈* 을 정리하면 아래와 같습니다.

$$
\begin{align}
\text{encryption :} &\qquad 5^3 \equiv 6 (\bmod 7) \newline
\text{multiplication :} &\qquad 6^2 = (5^3)^2 = 5^6 \equiv 1 (\bmod 7) \newline
\text{addition :} &\qquad 5^3 \cdot 5^2 = 5^5 \equiv 3 (\bmod 7) \newline
\end{align}
$$

이후부터는 위에서 설명한 **암호화** 를 아래와 같이 정해놓고 사용하겠습니다.

$$
E(v) = g^v (\bmod n)
$$

> 사실 위에서 설명한 내용으로는 “암호화된 두 값”을 **곱셈** 하는 것이 불가능합니다. 다시 말해, 아래와 같은 연산을 할 수 없습니다.
> $$
> 5^2 \star 5^3 = 5^{2 \times 3}
> $$
> 이는 이후에 살펴볼 ***Pairing*** 으로 해결할 수 있습니다.

### 3.3.4 암호화된 다항식 (Encrypted Polynomial)
이제 앞서 살펴본 `동형암호`, `모듈로 연산`을 활용해서 **프로토콜**을 수정해보겠습니다. 앞서 예로 들었던 $p(x), t(x)$ 를 다시 이용해 살펴보겠습니다.

우선 “*다항식을 알고 있음*” 은 `계수(coefficient)` 를 알고 있다는 것과 같음을 상기할 필요가 있습니다. 다항식 $p(x)$ 는 아래와 같습니다.

$$
\begin{align}
p(x) &= (x-1)(x-2)(x-3) \newline
        &= x^3-6X^2+11x-6 \newline
\end{align}
$$

따라서, $p(x)$ 를 알고 있다는 말은 각 항의 계수인 $1, -6, 11, -6$ 를 알고 있다는 말과 동일합니다. 여기에 아래와 같이 각 항을 임의의 값 $x$에 대해 암호화한 형태를 생각할 수 있습니다.

$$
E(x^0), E(x^1), E(x^2), E(x^3)
$$

이제 아래와 같이 “암호화된 형태” 의 다항식을 계산할 수 있습니다.

$$
\begin{align}
&E(x^3)^1 \cdot E(x^2)^{-6} \cdot E(x^1)^{11} \cdot E(x^0)^{-6} \newline
&= (g^{x^3})^1 \cdot (g^{x^2})^{-6} \cdot (g^{x^1})^{11} \cdot (g^{x^0})^{-6} \newline
&= (g^{x^3}) \cdot (g^{-6x^2}) \cdot (g^{11x^1}) \cdot (g^{-6x^0}) \newline
&= g^{x^3 -6x^2 + 11x^1 -6x^0} \newline
&= g^{p(x)}
\end{align}
$$

  이를 이용해서 임의의 값 $x$ 에 대한 다항식 $p(x)$ 값을 “암호화된 형태”로 연산하는 것이 가능합니다. 이제 **프로토콜** 을 수정해보겠습니다.

`Prover` 가 주장하는 것은 동일합니다.
> “*나는 $1, 2$ 를 해로 갖는 3차 다항식 $p(x)$을 알고 있다.*”
> 
> --> `Prover` 가 실제로 알고 있는 다항식은 $p'(x)$ 로 표기합니다.


| | | 방향 | | 설명 |
| --- | --- | --- | --- | --- |
| 1 |  |  | `Verifier` | `Verifier` 는 임의의 값 $s$ 를 정합니다. (*secret* value) |
| 2 |  |  | `Verifier` | `Verifier` 는 다항식의 차수 $i \quad (i=0, 1, \dots, d)$에 대하여 $E(s^i)=g^{s^i}$ 를 계산해둡니다. |
| 3 |  |  | `Verifier` | `Verifier` 는 t(s) = t 를 계산해둡니다. |
| 4 | `Prover` | $\leftarrow$ | `Verifier` | `Verifier` 는 `Prover` 에게 $E(s^0), E(s^1), \dots, E(s^d)$ 을 보냅니다. <br /> ($s$ 를 그대로 보내지 않는 점이 이전과 다릅니다)|
| 5 | `Prover` |  |  | `Prover` 는 $h'(x)=\frac{p'(x)}{t(x)}$ 를 계산합니다.  |
| 6 | `Prover` |  |  | (1) $E(p'(s))$ <br /> 아래와 같이 <u>`Verifier`로부터 받은 값</u>과 <u>"다항식을 알고 있다면 알고 있을 계수"</u>를 이용해 $E(p'(s))$ 를 구합니다.<br/> $$\begin{align} & E(s^0), E(s^1), \dots, E(s^d) \newline & c_0, c_1, \dots, c_d \newline \Rightarrow &\quad E(p'(s)) = E(s^0)^{c_0} E(s^1)^{c_1} \cdots E(s^d)^{c_d} \end{align}$$ <br /> (2) $E(h'(s))$ <br /> $h'(x)$ 를 계산했으므로 (1)에서와 비슷한 방법으로 $E(h'(s))$ 를 구할 수 있습니다. <br /> <br /> 편의상 두 값을 $p', h'$ 라 부릅니다. |
| 7 | `Prover` | $\rightarrow$ | `Verifier` | `Prover` 는 `Verifier` 에게 $p', h'$ 를 보냅니다. |
| 8 |  |  | `Verifier` | `Verifier` 는 $p' = h' \cdot t$ 임을 검사합니다. <br /> $$\begin{align}g^{p'} = {(g^{h'})}^{t} \newline \Rightarrow g^{p'} = g^{h' \cdot t}\end{align}$$|

정리하면, 위 수정된 프로토콜을 이용하면 `Prover`가 *secret value* 를 모르게 됩니다.
따라서, $r$ 값을 이용해 "거짓으로 증명"하는 방법을 막을 수 있습니다.
하지만, `Prover`가 $s^0, s^1, \dots, s^d$ 값 중 일부만 사용하는 등의 방법은 여전히 가능합니다.
극단적인 예로 `Prover` 는 $s_i$ 값을 전혀 사용하지 않고도 아래 수식을 만족하는 $p', h'$ 를 만들어낼 수 있습니다.
여기서 $r$ 은 `Prover` 가 임의로 생성한 *random value* 입니다.

$$
\begin{align}
h' &= z_h = g^r \newline
p' &= z_p = {(g^{t(s)})}^r \newline
\end{align}
$$

다음 장에서는 이 문제를 해소하는 방법을 살펴봅니다.

## 3.4 다항식 사용 강제하기
사실 엄청 직관적인 방법을 적용하면 됩니다.
**프로토콜**에서 일련의 연산을 수행할 때 임의의 값 $\alpha$ 만큼 *shift* 되어있는 값으로도 한번 더 연산을 수행합니다.
연산 수행이 끝난 뒤, 결과값이 정확히 $\alpha$ 만큼 *shift* 되어 있는지 확인하면 **의도한 값들**을 이용해서 연산했는지 검증할 수 있습니다.
즉, 일종의 *checksum* 으로 활용할 수 있습니다.
이 방법은 [[Dam91](https://www.iacr.org/cryptodb/data/paper.php?pubkey=1214)] 에서도 소개하고 있는 `Knowledge-of-Exponent Assumption (KEA)`라고도 합니다.
또한, 이렇게 *shift* 된 값들을 "더한다"는 내용은 [[Gro10](https://link.springer.com/chapter/10.1007/978-3-642-17373-8_19)] 에서도 소개된다고 합니다.

이러한 "제한"을 적용해 **프로토콜**을 수정하면 아래와 같습니다.

`Prover` 가 주장하는 것은 동일합니다.
> “*나는 $1, 2$ 를 해로 갖는 3차 다항식 $p(x)$을 알고 있다.*”
> 
> --> `Prover` 가 실제로 알고 있는 다항식은 $p'(x)$ 로 표기합니다.


| | | 방향 | | 설명 |
| --- | --- | --- | --- | --- |
| 1 |  |  | `Verifier` | `Verifier` 는 아래 두 값을 정합니다. <br /> $s$ (*secret* value) <br /> $\alpha$ (*shifted* value) |
| 2 |  |  | `Verifier` | `Verifier` 는 아래 두 값을 계산합니다. <br /> $E(s^i)=g^{s^i}$ <br /> $E(\alpha \cdot s^i)=g^{\alpha \cdot s^i}$ |
| 3 |  |  | `Verifier` | `Verifier` 는 t(s) = t 를 계산해둡니다. |
| 4 | `Prover` | $\leftarrow$ | `Verifier` | `Verifier` 는 `Prover` 에게 아래 두 값을 보냅니다. <br /> $E(s^i)$ <br /> $E(\alpha \cdot s^i)$|
| 5 | `Prover` |  |  | `Prover` 는 $h'(x)=\frac{p'(x)}{t(x)}$ 를 계산합니다.  |
| 6 | `Prover` |  |  | (1) $E(p'(s)), E(p'(\alpha \cdot s))$ <br /> 아래와 같이 <u>`Verifier`로부터 받은 값</u>과 <u>"다항식을 알고 있다면 알고 있을 계수"</u>를 이용해 $E(p'(s))$ 를 구합니다.<br/> $$\begin{align} & E(s^i) \newline & E(\alpha \cdot s^i) \newline& c_i \newline \Rightarrow &\quad E(p'(s)) = \Pi E(s^i)^{c_i} \newline \Rightarrow &\quad E(p'(\alpha \cdot s)) = \Pi E(\alpha \cdot s^i)^{c_i} \end{align}$$ 편의상 두 값을 $p', {p'}_{a}$ 라 부릅니다. <br /> <br /> (2) $E(h'(s))$ <br /> $h'(x)$ 를 계산했으므로 (1)에서와 비슷한 방법으로 $E(h'(s))$ 를 구할 수 있습니다. <br /> <br /> 편의상 이 값을 $h'$ 라 부릅니다.  |
| 7 | `Prover` | $\rightarrow$ | `Verifier` | `Prover` 는 `Verifier` 에게 $p', p'_a, h'$ 를 보냅니다. |
| 8 |  |  | `Verifier` | `Verifier` 는 `Prover` 를 검증합니다. $p' = h' \cdot t$ 임을 검사합니다. <br /> $$\begin{align}g^{p'} = {(g^{h'})}^{t} \newline \Rightarrow g^{p'} = g^{h' \cdot t}\end{align}$$ <br /> $p'_a$ 를 이용해서 동일한 방법을 한번 더 합니다.|
| 9 |  |  | `Verifier` | `Verifier` 는 ***validity*** 를 검증합니다. $$p'_a = (p')^{\alpha}$$ |

이제 `Prover` 가 `Verifier` 를 속일 수 있는 가능성은 현저히 낮아졌습니다.

다음 장에서는 `Verifier`가 `Prover`가 제공한 정보로부터 **알아내는 정보**가 없도록 하는 **Zero-Knowledge** 에 대해 살펴봅니다.

## 3.5 Zero-Knowledge
"Free" zero-knowledge 로 종종 불릴 정도로, 아주 간단한 방법으로 **영지식** 을 적용할 수 있습니다.

`Prover` 가 제공하는 정보는 $p', p'_a, h'$ 입니다.

여기서의 핵심은 *지식을 드러내지 않으면서도 연산은 가능한 방법* 을 적용하는 것입니다.
이는 앞서 살펴본 *Shift* 개념을 이용해 가능함을 직관적으로 이해할 수 있습니다.

따라서, **프로토콜**에 `Prover` 의 작업을 하나 추가하여 아래와 같이 정리할 수 있습니다.

| | | 방향 | | 설명 |
| --- | --- | --- | --- | --- |
| 1 |  |  | `Verifier` | `Verifier` 는 아래 두 값을 정합니다. <br /> $s$ (*secret* value) <br /> $\alpha$ (*shifted* value) |
| 2 |  |  | `Verifier` | `Verifier` 는 아래 두 값을 계산합니다. <br /> $E(s^i)=g^{s^i}$ <br /> $E(\alpha \cdot s^i)=g^{\alpha \cdot s^i}$ |
| 3 |  |  | `Verifier` | `Verifier` 는 t(s) = t 를 계산해둡니다. |
| 4 | `Prover` | $\leftarrow$ | `Verifier` | `Verifier` 는 `Prover` 에게 아래 두 값을 보냅니다. <br /> $E(s^i)$ <br /> $E(\alpha \cdot s^i)$|
| 5 | `Prover` |  |  | `Prover` 는 $h'(x)=\frac{p'(x)}{t(x)}$ 를 계산합니다.  |
| 6 | `Prover` |  |  | (1) $E(p'(s)), E(p'(\alpha \cdot s))$ <br /> 아래와 같이 <u>`Verifier`로부터 받은 값</u>과 <u>"다항식을 알고 있다면 알고 있을 계수"</u>를 이용해 $E(p'(s))$ 를 구합니다.<br/> $$\begin{align} & E(s^i) \newline & E(\alpha \cdot s^i) \newline& c_i \newline \Rightarrow &\quad E(p'(s)) = \Pi E(s^i)^{c_i} \newline \Rightarrow &\quad E(p'(\alpha \cdot s)) = \Pi E(\alpha \cdot s^i)^{c_i} \end{align}$$ 편의상 두 값을 $p', {p'}_{a}$ 라 부릅니다. <br /> <br /> (2) $E(h'(s))$ <br /> $h'(x)$ 를 계산했으므로 (1)에서와 비슷한 방법으로 $E(h'(s))$ 를 구할 수 있습니다. <br /> <br /> 편의상 이 값을 $h'$ 라 부릅니다.  |
| `7` | `Prover` |  |  | `Prover` 는 임의의 값 $\delta$ 를 생성해 앞서 계산한 값들을 아래와 같이 갱신합니다. $$\begin{align}p' &:= {(p')}^{\delta} \newline p'_a &:= {(p'_a)}^{\delta} \newline h' &:= {(h')}^{\delta} \newline \end{align}$$  |
| 8 | `Prover` | $\rightarrow$ | `Verifier` | `Prover` 는 `Verifier` 에게 $p', p'_a, h'$ 를 보냅니다. |
| 9 |  |  | `Verifier` | `Verifier` 는 `Prover` 를 검증합니다. $p' = h' \cdot t$ 임을 검사합니다. <br /> $$\begin{align}g^{p'} = {(g^{h'})}^{t} \newline \Rightarrow g^{p'} = g^{h' \cdot t}\end{align}$$ <br /> $p'_a$ 를 이용해서 동일한 방법을 한번 더 합니다.|
| 10 |  |  | `Verifier` | `Verifier` 는 ***validity*** 를 검증합니다. $$p'_a = (p')^{\alpha}$$ |

# 정리
이번 포스트에서는 아래의 방향으로 **프로토콜** 을 수정했습니다.
- 다항식 인수분해를 기반으로 한 증명 프로토콜
- 다항식 사용이 강제된 증명 프로토콜
- "영지식"이 반영된 증명 프로토콜

그리고 이에 관한 핵심적인 수학 개념은 아래 두 가지입니다.
- 모듈러스 연산
- 지수 연산(간단한 동형암호 예시)

다음 포스트에서는 "비대화형(non-interactive)" 을 추가하여 *Non-interactive Zero-knowledge Protocol* 개념을 완성합니다.

---
