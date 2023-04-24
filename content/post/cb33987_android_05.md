+++
title = "Android: Layout"
description = "CB33987, android programming class"
date = 2023-04-24
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect", "View", "Layout"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# Layout

---

## Layout
- `View` 의 한 종류로, `View` 들을 배치할 때 사용됩니다.

## Layout 종류
- `LinearLayout`: 선형 배치
- `RelativeLayout`: 상대적 위치 배치
- `FrameLayout`: 겹쳐서 배치
- `GridLayout`: 표 형태(Grid)로 배치
- `ConstraintLayout`: 계층 구조로 배치
- 이 밖에도 `AbsoluteLayout`, `StackLayout` 등 다양한 Layout 이 존재합니다.

<img src="../../images/post/android/layout_01.png" alt="adsf" width="400rem"/>
<center>
출처: Lecture Slide 
</center>

---

## LinearLayout
- 내부 `View` 들을 `가로, 세로 방향으로 나열`하는 `Layout` 입니다.

### Orientation
- 중요한 속성으로 `Orientation` 이 있으며, 아래 두 종류의 값을 갖습니다.
  - `vertical`: 세로(수직) 방향으로 내부 요소들을 나열합니다.
  - `horizontal`: 가로(수평) 방향으로 내부 요소들을 나열합니다.

---

### 중첩
- `Layout` 도 `View` 이므로, 당연히 다른 `Layout` 의 내부 요소가 될 수 있습니다.
- 즉, 여러 `Layout` 을 중첩하는 구조를 만들 수 있습니다.

---

### attribute: layout_weight
- `View` 에 가중치를 주는 속성이며, 이를 이용해 각각의 컴포넌트 크기를 조절할 수 있습니다.
- 예를 들어, 아래와 같이 여백이 남는 경우가 발생할 수 있습니다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼1" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼2" />
</LinearLayout>
```

<img src="../../images/post/android/layout_02.png" alt="adsf" width="400rem"/>

- 이 때, `layout_weight` 속성을 추가해주면 아래와 같이 여백을 채울 수 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="3"
        android:text="버튼1" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="버튼2" />
</LinearLayout>
```

<img src="../../images/post/android/layout_03.png" alt="adsf" width="400rem"/>

- 위 그림에서 두 버튼은 `3 : 1` 비율로 가로줄을 채우고 있습니다.
- 즉, `weight` 를 통해 <u>여백을 채울 수 있고</u>, <u>비율</u> 도 조절 가능함을 알 수 있습니다.

---

### attribute: gravity, layout_gravity
- `gravity` 는 ***정렬*** 과 관련한 속성입니다.
  - `layout_gravity`: <u>자신의 위치</u>를 정렬함
  - `gravity`: <u>내부 콘텐츠의 위치</u>를 정렬함
- `gravity` 를 명시하지 않으면, ***default*** 값은 `left|top` 입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <TextView
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:background="#F2F2D2"
        android:gravity="right|bottom"
        android:layout_gravity="center"
        android:text="Lorem Ipsum" />

</LinearLayout>
```

<img src="../../images/post/android/layout_04.png" alt="adsf" width="400rem"/>

---

## RelativeLayout
- 상대 `View` 의 위치를 기준으로 정렬하는 `Layout` 입니다.
- 크게 `배치` 속성과 `정렬` 속성으로 나뉩니다.
