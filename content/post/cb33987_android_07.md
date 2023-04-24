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


---
# 참고
- Action Bar, [블로그](https://recipes4dev.tistory.com/141)
