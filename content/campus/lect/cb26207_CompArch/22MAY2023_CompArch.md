+++
title = "CompArch 22MAY2023"
description = "AAR"
date = 2023-05-22
toc = true

[taxonomies]
categories = ["CompArch"]
tags = ["Memory", "Quick Note"]

[extra]
math=true
+++

# Cache Memory
- `CPU` 에 가까운 메모리
- 메모리 주소가 주어지고, Cache 에 그 내용이 들어감

---

## Directed Mapped cache
- `주소`를 기반으로 저장할 위치가 결정됨
- i.e. 메모리의 하위 $N$-bit 를 Cache 의 위치로 정함
  - 아주 간단한 접근법...

---

## Tags and Valid bits
- 특정 Cache 에 값이 이미 있는지 아닌지를 `Valid bit` 로 판단할 수 있음
- ***실제 주소***를 알기 위해 Cache Address 로 사용한 하위 비트를 제외한 상위 비트를 `Tag` 값으로 기록해둠
- 만약 `Valid bit` 이 $1$ 인 경우, <txtred>Miss</txtred> 후 <txtylw>Tag 값을 갱신</txtylw>함

---

## Address Subdivision

- 메모리 주소는 <txtylw>Byte</txtylw> 단위
- Cache 는 Data 를 <txtylw>32-bit</txtylw> 즉, <txtred>4-byte</txtred> 로 가짐
- `tag` 는 20-bit
- `index` 는 가운데 10-bit
- `data` 는 <txtred>4-byte</txtred> 이기 때문에, 메모리 주소의 하위 <txtred>2-bit</txtred> 를 이용해 표현함
---
- `cache` 는 보통 <txtylw>Multi-word</txtylw> 임.
  - Spatial Locality Exploit

---

## Block Size Considerations
- `Larger Blocks`: Cache Entry 수를 줄임

=> miss rate 의 증감

---

## Cache Miss
- `CPU` pipeline 중단
- 다음 계층의 메모리에서 <txtylw>Block</txtylw> 을 `fetch` 함
  - Spatial locality 를 위해 다수의 <txtylw>word</txtylw> 을 가져옴
- instructino miss
  - instructino fetch 과정을 다시 수행
- data miss
  - data 접근을 다시 수행

---

# 쓸 때 (Write-through)
- Write == Hit
- `cache` 정보가 <txtred>원본</txtred>
  - Write-through: 메모리와 캐시 둘 다 write
    - `쓰기`의 이점을 완전히 잃음 (메모리에 쓰는 것은 <txtred>매우</txtred> 느리기 때문)
  - write-back: 캐시의 블럭만 write update

---

<details>
<summary>놓침</summary>
</details>

---

## Cache Performance EX
- 

---

## Average Access Time
- ***AMAT***
  - $\text{Hit time} + \text{Miss rate} \times \text{Miss Penalty}$
  - ex) 
    - `CPU`: $1 \text{ns}$ clock 
    - hit time: $1$ cycle 
    - miss penalty: $20$ cycles
    - I-cache miss rate: $5 \text{%}$
    - $\therefore$ ***AMAT*** = $1 + 0.05 \times 20$ = $2$ns
      - 2 cycles per instruction
      - 2배 느려짐

$\therefore$ <txtred>MISS PENALTY</txtred> 가 퍼포먼스에 엄청 영향을 준다.

== CPI 감소

---

> 성능을 평가할 때, `Cache` 가 큰 영향을 미침

---

> - 비유) *도서관 자리 배정*

<br />

> - `Directed Mapped Cache`: 지정 좌석
>     - 성능이 별로 좋지는 않음
>     - 공간의 넉넉함에 비해 <txtred>성능이 많이 떨어짐 </txtred>
>   - 아무 곳이나 빈 자리에 앉게 하면 됨
>   - 제일 안 쓰는 자리를 앉게 할 수도 있음
> - `Associative Cache`
>   - 모든 entry 에 대한 'search' 가 필요함
>     - <txtylw>비싼 연산</txtylw>
> - `n-way set Associative Cache`
>   - $n$ 개만 지정해서 search
>     - <txtylw>덜 비싼 연산</txtylw>

- 당연히 `n-way set Associative Cache`를 많이 활용함

---
