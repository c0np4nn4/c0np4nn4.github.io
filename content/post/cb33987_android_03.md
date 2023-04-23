+++
title = "Android: Kotlin(2)"
description = "CB33987, android programming class"
date = 2023-04-22
toc = true

[taxonomies]
categories = ["Android", "Lect"]
tags = ["Android", "Lect"]

[extra]
math=true
+++


---

*2023 Spring, PNU, CB33987 (Professor Choi)*

# Kotlin (2)
- `Kotlin` 언어에 대해 좀 더 살펴봅니다.

***TL;DR***
- `예외처리`, `람다함수`, `고차함수`, `확장함수`, `오버로딩`, `오버라이딩`, `Null Safety` 에 대해 살펴봅니다.
---

## 예외처리
- `try-catch` 와 `throw` 의 두 방법이 있습니다.
  - `try-catch`: 정상 종료를 목적으로 함
  - `throw`: 에러 발생을 목적으로 함
```kotlin
fun main() {
  var divNum = 0
  try {
    1 / divNum
  } catch (e: Exception) {
    println(e)
  } finally {
    println("finally 내부 코드는 무조건 실행됨")
  }

  if (divNum == 0) {
    throw Exception("ERROR: 0 으로 나눌 수 없음")
  }
  println("1 / $divNum ==" + 1/divNum)
}
```

---

## 람다함수
- 간단히 함수를 정의하는 방법입니다.
- `함수형 프로그래밍` 개념이며, 함수를 param 으로 전달할 수 있습니다.
```kotlin
val function name: (param type) -> return type = { param -> body }
```
```kotlin
fun main() {
  val add: (Int, Int) -> Int = {
    // type 생략 가능
    a: Int, b: Int -> a + b
  }

  println(add(10, 20))
}
```

---

## 고차함수
- 람다 함수를 ***parameter*** 나 ***return value*** 로 사용하는 함수입니다.
- 이전에 살펴본 `filter` 함수도 이 고차함수임을 알 수 있습니다.

### 1. 함수를 인자로 받는 경우
- 함수를 `외부에서 정의` 하고, 내부에서는 적절히 사용합니다.
- 인자에서는 `(param type) -> return type` 을 명시합니다.
```kotlin
fun fun_as_param(msg: String, operation:(Int, Int) -> Int) {
  println("msg: " + str)

  val res = operation(10, 20)
  println("res: " + res.toString())
}

fun main() {
  fun_as_param("hello world") { a, b -> a + b }
}
```

### 2. 함수를 반환하는 경우
- `lambda` 함수를 반환합니다.
```kotlin
fun gen_adder(): (Int, Int) -> Int {
  return {a, b -> a + b}
}

fun main() {
  val adder = gen_adder()

  println("res: " + calc(10, 20))
}
```

---

## 확장함수
- `클래스` 내부에 method 를 추가하고 싶을 때 사용하는 방법입니다.
```kotlin
class Adder() {
  fun add(a: Int, b: Int): Int {
    return a + b
  }
}

// 확장함수
fun Adder.add(a: Int, b: Int, c: Int): Int {
  return a + b + c
}

fun main() {
  var adder = Adder()

  println(adder.add(1, 2))
  println(adder.add(1, 2, 3))
}
```

---

## 오버로딩
- 동일한 이름을 갖는 함수를 인자만 다르게 하여, 여러개 정의하는 방법입니다.

```kotlin
class Adder() {
  fun adder(a: Int, b: Int): Int {
    return a + b
  }

  // 오버로딩
  fun add(a: Int, b: Int, c: Int): Int {
    return a + b + c
  }
}
```

---

## 오버라이딩
- 자식 클래스에서 부모 클래스의 메서드를 재정의하는 방법입니다.
- 명시적으로 `override` 키워드를 사용합니다.
- 이렇게 함으로써, `필요한 부분만 상속받고 그외의 부분은 수정이 가능하다`는 이점이 있습니다.

```kotlin
open class Beverage() {
  open fun drink(money: Int): Int {
    return money
  }
}

class Fanta(): Beverage() {
  var price = 1500

  // 오버라이딩
  override fun drink(money: Int): Int {
    if (money >= price) {
      return money - price;
    } else {
      println("ERROR: Not enough money")
      return money;
    }
  }
}
```

---

## Null Safety
- `Kotlin` 은 기본적으로 `Null` 을 허용하지 않습니다.
- 하지만 Type 뒤에 '`?`'를 붙여 `Null` 을 허용할 수 있습니다.
- `Null` 이 허용된 변수를 아래와 같이 활용할 수 있습니다.
  - `세이프콜 (?.)`: 변수가 `Null` 이면 메서드를 실행하지 않습니다.
  - `엘비스 연산자 (?:)`: 변수가 `Null` 이면 `?:` 뒤의 값을 반환합니다.
```kotlin
fun main() {
  var msg1: String = null
  println(msg1?.length)

  var msg2: String? = null
  var len = (b?.length) ?: -1
  println(len)
}

// [output] 
// null
// -1
```

---

# 참고
- `고차함수`, [블로그](https://taehyungk.github.io/posts/android-kotlin-high-order-function/)
