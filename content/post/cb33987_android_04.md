+++
title = "Android(4): View"
description = "CB33987, android programming class"
date = 2023-04-24
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect", "View", "Widget"]

[extra]
math=true
+++

---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# View & Widget

---

## View
- 안드로이드 앱의 `화면을 구성`하는 모든 요소를 의미합니다.
- 즉, `View` 들이 모여 `Activity` 컴포넌트를 구성하고, `Activity` 가 모여 앱을 구성합니다.

---

## View 의 종류
- `View` 는 ***가시성(visibility)*** 에 따라 크게 `Widget` 과 `ViewGroup` 으로 구분할 수 있습니다.
  - `Widget`: `View` 중 <u>시각적인 요소</u>를 의미합니다.
    - ***Button***, ***TextView***, ***ImageView***, etc.
  - `ViewGroup`: `View` 중 <u>영역적인 요소</u>를 의미합니다.
    - ***Layout***, etc.

---

## View 요소 배치
- 화면에 `View` 요소들을 배치하는 것은 `XML` 언어를 이용하여 수행합니다.
- 마치 `HTML` 로 웹페이지의 뼈대를 만드는 것과 비슷합니다.
- 최상단에는 ***선언 태그*** 를 붙이고, 모든 `View` 요소들은 `Layout` 태그 내에 포함되게 됩니다.

```xml
<!-- 선언태그 -->
<?xml version="1.0" encoding="utf-8"?>

<!-- Layout -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    >

    <!-- Button Widget -->
    <Button
        android:layout_width = "wrap_content"
        android:layout_height = "wrap_content"
        android:text = "Button" />
</LinearLayout>
```

---

## View 의 속성(attribute)
- `View` 요소의 `특징` 을 정할 수 있는 방법입니다.
- *크기, 내부 텍스트, 정렬* 등등에 이용할 수 있습니다.
- 이 중 `크기 속성`은 <txtred>필수 정보</txtred> 입니다.

---

### View 의 크기
- `크기 속성`은 크게 ***참조 방식***과 ***지정 방식*** 으로 결정할 수 있습니다.
  - ***참조***
    - `match_parent`: 부모 요소의 공간 범위를 참조
    - `wrap_content`: 본인 및 하위 요소의 공간 범위를 참조
  - ***지정***
    - `<num> + dp`: Widget 최적 단위
    - `<num> + sp`: 텍스트 최적 단위

```xml
<?xml version="1.0" encoding="utf-8"?>

<!-- Layout -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    >

    <!-- Button Widget -->
    <Button
        android:layout_width = "wrap_content"
        android:layout_height = "wrap_content"
        android:text = "Button" />

    <!-- TextView Widget -->
    <!-- Text Size: 40sp -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Text View"
        android:textSize="40sp" />
</LinearLayout>

```

<img src="../../images/post/android/widget_01.png" alt="adsf" />

---

## View 의 영역
- `View` 의 영역에 관한 속성으로는 `Padding` 과 `Margin` 이 있습니다.
  - `Padding`: `Widget` 내부의 여백을 설정
  - `Margin`: `Widget` 외부의 여백을 설정

```xml
<?xml version="1.0" encoding="utf-8"?>

<!-- Layout -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:background="@color/purple_200"
    android:padding="30dp"
    android:orientation="vertical"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/teal_700"
        android:text="부모 패딩 30dp"
        android:textSize="30sp" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="30dp"
        android:background="#89eeff"
        android:text="태그 마진 30dp" />

</LinearLayout>
```

<img src="../../images/post/android/widget_02.png" alt="adsf" width="200rem"/>

---

## View 다중 배치
- 하나의 `Layout` 안에 여러 개의 `View` 요소 배치가 가능합니다.
- 이를 위해 시각적 표현과 관련한 여러 속성을 적절하게 사용해야 합니다.
  - `색상`
    - ***backgroundTint=<RGB 색상코드>***
  - `회전`
    - ***rotation=<각도>***
  - `가시성`
    - ***visibility=("invisible" | "gone" | "visible")***
  - `활성화`
    - ***enabled=("true" | "false")***
    - ***clickable=("true" | "false")***

