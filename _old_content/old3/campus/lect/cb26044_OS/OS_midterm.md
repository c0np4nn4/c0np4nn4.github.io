+++
title = "OS: midterm"
description = "OS"
date = 2023-04-15
toc = true

[taxonomies]
categories = ["Lect", "Exam", "Midterm", "OS"]
tags = ["Midterm", "OS"]

[extra]
math=true
+++

---
*2023 Spring, PNU, CB26044 (Professor Ahn)*

# 범위
- ***Ch1 ~ Ch11***
  - `CPU Virtualization`: ***Ch1 ~ Ch5***
  - `Memory Virtualization`: ***Ch6 ~ Ch11***

---

# Ch2: Process
## Running a Process
- Fetch I, Decode I, Execute I, Update PC
- CPU 를 가상화해서 마치 여러 Process 가 각각 CPU 를 독점하고 있다는 듯한 환상을 만들어줌
- Time Sharing 기법을 활용
- Process 의 생성은 다음 단계들로 설명할 수 있음
  - *Memory* 에 Program code 를 적재함
  - Program 의 Run-Time Stack 을 할당함
  - Program 의 Heap 을 할당함
  - 이 외의 Init 작업들을 함
  - entry point (main 함수) 를 통해 프로그램을 시작함
## Process Hierarchy
- Parent-Child Relationship 으로 관리됨
- ***Fork()***: 완전히 동일한 자식 프로세스 생성
- ***Exec()***: 현재 프로세스를 다른 프로세스로 교체

## Process Termination
- Normal exit: 정상 종료
- Error exit: (예상했던) 에러 종료
- Fatal error: (예상 못함) 치명적 에러로 종료
- Killed (by another process): 다른 프로세스에 의해 죽음
- Zombie Process: 종료는 되었지만 관련 데이터가 아직 메모리에 상주하고 있음

## Process States
- `Running` : 돌아감
- `Ready` : 돌아가기를 기다리고 있음
- `Blocked` : I/O 등으로 인해, 잠깐 다른 작업을 하는 상태
- <mark>***State Transition*** 흐름을 아는 것이 좋음</mark>

## Implementing Process
- PCB(Process Control Block) 을 정의해야함
- Process 에 관련된 모든 정보를 담고 있는 구조체임

## Context Switch
- 프로세스를 돌리다가 다른 프로세스에게 CPU 를 넘겨줄 때 발생하는 작업
- 필연적인 Overhead 발생
- <mark>Process, Hardware, Kernel(OS) 간의 Context Swtich 발생 시의 과정을 알고 있는 편이 좋음</mark>

## Policiy vs Mechanism
- Policy: WHAT
- Mechanism: HOW

---

# Ch3: Limited Direct Execution
## Direct Execution
- 코드가 `CPU` 에서 직접적으로 실행됨
- ***OS*** 는 `CPU` 를 넘겨줌과 동시에 동면과 같은 상태에 들어감
- 즉, ***OS*** 는 프로그램이 무슨 짓을 하든 제어할 수 없음
- 따라서, <mark>User Mode</mark>, <mark>Kernel Mode</mark> 로 모드를 구분하여 해결할 수 있음
- 이와 관련된 것이 `System Call`
- `System Call`은 kernel 안에 있는 함수로써, ***trap*** 으로 호출함
- `CPU` 바깥에서 발생하는 이벤트로, `Kernel` 이 처리해야 하므로, <mark>Kernel Mode</mark> 로 바뀜(H/W Level 의 동작임)

## Switching
- Process 간의 Switching 이 일어나려면, `CPU` 를 어떻게 주고받을지를 결정해야함
  - Cooperative (자발적) 
  - !Cooperative (비자발적) 
    - Timer Interrupt 등으로 주기적으로 CPU 를 돌려받음
    - Scheduler 가 어떤 Process 를 수행할지 결정함
