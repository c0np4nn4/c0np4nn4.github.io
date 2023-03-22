+++
title = "Scheduling (1)"
description = "Introduction of Scheduling"
date = 2023-03-22

[taxonomies]
categories = ["Lect"]
tags = ["Operating System", "OS", "Virtualization"]

[extra]
math=true
+++

---

# CPU Scheduling
- <mark>CPU Scheduling</mark> 이란 실행되기를 기다리며 `Queue` 에 들어가 있는 `Process` 들이 있을 때,
어떤 `Process` 를 실행할 것인지 결정하는 <mark>Policy</mark> 를 의미합니다.

## Scheduling Types
- <mark>Scheduler</mark> 의 성격에 따라 아래 두 종류를 생각할 수 있습니다.
  - ***Non-preemptive*** Scheduling
    - *Process* 가 <u>협조적</u>으로, CPU 를 내놓습니다.
  - ***Preemptive*** Scheduling
    - *Scheduler* 가 *Process* 의 동작을 통제할 수 있습니다.

## Five Assumptions
- 아래 다섯 가지 조건을 토대로, `이상적인 상황`을 가정해 볼 수 있습니다.
  - 각각의 작업(*process*) 들은 <mark>수행 시간이 동일</mark> 합니다.
  - 모든 작업들은 <mark>같은 시간에 도착하여 시작</mark>됩니다.
  - 작업이 한번 시작되면 <mark>완료할 때까지 쭉 수행</mark>됩니다.
  - 모든 작업은 <mark>CPU 만을 사용</mark>합니다.
  - 각 작업의 <mark>전체 수행 시간을 이미 알고</mark> 있습니다.
- 아래에서는 이러한 `이상적인 상황` 으로부터 `현실적인 상황` 으로 개념을 발전시킵니다.

## Scheduling Metrics
- <mark>Scheduling</mark> 의 성능을 평가하는 지표는 아래 세 종류가 있습니다.
  - <mark>Turnaround time</mark>
    - 작업(*process*)이 도착(*arrival*)한 다음 완료(*completion*)될 때까지 걸리는 시간입니다.
    $$T_{\text{turnaround}} = T_{completion} - T_{arrival}$$
  - <mark>Response Time</mark>
    - 작업이 도착한 후 첫 번째로 작업(*first run*)을 시작할 때까지 걸리는 시간입니다.
    $$T_{\text{response}} = T_{firstrun} - T_{arrival}$$
  - <mark>Fairness</mark>
    - 작업들이 얼마나 공평하게 수행되는지에 대한 지표입니다.

---

# FIFO
- 아주 직관적이고 간단한 방법으로, `FIFO` 를 생각해 볼 수 있습니다.
- 작업이 끝날 때까지 기다려야 하므로, ***Non-preemptive*** 입니다.
- 모든 작업이 줄을 서고 있으면, 언젠가는 **CPU** 를 할당받습니다.
  - 즉, <mark>Fairness</mark> 는 좋습니다.
- 이해를 돕기 위해 $A, B, C$ 세 작업이 `Scheduling` 을 위한 `Queue` 에 들어왔다고 가정하겠습니다.

```
+---+---+---+
|   |   |   |
|   |   |   |
| A | B | C |
|   |   |   |
|   |   |   |
+---+---+---+
+---+---+---+-----------
0   10  20  30
```

- 위에서 정한 가정 중 <mark>도착 시간</mark>이 같다는 조건이 있지만, 아주 간발의 차로 $A$, $B$, 그리고 $C$ 의 순서가 정해진다고 보면 되겠습니다.
- 이 때의 <mark>평균 turnaround time</mark> 을 구해보면 아래와 같습니다.
$$\textbf{Avg.}\text{turnaround time} = \frac{(10 - 0) + (20 - 0) + (30 - 0)}{3} = 20 \text{ secs}$$

## No more FIFO, Convoy effect
- `FIFO` 가 꽤 괜찮은 방법처럼 보이지만, 가정한 <mark>조건 1</mark>번을 제외하면 상황이 달라집니다.
  - <mark>조건 1번</mark> : 각 작업은 모두 <mark>수행 시간이 동일</mark>합니다.
- 예를 들어, $A$ 의 수행시간이 `100 secs` 라고 해보겠습니다.

```
+----------------------------+---+---+
|                            |   |   |
|                            |   |   |
|             A              | B | C |
|                            |   |   |
|                            |   |   |
+----------------------------+---+---+
+----------------------------+---+---+---------
0                            100 110 120
```
- 이 때의 <mark>평균 turnaround time</mark> 은 아래와 같습니다.
$$\textbf{Avg.}\text{turnaround time} = \frac{(100 - 0) + (110 - 0) + (120 - 0)}{3} = 110 \text{ secs}$$
- $A$ 가 긴 시간을 잡아먹기 때문에, 상대적으로 수행 시간이 짧은 $B$, $C$ 가 <u>불필요한 기다림</u>을 경험해야 합니다.
- 이 때, 작업의 순서를 바꾸더라도 <mark>최종 완료 시간은 동일</mark>하다는 점에 주목할 필요가 있습니다.
- 따라서, 최적화를 위해서는 `수행시간이 짧은 것부터 작업하도록 하는` 방식을 택할 수 있습니다.

---
# Shortest Job First (SJF)
