+++
title = "React Basic"
description = "IDK"
date = 2023-07-22

[taxonomies]
categories = ["Web","React"]
tags = ["React", "Javascript", "Front-end"]

[extra]
math=true

+++

---

- 시간은 2시쯤 되었지만.. 잠이 안오기도 하고 어차피 해야 할 <txtylw>프론트엔드 공부</txtylw>였기에  **4시간** 정도 *`리액트 입문`* 으로 소개된 27개의 아래 링크의 글들을 읽어보았습니다.
- 이 

> [벨로퍼트와 함께하는 모던 리액트](https://react.vlpt.us/basic/)

- 위 글은 개인적으로 크게 세 파트로 나눠진다고 이해했습니다.
    - <txtylw>React</txtylw> 입문
        - `React`의 필요성
        - 컴포넌트
        - JSX
        - props
        - 조건부 렌더링
        - useState (Hook 개념)
        - onChange, onClick 등 `event` 관련 함수
        - useRef (특정 DOM 선택)
        - useEffect (mount, unmount, update)
    - <txtylw>최적화</txtylw>
        - useMemo (값 재사용)
        - useCallback (함수 재사용)
        - React.memo
        - useReducer (dispatch 활용)
        - 커스텀 Hook
        - React.createContext (Context API)
        - Immer (불변성 관리)
    - <txtylw>클래스형 컴포넌트</txtylw>
        - 클래스형 컴포넌트 개념
        - Lifecycle Method (생략함)
            - componentDidCatch (생략함)
- 그리고 마지막으로 `Prettier`, `ESLint`, `Snippet` 등을 소개하는데 앞선 두 개는 현재 사용하는 `LunarVim`를 이용해서 LSP 설정을 해주고 있기 때문에.. 개념 정도만 훑고 넘어갔습니다.
---
- 만약 규모가 큰 프론트엔드 프로젝트를 진행한다면, 최적화를 위해 사용되는 여러 방법들을 어떻게 관리할 수 있을까 좀 궁금해졌습니다.
- 기회가 된다면 `프로그램 개발 방법론` 같은 수업을 한번 들어보고 학부를 졸업하는 것도 좋을 것 같다는 생각이 듭니다.
- 여담으로, `Dispatcher`, `Reducer`의 개념이 좀 반가웠습니다.
