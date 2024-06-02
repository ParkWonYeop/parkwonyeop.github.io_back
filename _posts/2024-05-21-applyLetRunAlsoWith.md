---
layout: post
read_time: true
show_date: true
title: apply, let, run, also, with
img: ":title.png"
date: "2024-05-21 18:23:20"
description: 
tags:
  - Kotlin
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론

코틀린에서 apply, let, run, also, with는 범위 지정 함수(Scope function)으로 불리며, 특정 객체에 대한 작업을 블록 안에 넣어 실행할 수 있도록 하는 함수이다.  
오늘은 이 함수들의 차이에 대해서 알아보자.  

## apply

apply는 수신 객체의 프로퍼티를 변경한 후, 수신 객체 자체를 반환하는 함수이다.  
객체 생성 시에 다양한 프로퍼티를 설정해야 하는 경우에 자주 사용된다.  

```Kotlin
val person = Person().apply {
  name = "wonyeop"
  age = 26
}
```

## run

apply와 같이 동작하지만, 객체를 반환하는대신 마지막 라인의 결과를 반환한다.  

```Kotlin
val person = Person().apply {
  name = "wonyeop"
  age = 26
}

val result = person.run {
  age++
}

println(result) // 27
```

## with

run과 거의 같게 동작하지만, run은 확장 함수로 사용되고, with은 수신 객체를 파라미터로 받아서 사용한다.  

```Kotlin
val person = Person().apply {
  name = "wonyeop"
  age = 26
}

val result = with(person) {
  age++
}

println(result) // 27
```

## let

let은 객체의 확장함수이며, 객체를 it으로 사용할 수 있고, 반환값은 람다식의 결과이다.  

```kotlin
val person = Person().apply {
  name = "wonyeop"
  age = 26
}

val result = person.let {
  it.age = 24
  "{age is $age}"
}

println(result) // age is 24
```

## also

also는 객체의 확장함수이며, 객체를 it으로 사용할 수 있고, 반환값은 객체 자체이다.  
also는 객체와 관련된 동작을 수행하는데 사용된다.  

```kotlin
val person = Person().apply {
  name = "wonyeop"
  age = 26
}

person.also{it.age++}

println(person.age) // 27
```