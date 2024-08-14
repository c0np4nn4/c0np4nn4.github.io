+++
title = "0x00. 운영체제 개요"
description = "OS"
date = 2023-06-20
toc = true

[taxonomies]
categories = ["OS"]
tags = ["os"]

[extra]
math=true
+++

---

# OS
- 운영체제(<txtred>Operating System</txtred>)는 전체 <u>컴퓨터 시스템을 쉽고, 안전하게 사용</u>할 수 있도록 돕는 프로그램입니다.
- <u>컴퓨터 시스템을 사용한다</u>는 것은 <u>*프로그램 실행*</u> 을 의미한다고 볼 수 있습니다.
- 따라서, 운영체제는 <txtylw>Software</txtylw> 인 <u>*프로그램*</u> 을 실행할 때, <txtylw>Hardware</txtylw>인 <u>*CPU, Memory, Disk*</u> 등을 잘 관리하는 역할 및 책임을 가진 프로그램입니다.
- 운영체제에 대한 학습의 핵심 주제는 아래 세 가지 입니다.
> - 가상화(<txtred>*Virtualiation*</txtred>)
> - 병렬화(<txtred>*Concurrency*</txtred>)
> - 영속성(<txtred>*Persistence*</txtred>)

---

# 📌 Virtualization


## 📎 CPU 가상화
- `CPU`는 프로그램을 실행할 때 사용되는 <txtylw>Hardware</txtylw> 입니다.
- 프로그램의 실행은 `CPU`를 통해 아래 세 단계로 설명될 수 있습니다.
> - 명령어를 `CPU`에 반입(<txtylw>fetch</txtylw>)합니다.
> - 명령어를 `CPU`에서 해석(<txtylw>decode</txtylw>)합니다.
> - 명령어를 `CPU`에서 실행(<txtylw>execute</txtylw>)합니다.

- `하나의 프로그램`을 실행하려면 `하나의 CPU`가 필요합니다.
- 그러나, 실제로는 다수의(<txtylw>multiple</txtylw>) `프로그램`이 <txtylw>하나</txtylw>의 `CPU`를 이용하여 실행될 수 있습니다.
- 이는 <txtred>OS</txtred>가 시스템에 매우 많은 수의 <txtylw>가상 CPU</txtylw>가 존재하는 듯한 환상을 만들어 냈기 때문입니다.
- 이처럼, 소수의 `CPU`를 무한개의 `CPU`가 존재하는 것처럼 변환하여 동시에 많은 수의 프로그램을 실행시키는 `CPU 가상화`(<txtred>CPU Virtualization</txtred>)라고 합니다.

---

## 📎 메모리 가상화
- 마찬가지로 `Memory`에도 `가상화`를 적용할 수 있습니다.
- `Memory 가상화`(<txtred>Memory Virtualization</txtred>)을 하게 되면, 각 프로그램이 마치 고유의 `메모리`를 갖고 있는 것처럼 동작하도록 할 수 있습니다.
    - <u>프로그램 A</u> 와 <u>프로그램 B</u> 가 동일한 주소 `0x1000`을 각자 참조하되, 서로 다른 값을 갖고 있을 수 있습니다.
- 즉, 실제 물리 메모리(<txtylw>Physical Memory</txtylw>)가 아닌 가상 주소 공간(<txtylw>Virtual Address Space</txtylw>)을 각 프로그램에 할당하고, 둘 사이의 *mapping* 을 <txtred>OS</txtred>가 관리하는 형태로 `가상화`를 할 수 있습니다.

---

# 📌 Concurrency
- 프로그램이 <u>한 번에 많은 일</u>을 하려 할 때(== ***동시에*** ), 발생하는 문제들에 관한 내용입니다.
> - 예를 들어, 변수 `count` 에 반복문을 $n$번 돌며 `+1` 하는 스레드가 있다고 해보겠습니다.
> - 만약 프로그램이 두 스레드를 실행시켰고, 사용자가 $n=500$ 을 입력하면, `count`의 <txtylw>예상값</txtylw>은 $1,000$ 입니다.
> - 하지만, `count` 값을 증가시키는 부분이 <txtred>Atomic</txtred>하게 동작하지 않는다면, $n=797$ 등의 값을 얻을 수도 있습니다.

---

# 📌 Persistence
- `DRAM`과 같은 `Memory`는 휘발성(<txtylw>volatile</txtylw>) 방식으로 값을 저장합니다.
- 따라서, 컴퓨터의 전원 공급이 끊겼을 때, 데이터를 저장하고 있을 ***저장 장치*** 가 필요합니다.
    - 보통, `HDD`와 `SSD`를 사용합니다.
- 또, <txtred>OS</txtred>에서 <txtylw>Hardware</txtylw>인 `Disk`를 관리하기 위해 <txtred>FileSystem</txtred>이라는 <txtylw>Software</txtylw>를 이용합니다.

---
