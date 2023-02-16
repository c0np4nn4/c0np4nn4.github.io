+++
title = "🔐 Mersenne Twister"
date = 2022-12-12
template = "page.html"
+++

- **Mersenne Twister** 는 1997년에 *Makoto Matsumoto* 와 *Takuji Nishimura* 가 개발한 `PRNG`다.

- 이름의 **Mersenne**은 `Mersenne Prime` 를 주기(period)로 사용하는 데서 왔다.


### Mersenne Prime
- `Mersenne Number`는 $M_n = 2^n - 1$ 인 수를 말한다.

- $M_n$ 중 소수인 수를 `Mersenne Prime` 이라 부른다.

### *k-distribution*
- 반복 주기가 $P$ 인  $w$-bit 의 유사난수의 수열 $x_i$ 에 대하여 아래의 조건을 만족할 때,
$v$-bit 의 정확도로 **k-distributed** 되어 있다고 할 수 있다.

- 우선 함수 $\text{trunc}_v(x_i) = X_i$ 에서 $X_i$를 정수 $x$ 의 상위 $v$-bit 로 만든 수라고 하자.

- 다음으로 주기 $P$에 대한 $k$ 개의 $v$-bit 벡터를 생각해보자.

$$X_i, \\ X_{i+1}, \\ \dots, X_{i+k-1} \\ \\ (0 \leq i < P)$$

- 이 때, $2^{kv}$ 의 가능한 총 조합이 한 주기 안에서 동일한 횟수로 발생한다는 것을 **k-distribution** 라고 한다.

### Algorithm
- $w$-bit 길이의 word size 에 대하여, 
`Mersenne Twister` 는 $[0, 2^w-1]$ 범위의 정수를 생성한다.

- `Mersenne Twister` 알고리즘은 Binary Field $\mathbb{F}_2$ 상에서의 행렬 선형 점화식(*matrix linear recurrence*)을 기반으로 동작한다.
- 알고리즘은 *<u>T</u>wisted <u>G</u>eneralised <u>F</u>eedback <u>S</u>hift <u>R</u>egister of <u>R</u>ational Form*
(**TGFSR(R)**) 로, *state bit reflection*과 *tempering* 과정이 더해진 형태이다.

- 기본적인 아이디어는 간단한 점화식을 통해 일련의 $x_i$ 를 정의하고,
결괏값으로 $x_i^T$ 값을 정의하는 것이다.

- 이 때, $T$ 는 *invertible $\mathbb{F}_2$-matrix*로써, **Tempering matrix** 라 부른다.

- 알고리즘을 정의하기 위해 사용된 `값`들을 나열하자면 아래와 같다.
  - $w$: word size
  - $n$: degree of recurrence
  - $m$: middle word, 점화식에서 사용되는 `offset`, $1 \leq m < n$
  - $r$: seperation point of one word, 한 word 내에서의 값이기 때문에 $0 \leq r \leq w - 1$
  - $a$: coefficients of the rational normal form twist matrix
  - $b, c$: TGFSR(R) Tempering bitmasks
  - $s, t$: TGFSR(R) Tempering bit shifts
  - $u, d, l$: Tempering 과정에서 사용될 기호들

- 수열 $x_i$ 를 생성하는 방정식은 아래와 같다.

$$x_{k+n} := x_{k+m} \oplus ((x_k^u \\ | \\ x_{k+1}^l) A ) \\ \\ \\ \\ \\ \\ k = 0, 1, \dots$$


- 방정식에서 사용된 기호를 간략히 설명하면 아래와 같다.
  - $|$: &nbsp; concatenation
  - $\oplus$: &nbsp; XOR
  - $x_k^u$: &nbsp; $x_k$ 의 상위 $w - r$ 비트
  - $x_{k+1}^l$: &nbsp; $x_{k+1}$ 의 하위 $r$ 비트
  - $A$: &nbsp; *Twist Transformation* 으로써 아래와 같이 rational normal form으로 정의된다.

$$
A = 
  \begin{pmatrix}
  0 & I_{w-1} \newline
  a_{w-1} & (a_{w-2}, \dots, a_0)
  \end{pmatrix}
$$

- 여기서 $I_{w-1}$는 &nbsp; $(w-1)$ x $(w-1)$ Identity Matrix 를 의미한다.

- 알고리즘이 Rational normal form 을 갖기 때문에, 아래와 같이 행렬 곱을 표현할 수 있다고 한다.

- 이 때, 연산은 $\mathbb{F}_2$상에서 일어나므로, 덧셈 연산이 XOR 로 대체된다.

$$
\mathbf{x}A = 
\begin{cases}
  \begin{align}
    & \mathbf{x} \gg 1   \\ \\ & x_0 = 0 \newline 
    & (\mathbf{x} \gg 1) \oplus \mathbf{a} \\  \\ \\ \\ & x_0 = 1
  \end{align}
\end{cases}
$$

- 여기서 $x_0$ 는 최하위 비트이다.

- 그리고 앞서 언급한 *equidistribution* 을 위해 *temerping* 단계를 거치게 되는데, 과정은 아래와 같다.

