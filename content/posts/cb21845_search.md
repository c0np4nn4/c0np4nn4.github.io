+++
title = "CB21845, 1, solving problems by searching"
date = "2023-09-10"
+++

> *본 포스트는 부산대학교 CB21845 인공지능 수업을 듣고 작성하였습니다.*

# What is Problem Solving
**Problem Solving** 이란, `initial state`로부터 `goal state`로 상태를 변환하는 `sequence of operators (path)`를 ***찾는(search)*** 과정이라 할 수 있습니다.

<center>
<img alt="init_to_goal" src="../../Class/CB21845_AI/1_1.png" />
</center>

그리고, 때로는 `sequence of operators`의 크기를 줄이는 것이 중요하기도 합니다.

예를 들어, 잘 알려진 ***Traveling Salesman Problem*** (`TSP`) 을 생각해 볼 수 있습니다.

<center>
<img alt="tsp problem" src="https://optimization.cbe.cornell.edu/images/e/ea/48StatesTSP.png" />
</center>

`TSP`는 판매원(salesman)이 각 도시를 **단 한 번**만 방문하면서 **모두** 방문하는 방법을 찾는 문제입니다.

문제를 해결하는 가장 직관적인 방법은 ***모든 경우의 수를 모두 탐색*** 한 뒤 가장 cost 가 낮은 것을 고르는 것입니다.
하지만, 도시가 $N$ 개일 경우 총 $N!$ 가지의 경우를 모두 비교해야 하므로 매우 **효율적이지 못한** 방법입니다.

따라서, *문제를 해결* 하는 것이 아니라 ***정답에 가까운 결과*** 를 얻어낼 수 있도록 하는 알고리즘도 고려해 볼 수 있습니다.
이러한 유형의 방법을 *Heuristic (휴리스틱)* 이라 부르며, 쉽게 말해 **어림짐작** 하는 방식입니다.
절대 **정답**임을 보장할 수는 없지만 **최적해**를 찾음으로써 **효율적으로** 답을 찾을 수 있습니다.

`Heuristic`과 관련해 유명한 예시로 ***튜링 테스트*** 가 있습니다.

<center>
<img alt="turing test" src="https://upload.wikimedia.org/wikipedia/commons/5/55/Turing_test_diagram.png" />
</center>

그림의 `C`는 전달받은 답을 검사하는데, 만약 답을 한 주체가 **기계**인 `A`인지 **사람**인 `B`인지 분간할 수 없다면 통과하는 테스트입니다.

테스트는 ***정확한 답*** 을 줄 수 있는지 검사하는 것이 아니라, *얼마나 사람이 내놓는 답과 유사한지* 를 검사합니다.
따라서, 사람인지 기계인지 어림짐작하는 `Heuristic`한 검사임을 알 수 있습니다.

---

`Problem Solving` 은 *informal* 한 문제를 *formal* 한 형태로 기술함으로써 시작됩니다.
`Problem Solving` 을 위해 문제에 대한 아래의 개념들을 정의할 필요가 있습니다.
> *Initial (start) state*
> 
> *Operators*
>
> *Goal state*
> 
> *Path cost functions*

이 중 *Init state* 와 *Operator* 가 ***State Space*** 를 구성합니다.
쉽게 말해, <u>초기 상태가 얼마나 복잡한지</u>와 <u>상태 변환 후 얼마나 다양한 결과가 나오는지</u>에 따라 **모든 경우의 수**가 결정되는 것입니다.

*Path cost function* 은 *Goal State* 로 향하는 과정에서의 비용을 계산하기 위해 활용됩니다.

`Problem`에 대한 **Solution**은 <u>Goal State</u> 자체일 수도 있지만, 가장 <u>Cost 가 낮은 State</u> 를 **Optimal Solution** 으로 가질 수도 있습니다.

