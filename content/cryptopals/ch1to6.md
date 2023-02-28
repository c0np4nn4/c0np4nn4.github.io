+++
title = "Challenge 1 ~ 6"
date = 2023-02-26

[taxonomies]
categories = ["Cryptopals"]
tags = ["Cryptopals","Rust"]

[extra]
toc = true
keywords = "Cryptography, Cryptopals, Rust, Cryptanalysis"
+++

---

> - 전체 코드는 [[Github Link](https://github.com/c0np4nn4/cryptopals/tree/main/cryptopals/challenges/src/set1)] 에서 확인할 수 있습니다.

---

# Challenge 1 [[#](https://cryptopals.com/sets/1/challenges/1)]

> <u>**Convert hex to base64**</u>

- <mark>Base64</mark> 로 hex string 값을 다루는 문제입니다.

- 학습도 할 겸 <mark>Base64</mark> 인코딩, 디코딩을 [직접 구현](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/utils/src/base64/mod.rs#L86)하여 해결했습니다.

- <mark>Base64</mark>에 대한 설명은 [[링크](@/cryptopals/Base64.md)] 에 있습니다.

---

# Challenge 2 [[#](https://cryptopals.com/sets/1/challenges/2)]

> <u>**Fixed XOR**</u>

- 동일한 길이의 두 hex string에 대한 XOR 연산을 구현하는 문제입니다.

- 우선 아래와 같이 hex string 을 Vec<u8> 로 변환했습니다.

> <details>
> <summary>Code</summary>
> <p>
> 
> ```rust,linenos
> pub fn decode_hex(mut data: String) -> Result<Vec<u8>, TypeError> {
>     if data.is_empty() {
>         return Err(format!("data is empty").into());
>     }
> 
>     if data.len() % 2 == 1 {
>         let last_char = data
>             .chars()
>             .nth(data.len() - 1)
>             .ok_or("Cannot get last character from the data")?;
> 
>         data = data[..data.len() - 1].to_string();
> 
>         data = format!("{}0{}", data, last_char);
>     }
> 
>     let mut res = Vec::<u8>::new();
> 
>     for w in (0..data.len()).step_by(2) {
>         let b = u8::from_str_radix(&data[w..w + 2], 16)?;
>         res.push(b);
>     }
> 
>     Ok(res)
> }
> ```
> 
> </p>
> </details>

- 이후 아래와 같이 XOR 연산을 진행하여 해결했습니다.

> <details>
> <summary>Code</summary>
> <p>
> 
> ```rust,linenos
> pub fn fixed_xor(l: Vec<u8>, r: Vec<u8>) -> Result<Vec<u8>, XorError> {
>     if l.len() != r.len() {
>         return Err(format!("length is different, {} and {}", l.len(), r.len()).into());
>     }
> 
>     let mut res = Vec::<u8>::new();
> 
>     for idx in 0..l.len() {
>         res.push(l[idx] ^ r[idx]);
>     }
> 
>     Ok(res)
> }
> ```
> 
> </p>
> </details>

---

# Challenge 3 [[#](https://cryptopals.com/sets/1/challenges/3)]

> <u>**Single-byte XOR cipher**</u>

- *Single-byte XOR* 된 암호문 $C$ 를 이용하여 평문 $P$ 를 구하는 문제입니다. 

- 즉, <kbd>1byte</kbd> 값 $K$ 를 찾는 문제입니다. 

- <kbd>1byte</kbd>는 <kbd>8-bit</kbd>이므로, <mark>Brute Force</mark>로 충분히 $K$ 를 복원할 수 있습니다. 

- 문제에서 제시한 <mark>Frequency Attack</mark> 을 이용해 가장 적합한 답을 찾아내는 코드를 [구현](https://github.com/c0np4nn4/cryptopals/blob/main/cryptopals/utils/src/attack/mod.rs#L83)하여 해결했습니다.

---

# Challenge 4 [[#](https://cryptopals.com/sets/1/challenges/4)]

> <u>**Detect single-character XOR**</u>

- hex string 들로 이루어진 문제 파일이 주어지고, 그 중 하나가 *Single-byte XOR* 되었다고 합니다.

- `Challenge 3` 에서 구현했던 <mark>Frequency Attack</mark> 을 이용하여 해결할 수 있습니다.

> 1. 각각의 hex string 에  대해 <mark>Frequency Attack</mark>을 행합니다.
>
> 2. 결과값들 중 가장 <mark>Frequency Attack</mark> 의 `Score` 가 높은 평문이 정답입니다.

---

# Challenge 5 [[#](https://cryptopals.com/sets/1/challenges/5)]

> <u>**Implement repeating-key XOR**</u>

- <mark>Key</mark> 값을 확장하는 방법 중, 단순 반복하는 방법을 구현하는 문제입니다.

- `Challenge 2` 를 이용할 수도 있지만, 아래와 같이 코딩하여 해결하였습니다.

> <details>
> <summary> Code </summary>
> <p>
>
> ```rust, linenos
> pub fn repeating_key_xor(pt: String, key: String) -> Result<String, XorError> {
>     let pt = pt.as_bytes();
> 
>     let key = key.as_bytes();
> 
>     let mut expanded_key = Vec::<u8>::new();
> 
>     for idx in 0..pt.len() {
>         expanded_key.push(key[idx % key.len()]);
>     }
> 
>     let res: Vec<u8> = pt
>         .iter()
>         .zip(expanded_key.iter())
>         .map(|(l, r)| l ^ r)
>         .collect();
> 
>     let res: String = types::u8_vec_to_hex_string(res)?;
> 
>     Ok(res)
> }
> ```
> 
> </p>
> </details>

---

# Challenge 6 [[#](https://cryptopals.com/sets/1/challenges/6)]

> <u>**Break repeating-key XOR**</u>

- `Challenge 5` 에서 구현해 본 <mark>Repeating-Key XOR</mark> 로 암호화된 값을 복원하는 문제입니다.

- 문제 해결 단계는 아래와 같습니다.
  1. <mark>Key Size</mark> 를 찾습니다.
  2. <mark>Key</mark> 를 복원합니다.
  3. 평문을 복원합니다.

## Solution

### 1. <mark>Key Size</mark> 찾기

> #### Hamming Distance
> > - *hamming_distance* 를 이용합니다.
> >   - *hamming_distance*는 두 데이터 간의 차이 정도를 나타낸 값입니다.
> > 
> #### Bit Pattern
> 
> > - 문자열 (알파벳, 숫자)은 아래와 같은 bit pattern을 가집니다.
> > >  - 대문자 'A' ~ 'Z' 는 상위 3비트가 '010' 으로 고정되어 있습니다.
> > >  - 소문자 'a' ~ 'z' 는 상위 3비트가 '011' 로 고정되어 있습니다.
> > >  - 숫자 '0' ~ '9' 는 상위 4비트가 '0011' 로 고정되어 있습니다.
> > 
> > - 즉, 임의의 1byte 값과 달리, 두 문자에 관한 *hamming_distance* 는 평균적으로 더 **<u>낮은</u>** 값을 갖게 됩니다.
> 
> #### Codes
> 
> > - <mark>Key Size</mark> 의 값이 $ks$ 라고 가정할 때, 평문 $P$, 암호문 $C$, 그리고 <mark>Key</mark> $K$에 대하여 아래를 생각해볼 수 있습니다.
> > 
> > $$
> > \begin{align*}
> > C_{1 \cdot ks + t} &= P_{1 \cdot ks + t} \\ \oplus K_t \newline
> > C_{2 \cdot ks + t} &= P_{2 \cdot ks + t} \\ \oplus K_t \newline
> > \therefore C_{1 \cdot ks + t} \oplus C_{2 \cdot ks + t}  &= P_{1 \cdot ks + t} \oplus P_{2 \cdot ks + t} \newline
> > \end{align*}
> > $$
> > 
> > - 즉, 적절한 <mark>Key Size</mark>를 찾으면, 암호문만을 이용해서 두 평문 간의 *hamming_distance* 를 찾을 수 있습니다.
> > 
> > - *hamming_distance* 를 구하는 함수 `get_hamming_distance` 는 아래와 같이 구현하였습니다.
> > 
> > <details>
> > <summary>Code</summary>
> > <p>
> > 
> > > ```rust,linenos
> > > pub type BoxedError = Box<dyn Error>;
> > >
> > > pub fn get_hamming_distance(l: &[u8], r: &[u8]) -> Result<u64, BoxedError> {
> > >     if l.len() != r.len() {
> > >         return Err(format!(
> > >             "length is different, {} and {}",
> > >             l.len(),
> > >             r.len()
> > >         )
> > >         .into());
> > >     }
> > > 
> > >     let mut res = Vec::<u8>::new();
> > > 
> > >     for idx in 0..l.len() {
> > >         res.push(l[idx] ^ r[idx]);
> > >     }
> > > 
> > >     let mut distance: u64 = 0;
> > >     res.into_iter().for_each(|b| {
> > >         distance += b.count_ones() as u64;
> > >     });
> > > 
> > >     Ok(distance)
> > > }
> > > 
> > > ```
> > </p>
> > </details>
> > 
> > - $ks$ 의 크기를 2 ~ 40 범위의 값으로 생각하고, 아래와 같이 구현하였습니다.
> > 
> > <details>
> > <summary>Code</summary>
> > <p>
> > 
> > > ```rust,linenos
> > >
> > >     type Score = f64;
> > >
> > >     let mut hamming_distances: HashMap<_, Score> = HashMap::default();
> > > 
> > >     for key_size in 2..=40 {
> > >         let ct_clone = ct.clone();
> > > 
> > >         let mut score: f64 = 0.0;
> > > 
> > >         let key_size = key_size as usize;
> > > 
> > >         for idx in 0..(ct_clone.len() / key_size) - 1 {
> > >             score += get_hamming_distance(
> > >                 &ct_clone[(idx + 0) * key_size..(idx + 1) * key_size],
> > >                 &ct_clone[(idx + 1) * key_size..(idx + 2) * key_size],
> > >             )? as f64;
> > >         }
> > > 
> > >         let score = score / (ct_clone.len() / key_size) as f64 / key_size as f64;
> > > 
> > >         hamming_distances.insert(key_size, score);
> > >     }
> > > 
> > > ```
> > </p>
> > </details>
> > 
> > - 그리고 그 중 가장 <mark>Key Size</mark> 일 확률이 높은 값을 구했습니다.
> > 
> > <details>
> > <summary>Code</summary>
> > <p>
> > 
> > > ```rust, linenos
> > >     let key_size: usize = hamming_distances
> > >         .iter()
> > >         .min_by(|a, b| {
> > >             a.1.partial_cmp(&b.1)
> > >                 .ok_or("expect to compare values")
> > >                 .unwrap()
> > >         })
> > >         .map(|(k, _v)| k)
> > >         .ok_or("expect to get key_size(candidate)")?
> > >         .to_owned();
> > > 
> > > ```
> > 
> > </p>
> > </details>

### 2. <mark>Key</mark> 복원

> - 앞선 수식에서 $(0 \le t \le ks)$ 인 값 $t$ 에 대하여 아래의 평문 문자들은 모두 <u>**같은 키 값**</u> $K_t$ 로 암호화되었음을 보았습니다. 
> $$P_{(0 \cdot ks) + t}, P_{(1 \cdot ks) + t}, \dots, P_{(n \cdot ks) + t}$$
> 
> - 따라서, $P_{(n \cdot ks) + t}$ 값들을 하나의 `Block` 으로 모은 뒤, `Challenge 3` 에서 했던 <mark>Frequency Attack</mark>으로 <mark>Key</mark> 를 복원할 수 있습니다.
> 
> - 코드에서는 `Chunk` 라는 이름으로 암호문 값들을 분류하였습니다.
> 
> <details>
> <summary>Code</summary>
> <p>
> 
> > ```rust, linenos
> >     let mut chunks: HashMap<usize, Vec<u8>> = HashMap::<usize, Vec<u8>>::new();
> > 
> >     for idx in 0..key_size {
> >         chunks.insert(idx, vec![]);
> >     }
> > 
> >     for (idx, byte) in ct.iter().enumerate() {
> >         let index = &idx.rem_euclid(key_size);
> > 
> >         let chunk = chunks
> >             .get_mut(index)
> >             .ok_or(format!("expect chunk[{}]", index))?;
> > 
> >         chunk.push(byte.clone());
> >     }
> > 
> > ```
> 
> </p>
> </details>
> 
> - 다음으로, `Challenge 3` 에서와 같이 <mark>Frequency Attack</mark>을 이용했습니다.
> 
> <details>
> <summary>Code</summary>
> <p>
> 
> > ```rust, linenos
> >     let mut pt_chunks: HashMap<usize, Vec<u8>> = HashMap::<usize, Vec<u8>>::new();
> >     let mut rec_key: Vec<u8> = vec![];
> > 
> >     for idx in 0..key_size {
> >         let ct = chunks.get(&idx).ok_or("expect to get ciphertext chunk")?;
> > 
> >         let SingleCharKeyAttackResult { pt, key, .. } = single_char_key_attack(ct.clone())?;
> > 
> >         pt_chunks.insert(idx, pt);
> >         rec_key.push(key as u8);
> >     }
> > 
> > ```
> 
> </p>
> </details>
> 
### 3. 평문 복원

- 이제 <mark>Frequency Attack</mark>으로 복원된 평문들 중 가장 `Score`가 높을 때의 값을 고르면 됩니다.
