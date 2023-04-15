+++
title = "OS: midterm"
description = "Intent"
date = 2023-04-15
toc = true

[taxonomies]
categories = ["Lect", "Exam", "Midterm", "OS"]
tags = ["Midterm", "OS"]

[extra]
math=true
+++

---
*2023 Spring, PNU, CB26044 (Prof. Ahn)*

# 범위
- ***Ch1 ~ Ch11***
  - `CPU Virtualization`: ***Ch1 ~ Ch5***
  - `Memory Virtualization`: ***Ch6 ~ Ch11***

---

# Ch2: Process
- Process

# Ch3: Limited Direct Execution
- Process

---

# Ch4: Scheduling(1)
- Scheduling 의 두 Type
  - <mark style="background: orange">Non-preemptive</mark>: Cooperative
  - <mark style="background: chartreuse">Preemptive</mark>: No Cooperative
<br />
<br />
- 주요 개념
  - ***Turnaround Time***
    - $T = T_{finish} - T_{arrival}$
    - 작업이 도착한 뒤 ``끝날 때까지``걸린 시간
  - ***Response Time***
    - $T = T_{first \\ start} - T_{arrival}$
    - 작업이 도착해서 `처음` 수행할 떄까지의 시간
## FIFO
- <mark style="background: orange">Non-preemptive</mark>
- ***Convoy Effect***
  - 빨리 끝날 일을 나중에 해야할 수도 있음

## SJF
- <mark style="background: orange">Non-preemptive</mark>
- 오래 걸리는 일을 시작해버리면, 또 기다리고만 있어야 함 

## STCF
- <mark style="background: chartreuse">Preemptive</mark>
- 작업 수행 중, 다른 일을 할지 <u>**결정**</u>함

## RR
- <mark style="background: chartreuse">Preemptive</mark>
- `Fair` 한 방법
- ***Response Time*** 에 대한 고려
- `Time Slice` 만큼만 일을 하고, Ready Queue 에 돌려 보냄
  - *Short* `Time Slice`
    - ***Response Time*** 향상
    - Performance 에는 악영향 (i.e. ***Context Switching***)
  - *Long* `Time Slice`
    - ***Response Time*** 저하
    - Performance 악영향이 적음

### w/ I/O
- `I/O` 요청이 오면 아예 Ready Queue 에서 빼버려야 함.
- Block Queue 로 보내고, 다음 Task 를 수행하면 됨

---

# Ch5: Scheduling(2)
## MLFQ
- ***Multi-Level Feedback Queue***
  - Queue 가 여러 층으로 이루어진 구조
  - 각 층이 <u>*우선순위*</u> 를 가짐
  - 아래 다섯 Rule 을 기억하자

> 1. 더 높은 우선순위의 작업을 먼저 수행한다.
> 2. 우선순위가 같다면 <mark style="background: chartreuse">RR</mark> 을 따른다.
> 3. 작업이 도착하면, <u>가장 우선순위가 높은 Queue</u> 에 배치한다.
> 4. 한 Queue 에서의 수행시간을 <u>누적하여 체크</u>한다.
> 5. 주기적으로 모든 작업을 가장 우선순위가 높은 Queue 로 올린다. (***Boost***)

---

# Ch6: Virtual Memory
## (Virtual) Address Space
- `물리 메모리`에 대한 ***Abstraction***
- *Process* 에 대한 정보 (Code, Heap, Stack, etc.) 를 담고 있음
  - *Code*: 명령어
  - *Heap*: 동적으로 할당되는 메모리 공간
  - *Stack*: return addr, local variable, args 등을 저장
- 프로그램이 다루는 주소(%p 등을 이용해 확인되는 주소)는 ***Virtual Address*** 임
  - ***OS*** 가 이를 실제 주소로 번역하여 다룸

## Address Translation (Relocation)
- *<u>Virtual Address</u> 와 <u>Physical Address</u> 간의 `mapping`*
- *`Address Translation`*: *<u>Physical Address</u>* = ***function*** *(<u>Virtual Address</u>)* 
- 빠르게 수행되야 하므로, ***H/W*** 의 도움을 받음
- ***Virtual Address*** 는 **0x0000....0000**에서 시작함
- 이를 `물리 메모리`의 ***Physical Address***로 변환하는 작업이 필요함

### Static Relocation
- <mark style="background: orange">Software-based</mark> ***Relocation***
- <mark style="background: red">Protection</mark> Issue
  - 메모리 공간에 대한 보호가 없음
- 한번 물리 메모리에 매핑되면 변경하기 쉽지 않음

### Dynamic Relocation
- <mark style="background: chartreuse">Hardware-based</mark> ***Relocation***
- ***MMU*** (Memory Management Unit) 라는 <mark style="background: chartreuse">Hardware</mark> 의 도움
- <mark style="background: chartreuse">H/W</mark> 로 Protection 이 가능함
  - <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> Register 이용
- ***OS*** 는 ***Process*** 에 대해 ***Free-List*** 를 참조하여 <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 를 잘 다뤄야 함. 
  - <mark>Process Starts</mark>
    - ***Free-List*** 을 이용해 사용 가능한 공간을 확인하고, 적절히 <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 에 할당
  - <mark>Process Ends</mark>
    - <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 값을 토대로, 메모리 공간을 ***Free-List*** 에 반환
  - <mark>Context Switch</mark>
    - <u>기존 Process</u> 의 ***PCB*** 에 <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 정보를 저장하고, <u>다음 Process</u> 에 대한 값들을 복원

---

# Ch7: Segmentation
## [1] Concept
- `Addres Space` 를 여러 부분으로 쪼개는 방법
  - <u>Code Segment</u>, <u>Heap Segment</u>, <u>Stack Segment</u>, etc.
- 각 *Segment* 에 대한 <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 를 관리
- 하나의 `(Virtual) Address Space` 를 통째로 `물리 메모리`에 매핑? -> 공간 확보 어려움

## [2] Address Translation
- *`Address Translation`*: *Physical Address* =  <mark style="background: khaki">Base</mark> + *Offset*

## Extra Bits
### Segment Bits
- ***어떤 종류*** 의 Segment 인지를 *<u>Address</u>* 에 명시함
- 예를 들어, 상위 *2-bit* 를 `Segment bits` 로 둘 수 있음.
- (
<mark style="background: ivory">nil</mark>, 
<mark style="background: lightsalmon">CODE</mark>, 
<mark style="background: lime">HEAP</mark>, 
<mark style="background: magenta">STACK</mark>
)
= 
(
<mark style="background: ivory">00</mark>, 
<mark style="background: lightsalmon">01</mark>, 
<mark style="background: lime">10</mark>, 
<mark style="background: magenta">11</mark>
) 
- 예를 들어, *address* 값이 *<u>11</u>010...10010010* 라면, <mark style="background: magenta">STACK</mark> 영역에 대한 주소

### Protection Bits
- 다수의 코드가 같은 메모리 영역을 이용하는 경우, 적절한 권한을 부여하는 방법
- 각 Segment 에 대해 `Protection Bit` 를 추가로 두어, ***RWX*** 권한을 지정함

## (External) Fragmentation
- 임의 공간들이 할당되고 해제되면서 작은 공간 조각들 자연스레 형성
- 만약 큰 공간이 필요한 경우, 적절히 배치하지 못하는 상황이 발생함
- 이를 위해 *공간을 다시 재배열* 하는 등의 추가 작업이 필요함

---

# Ch8: Paging
## [1] Concept
## [2] Address Translation

# Ch9: TLB
- Process

# Ch10: Smaller Table
- 

# Ch11: Swapping
- 

