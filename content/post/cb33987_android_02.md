+++
title = "Android(2): Kotlin(1)"
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

# Kotlin
- `Kotlin` 언어는 `JVM(Java Virtual Machine)` 에서 동작하는 프로그래밍 언어 입니다.
- 2017 년에 Google 에서 *Android 공식 언어* 로 지정하였습니다. ([jetbrains blog](https://blog.jetbrains.com/kotlin/2017/05/kotlin-on-android-now-official/))
- `Kotlin` 은 **객체지향** 과 **함수형 프로그래밍** 스타일을 지원한다고 합니다.
- `Kotlin` 의 주요 특징으로는 `안정성`이 있는데, 이는 ***Null*** 에 대해 언어 자체가 안정성을 보장하기 때문입니다.
  - ***Null Safety*** 에 관한 참고 내용: [Blog](https://shinjekim.github.io/kotlin/2019/10/01/Kotlin-%EB%84%90-%EC%95%88%EC%A0%84%EC%84%B1(Null-Safety)/)

***TL;DR***
- `Kotlin` 언어를 학습합니다.
- `변수`, `함수`, `Collection`, `조건문`, `반복문`, `클래스` 를 살펴봅니다.


---

## 변수
- `Kotlin` 에서는 두 가지 방식으로 변수를 선언합니다.
  - `var` 키워드는 `mutable variable` 입니다.
  - `val` 키워드는 `immutable variable` 입니다.
```kotlin
fun main() {
  // mutable variable
  var num1: Int = 10

  // immutable variable
  val num2: Int = 10

  num1 = 20; // OK
  num2 = 30; // ERROR
}
```


## 함수
- `Kotlin` 에서 함수는 `fun` 으로 선언합니다.
```kotlin
// 두 Int 를 더한 뒤 게산 결과를 String 으로 반환하는 함수
fun plus_number(number1: Int, number2: Int): String {
  var sum = (number1 + number2).toString()
  return sum
}

// parameter 가 없는 함수
fun no_param(): Int {
  return 100
}

// return 이 없는 함수
fun no_return() {
  val a: Int = 0;
}
```


## Collection
- `Collection` 은 ***List***, ***Map*** 등 다양한 자료구조 타입을 의미합니다. (참고: [Blog](https://codechacha.com/ko/collections-in-kotlin/))

### Array
```kotlin
// 초기화된 array 를 선언합니다.
var array: Array<Int>  = arrayOf(1, 2, 3, 4, 5);

// 새로운 원소를 추가할 때는 `plus()` method 를 이용합니다.
array.plus(6)
```

### List
- `List` 는 *Mutable* 과 *Immutable* 을 모두 지원하는 `Collection` 입니다.
- 기본적으로 <u>*Immutable*</u> 이며, *Mutable* 의 경우에는 명시해주어야 합니다.
```kotlin
// 초기화된 immutable list 를 선언합니다.
var imm_list: List<Int> = listOf(10, 20, 30, 40, 50)

// immutable list 이기 때문에, ERROR 는 아니지만 값이 추가되지는 않습니다.
imm_list.plus(60)

// 초기화된 mutable list 를 선언합니다.
var mut_list: MutableList<Int> = mutableListOf(100, 200, 300, 400, 500)

// multable list 는 `add` 를 사용합니다.
mut_list.add(600)

// (+) filter 를 이용하면 Collection 내부 데이터 중 원하는 것들만 참조할 수 있습니다.
val imm_list_filt = imm_list.filter{it % 3 == 0}
val mut_list_filt = mut_list.filter{it % 3 == 0}

println("imm_list 출력: $imm_list_filt")
println("mut_list 출력: $mut_list_filt")
```

## 조건문
- ***If-else***
```kotlin
fun main() {
  var grade: Array<String>
  grade = arrayOf("A+", "A", "B+", "B", "C+", "C", "D+", "D", "F")

  var ranking: Int = 1
  var idx: Int = 8

  if (ranking <= 5) idx = 0
  else if (ranking <= 10) idx = 1
  else if (ranking <= 15) idx = 2
  else if (ranking <= 20) idx = 3
  else if (ranking <= 25) idx = 4
  else if (ranking <= 30) idx = 5
  else if (ranking <= 35) idx = 6
  else if (ranking <= 40) idx = 7
  else if (ranking <= 45) idx = 8

  println!("Grade: " + grade[idx])
}
```
- ***When***
```kotlin
fun main() {
  var grade: Array<String>
  grade = arrayOf("A+", "A", "B+", "B", "C+", "C", "D+", "D", "F")

  var ranking: Int = 1
  var idx: Int = 8

  // 범위를 이용해서도 가능
  when(ranking) {
    in 0..5 -> idx = 0
    in 6..10 -> idx = 1
    in 11..15 -> idx = 2
    in 16..20 -> idx = 3
    in 21..25 -> idx = 4
    in 26..30 -> idx = 5
    in 31..35 -> idx = 6
    in 36..40 -> idx = 7
    else -> idx = 8
  }

  println!("Grade: " + grade[idx])
}
```

## 반복문
- ***for***
```kotlin
var numbers: Array<Int> = arrayOf(1, 2, 3, 4, 5)

for (i in numbers) print(i + " ")
```

- ***while***
```kotlin
var i: Int = 10

while (i > 0) i--
```

## 클래스
```kotlin
public class Novel constructor(val name: String, page: Int) {
  // 'name' 변수는 parameter 에서 내부 변수로 선언함

  // 내부 변수
  var page = page
  
  fun print_info() {
    println("name: $name")
    println("page: $page")
  }
}

fun main() {
  // 객체 생성
  var harry_potter = Novel("HarryPotter", 268)

  harry_potter.print_info()
}
```

- *constructor* 에 관한 보다 자세한 설명은 [블로그, velog](https://velog.io/@conatuseus/Kotlin-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%BF%8C%EC%8B%9C%EA%B8%B0) 에 잘 정리되어 있습니다.

### 상속
- 부모 클래스를 물려받아 기능을 수정, 추가하는 개념입니다.
- `Kotlin` 에서는 `open` 키워드를 명시적으로 적어줘야지만 `상속`이 가능한 클래스 및 함수로 사용할 수 있습니다.
  - `Kotlin` 에서의 *Keyword* 에 관해서는 [블로그](https://velog.io/@conatuseus/Kotlin-%ED%82%A4%EC%9B%8C%EB%93%9C-%EC%A0%95%EB%A6%AC-open-internal-companion-data-class-%EC%9E%91%EC%84%B1%EC%A4%91)를 참고해보면 좋습니다.
- 즉, `open class`는 <u>부모 클래스가 될 수 있는 클래스</u> 입니다.
```kotlin
open class Book(val author: String) {
  open fun print_author() {
    println("author: $author")
  }
}

public class Novel constructor
  (val name: String, page: Int, author: String): Book(author) {
  var page = page
  
  fun print_info() {
    println("name: $name")
    println("page: $page")
  }
}

fun main() {
  // 객체 생성
  var harry_potter = Novel("HarryPotter", 268, "J.K.Rowling")

  harry_potter.print_info()
  harry_potter.print_author()
}
```

---

# Exercise
## 1) 클래스 + 배열 연습
- 여러 음악들 중 Hiphop 음악만을 찾는 예제입니다.
```kotlin
enum class Genre { Hiphop, Rock, Pop, RnB }

class Music(val title: String, val artist: String, val genre: Genre) {
    fun print_info() {
        println("title: $title, artist: $artist, genre: $genre")
    }
}

fun main() {
    var playlist: MutableList<Music> = mutableListOf()

    playlist.add(Music("hello", "Jimmy", Genre.Hiphop))
    playlist.add(Music("Bye Bye", "Kate", Genre.Rock))
    playlist.add(Music("See You", "Luke", Genre.Pop))
    playlist.add(Music("Welcome", "Haley", Genre.RnB))
    playlist.add(Music("Wassup", "Matt", Genre.Hiphop))

    // filter 를 이용해 "Hiphop" 음악을 찾음
    var hiphop_playlist = playlist.filter { it.genre == Genre.Hiphop }

    // for, in 을 이용
    for (music in hiphop_playlist) {
        music.print_info()
    }
}
```
- ***결과***
```
title: hello, artist: Jimmy, genre: Hiphop
title: Wassup, artist: Matt, genre: Hiphop
```

## 2) 클래스 + 2차원 배열  연습
- 행렬을 표현하는 문자열을 받아서 행렬 클래스로 변환하는 프로그램을 작성합니다.

```kotlin
class Matrix(private val matrixAsString: String) {
    val datas: List<List<Int>>

    init {
        datas = matrixAsString
            // 공백이 하나 이상일 때, 공백 하나로 바꿉니다.
            .replace("[ ]+".toRegex(), " ")
            // '\n' 을 기준으로 split 합니다.
            // [1 2 3, 4 5 6, 7 8 9] 가 됩니다.
            .split("\n")
            .map {
                // 1 2 3 을 [1, 2, 3] 으로 바꿉니다.
                it.trim().split(" ").map {
                    // 문자인 숫자를 int 로 변환합니다.
                    it.toInt() }
            }
    }

    fun col(colIdx: Int): List<Int> {
        return datas.map { it[colIdx - 1] }
    }
    fun row(rowIdx: Int): List<Int> {
        return datas.get(rowIdx - 1)
    }
}

fun main() {
    val matrixString = "1 2 3\n4 5 6\n7 8 9"
    println("matrix: ")
    println(matrixString)

    val matrix = Matrix(matrixString)
    println("matrix class:")
    println("row 2: ${matrix.row(2)}")
    println("col 1: ${matrix.col(1)}")
}
```

- ***결과***
```
row 2: [4, 5, 6]
col 1: [1, 4, 7]
```