- Context Switching
  - 현재 Register 를 Kernel Stack 에 저장함
    - General Purpose Registers
    - PC
    - Kernel Stack Pointer
  - 다음에 실행될 Process 의 값들을 Kernel stack 에서 불러옴
  - 다음에 실행될 Process 의 Kernel Stack 으로 돌아감
  - User Stack
    - User Function, User Library 을 호출할 때 씀
  - Kernel Stack
    - System call 후 Kernel 내 함수들을 호출할 때 추가적으로 쓸 Stack
<mark>Context Switching 과정을 알고 있는 편이 좋음</mark>

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

## [1] Address Translation (Relocation)
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
- 메모리 공간을 `Page` 라 불리는 <u>일정한 단위</u> 로 쪼개는 방법
- *Virtual Address*: <mark style="background: orange">Page</mark>, *Physical Address*: <mark style="background: chartreuse">Frame</mark>
- 각 *Process* 별로 ***Page Table*** 을 관리
- <u>***External Fragmentation*** 을 해결함</u>

## [2] Address Translation
- Virtual Address 아래 두 부분으로 구분됨
  - <mark style="background: cyan">VPN</mark> (Virtual Page Number)
  - <mark style="background: lightsalmon">offset</mark>
- Physical Address 는 <mark style="background: cyan">VPN</mark> 대신, <mark style="background: lime">PFN</mark> (Physical Frame Number) 를 가짐.
- (<mark style="background: cyan">VPN</mark>, <mark style="background:lightsalmon">Offset</mark>) --> (<mark style="background: lime">PFN</mark>, <mark style="background:lightsalmon">Offset</mark>)
- 위와 같은 *Mapping* 을 관리하는 곳이 <mark style="background: yellow" >Page Table</mark>

## Calc. Page Table Size
- 아래 상황을 가정
> - *Virtual Address* 의 크기: <u>32 bits</u> 
> - *Physical Address* 의 크기: <u>20 bits</u>
> - ***Page Size***: <u>4KB</u>

- ***Page Size*** 가 $2^{12}$ bits
  - 다시 말해, *<u>12 bits</u> 로 Page 공간을 모두 가리키는 것이 가능함*
  - 즉, Address에서 <mark style="background:lightsalmon">Offset</mark> 을 위한 공간: <u>12 bits</u>
- *Virtual Address* 크기가 <u>32 bits</u> 이므로, <mark style="background: cyan">VPN</mark> 크기는 <u>20 bits</u>
  - 즉, 가리킬 수 있는 ***<mark style="background: yellow">Page Table 의 Entry</mark>*** 개수는 $2^{20}$개
- <u>4KB</u> 짜리가 $2^{20}$ 개 있으므로, <mark style="background: yellow">Page Table</mark>의 총 크기는 ***<u>4MB</u>***

## Page Table Entry
- <mark style="background: lime">PFN</mark>: VPN 에 대응되는 Frame Number
- <mark style="background: lime">Flags</mark>: Page 에 대한 상태를 나타내는 정보
  - `Valid Bit`: 유효한 주소 변환인지 확인
  - `Protection Bit`: RWX 권한
  - `Present Bit`: 물리 메모리에 적재된 Page 인지 확인 (Swapping과 연관)
  - `Dirty Bit`: 메모리에 올라온 후 수정 여부
  - `Reference Bit`: Page 가 접근되었는 지여부

## Code. Accessing Memory w/ Paging
```c
// virtual address 에서 VPN 을 획득
VPN = (VirtualAddress & VPN_MASK) >> SHIFT

// PTE(page table entry) 를 참조할 때 사용할 주소를 계산
PTE_offset = VPN * sizeof(PTE)
PTE_addr = PTBR + PTE_offset

// PTE 를 가져옴
PTE = AccessMemory(PTE_addr)

// Page 에 접근할 수 있는지를 체크
if (PTE.VALID == FALSE) {
  RaiseException(SEGMENTATION_FAULT)
} else if (CanAccess(PTE.ProtectBits) == False) {
  RaiseException(PROTECTION_FAULT)
} else {
  // 접근 가능하므로, 물리 주소를 계산
  offset = VirtualAddress & OFFSET_MASK
  Phys_addr = (PTE.PFN << PFN_SHIFT) | offset
  Register = AccessMemory(PhysAddr)
}
```

