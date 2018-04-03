---
type: tutorial
layout: tutorial
title:  "Targeting Multiple Platforms"
description: "Compiling with Kotlin/Native for multiple platforms"
authors: Eugene Petrenko
date: 2018-03-30
showAuthorInfo: false
## /*EVAN-5121*/
---

Kotlin/Native support compiling code for different platforms including
Windows (x86_64),
Linux (x86_64, arm32, MIPS, MIPS little endian),
MacOS (x86_64),
iOS (arm64),
Android (arm32, arm64),
WebAssembly (wasm32).
In this tutorial, we'll see how to

* [Specify a Target Platform](#specifying-target-platform)
* [Building for a Specific Platform](#building-for-a-specific-platform)
* [Build a Console Utility](#building-a-console-utility)

To start with you need to have Kotlin/Native compiler on your machine. Check out to the 
[A Basic Kotlin/Native Application](basic-kotlin-native-app.md) tutorial for the instructions.
We assume that you have `konanc` command available from the system PATH environment variable.

## Specifying Target Platform

The list of supported target platforms of `konanc` depends 
on the operating system. You may list them via `konanc -list_targets`
console command. For example, on macOS (OS X) with Kotlin/Native v0.6.2:
```
> konanc -list_targets
macbook:                      (default)
iphone:
iphone_sim:
android_arm32:
android_arm64:
wasm32:
zephyr_stm32f4_disco:
```

The default target is highlighted with `(default)` and used if no `-target` argument was 
specified.

## Building for a Specific Platform

Let's create a sample Kotlin program and save it as `main.kt`
```kotlin
fun main(args: Array<String>) {
  println("Hello Kotlin/Native!")
}
```

We use `-target` argument of the `konanc` to specify the platform. It is also 
helpful to use `-output` to clearly 
the compiler on where to create the compiled binary, e.g.

    konanc -target iphone -output bin/iphone main.kt

## Building a Console Utility

To create a command line utility for all 
operation system, namely Windows, Linux and macOS we 
need to call Kotlin/Native compiler on every of OS:

    konanc main.kt

There is no need to specify `-target` parameter explicitly because 
the default value will work the best.  

[Gradle build system](gradle-for-kotlin-native.md) helps to simplify
the setup for every operating system. It helps to download and run 
Kotlin/Native compiler on a build machine