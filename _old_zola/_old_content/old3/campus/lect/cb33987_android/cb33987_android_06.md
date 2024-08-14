+++
title = "Android(6): Activity"
description = "CB33987, android programming class"
date = 2023-04-24
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect", "Activity", "Intent"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# Activity

---

# Component
- `Android` 아키텍처의 가장 중요한 요소로써, 앱을 구성하는 단위 입니다.
- 또한, 각 컴포넌트는 `독립적인 실행단위`로 진입 지점이 따로 없어 `앱 라이브러리` 등으로도 활용될 수 있습니다.

## 종류
- ***Activity***: 화면을 구성하는 가장 기본적인 `Component` 입니다.
- ***Service***: `Activity`와 상관없이 백그라운드에서 동작하는 `Component` 입니다.
- ***Broadcast Receiver***: 응용프로그램 및 장치에 메세지를 전달하기 위해 사용되는 Broadcasting 메시지가 발생하면 반응하는 `Component` 입니다.배터리 부족, 메세지 수신 등의 이벤트를 생각해볼 수 있습니다.
- ***Content Provider***: 앱 간 데이터 공유등을 위해 이용하는 `Component` 입니다.

---

# Intent
- `Android Component` 클래스는 시스템이 생성하고 실행합니다.
- 따라서, 특정 `Component` 를 실행하기 위해서는 시스템에 ***데이터*** 를 전달하여 생성을 요청해야 합니다.
- 이 ***데이터*** 를 담는 객체가 바로 `Intent` 입니다.
- 즉, `Intent`는 ***Component 실행을 위해 시스템에 전달하는 메세지*** 입니다.
- 이를 활용하면, 외부 앱의 `Component` 를 `Intent` 로 실행할 수도 있습니다.


---

# Activity
- `Activity` 는 `Component` 중 "***화면을 구성하는 `Component`***" 입니다.
- 앱을 실행하면 지정된 `Activity` 가 실행되며, 사용자는 해당 `Activity` 가 그려주는 화면을 보게 됩니다.
- `Activity` 는 `AndroidManifest.xml` 파일에 명시되며, `<activity>` 태그로 등록해야 합니다.
  - 이와 마찬가지로 `service`, `broadcast receiver`, `content provider` 도 매니페스트 파일에 등록해야 합니다.

## LifeCycle
- `Activity` 가 생성되어 소멸하기까지의 과정을 살펴볼 필요가 있습니다.
- `Activity` 클래스는 `Activity`의 상태 변화 관련 메서드를 제공합니다.
- `Activity` 의 상태는 크게 아래 세 가지로 나눠볼 수 있습니다.
  - `활성`: 화면이 출력되어 있고, 사용자가 이벤트를 발생시킬 수 있는 상태
  - `일시 정지`: 화면은 출력되고 있지만, 이벤트 발생은 불가능한 상태
  - `비활성`: 화면 출력이 되지 않는 상태

<img src="https://developer.android.com/guide/components/images/activity_lifecycle.png?hl=ko" alt="adsf" width="600rem" style="background:skyblue"/>
<center>
출처: <a href="https://developer.android.com/guide/components/activities/activity-lifecycle?hl=ko" >Android Docs</a>
</center>

---

## 양방향 Activity (Exercise)
- `Intent` 를 이용해서 양방향으로 소통하는 `Activity` 를 구현해볼 수 있습니다.
- 편의상 처음 시작하는 `Activity` 를 `MainActivity`라 부르고, 다음으로 시작하는 `Activity` 를 `SubActivity` 라 부릅니다.
- 둘 사이에 데이터를 주고 받을 때는, `Intent` 로 필요한 정보를 담은 뒤 ***시스템*** 에 요청을 보내야 합니다.
- 진행 과정을 나열하면 아래와 같습니다.
> - `MainActivity`
>   - `Intent` 에 `putExtra()` 메서드를 이용해 필요한 데이터를 저장해둡니다.
>   - `SubActivity`로부터 결과값을 돌려받기 위해 `startActivityForResult()` 메서드를 사용해 `Intent` 를 전송합니다.
> - `SubActivity`
>   - `getExtra()` 메서드를 사용해 데이터를 받습니다.
>   - 일련의 작업 후, `Intent` 에 `putExtra()` 메서드로 데이터를 저장합니다.
>   - `setResult()` 메서드를 통해 `MainActivity` 로 데이터를 보냅니다.
>   - `finish()` 메서드를 호출해 `SubActivity` 를 종료합니다.
> - `MainActivity`
>   - `onActivityResult()` 메서드를 오버라이딩하여 두 액티비티의 상황에 맞게 코드를 작성하고, `SubActivity` 가 보낸 데이터를 `getExtra()` 메서드로 받습니다.


<img src="../../images/post/android/activity_01.png" alt="adsf" width="400rem"/>
<center>
출처: Lecture Slide
</center>