## Page Fault
- 유효하지 않은 Page 에 접근할 때, 발생할 수 있는 Exception.
- 크게 세 종류로 나눌 수 있음.
  - <u>Major</u>
    - 유효한 Page 이지만, Memory 에 올라와있지는 않음
    - ***OS*** 가 해당 Page 를 가져오기 위해 정보를 알고 있어야 함
    - `Disk I/O` 가 필요함
  - <u>Minor</u>
    - `Disk I/O` 없이 해결될 수 있는 문제
    - `PreFetched` 를 이용하는 등
  - <u>Invalid</u>
    - ***Segmentation Violation***
## Pros and Cons
- ***Pros***
  - ***External Fragmentation*** 없음
  - **고정 단위** 로 메모리를 다룸
- ***Cons***
  - Memory Reference Overhead (Performance) --> <mark style="background: yellow">TLB</mark>
  - Page Table Size Issue (Storage) --> <mark style="background: yellow">Smaller Table (Multi-Level table)</mark>

# Ch9: TLB
- ***Make Address Translation FAST***

## [1] Translation Lookaside Buffer
- ***MMU*** 내부의 ***<u>Cache</u>***
- 자주 쓰이는 mapping 을 기억함
- ***PTE*** 전체를 저장하고 있음 (단순히 *PFN* 만 갖고 있지 않음)
- ***Locality*** 를 이용하여 성능을 향상시킨 것
  - *Array* 예시 생각해보면 됨
    - a[1], a[2], a[3], ... 를 각각 찾는 것보다 Cache 에 올려놓고 찾는게 더 빠름

## [2] Address Translation
- virtual address 를 기반으로, 우선 ***TLB*** 를 찾아봄
- ***TLB*** 에 있으면(`HIT`), 그대로 이용함
- ***TLB*** 에 없으면(`MISS`), ***TLB*** 를 update 하고 다시 수행함

## Issue. Context Switch
- 서로 다른 ***Process*** 가 같은 <mark style="background: cyan">VPN</mark> 을 사용할 수 있음
- 따라서, ***TLB Table*** 에 <mark style="background: yellow">ASID</mark>( *Address Space IDentifier* ) 를 추가로 두어, Process 를 구분함

## LRU (Replacement Policy)
- ***Least Recently Used***
- 가장 마지막에 사용한 것부터 교체하는 정책
- ***시간 지역성*** 을 이용한 것으로 볼 수 있음

## Performance
- ***TLB HIT*** 가 전체 Performance 에 큰 영향을 줌
- <mark style="background: orange">Page size</mark> 를 증가시키면, 공간지역성을 이용할 수 있음
- <mark style="background: yellow">TLB size</mark> 를 증가시키면 좋음. (비용이 비쌈)

---

# Ch10: Smaller Table
## [1] Problem
- <mark style="background: yellow" >Page Table</mark> 내부에 <u>사용하지 않는 공간</u> 이 많음

## Hybrid Approach: Paging + Segment
- ***Address Space*** 를 ***<u>Segmentaiton</u>*** 한 뒤, ***<u>Paging</u>*** 함
- 각 Segment 의 <mark style="background: khaki">Base</mark>, <mark style="background: khaki">Bound</mark> 는 Page Table 을 가리키게 됨
- 각 Segment 마다 Page Table 이 존재함
- <mark style="background: red">External Fragmentation 문제가 다시 발생함</mark>