```xml
<?xml version="1.0" encoding="utf-8"?>

<!-- Layout -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:background="@color/purple_200"
    android:padding="30dp"
    android:orientation="vertical"
    android:gravity="center"
    >

    <!-- 기본 버튼 -->
    <Button
        android:backgroundTint="#999999"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="기본 버튼" />

    <!-- Visibility -->
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->
    <!-- [1] Invisible:  화면에 나타나지 않고, 영역을 차지함 -->
    <Button
        android:backgroundTint="#00FF00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="invisible"
        android:text="Visibility: Invisible" />

    <!-- [2] Gone: 화면에 나타나지 않고, 영역을 차지하지 않음 -->
    <Button
        android:backgroundTint="#00FF00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="gone"
        android:text="Visibility: Gone" />

    <!-- [3] Visible: 화면에 나타남 -->
    <Button
        android:backgroundTint="#00FF00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="visible"
        android:text="Visibility: Visible" />
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->

    <!-- Enabled -->
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->
    <!-- [1] true:  활성화 -->
    <Button
        android:backgroundTint="#FF0000"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:enabled="true"
        android:text="enabled: true" />

    <!-- [2] false:  비활성화 -->
    <Button
        android:backgroundTint="#FF0000"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:enabled="false"
        android:text="enabled: false" />
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->

    <!-- Clickable -->
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->
    <!-- [1] true: 클릭 가능함 -->
    <Button
        android:backgroundTint="#0000FF"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:clickable="true"
        android:text="clickable: true" />

    <!-- [2] false: 클릭 불가능 -->
    <Button
        android:backgroundTint="#0000FF"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:clickable="false"
        android:text="clickable: false" />
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->

    <!-- Rotation -->
    <!-- =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-= -->
    <!-- [+] rotation <num>: <num> 만큼 회전함 -->
    <Button
        android:backgroundTint="#FF00FF"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:rotation="20"
        android:text="Rotation: 20" />
</LinearLayout>

```

<img src="../../images/post/android/widget_03.png" alt="adsf" width="300rem"/>

---

## Widget 다루기
- `Widget` 은 보통 `알고리즘을 시작하는 트리거`로 사용됩니다.
- `Button` 을 클릭하여 메세지를 전송하는 앱을 생각해볼 수 있습니다.
- `Widget` 을 활용하여 어떤 알고리즘을 수행하는 큰 그림은 아래와 같습니다.
> - `XML` 로 `Widget` 을 배치합니다.
> - `Kotlin` 에서 `Widget` 을 코드와 연결합니다.
> - `Kotlin` 에서 `Widget` 을 활용해 알고리즘을 수행합니다.
- 즉, `XML` 을 프론트엔드, `Kotlin` 을 백엔드로 생각할 수 있습니다.

## Widget 연결하기
- `tools:context=".<클래스명>"` 을 활용하면 **자동 완성**기능을 사용할 수 있어, 보다 수월하게 코딩할 수 있습니다.
- `Widget` 의 `id`를 등록하면 `Kotlin` 에서 해당 객체를 다룰 수 있습니다.
  - `android:id = "@+id/<고유식별자>`

## Widget 객체
- `Kotlin` 에서 `Widget` 태그에 접근하기 위한 객체를 생성해야 합니다.
```xml
<Button
    android:backgroundTint="#FF00FF"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="버튼"
    android:id="@+id/Btn" />

```
```kotlin
class MainActivity: AppCompatActivity() {
  lateinit var btn: Button

  // ...

  btn = findViewById<Button>(R.id.Btn)

  // ...

  btn.setOnclickListener {
  
  }
}
```

- 태그와 연결된 객체는 태그에 접근하여 Read, Write 및 속성 변경 등이 가능합니다.
  



---

# 참고
- dp, sp의 차이: [블로그](https://holika.tistory.com/entry/%EB%82%B4-%EB%A7%98%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A6%AC%ED%95%9C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-dp%EC%99%80-sp-%ED%8F%B0%ED%8A%B8-%ED%81%AC%EA%B8%B0%EB%A1%9C-%EB%AC%B4%EC%97%87%EC%9D%84-%EC%8D%A8%EC%95%BC-%ED%95%98%EB%8A%94%EA%B0%80)
- View: [tistory 블로그](https://myugyin-watchtower.tistory.com/6)
- Tools:Context [Android Docs](https://developer.android.com/studio/write/tool-attributes?hl=ko)
