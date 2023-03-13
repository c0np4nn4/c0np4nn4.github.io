+++
title = "Sort"
date = 2023-02-28

[taxonomies]
categories = ["Algorithm"]
tags = ["Sort", "Merge Sort", "Quick Sort"]

[extra]
toc = true
+++

---

# Basic Sort
- 삽입, 선택, 버블 정렬 등이 있습니다.

## 선택 정렬
- 선택 정렬 (selection sort)은 아래와 같이 동작합니다.
  - 주어진 리스트에서 <kbd>Min</kbd>  값을 찾습니다.
  - 찾은 값을 리스트의 맨 앞자리 값과 `Swap` 합니다.
  - 맨 앞자리를 제외한 나머지 리스트에 대해 위 두 과정을 반복합니다.

- 모든 리스트를 돌아야 하기 때문에, $n + (n-1) + \dots + 1 = \frac{(n)(n+1)}{2} = O(n^2)$ 의 시간 복잡도를 갖습니다.

<details>
<summary><mark>Code</mark></summary>
<p>

```cpp, linenos
// selection_sort();
for (int i = 0; i < n; i++) {
  int min_idx = 0;

  for (int j = i; j < n; j++) {
    if (arr[j] < arr[min_idx]) min_idx = j;
  }

  swap(arr[i], arr[min_idx]);
}
```
 
 </p>
 </details>

## 버블 정렬
- 버블 정렬 (bubble sort)은 인접한 두 수를 하나씩 정렬해가며 동작합니다.

- 인접한 두 수 $(a, b)$ 에 대하여,
  - $a < b$ 라면 pass
  - $a > b$ 라면 `Swap` 

---

# Merge Sort
- **STL** 을 사용할 수 없을 때, 재귀적으로 구현해서 사용되는 것이 <u>**권장**</u>되는 알고리즘 입니다.
- 서로 다른 두 리스트 $A, B$ 를 병합(merge)하여 새로운 리스트 $C$로 만드는 개념을 활용합니다.

<center>
<img src="/algorithm/img/merge_sort.png" alt="Merge Sort Image" w=600 h=400) />
</center>

- 위와 같은 병합에서의 시간 복잡도는 각 리스트의 모든 원소를 돌아야 하므로, 각각의 크기가 $A, B$ 일 때 $O(A + B)$ 가 됩니다.

- <mark>Merge Sort</mark> 는 아래의 단계로 나누어 볼 수 있습니다.
  - <u>하나의 리스트</u>를 <u>두 개</u>로 나눈다.
  - 나눈 리스트를 정렬한다.
  - <u>두 개의 리스트</u>를 <u>하나</u>로 합친다.

- 위 그림에서는 크기 <kbd>8</kbd> 짜리 리스트 하나를 <kbd>4</kbd> 짜리 리스트 두 개로 나누고 있습니다.

- 이를 재귀적으로 구현하면 임의의 길이 <kbd>$N$</kbd>에 대한 <mark>Merge Sort</mark>가 가능함을 알 수 있습니다.

- 아래와 같은 리스트가 있다고 해보겠습니다.

<center>
<img src="/algorithm/img/sample_list.png" alt="Merge Sort Image" w=600 h=400) />
</center>

- "*하나의 리스트를 둘로 나눈다*" 의 재귀적 결과는 아래와 같습니다.

<center>
<img src="/algorithm/img/list_split.png" alt="Merge Sort Image" w=600 h=400) />
</center>

- "*나눈 리스트를 정렬한다.*" 와 "*두 개의 리스트를 하나로 합친다.*" 의 결과는 아래와 같습니다.

<center>
<img src="/algorithm/img/overview.png" alt="Merge Sort Image" w=600 h=400) />
</center>


## 시간복잡도

- 시간복잡도 관점에서 알고리즘의 성능을 살펴보겠습니다.

<center>
<img src="/algorithm/img/overview_split.png" alt="Merge Sort Image" w=600 h=400) />
</center>

- 각 줄의 리스트에 대해 $1, 2, \dots, 2^k$ 번 <mark>split</mark> 하는 동작을 수행합니다.

- 따라서, $1 + 2 + \dots + 2^k = 2N - 1 = O(N)$ 입니다.

<center>
<img src="/algorithm/img/overview_merge.png" alt="Merge Sort Image" w=600 h=400) />
</center>

- 앞서 $A, B$ 인 두 리스트를 병합할 때의 시간복잡도는 $O(A+B)$ 임을 보았습니다.

- <u>병합</u> 과정을 자세히 살펴보면 $N/(2^t)$ 번의 연산이 $(2^t)$ 번 필요하다는 것을 알 수 있습니다.

- 길이가 <kbd>2</kbd>인 리스트 2개를 병합하여 길이 <kbd>4</kbd> 인 리스트를 만드는 줄을 살펴보겠습니다.

- 병합에는 총 <kbd>4</kbd>가 필요하고, 이 과정이 오른쪽 리스트 `((1, 3), (2, 7))`까지 합쳐 <kbd>2</kbd>번 있으므로, 한 번의 병합에 <kbd>8</kbd>, 즉 $N$ 이 필요함을 알 수 있습니다.

- 또, <u>분할</u> 과정에서 길이 <kbd>1</kbd>인 리스트까지의 층이 $\log N$ 임을 알 수 있습니다.

- 따라서, <u>병합</u> 과정에서 $N$ 의 작업을 $\log N$ 번 해야하므로 시간복잡도는 $O(N \log N)$ 가 됩니다.

- 최종적으로 <u>분할</u>과 <u>병합</u> 중 더 큰 시간복잡도인 $O(N \log N)$ 이 <mark>Merge Sort</mark> 의 시간복잡도가 됩니다.

## Stable Sort

- 앞서 살펴본 예시는 각 원소가 **확연히** 다른 경우였습니다.

- 만약 동일한 원소를 대상으로 정렬이 필요한 경우, 어느 것을 먼저 넣어야 할지에 대해 생각해 볼 필요가 있습니다.

- 이 때, 리스트의 순서 ($A, B$ 같은 알파벳 순서 등)를 유지하며 정렬하는 것으로 **Stable Sort**를 이해하면 되겠습니다.

- 즉, <u>이전 상태의 정렬된 성질을 유지</u>하며 <u>새로이 정렬</u>되는 개념으로 생각하면 되겠습니다.

---

# Quick Sort

- 대부분의 라이브러리에 구현된 <mark>Sort</mark> 함수는 이 <mark>Quick Sort</mark> 를 이용하고 있다고 합니다.

- <mark>Quick Sort</mark> 도 <mark>Merge Sort</mark> 처럼 **Divide-and-Conquer** 방식의 재귀적 호출로 정렬하는 알고리즘입니다.

- <mark>Quick Sort</mark> 에서는 <kbd>Pivot</kbd> 이라는 개념을 활용하고, 기존 리스트 내에서 정렬을 마치는 **In-Place Sort** 방식으로 동작한다고 합니다.

- 일단은 [wiki](https://en.wikipedia.org/wiki/Quicksort) 로 설명을 대체하겠습니다.