## Problem, "8-puzzle"
<center>
<img alt="eight puzzle" src="https://media.geeksforgeeks.org/wp-content/uploads/puzzle-1.jpg" />
</center>

***8-puzzle*** 문제에 대해, `Problem Solving`을 위해 필요한 개념들을 정의하면 아래와 같습니다.
> *State*
>   - 9개의 칸에 숫자들이 배치된 상태
> 
> *Operators*
>   - `빈 칸`이 상, 하, 좌, 우 중 하나로 이동
>
> *Goal state*
>   - `정답`으로 상정한 것과 동일한 상태
> 
> *Path cost functions*
>   - 한 번의 움직임에 `1`을 부과

## Problem, "8-Queens"
<center>
<img alt="eight queens" src="https://camo.githubusercontent.com/4fef54b90f5acf68e6dd87bd2e3af094790dce85d1b055a161879d955df7a151/687474703a2f2f6d617468776f726c642e776f6c6672616d2e636f6d2f696d616765732f6570732d6769662f517565656e734d61785f3830302e676966" />
</center>

체스 보드 위에 $N$ 개의 퀸이 *서로를 공격하지 않는 상태로 배치* 된 상황을 찾는 문제입니다.
> *State*
>   - 0 ~ 8개의 퀸이 보드 상에 배치된 상태
> 
> *Operators*
>   - 퀸을 보드 상의 빈 칸에 배치
>
> *Goal state*
>   - $8$ 개의 퀸이 모두 *서로를 공격하지 않는 상태로 배치* 된 상태

그러나, 만약 현재의 *Operator* 대로 알고리즘을 수행하면 ***모든 보드 상의 빈 칸을 탐색해야 한다*** 는 오버헤드가 발생합니다.
따라서, 아래와 같이 `formulation`에 수정을 가할 수 있습니다.
> *Operators*
>   - 퀸을 보드 상의 빈 칸에 배치, 단 윗 줄(row)에서부터 차례로 탐색
>

즉, 각 row 를 단위로 내려오면서 탐색하게 되면 ***State Space*** 를 아래와 같이 감축할 수 있습니다.
$$3 \times 10^{14} \rightarrow 2,057$$

---

## Search Tree
`Search` 과정을 **Tree**로 그려서 문제 풀이 과정을 나타낼 수 있습니다.
**Root Node** 는 `Init State`, **Branch**는 `Action(Op)`, **Node**들은 대응되는 `State`가 됩니다.

<center>
<img alt="state tree" src="https://raw.githubusercontent.com/xibsked/menka/master/books/artificial-intelligence/059b357f787ed1b736f3188b23c143271.png" />
</center>

이 때, *확장될 수 있는* **Leaf Node** 들을 `Frontier (aka Open List)`라 부릅니다.

그리고, `Search Tree` 개념을 이용해 아래와 같은 알고리즘을 작성할 수 있습니다.

```Ruby
function TREE-SEARCH(problem) returns a solution or failure
{
    initialize the FRONTIER using the initial state

    loop do
    {
        if the FRONTIER is EMPTY
        {
            return failure
        }

        else
        {
            choose a LEAF NODE and remove it from the FRONTIER

            if the NODE contains a Goal State
            {
                return solution
            }

            expand the chosen NODE, adding the result nodes to the FRONTIER
        }
    }
}
```

## Search Graph
만약 아래로 자라나는 **Tree** 가 아니라 여러 노드가 연결된 **Graph** 의 경우에는 조금 다른 방식으로 탐색을 해야합니다.

<center>
<img alt="graph" src="https://www.101computing.net/wp/wp-content/uploads/A-Star-Search-Algorithm-Graph.png" />
</center>

자명하지만 ***한 번 방문한 노드를 재방문*** 하는 것은 매우 **비효율적** 이므로,
이를 방지하기 위해 ***방문한 노드 정보를 기억*** 하기 위한 `Closed List` 를 함께 관리해줘야 합니다.

---

(wip)
