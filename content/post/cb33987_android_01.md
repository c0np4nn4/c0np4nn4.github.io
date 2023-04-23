+++
title = "Android: Introduction"
description = "CB33987, android programming class"
date = 2023-04-21
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# Android Programming

***TL;DR***
- Android Programming 을 본격적으로 하기에 앞서, Android 에 관해 살펴봅니다.
- Android 가 `무엇`인지, 어떤 `특징`과 `구성`을 갖는지 살펴봅니다.

## Android 개요
- `Linux Kernel` 기반의 모바일 운영체제(platform) 입니다.
- 2008년에 구글에서 **Android 1.0 버전**을 출시한 이후, 현재까지도 꾸준히 이용되고 있습니다.
- Software Stack 방식을 취한다고 합니다. ([참고](https://ko.wikipedia.org/wiki/솔루션_스택))
- 전 세계 모바일 플랫폼의 절대 다수를 차지한다고 합니다. (ios + android: 99+%)

---

## Android 특징
- `Java`, `Kotlin` 으로 개발할 수 있습니다.
- 개발된 Application 을 다양한 방법으로 배포(*Publish*) 가능합니다.
- `Open Source` Software Stack (platform) 입니다.
- Apache v2 license 로 배포됨

---

## Android 운영체제 구조
<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bb/Android_open_source_project.png/300px-Android_open_source_project.png" />
출처: wikipedia (https://en.wikipedia.org/wiki/Android_software_development)
</center>

- ***Linux Kernel***
  - <txtred>운영체제의 핵심 기능</txtred>을 수행
  - H/W 에 대한 제어 및 관리
- ***HAL (H/W Abstraction Layer)***
  - 같은 종류의 하드웨어에 대한 공통 명령어 집합
  - Android Framework에 H/W 기능을 이용할 수 있게 *표준 인터페이스* 를 제공
- ***Native Libraries***
  - Java Framework 외의 C/C++ 라이브러리 제공 (Webkit, OpenMAX AL, etc.)
- ***ART (Android Runtime)***
  - <txtred>앱에 대한 전체 실행을 주관</txtred>
- ***Android Framework***
  - Java로 구현된 API Framework
  - 모든 Android 앱이 사용하는 Toolkit
- ***Apps***
  - Application Programs

---

## Android 앱 구성요소
- `Java`, `Kotlin` 으로 작성되는 *Android Program* 은 `클래스`로 구성됩니다.
- 이는 크게 `일반 클래스`와 `컴포넌트 클래스`로 구분될 수 있습니다.
  - `일반 클래스`: 개발자가 정의한 클래스
  - `컴포넌트 클래스`: Android 시스템의 컴포넌트 클래스를 상속받아 생성한 클래스
- Android Program 은 다수의 컴포넌트로 구성됩니다.

---

## Android 4대 컴포넌트
- ***Activity***
  - <txtred>UI</txtred>와 관련된 컴포넌트
- ***Service***
  - <txtred>Background</txtred>에서 작업을 처리하는 컴포넌트
  - (i.e. 음악 앱의 경우, 화면 없이도 음악을 재생)
- ***Content Provider***
  - 앱 내에서 생성, 관리하는 <txtred>데이터를 다른 앱에 제공</txtred>
  - (i.e. 프사 변경 시 갤러리 App 으로부터 사진 정보를 가져옴)
- ***Broadcast Receiver***
  - 시스템 이벤트가 발생할 때 실행되게 하는 컴포넌트
  - 기기에서 발생한 이벤트를 App 에 전달함
  - (i.e. 부팅 완료, 배터리 방전, 앱 팝업(카톡 알림), etc.)

### 컴포넌트의 실행 특징
- 컴포넌트는 <txtred>독립적인 실행단위</txtred> 입니다.
- 이 독립성을 통해 조금 더 유연하게 프로그램을 활용할 수 있습니다.
  - (<u>카톡 아이콘을 눌러 실행시키는 경우</u>, <u>카톡 알람을 통해 실행되는 경우</u>가 모두 가능)
- 즉, <txtred>실행시점이 다양</txtred>할 수 있습니다.
- 또한, <txtred>앱 라이브러리</txtred> 라는 개념을 사용합니다.
- 이는 다른 앱의 컴포넌트를 라이브러리처럼 사용할 수 있다는 개념입니다.
  - (카톡에서 카메라 앱을 실행하는 것이 가능)

---

## Android 리소스
- 코드에서 사용하는 추가 파일을 의미합니다.
- 정적인 콘텐츠(문자열 등)를 생각하면 됩니다.
- 즉, <txtred>코드와 데이터를 분리</txtred>하여 유지관리에 도움이 됩니다.
- 앱에서 발생하는 데이터, 이벤트 결과 등을 리소스로 관리하면 편리합니다.
- 문자열 외에 <txtred>색상</txtred>, <txtred>레이아웃</txtred>, <txtred>이미지</txtred> 등 여러 데이터들을 다룰 수 있습니다.

---

## 개발자 워크플로우
<img src="https://developer.android.com/static/studio/images/developer-workflow.png?hl=ko" height="500dp"/>

- `Setup`
  - 작업환경 세팅입니다.
- `Write`
  - Android Studio 의 각종 Tool 을 활용하여 프로그램을 만듭니다(개발).
- `Build & Run`
  - APK 패키지로 프로젝트를 빌드합니다.
- `Iterate`
  - 디버깅과 최적화 단계입니다.
- `Publish`
  - 앱을 배포하는 단계입니다.

---

## Android Studio
- Android 전용 IDE 입니다.
- IntelliJ IDEA 를 기반으로 제작되었으며, `Java`, `Kotlin`, `C++` 를 지원합니다.
- 아래 특징을 갖습니다.
  - `범용성`: 모든 Android 기기용 프로그램을 개발할 수 있습니다.
  - `유연성`: Gradle 을 이용한 Build 로 유연한 프로그래밍 환경을 제공합니다.
  - `효율성`: 다중 APK 지원
  - `호환성`: Window, Linux, Mac 등 여러 운영체제에서 사용 가능합니다.
  - `편리성`: Android Program 개발을 위한 여러 Tool 제공
  - `확장성`: 다양한 오픈소스 라이브러리를 사용할 수 있습니다.

---

## Android Project 파일구조

<img src="../../images/post/android/android_files.png" alt="adsf" />

- ***app***
  - ***manifests***
    - AndroidManifest.xml
    - App 구성에 관한 필수 정보
  - ***java***
    - 소스 코드
  - ***res***
    - 리소스
- ***Gradle Scripts***
  - Build 및 Library 설정

---

## AAB 파일 생성
- 프로젝트를 AAB(Android App Bundle) 파일로 구운 뒤, 플레이스토어에 업로드 하면 최적화된 APK 를 생성

---

# Exercise
- 아래 View 들을 활용하여 GUI 화면을 만들어 봅니다.
  - **Button** 
  - **CalendarView** 
  - **textView**

<img src="../../images/post/android/android_ex01_01.png" alt="adsf" />

---
# 참고 자료
- 컴포넌트, velog: [Velog](https://velog.io/@hoyaho/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-4%EB%8C%80-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EF%BD%9C-Hello-Android)
- 리소스, tistory: [Tistory](https://aroundck.tistory.com/286)
- 프로젝트 구조, tistory: [Tistory](https://curryyou.tistory.com/366)
