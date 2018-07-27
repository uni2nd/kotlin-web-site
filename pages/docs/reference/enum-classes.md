---
type: doc
layout: reference
category: "Syntax"
title: "Enum Classes"
---

# 열거형 클래스 Enum Classes

열거형 클래스의 일반적인 사용법은, 타입에 안전한 열거를 사용하고자 할 때 입니다:

The most basic usage of enum classes is implementing type-safe enums:

``` kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```

각 열거 상수는 오브젝트입니다. 열거 상수는 쉼표로 구분합니다.

Each enum constant is an object. Enum constants are separated with commas.

## 초기화 Initialization

각 열거는 열거형 클래스의 인스턴스이기 때문에 다음처럼 초기화할 수 있습니다:

Since each enum is an instance of the enum class, they can be initialized as:

``` kotlin
enum class Color(val rgb: Int) {
        RED(0xFF0000),
        GREEN(0x00FF00),
        BLUE(0x0000FF)
}
```

## 익명 클래스 Anonymous Classes

열거 상수는 각자의 익명 클래스를 선언할 수도 있습니다:

Enum constants can also declare their own anonymous classes:

``` kotlin
enum class ProtocolState {
    WAITING {
        override fun signal() = TALKING
    },

    TALKING {
        override fun signal() = WAITING
    };

    abstract fun signal(): ProtocolState
}
```

그들 각자는 기본 메소드를 오버라이드하는 메소드를 갖습니다. 열거형 클래스에 멤버를 하나라도 정의했다면, 열거 상수와 그들 멤버간은 자바처럼 세미콜론으로 구분해야 한다는 사실을 명심하시기 바랍니다.

with their corresponding methods, as well as overriding base methods. Note that if the enum class defines any
members, you need to separate the enum constant definitions from the member definitions with a semicolon, just like
in Java.

열거값은 이너 클래스와 달리 중첩된 타입을 포함할 수 없습니다 (코틀린 1.2 에서 사용중지되었음).

Enum entries cannot contain nested types other than inner classes (deprecated in Kotlin 1.2).

## 열거형 클래스에서 인터페이스 구현하는 법 Implementing Interfaces in Enum Classes

열거형 클래스는 인터페이스를 구현할 수도 있는데 (클래스는 제외),. 열거형 클래스의 선언부에 아래처럼 인터페이스를 추가하면 됩니다.

An enum class may implement an interface (but not derive from a class), providing either a single interface members implementation for all of the entries, or separate ones for each entry within its anonymous class. This is done by adding the interfaces to the enum class declaration as follows:

<div class="sample" markdown="1">

``` kotlin
import java.util.function.BinaryOperator
import java.util.function.IntBinaryOperator

//sampleStart
enum class IntArithmetics : BinaryOperator<Int>, IntBinaryOperator {
    PLUS {
        override fun apply(t: Int, u: Int): Int = t + u
    },
    TIMES {
        override fun apply(t: Int, u: Int): Int = t * u
    };
    
    override fun applyAsInt(t: Int, u: Int) = apply(t, u)
}
//sampleEnd

fun main(args: Array<String>) {
    val a = 13
    val b = 31
    for (f in IntArithmetics.values()) {
        println("$f($a, $b) = ${f.apply(a, b)}")
    }
}
```
</div>

## 열거 상수 사용하기 Working with Enum Constants

자바처럼, 코틀린의 열거형 클래스도 합성 메소드를 이용하여 선언한 열거 상수를 리스트 형식으로 받거나, 이름으로 열거 상수를 얻는 것이 가능합니다. 이런 메소드의 시그니처는 다음과 같습니다 (열거형 클래스의 이름을 `EnumClass` 라고 가정하자):

Just like in Java, enum classes in Kotlin have synthetic methods allowing to list
the defined enum constants and to get an enum constant by its name. The signatures
of these methods are as follows (assuming the name of the enum class is `EnumClass`):

``` kotlin
EnumClass.valueOf(value: String): EnumClass
EnumClass.values(): Array<EnumClass>
```

만약 주어진 이름에 해당하는 열거 상수가 정의되지 않은 경우, `valueOf()` 라는 메소드는 `IllegalArgumentException` 을 던집니다.

The `valueOf()` method throws an `IllegalArgumentException` if the specified name does
not match any of the enum constants defined in the class.

코틀린 1.1 부터는 `enumValues<T>()` 와 `enumValuesOf<T>()` 함수를 이용하여 열거 클래스의 상수에 일반적인 방법으로 접근할 수 있습니다.

Since Kotlin 1.1, it's possible to access the constants in an enum class in a generic way, using
the `enumValues<T>()` and `enumValueOf<T>()` functions:

``` kotlin
enum class RGB { RED, GREEN, BLUE }

inline fun <reified T : Enum<T>> printAllValues() {
    print(enumValues<T>().joinToString { it.name })
}

printAllValues<RGB>() // prints RED, GREEN, BLUE
```

모든 열거 상수는 이름과 열거형 클래스의 정의된 순서를 얻을 수 있는 속성을 갖고 있습니다:

Every enum constant has properties to obtain its name and position in the enum class declaration:

``` kotlin
val name: String
val ordinal: Int
```

또한 열거 상수는 [Comparable](/api/latest/jvm/stdlib/kotlin/-comparable/index.html) 인터페이스를 상속하며, 열거형 클래스에 정의한 순서대로 처리됩니다.

The enum constants also implement the [Comparable](/api/latest/jvm/stdlib/kotlin/-comparable/index.html) interface,
with the natural order being the order in which they are defined in the enum class.
