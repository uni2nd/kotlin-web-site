---
type: doc
layout: reference
category: "Basics"
title: "기본 문법"
---

# 기본 문법

## 패키지 선언

패키지 선언은 소스 파일의 최상단에 하셔야 합니다:

``` kotlin
package my.demo

import java.util.*

// ...
```

디렉토리와 패키지 구조를 굳이 맞추지 않아도 됩니다: 소스 파일은 파일 시스템의 어디에나 있을 수 있습니다.

[패키지](packages.html) 문서를 참고하세요.

## 함수(functions) 선언

두 개의 `Int` 파라미터와 `Int` 반환 타입을 가진 함수:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun sum(a: Int, b: Int): Int {
    return a + b
}
//sampleEnd

fun main(args: Array<String>) {
    print("sum of 3 and 5 is ")
    println(sum(3, 5))
}
```
</div>

식 본문과 유추 반환 타입을 가진 함수:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun sum(a: Int, b: Int) = a + b
//sampleEnd

fun main(args: Array<String>) {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}
```
</div>

반환값이 (의미)없는 함수:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
//sampleEnd

fun main(args: Array<String>) {
    printSum(-1, 8)
}
```
</div>

`Unit` 반환 타입은 명시하지 않아도 됩니다:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
//sampleEnd

fun main(args: Array<String>) {
    printSum(-1, 8)
}
```
</div>

[함수](functions.html) 문서를 참고하세요.

## 변수 선언

불변(읽기 전용) 지역 변수:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val a: Int = 1  // immediate assignment
    val b = 2   // `Int` type is inferred
    val c: Int  // Type required when no initializer is provided
    c = 3       // deferred assignment
//sampleEnd
    println("a = $a, b = $b, c = $c")
}
```
</div>

가변 변수:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    var x = 5 // `Int` type is inferred
    x += 1
//sampleEnd
    println("x = $x")
}
```
</div>

최상위레벨 변수:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
val PI = 3.14
var x = 0

fun incrementX() { 
    x += 1 
}
//sampleEnd

fun main(args: Array<String>) {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}
```
</div>

[프로퍼티와 필드](properties.html) 문서를 참고하세요.


## 주석

코틀린도 자바나 자바스크립트처럼 1줄 주석, 블럭 주석을 지원합니다.

``` kotlin
// This is an end-of-line comment

/* This is a block comment
   on multiple lines. */
```

코틀린의 블럭 주석은 자바와는 달리 중첩해서 쓸 수 있습니다.

문서화를 위한 주석 문법 내용은 [코틀린 코드 문서화하기](kotlin-doc.html) 문서를 참고하세요.

## 문자열 템플릿 사용하기

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    var a = 1
    // simple name in template:
    val s1 = "a is $a" 
    
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
//sampleEnd
    println(s2)
}
```
</div>