$$
\begin{align}
  y & \equiv x \oplus (( x \gg u) \\ \text{&} \\ d) \newline
  y & \equiv y \oplus (( x \ll s) \\ \text{&} \\ b) \newline
  y & \equiv y \oplus (( x \ll t) \\ \text{&} \\ c) \newline
  z & \equiv y \oplus (y \gg l)
\end{align}
$$

- 정리하면, 점화식으로 $x$ 를 얻고, *tempering* 과정을 거쳐 $z$ 를 결과값으로 얻게 되는 구조이다.

- **MT19937** 에서 사용하는 coefficient 들은 아래와 같다.
$$
\begin{align}
  (w, n, m, r) & = (32, 624, 397, 31) \newline
  a & = \text{0x9908b0df} \newline
  (u, d) & = (11, \text{0xffffffff}) \newline
  (s, b) & = (7, \text{0x9d2c5680}) \newline
  (t, c) & = (15, \text{0xefc60000}) \newline
  l & = 18 \newline
\end{align}
$$

### Initialization
- **Mersenne Twister** 에서 사용하는 `state`는 $w$-bit 길이의 $n$ 개의 벡터 배열이다.

- 벡터 배열을 초기화하기 위해서는 `seed` 값이 필요한데, $x_0$ 부터 $x_{n-1}$ 까지의 난수 수열에 대해 
`seed`를 $x_0$ 으로 두고 아래의 방정식에 따라 수열을 생성한다.

$$
x_i = f \times (x_{i-1} \oplus (x_{i-1} \gg (w-2))) + i
$$

- 초기화 과정이 끝난 뒤에는 $x_0$ 이 아니라 $x_n$ 을 `seed` 처럼 생각해서 생성하면 된다.

- MT19937 (32-bit)는 $f$ 값으로 $1812433253$ 을 사용한다.

### Implementation
- 구현에 필요한 조각들을 생각해보면 크게 `Init`, `Twist`, `Extract` 이 있음을 알 수 있다.

  - `Init` 은 `seed` 값을 받아서 $n$ 개의 난수 수열들을 생성한다.

  - `Twist` 는 Twist 알고리즘을 이용해서 새로운 $n$ 개의 난수 수열들을 생성한다.

  - `Extract` 은 난수 수열 중 다음으로 얻을 난수를 *tempering* 과정 후 얻어내는 과정이다.

- `Rust` 를 이용해서 구현하면 아래와 같다.

```rust,linenos
const W: u32 = 32;
const N: usize = 624;
const M: usize = 397;
const R: u32 = 31;

// =-=-= Twist =-=-=
//
const A: u32 = 0x9908_b0df;

// =-=-= Extract =-=-=
//
const U: u32 = 11;
const D: u32 = 0xffff_ffff;
//
const S: u32 = 7;
const B: u32 = 0x9d2c_5680;
//
const T: u32 = 15;
const C: u32 = 0xefc6_0000;
//
const L: u32 = 18;
//
const F: u32 = 1812433253;

const MASK: u32 = 0xffff_ffff;
const LOWER_MASK: u32 = (1 << R) - 1;
const UPPER_MASK: u32 = (!LOWER_MASK) & MASK;

pub struct MT19937 {
    pub state: [u32; N],
    pub idx: usize,
}

impl MT19937 {
    pub fn new() -> Self {
        let mut state:[u32; N] = [0; N];

        for i in 0..N {
            state[i] = 0;
        }

        let idx: usize = 0;

        MT19937 {
            state,
            idx
        }
    }

    pub fn init(&mut self, seed: u32) {
        self.state[0] = seed;

        self.idx = N;

        for i in 1..N {
            let q: u64 = (self.state[i - 1] ^ (self.state[i - 1] >> (W - 2))) as u64;

            let q: u64 = F as u64 * q;

            let q: u64 = q + i as u64;

            let q: u32 = (q as u32) & MASK;

            self.state[i] = q;
        }
    }

    fn twist(&mut self) {
        for i in 0..N {
            let x = (self.state[i] & UPPER_MASK)
                        | (self.state[(i + 1) % N] & LOWER_MASK);

            let mut xA = x >> 1;

            if x % 2 == 1 {
                xA = xA ^ A;
            }
            self.state[i] = self.state[(i + M) % N] ^ xA;
        }
        self.idx = 0;
    }

    pub fn extract(&mut self) -> u32 {
        if self.idx >= N {
            if self.idx > N {
                // println!("ERROR");
                self.init(5489);
            }
            self.twist();
        }

        let mut y = self.state[self.idx];

        y = y ^ ((y >> U) & D);
        y = y ^ ((y << S) & B);
        y = y ^ ((y << T) & C);
        y = y ^ (y >> L);

        self.idx = self.idx + 1;

        return y & MASK;
    }
}
```

### Reference
- Wikipedia: <u>[Link](https://en.wikipedia.org/wiki/Mersenne_Twister)</u>
- For comparison, *Mersenne Twister* crate: <u>[Link to crate.io](https://crates.io/crates/mersenne_twister)</u>
- Python version implementation, github: <u>[Link](https://github.com/anneouyang/MT19937)</u>


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
