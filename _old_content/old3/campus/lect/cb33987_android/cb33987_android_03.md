+++
title = "Android(3): Kotlin(2)"
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
  - `try-catch`: 정상 종료를 목적으로 합니다.
    - `try`: 에러가 발생하는지 체크합니다.
    - `catch`: 에러가 발생할 경우 분기되는 블록입니다.
    - `finally`: 에러 발생 여부와 관계없이 무조건 수행되는 블록입니다.
  - `throw`: `catch` 블록에서 자주 쓰이며, 정의된 에러를 발생시킵니다.
```kotlin
fun main() {
  var divNum = 0
  // 나눗셈을 시도합니다.
  try {
    1 / divNum
  }
  // 에러 발생시, `catch` 내부의 코드를 수행합니다.
  catch (e: Exception) {
    println(e)
    // 에러가 발생했음을 전파합니다.
    throw Exception("ERROR: 0 으로 나눌 수 없음")
  } 
  // 에러 발생 여부와 관계 없이, `finally` 내부의 코드를 수행합니다.
  finally {
    println("finally 내부 코드는 무조건 실행됨")
  }

  println("1 / $divNum ==" + 1/divNum)
}
```

---

## 람다함수
- 간단히 함수를 정의하는 방법입니다.
- `함수형 프로그래밍` 개념이며, ***함수를 param 으로 전달할 수 있다*** 는 특징이 있습니다.
```kotlin
val function_name: (PARAM_TYPE) -> RETURN_TYPE = { param -> body }
```
- `param type` 은 `param` 에서 명시하고 있을 경우 생략 가능합니다.
```kotlin
val function_name: () -> RETURN_TYPE = { param: Type -> body }
```
- 또한, `body` 의 마지막 줄 실행 결과가 `return value`가 됩니다.

```kotlin
fun main() {
  val add: (Int, Int) -> Int = {
    // Type(Int) 생략 가능
    // a + b 가 마지막 줄이므로, 계산 결과가 return 됨
    a: Int, b: Int -> a + b
  }

  // output == 30
  println(add(10, 20))
}
```


---

## 고차함수
- 람다 함수를 ***parameter*** 나 ***return value*** 로 사용하는 함수입니다.
- 이전에 살펴본 `filter` 함수도 이 고차함수의 일종입니다.
```kotlin
var filt_list = list.filter { x > 0 };
```

---

### 1. 함수를 인자로 받는 경우
- 함수를 `외부에서 정의` 하고, 내부에서는 적절히 사용합니다.
- 인자에는 `(param type) -> return type` 을 명시합니다.
```kotlin
fun fun_as_param(msg: String, operation:(Int, Int) -> Int) {
  println("msg: " + str)

  val res = operation(10, 20)
  println("res: " + res.toString())
}

fun main() {
  // 10 + 20 == 30 을 출력합니다.
  fun_as_param("hello world") { a, b -> a + b }
}
```
- 동작을 `외부에서 정의` 하고 `내부에서 사용` 한다는 점을 기억할 필요가 있습니다.

---

### 2. 함수를 반환하는 경우
- `lambda` 함수를 반환합니다.
```kotlin
fun gen_adder(valid: Boolean): (Int, Int) -> Int {
  if (valid) return {a, b -> a + b}
  else return {a, b -> a * b * 0}
}

fun main() {
  val adder = gen_adder(true)

  println("res: " + calc(10, 20))
}
```

- 인자에 따라 함수의 동작을 다양하게 반환받을 수도 있습니다.

---

## 확장함수
- `클래스` 내부에 method 를 추가하고 싶을 때 사용하는 방법입니다.
- `클래스` 를 직접 수정할 수 없을 때 유용하게 쓰일 수 있습니다. 
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
- `동일한 이름`을 갖는 함수를 인자만 다르게 하여, 여러개 정의하는 방법입니다.

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
- ***자식 클래스*** 에서 ***부모 클래스*** 의 메서드를 <txtred>재정의</txtred>하는 방법입니다.
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
