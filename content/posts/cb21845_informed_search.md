+++
title = "CB21845, Informed Search"
date = "2023-10-03"
+++

> *본 포스트는 부산대학교 CB21845 인공지능 수업을 듣고 작성하였습니다.*

# Evaluation Function
***[Informed Search](https://m.blog.naver.com/ndb796/220578642298)*** 는 ***[Evaluation Function](http://www.aistudy.com/heuristic/evaluation_function.htm)*** 를 적용하여 *Solution* 을 이루는 node 의 순서를 결정합니다.

***Evaluation Function*** 은 노드 $n$에 대하여 $f(n)$으로 표기합니다. $f(n)$ 은 <u>"노드 $n$을 통해 *goal* 까지 가는 거리(distance)"</u>라 생각할 수 있습니다. 

$f(n)$ 을 구성하는 함수는 $g(n)$과 $h(n)$이 있습니다.

$$f(n) = g(n) + h(n)$$

- $g(n)$: <u>시작 노드</u>부터 노드 $n$ 까지  오면서 든 ***Cost*** 입니다.
- $h(n)$: 노드 $n$ 부터 <u>목표 노드</u>까지 가는 길에 들 것으로 예상되는 ***Estimated Cost*** (heuristic) 입니다.

그리고 Priority Queue 를 이용해 *frontier node*를 관리합니다. 따라서 `최솟값` 등을 구하는 것이 용이합니다.

---

# Types
여기서는 두 종류의 ***Informed Search***를 살펴봅니다

---

## Greedy Best-First Search
***Evaluation Function***으로 오직 $h(n)$만 사용합니다.

즉, $f(n)$이 아래와 같은 형태를 가집니다.

$$f(n) = h(n)$$

다시 말해,  `현재 시야에서 가장 좋아보이는 노드를 선택`(greedy) 하는 방법입니다. 여기서 ***"가장 좋아보이는"*** 은 당연히 *goal*  까지 가는 것을 염두에 둔 의미입니다.

> ***Uniform-cost search***는 이와 달리 $f(n) = g(n)$의 *Evaluation Function*을 가짐을 상기할 필요가 있습니다. $g(n)$은 *goal* 을 바라보는 값이 아니므로 ***Uninformed Search***  임을 생각해볼 수 있습니다.

### Tree-Search Based
<u>***Not Optimal***</u>, <u>***Not Complete***</u> 이며 ***Infinite Loop***가 발생할 수도 있습니다. 이는 조상 노드에 대한 정보를 기억함으로써 해결할 수 있습니다.

*Leaf Node*를 계속 유지하고 있어야 하므로 *Exponential*한 크기의 메모리가 필요함을 직관적으로 알 수 있습니다.

최대 $m$의 depth를 가지는 Search Space에 대하여 $O(b^m)$ 의 <u>시간복잡도</u>와 <u>공간복잡도</u>를 가집니다.

### Graph-Search Based
$g(n)$대신에 $h(n)$를 쓴다는 점만 다를 뿐, ***Uniform-cost search***와 구현은 동일합니다.

---

## A* Search
***Total Estimated Solution Cost***를 최소화하는 탐색입니다.

<u>***Greedy Best-First Search***</u> 와 <u>***Uniform-cost Search***</u> 를 결합한 형태로, *Evaluation Function*  이 아래와 같습니다.

$$f(n) = g(n) + h(n)$$

$g(n)$대신에 $g(n) + h(n)$를 쓴다는 점만 다를 뿐, ***Uniform-cost search***와 구현은 동일하다고 볼 수 있습니다.

### Tree-Search Based
만약 $h(n)$ 가 ***Overestimates*** 되지 않으면, **A\* Search** 가 *Optimal* 함을 증명할 수 있습니다.

예를 들어, **Optimal Solution Cost** 를 $C^{*}$라 해보겠습니다. 

만약 *Suboptimal(optimal이 아닌) Goal node* $G_2$가 *frontier*에 들어왔다고 하면 아래와 같이 ***Evaluation Function***으로 *Cost* 를 계산할 수 있습니다.

$$
\begin{align}
f(G_2)&=g(G_2) + h(g_2) \newline
&=g(G_2) \\ \\ \\ \\ (\because h(G_2) = 0) \newline
&\therefore g(G_2) > C^{*}
\end{align}
$$

그런데 만약 *optimal solution path* 위의 한 노드 $n$ 에 대한 *Cost*를 계산해보면 아래와 같습니다

$$
\begin{align}
f(n) = g(n) + h(n) \leq C^{*}
\end{align}
$$

이는 $h(n)$ 값이 ***overestimates***되지 <u>않았기</u> 때문에 가능합니다. 아래 수식에 따라 낮은 *Cost* 를 갖는 노드를 *expand* 하므로,  $G_2$ 는 절대 *expand* 되지 않음을 알 수 있습니다.

$$f(n) < g(G_2)$$

다시 말해,  $h(n)$가 ***admissible*** 할 때 ***Optimality***가 보장됩니다.

### Graph-Search Based
**Graph-Search**는 *redundant path* 를 갖지 않기 위해  closed list 를 관리합니다. 따라서, ***Optimal Solution Path*** 를 만들지도 못할 가능성이 있습니다. 이를 해결하기 위해 `Consistency`라는 개념을 추가하면 ***Optimal***하게 만들 수 있습니다.

`Consistent Heuristic` 는 <u>현재 노드</u> $n$ 에서 계산된 $h(n)$ 값이 <u>이웃 노드</u> $n'$ 로 이동하는데 드는 비용 $c'$과 $h(n')$ 보다 항상 작다는 성질입니다.

$$h(n) < c' + h(n')$$

즉, 비용값은 경로를 찾아가며 <u>항상 같거나 증가하는 방향</u>성을 가짐을 의미합니다.

### Evaluation
따라서, ***A\* Search***는 <u>***Optimal***</u>하고 <u>***Complete***</u> 합니다.

또,  <u>시간복잡도</u>는 $h(n)$에 따라 *Exponential* 하며 <u>공간복잡도</u>는 $O(b^m)$ 입니다.
