---
layout: post
read_time: true
show_date: true
title: Destructuring & Component함수
img: ":title.png"
date: "2024-05-18 18:23:20"
description: 
tags:
  - Kotlin
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론

데이터 클래스의 특성 중 하나인 convention 원리와 관련된 특성인 구조 분해 선언에 대해 알아보자.  

## 구조 분해

구조 분해를 사용하면 객체가 가지고 있는 여러 값을 분해해서 여러 변수를 한꺼번에 초기화 할 수 있다.  

```
data class Car(
  private val model : String,
  private val year : Int
)
--
val car = Car("그랜저",11)

val (model, year) = car

println(model)
>> 그랜저
println(year)
>> 11
```

위와 같이 여러 변수를 괄호로 묶어서 변수를 선언하여 객체의 값을 가져오는 것이 구조분해라고 한다.  
이 구조 분해 선언 내부에서는 각 변수를 초기화하기 위해 componentN이라는 함수를 호출하게 되며, 여기서 N은 구조 분해 선언에 있는 변수 위치에 따라 붙는 번호다.  

```
val (a,b) = data
--
val a = data.component1()
val b = data.component2()
```

위와 같은 과정으로 component 함수가 작동한다.  

이러한 구조 분해 선언은 함수의 리턴 값이 여러개일때 유용하다.  
그렇다면 사용하지 않는 값을 처리할때는 어떻게 해야할까?  

```
data class Car(
  private val model : String,
  private val cc : Int,
  private val year : Int
)
--
val car = Car("그랜저", 3000, 11)
val (model, _, year) = car
```

위와 같이 사용하지 않는 값에 대해서는 _(언더바) 로 처리할 수 있다.  
