+++
title = "CB23606, 2, Phases"
date = "2023-09-10"
+++

# Overview

<center>
<img alt="phase overview" src="../../Class/CB23606_Compiler/2_1.png" />
</center>

위 그림과 같이 `전단부`와 `후반부`와 구성된 컴파일러는 가운데에 **모래시계**와 같은 형태의 단계가 있으며, 트리구조로 표현되는 ***중간 표현(IR)*** 이 있음을 이해하면 됩니다.

`전단부`에서는 `소스 코드`를 **분석(Analysis)** 하여 여러 의미 있는 정보를 수집합니다.
그리고, `후반부`에서는 중간표현인 *Tree* 에 대한 정보를 토대로 의미를 **생성(합성, 종합)** 합니다. 

컴파일러의 전체 과정을 좀 더 자세히 나타내면 아래와 같습니다.

<center>
<img alt="compilation overview" src="../../Class/CB23606_Compiler/2_2.png" />
</center>

각각 단계에 대한 자세한 사항은 추후 기록할 예정입니다.