## Multi-Level Page Table
- 이때까지의 <mark style="background: yellow">Page Table</mark> 은 전부 *linear array*
- 이를 ***Tree*** 구조로 고침
- 쓰이지 않는 영역의 공간을 그대로 들고 있는 것이 아니라 *<u>쓰이지 않음</u>* 을 표현할 수 있음
<br/>
<br/>
- Page Table</mark> 을 ***Page*** 단위로 쪼갬
- 집중해서 이해 안하면 용어들 때문에 굉장히 혼란스러울 수 있음..
<br />
<br />
- ***VPN*** 을 받아서 ***PFN*** 을 알아내는 기존의 <mark style="background: yellow">Page Table</mark> 을 상상
- 그 <mark style="background: yellow">Page Table</mark>을 <mark style="background: Orange">Page</mark> 크기 단위로 분할함
- 그럼 하나의 <mark style="background: orange">Page</mark> 에 여러 개의<mark style="background: lime">Page Table Entry</mark> 가 들어감
- 만약 이런 <mark style="background: orange">Page</mark> 내의 <mark style="background: lime">Page Table Entry</mark> 가 모두 비어있는 상태라면, 공간을 할당할 이유가 없음
- 또, 새로 쪼개진 <mark style="background: orange">Page</mark> 들에 대한 <mark style="background: teal">Page Table</mark> 이 필요함
- 이 <mark style="background: teal">Page Table</mark> 를 <mark style="background: teal">Page Directory</mark>라 부름
- 즉, <mark style="background: teal">Page Directory</mark>의 Entry 인 <mark style="background: magenta">Page Directory Entry</mark> 는 (다시 보니)이름 그대로 <mark style="background: orange">Page</mark> 를 가리키고, 각 <mark style="background: orange">Page</mark> 는 <mark style="background: yellow">Page Table</mark> 의 일부를 나타냄

***그래서 영어 자료에서 <u>Page Of Page Table</u> 이라는 말이, 처음엔 굉장히 어렵게 느껴지는데 한번 이해하면 굉장히 명료한 단어가 됨...***

