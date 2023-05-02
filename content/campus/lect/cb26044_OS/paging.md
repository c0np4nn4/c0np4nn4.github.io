+++
title = "Paging"
description = "Memory Virtualization (3)"
date = 2023-04-04
toc = true

[taxonomies]
categories = ["Lect", "OS"]
tags = ["Operating System", "OS", "Virtualization"]

[extra]
math=true
+++

---

# Concept
- `메모리` 공간 관리를 위한 접근법은 크게 `Segmentation`과 `Paging`이 있습니다.
>  - `Segmentation`
>    - ***Variable Size*** 로 메모리 공간을 사용함
>    - *Fragmentation* 발생
> <br /> 
> <br /> 
>  - `Paging`
>    - ***Fixed Size*** 메모리 공간을 사용함
- `Paging` 은 <u>Physical Memory</u> 를 `Frame` 이라는 단위로 쪼개서 관리합니다.
- 그리고, 각 Process 는 `Page Table` 을 관리하여 <mark>Virtual Address</mark> 와 <mark>Physical Address</mark>를 mapping 합니다.

---

# Paging
- Process 가 사용하는 <mark>Physical Address</mark>를 불연속적으로 사용할 수 있게 해줍니다.
  - `page`: <mark>Virtual Memory</mark> 의 단위
  - `frame`: <mark>Physical Memory</mark> 의 단위
  - `page` 의 크기는 *2의 거듭제곱* 형태
<br />
<br />
- `OS` 가 메모리 관리를 하기 쉽게 해줍니다.
  - `OS`는 비어있는 `frame` 들을 tracking 합니다.
  - 그러다가 "$n$ 개의 `page`"를 사용하는 프로그램이 실행되면, "$n$ 개의 `frame`" 을 찾아서 프로그램을 로드합니다.
  - `Page Table` 을 두어 `page` 와 `frame` 간의 mapping 을 저장합니다.
  - 이 과정에서 <u>동일한 크기의 `frame`을 사용</u>하므로,  <mark>external fragmentation</mark>은 일어나지 않습니다.

## Advantages
- `Flexibility`
  - Address Space 에 대한 추상화를 손쉽게 할 수 있습니다.
  - 이를 통해, *heap*, *stack* 등 각 Segment 의 특성을 고려하지 않아도 됩니다.
<br />
<br />
- `Simplicity`
  - `Page` 와 `Frame` 의 크기가 동일하기 때문에 할당하고 해제하기가 간단해집니다.

---

# Address Translation
- `virtual address` 는 아래와 같은 구조로 이루어져 있습니다.

```
|--- VPN ---|-------- offset -------|
+-----+-----+-----+-----+-----+-----+
| Va5 | Va4 | Va3 | Va2 | Va1 | Va0 |
+-----+-----+-----+-----+-----+-----+
```

- `VPN`
  - Virtual Page Number 로, 몇 번째 `Page` 인지를 나타냅니다.
  - 즉, `Page Table` 의 *index* 로 생각할 수 있습니다.
- `Offset`
  - `Page` 내에서의 offset 을 의미합니다.
<br />
<br />
- `Page Table` 을 통해 `PFN`을 얻습니다.
  - Page Frame Number 입니다.
- 최종적으로, Physical Address 는 아래와 같이 구할 수 있습니다.
  - <mark><PFN, OFFSET></mark>
- 따라서, `Page Table` 은 `VPN` 을 `PFN`으로 mapping 하는 역할임을 알 수 있습니다.
<br />
<br />
## Example
- 예를 들어, 아래의 상황을 가정하겠습니다.
  - `Virtual Address Space`
    - Address Space: <mark>64-byte</mark>
    - Page Size: <mark>16-byte</mark>
  - `Physical Memory`
    - Memory Space: <mark>128-byte</mark>
    - Page Size: <mark>16-byte</mark>
  - 아래 그림과 같이 $4$개의 `Page` 가 있음을 알 수 있습니다.
```
  0 +-----+
    |     | page 0
 16 +-----+
    |     | page 1
 32 +-----+
    |     | page 2
 48 +-----+
    |     | page 3
 64 +-----+
```

