+++
title = "Android(5): Layout"
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

### 배치와 정렬
- `RelativeLayout` 내부의 `View` 들은 `배치` 속성과 `정렬` 속성을 가집니다.
  - `배치`
    - ***layout_<>***: 상대 `View` 기준 배치
      - `layout_above`, `layout_below`, `layout_toLeftOf`, `layout_toRightOf`, etc.
  - `정렬(align)`
    - ***layout_align<>***: 상대 `View` 기준 정렬
      - `layout_alignTop`, `layout_alignBottom`, etc
    - ***layout_alignParent<>***: 부모 `View` 기준 정렬
      - `layout_alignParentTop`, `layout_alignParentBottom`, etc

### ID 지정 
- `RelativeLayout` 에서는 ***상대*** 라는 존재를 지칭하는 것이 중요합니다.
- 따라서, 기준이 되는 `View`의 <txtred>id 선언</txtred>이 필수입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"
        android:id="@+id/image"
        android:layout_centerInParent="true" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="toLeftOf"
        android:layout_toLeftOf="@id/image" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="toRightOf"
        android:layout_toRightOf="@id/image" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignTop="@id/image"
        android:layout_toLeftOf="@id/image"
        android:text="toLeftOf &amp; alignTop" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@id/image"
        android:layout_toRightOf="@id/image"
        android:text="toRightOf &amp; alignBottom" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_toLeftOf="@+id/image"
        android:text="toLeftOf &amp; alignParentBottom" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_toRightOf="@+id/image"
        android:text="toRightOf &amp; alignParentBottom" />



</RelativeLayout>
```

<img src="../../images/post/android/layout_05.png" alt="adsf" width="400rem"/>

---

## FrameLayout
- `View`를 겹쳐서 출력하는 `Layout` 입니다.
- 추가한 `View` 순서대로 계속 겹쳐서 보여줍니다.
- `Visibility` 속성값을 이용해서 겹쳐진 `View` 를 보여주는 등의 방법으로 활용할 수 있습니다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"
        />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button1" />
</FrameLayout>
```

<img src="../../images/post/android/layout_06.png" alt="adsf" width="400rem"/>

---

## GridLayout
- 행과 열로 구성된 테이블 화면을 만드는 `Layout` 입니다.
- `GridLayout` 이 갖는 주요 ***속성*** 은 아래와 같습니다.
  - `Orientation`: ("vertical" | "horizontal")
  - `rowCount`, `columnCount`: 행과 열의 개수를 지정합니다.
- `GridLayout` 내의 요소들이 갖는 주요 ***속성*** 은 아래와 같습니다.
  - `layout_row`, `layout_column`: 위치할 행과 열의 정보를 명시합니다.
  - `layout_gravity`: 특정 `View` 를 확장해서 출력합니다.
  - `layout_columnSpan`, `layout_rowSpan`: 주변 행렬을 병합합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="match_parent"
    android:columnCount="3"
    android:orientation="horizontal"
    >
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_rowSpan="2"
        android:layout_columnSpan="2"
        android:layout_gravity="fill"
        android:text="A" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="B" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="C" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="D" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="E" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="F" />
</GridLayout>
```

<img src="../../images/post/android/layout_07.png" alt="adsf" width="400rem"/>

---