- 얼핏보면, 굉장히 redundant 한 구조 같지만 ***Invalid 공간을 절약*** 할 수 있다는 차이가 있음
- 한 [ [네이버 블로그](https://blog.naver.com/PostView.naver?blogId=nywoo19&logNo=221514954890&parentCategoryNo=&categoryNo=46&viewDate=&isShowPopularPosts=false&from=postView) ]
에서 굉장히 좋은 계산 문제 예시를 발견함

> - *Virtual Address Space*: 32 bits
> - *Page Size*: 4KB == $2^{12}$ bits
> - *PTE Size* = *PDE Size* = $4$ byte 라고 가정하면,
> - *Virtual Address(32 bits)* = *Offset(12 bits)* + *VPN(20 bits)*
<br />
<br />
> - *VPN* 은 <u>Page Table Index</u> 이므로, Page Table Entry 가 총 $2^{20}$ 개
> - 따라서, Page Table 의 전체 크기는 $2^{20} \times 4\text{byte} = 4\text{MB}$
<br />
<br />
> - Page Directory 는 <u>Page Table 전체 크기</u>를 <u>Page 크기</u>로 나눈 것
> - 따라서, Page Directory Entry 개수는 $2^{22} / 2^{12} = 2^{10}$ 개
> - 즉, Page Directory 의 크기는 $2^{10} * 4\text{byte} = 4\text{KB}$

---

# Ch11: Swapping
## [1] Concept
- 만약 *Virtual Memory* 가 *Physical Memory* 보다 크다면?
- ***Secondary Storage*** 를 Cache 처럼 사용해야함
- `Swap Space`: Disk 의 일정 공간을 Memory 를 위한 공간으로 지정해둠

## Page Fault
- 접근하려는 `Page` 가 Memory 에 없는 경우 발생하는 Exception
- `Present bit` 를 통해 `Swap Space` 에 있는 Page 를 가져오면 됨
- 이 작업을 `Page Fault Hanlder` 라는 코드조각이 해결함

## Page Replacement Policy
- Memory 가 가득 찼을 때, 적당한 녀석들을 Swap Space 로 쫓아내야함
- 사실 ***OS*** 는 항상 일정 수준 이상의 ***free space*** 를 확보하고 있다고 함
  - Swap Daemon, Page Daemon 등의 kernel thread 를 돌림
  - LW Pages 이상 HW Pages 이하의 ***Free Space*** 를 계속 확보하고 있음
- 사실 Swap 해도 괜찮은 Page 는 따로 있음
  - Kernel 관련한 데이터들은 memory 에 상주해야함
  - `User Code` 는 이미 Disk 에 있으므로, 굳이 Swap 해둘 필요가없음
  - `User Data` 도 비슷하지만, 만약 수정된 점이 있다면 유지, 저장(Swap)하긴 해야함
  - `User Heap/Stack` 은 프로그램 실행 이후의 정보이므로 Swap 해줘야함

### AMAT
- ***Replacement Policy*** 에 대한 평가지표로 ***<u>Cache Miss</u>*** 를 최소화할 수 있는지를 활용할 수 있음
- ***Average Memory Access Time*** 이라는 계산 결과로 평가할 수 있음
> $$AMAT = (P_{Hit} * T_{M}) + (P_{Miss} * T_{D})$$
> - $P_{Hit}$: Hit 확률
> - $P_{Miss}$: Hit 확률
> - $T_{M}$: Memory 접근 비용
> - $T_{D}$: Disk 접근 비용

### Optimal Policy
- ***미래를 모두 알고 있음***
- 그래서 miss 가 무조건 최소가 되도록 할 수 있음
- 당연히 비현실적이고, 평가 기준으로 활용함

### FIFO
- 단순한 모델
- 당연히 성능은 처참함
- 심지어 메모리 증설을 했는데도 Miss Rate 가 늘어나는 기현상이 발생함 (`BELADY'S ANOMALY`)

### Random
- 오히려 Random 으로 보내버리는게 더 나은 경우도 있음
- 당연히 안전성이 없기 때문에 사용할 수 없음

### LRU, LFU
- 따라서, 과거의 패턴을 근거로 Replace 하는 것이 타당함 (그리고 효과적이기도 함)
- `LRU`: 최근에 얼마나 접근했는가
- `LFU`: 얼마나 자주 접근했는가
<br />
<br />
- `LRU` 를 좀 더 자세히 살펴봄
  - `Stack` 을 이용함
  - 마지막으로 접근한 정보(i.e. age)를 <mark style="background: yellow">tracking</mark> 해야 하므로, 구현 난이도가 상당함
- Worload 의 형태에 따라 세 가지를 생각할 수 있음
  - Randomly Accessing: Random Policy 와 같은 효과
  - 80-20 (파레토): *Locality* 를 고려했을 때, LRU 가 Optimal 에 가까운 양상을 보임
  - Looping: Cache 공간보다 Looping Range 가 더 클 경우, 계속 miss 가 나버림
- 앞서 말한 <mark style="background: yellow">tracking</mark>을 위해 H/W 의 힘을 빌려야 함
- `Use bit`
  - 어떤 Page 가 접근되면, H/W 는 해당 Page 의 `Use Bit` 를 1로 만들어줌
  - ***OS*** 가 Page 들을 Circular List 의 구조로 관리하며 `Use Bit` 가 1 이면, 0 으로 만든 뒤 지나가고 0 이면 쫓아냄
- 이러한 `clock` 방법은 `LRU` 보다는 성능이 나쁘지만, 적어도 *Loop Situation* 은 발생하지 않음
- `Dirty Page`라면 disk 에 저장한 뒤 쫓아내야함
- 유용할지는 아무도 장담할 수 없지만, ***OS*** 는 몇몇 Page 들을 Memory 에 미리 읽어오기도 함
  - `Prefetching`: ***OS*** 가 몇몇 page 가 곧 사용될 것으로 예상하고 가져옴
  - `Clustering, Grouping`: Write 관련, 한 번에 여러개를 저장하기
- `Thrashing`
  - 동시에 돌고 있는 `Process`의 수가 너무 많으면, miss 가 엄청나게 일어나고, page 를 불러오려는 시도만 하다가정작 process 를 수행할 수가 없게 됨.
  - 즉, CPU Utilization 정도가 급격하게 하락함