[문자열 템플릿](basic-types.html#string-templates) 문서를 참고하세요.

## 조건식 사용하기

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
//sampleEnd

fun main(args: Array<String>) {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
</div>


*if*{: .keyword } 를 식처럼 사용:

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun maxOf(a: Int, b: Int) = if (a > b) a else b
//sampleEnd

fun main(args: Array<String>) {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
</div>

[*if*{: .keyword }-expressions](control-flow.html#if-expression) 문서를 참고하세요.

## 널(null)이 가능한 변수 사용 & *널*{: .keyword } 확인법

A reference must be explicitly marked as nullable when *null*{: .keyword } value is possible.

Return *null*{: .keyword } if `str` does not hold an integer:

``` kotlin
fun parseInt(str: String): Int? {
    // ...
}
```

Use a function returning nullable value:


<div class="sample" markdown="1" data-min-compiler-version="1.1">

``` kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

//sampleStart
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("either '$arg1' or '$arg2' is not a number")
    }    
}
//sampleEnd


fun main(args: Array<String>) {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}
```
</div>

or


<div class="sample" markdown="1" data-min-compiler-version="1.1">

``` kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)
    
//sampleStart
    // ...
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }

    // x and y are automatically cast to non-nullable after null check
    println(x * y)
//sampleEnd
}

fun main(args: Array<String>) {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("99", "b")
}
```
</div>

See [Null-safety](null-safety.html).

## Using type checks and automatic casts

The *is*{: .keyword } operator checks if an expression is an instance of a type.
If an immutable local variable or property is checked for a specific type, there's no need to cast it explicitly:


<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}
//sampleEnd


fun main(args: Array<String>) {
    fun printLength(obj: Any) {
        println("'$obj' string length is ${getStringLength(obj) ?: "... err, not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
</div>

or

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}
//sampleEnd


fun main(args: Array<String>) {
    fun printLength(obj: Any) {
        println("'$obj' string length is ${getStringLength(obj) ?: "... err, not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
</div>

or even

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&`
    if (obj is String && obj.length > 0) {
        return obj.length
    }

    return null
}
//sampleEnd


fun main(args: Array<String>) {
    fun printLength(obj: Any) {
        println("'$obj' string length is ${getStringLength(obj) ?: "... err, is empty or not a string at all"} ")
    }
    printLength("Incomprehensibilities")
    printLength("")
    printLength(1000)
}
```
</div>

See [Classes](classes.html) and [Type casts](typecasts.html).

## Using a `for` loop

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
//sampleEnd
}
```
</div>

or

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
//sampleEnd
}
```
</div>


See [for loop](control-flow.html#for-loops).

## Using a `while` loop

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }
//sampleEnd
}
```
</div>


See [while loop](control-flow.html#while-loops).

## Using `when` expression

<div class="sample" markdown="1">

``` kotlin
//sampleStart
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
//sampleEnd

fun main(args: Array<String>) {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```
</div>


See [when expression](control-flow.html#when-expression).

## Using ranges

Check if a number is within a range using *in*{: .keyword } operator:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }
//sampleEnd
}
```
</div>


Check if a number is out of range:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val list = listOf("a", "b", "c")
    
    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range too")
    }
//sampleEnd
}
```
</div>


Iterating over a range:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    for (x in 1..5) {
        print(x)
    }
//sampleEnd
}
```
</div>

or over a progression:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    for (x in 1..10 step 2) {
        print(x)
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x)
    }
//sampleEnd
}
```
</div>

See [Ranges](ranges.html).

## Using collections

Iterating over a collection:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
    val items = listOf("apple", "banana", "kiwifruit")
//sampleStart
    for (item in items) {
        println(item)
    }
//sampleEnd
}
```
</div>


Checking if a collection contains an object using *in*{: .keyword } operator:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
    val items = setOf("apple", "banana", "kiwifruit")
//sampleStart
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
//sampleEnd
}
```
</div>


Using lambda expressions to filter and map collections:


<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
    val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
//sampleStart
    fruits
        .filter { it.startsWith("a") }
        .sortedBy { it }
        .map { it.toUpperCase() }
        .forEach { println(it) }
//sampleEnd
}
```
</div>

See [Higher-order functions and Lambdas](lambdas.html).

## Creating basic classes and their instances:

<div class="sample" markdown="1">

``` kotlin
fun main(args: Array<String>) {
//sampleStart
    val rectangle = Rectangle(5.0, 2.0) //no 'new' keyword required
    val triangle = Triangle(3.0, 4.0, 5.0)
//sampleEnd
    println("Area of rectangle is ${rectangle.calculateArea()}, its perimeter is ${rectangle.perimeter}")
    println("Area of triangle is ${triangle.calculateArea()}, its perimeter is ${triangle.perimeter}")
}

abstract class Shape(val sides: List<Double>) {
    val perimeter: Double get() = sides.sum()
    abstract fun calculateArea(): Double
}

interface RectangleProperties {
    val isSquare: Boolean
}

class Rectangle(
    var height: Double,
    var length: Double
) : Shape(listOf(height, length, height, length)), RectangleProperties {
    override val isSquare: Boolean get() = length == height
    override fun calculateArea(): Double = height * length
}

class Triangle(
    var sideA: Double,
    var sideB: Double,
    var sideC: Double
) : Shape(listOf(sideA, sideB, sideC)) {
    override fun calculateArea(): Double {
        val s = perimeter / 2
        return Math.sqrt(s * (s - sideA) * (s - sideB) * (s - sideC))
    }
}
```
</div>

See [classes](classes.html) and [objects and instances](object-declarations.html).
