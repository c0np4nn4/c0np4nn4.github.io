+++
title = "Android(7): Fragment"
description = "CB33987, android programming class"
date = 2023-04-24
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect", "Fragment", "ViewPager"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# Jetpack
- 구글에서 개발한 `Android` 앱 개발용 ***확장 라이브러리*** 입니다.
- `Androidx` 로 시작하는 패키지명을 사용합니다.
- 아래의 다양한 목적으로 사용됩니다.
  - 앱 개발 시 `권장 아키텍쳐` 제공
  - 다양한 기능 제공(e.g. 스와이프를 통한 뷰 변경)
  - 호환성 문제 해결
---
- 이 중 화면 구성과 관련된 `androidx` 라이브러리는 다음과 같습니다.
  - androidx.`appcompat`: 앱 API 레벨 호환성 해결
  - androidx.`fragment`: 액티비티처럼 동작하는 뷰
  - androidx.`recyclerview`: 목록 화면 구성
  - androidx.`viewpager2`: 스와이프로 넘기는 화면 구성
  - androidx.`drawerlayout`: 옆에서 서랍처럼 열리는 화면 구성
---

# Appcompat
- `Android` 앱의 화면을 구성하는 `Activity` 를 만드는데 사용될 수 있습니다.
- 특이한 것은 `API` 레벨마다 다르게 제공되는 `Widget` 을 통합하여 사용할 수 있다는 점입니다.
  - 이를 통해, `호환성 문제`를 해결할 수 있습니다.
- 이 밖에도, `앱 테마` 와 액티비티 상단의 `액션 바`등을 다룰 수 있게 도와줍니다.

## Appcompat- API 호환성 해결
- Gradle 파일의 *dependency* 항목에 `appcompat` 을 추가합니다.
<img src="../../images/post/android/appcompat_01.png" alt="adsf" width="600rem"/>

- `Activity` 파일에서 `appcompat` 라이브러리를 추가합니다.
<img src="../../images/post/android/appcompat_02.png" alt="adsf" width="600rem"/>

- `AppCompatActivity` 클래스를 상속받아 작성합니다.
<img src="../../images/post/android/appcompat_03.png" alt="adsf" width="600rem"/>

---

# Fragment
- `액티비티의 모듈식 세션` 으로 생각하면 됩니다.
- `Fragment` 클래스를 선언하고, 원하는 동작을 `View` 형태로 구현합니다.
- `XML` 파일에 `Fragment` 를 출력할 `FrameLayout` 을 생성합니다.
- `FragmentManager` 객체를 생성하고, `FragmentTransaction` 객체를 이용해 `Fragment`를 추가하거나 제거합니다.

---

# RecyclerView
- Androidx 가 제공하는 `ViewGroup` 중, 여러 항목을 리스트로 나열해주는 `View` 입니다.
- 아래의 구성 요소를 갖습니다.
  - `Adapter`: 아이템 뷰를 생성하는 역할을 합니다.
  - `LayoutManager`: 아이템 뷰가 나열되는 형태를 관리하는 역할을 합니다.
  - `ViewHolder`: 아이템 뷰가 저장되는 객체입니다. `Adapter`에 의해 관리됩니다.
---
- `Recycler View` 를 구현하는 단계는 아래와 같습니다.
  - `XML` 에 `RecyclerView` 추가
  - `RecyclerView` 위에 표시될 아이템에 대한 `item.xml` 작성
  - `RecyclerView.Adapter` 를 상속받아 `Adapter` 구현
    - *getItemCount()*
    - *onCreateViewHolder()*
    - *onBindViewHolder()* 
  - `메인.kt` 에서 `LayoutManager`, `Adapter` 를 할당

---

# ViewPager
- 스와이프로 화면을 전환하는 `View` 입니다.
- 아래 두 가지 방법으로 구현이 가능합니다.
- `RecyclerView.Adapter` 이용
  - `ViewPager.Adapter` 로 `Adapter` 를 변경하여 구현합니다.
- `FragmentStateAdapter` 이용
  - `Fragment` 를 담고 있는 ***리스트*** 를 선언합니다.
  - `createFragmet()` 함수에서 반환하는 `Fragment` 객체를 출력합니다.
---
- `RecyclerView.Adapter` 를 이용하는 경우, `LayoutManager` 는 따로 선언할 필요 없이, `Orientation` 만 지정해주면 됩니다.

---

# Drawer Layout
- Activity 화면에 보이지 않던 내용을 서랍처럼 꺼내서 보여주는 기능입니다.
---
- 최상위 `Layout` 을 `DrawerLayout` 으로 정합니다.
- 그 아래에 `Layout` 과 `Widget` 들을 선언해줍니다.
- `메인.kt` 에서는 `ActionBarDrawerToggle()` 함수를 이용해 `toggle` 을 제어하는 코드를 작성합니다.

---



---
# 참고
- Action Bar, [블로그](https://recipes4dev.tistory.com/141)
- RecyclerView, [블로그](https://velog.io/@ryalya/Android-RecyclerView)
- View Binding, [블로그](https://velog.io/@iamjm29/Android-viewBinding%EC%9C%BC%EB%A1%9C-%EA%B9%94%EB%81%94%ED%95%9C-%EC%BD%94%EB%93%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0)