- `Page` 의 수가 $4$개 이므로, <mark>2-bit</mark>로 충분히 특정 인덱스를 지정할 수 있습니다.
- 또, 각 `Page` 의 크기가 <mark>16-byte</mark> 이므로, <mark>4-bit</mark>로 byte 단위의 offset 지정이 가능합니다.
- 따라서, 아래 그림과 같이 <mark>2+4 = 6-bit</mark> 의 Virtual Address 를 이용하면 `Page Table` 의 모든 값을 참조할 수 있습니다.
```
|--- VPN ---|-------- offset -------|
+-----+-----+-----+-----+-----+-----+
| Va5 | Va4 | Va3 | Va2 | Va1 | Va0 |
+-----+-----+-----+-----+-----+-----+
```

- 또, `Physical Memory` 는 $8$개의 `Frame` 으로 이루어짐을 쉽게 알 수 있습니다.
- `Page Table` 을 통해, `Page Index` 에 대응되는 `Frame Index`를 mapping 한 뒤, `Physical Memory`를 참조하는 과정을 그림으로 그리면 아래와 같습니다.

```
      +---------------------------------+                    
      |                                 |                    +---------+
+---+---+                               v                    |         |
| P | D |                         +---+---+                  +---------+
+---+---+                         | F | D |----------------> |         |
  |  <logical address>            +---+---+                  +---------+
  |                                 ^ <physical address>     |         |
  |              +-----+            |                        +---------+
  |              |     |            |                        |         |
  |              +-----+            |                        +---------+
  |              |     |            |                            ...    
  |              +-----+            |                        +---------+
  +--------------|  F  |------------+                        |         |
                 +-----+                                     +---------+
                 |     |                                     |         |
                 +-----+                                     +---------+
              <page table>                                <physical memory>
```

## Example2
- 아래와 같은 상황을 가정해보겠습니다.
  - Virtual Address: <mark>32-bit</mark>
  - Physical Address: <mark>20-bit</mark>
  - Page Size: <mark>4K-byte</mark>
- `Page Size`를 `Offset`이 표현할 수 있어야 하므로,
  - `Offset`: <mark>12-bit</mark>
- 따라서, Virtual Address 의 남은 부분 ($32 - 12 = 20$) 은 `VPN` 이 됩니다.
  - `VPN`: <mark>20-bit</mark>

```
|------- 20 ------|------ 12 -------|
+-----+-----+-----+-----+-----+-----+
| vpn | ... | vpn | ofs | ofs | ofs |
+-----+-----+-----+-----+-----+-----+
```

- 그런데, <mark>20-bit</mark>는 $2^{20} = 4MB$ 를 나타낼 수 있는 크기입니다.
- 한 Process 마다 `Page Table` 만으로 $4MB$ 를 사용하는 것은 제법 큰 오버헤드임을 알 수 있습니다.
- 이러한 `Page Table` 들은 `OS` 의 `Kernel Memory` 에서 관리됩니다.

---

# Page Table Entry
- `Page Table`의 각 `Page` 들은 `Page Table Entry` 라는 규격화된 형태로 관리됩니다.
- 대략 아래 그림과 같이 생겼습니다.
```
31                        12    8       0
+-------------------------+-----+-------+
|           PFN           |/////| Flags |
+-------------------------+-----+-------+
```

## Common Flags
- `Page Table Entry` 는 상태를 나타내는 `Flag` 정보를 포함하고 있습니다.
  - Valid Bit: 유효성 여부
  - Protection Bit: 해당 `Page`에 RWE가 가능한지
  - Present Bit: `Physical Memory`에 존재하는지
  - Dirty Bit: Memory 에 올라온 후 수정되었는지
  - Reference Bit: `Page` 가 참조되었는지

---

# Paging: Too Slow
- `Paging`은 Physical Memory 에 접근하기 위해 `Page Table`을 이용합니다.
- 즉, `Page Table Entry` 를 통해 Memory Address 를 얻어오는 <u>부가적인 작업</u>이 필요하고, 이는 전체 시스템의 성능을 저하시키는 결과를 초래할 수 있습니다.
- 따라서, H/W, S/W 관점에서 잘 설계해야 함을 알 수 있습니다.

---
