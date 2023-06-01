+++
title = "CompArch 31MAY2023"
description = "Note"
date = 2023-05-31
toc = true

[taxonomies]
categories = ["CompArch"]
tags = ["Memory", "Quick Note"]

[extra]
math=true
+++

---
# Virtual Machine
- `Host`가 `guest` 의 환경을 ***emulate*** 하는 것
- 가상화로 computing resources 를 효율적으로 사용할 수 있음
  - *Cloud Computing*

---
# Instruction Set Support
- <txtred>***새로운 CPU 는 virtualization support 를 잘해야 함***</txtred>

<details>
<summary>Virtual Memory</summary>
---
# Virtual Memory
- `Cache`와 유사
- `Main Memory` 와 `Disk` 간의 관계

---
# Address Translation
- <txtylw>***Virtual Address Space***</txtylw> 의 주소를 <txtylw>***Physical Address***</txtylw> 로 `mapping` 하는 과정
- `VA` 는 `PA` + `Disk (swap space)` 로 관리됨
<br/>
<br/>
- `VA` 의 크기가 $48$-bit 이고, `Page Table`의 크기가 $4$-kb 라고 하면, `Page Offset`은 $12$-bit 가 됨

---
# Page Fault Penalty
- `Disk`로부터 `page`가 fetch 되어야 하기 때문에, I/O 과정에서 ***엄청난*** 패널티가 발생함
- 따라서, `Page Fault Rate`를 줄이는 것이 <txtylw>***매우***</txtylw> 중요함

---
# Page Table
- `Page`가 실제로 어디에 저장되어 있는지를 기억하고 있는 TABLE

---
# *Translation* using page table
- trans(`page table number`) + `page offset`

---
# Replacement and Writes
- `Page Fault Rate` 를 줄이기 위해 사용하는 정책: `LRU`


---
## *Fast Translation* using TLB
- `Tag` 값을 이용함
- `Virtual Address`의 <txtylw>Cache</txtylw> 역할

---
### TLB Miss
- TLB 에는 없고, Page Table 에 있는 경우
- fetch 해오면 됨
<br/>
<br/>
- TLB miss 가 발생했을 떄
  - Memory 에 존재하면, `H/W`가 알아서 fetch 하면 됨
  - Memory 에 없으면, `OS`가 <txtred>page fault handling</txtred> 을 호출
    - `Disk` 에서 page 를 찾음
    - *Replace*
<br/>
<br/>
> - `CPU` Cache >> H/W 에서 다 구현이 됨
> 
> - `VM` 관련된 부분은 OS 가 handling

---
## TLB and Cache Interaction
- `CPU` -> `TLB` -> `PA` -> `Cache` 의 흐름으로 진행됨
- 차선책) `Virtual Cache`를 두는 방법
  - 기존의 `Cache` 는 `Physical Address`를 기준으로 함 (Translation 이 선행됨)
  - `Virtual Address`를 translation 하기 전에 `caching`하는 개념
  - `hit` --> translation X
  - `miss` --> translation 부터 기존의 흐름을 그대로 따라감
  - 그러나, <txtred>Aliasing</txtred> 에서 문제가 발생함
    - *물리적으로 동일한 address 에 대해, 서로 다른 virtual address 가 참조하려는 경우 문제가 발생함*

</details>

---
## Memory Protection
- `OS`와 `H/W`가 메모리를 보호할 수 있음
- H/W 는 kernel mode 를 활용 (privileged mode)

---
## Memory Hierarchy
- locality 특성을 이용한 caching...

---
## Block Placement
- Direct mapped (1-way)
  - one choice for placement
- N-way set associative
  - n choices within a set
- Fully associative
  - any location
- 높은 Associativity 는 miss rate 를 ***낮추는데 효과가 있음***
  - 대신 복잡해지고, 느려지고, 비싸짐

---
## Finding a Block
- 상기한 방법 각각의 block 을 찾는 과정 차이를 알 필요가 있음
- H/W cache
  - comparison 을 줄여서 비용을 줄일 수 있음
- Virtual Memory
  - fully associativity 로 miss rate 를 줄일 수 있음

---
## Replacement
- LRU
  - n-way 에서 n 이 작을 때 유리함
  - n 이 커지면 random 이 오히려 유리함
- Random
- VA 는 LRU 를 이용함

---
## Write Policy
- Write-through: 
  - 모든 레벨의 메모리를 모두 업데이트함
- Write-back
  - 일단 상위 레벨만 업데이트하고, 나중에 하위 레벨을 업데이트함

---
## Sources of Misses
- Compulsory misses (cold miss)
  - 필연적으로 발생하는 miss
- Capacity miss
  - Cache 크기의 한계로 발생하는 miss
- Conflict miss
  - non-fully associative cache 에서 발생함 (i.e. direct mapped)

---
## Cache Design trade-off
- Cache size++
  - capacity miss 낮춤
  - access time 증가할 수 있음
- Associativity++
  - conflict miss 낮춤 (spatial locality)
  - access time 증가할 수 있음
- block size++
  - compulsory miss 감소
  - miss penalty 증가할 수 있음

---
## Cache Control
> - Block size, Cache Size 등이 주어졌을 때, `word size`를 이용해서 address 를 표시할 수 있어야 함

---
## Interface Signal
- `CPU` 는 `Cache`가 마치 `Memory`인 것처럼 <txtylw>Read/Write</txtylw>함

---
## Finite State Machine
- `FSM` 의 전체적인 동작 흐름을 이해해야함
  - CPU request, Miss, Hit 등의 `event`
  - 

---
## Cache Coherence Problem
