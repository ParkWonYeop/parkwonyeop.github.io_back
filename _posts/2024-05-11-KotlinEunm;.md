---
layout: post
read_time: true
show_date: true
title: Kotlin Enum ;
img: ":title.png"
date: "2024-05-11 18:23:20"
description: 
tags:
  - Kotlin
author: ParkWonyeop
published: true
category: Kotlin
---
## ENUM ;?

코틀린에서는 보통 ;(세미콜론)을 사용하지 않는다.  
하지만 거의 유일하게 세미콜론을 사용하는 곳이 있는데 ENUM이다.  
그 이유는 상수 목록과 메서드 정의를 구분하기 위해서 이다.  

```Kotlin
enum class Color(val r: Int, val g: Int, val b: Int) { // 상수의 프로퍼티를 정의
    RED(255, 0, 0), ORANGE(255, 165, 0), // 각 상수를 생성할 때 그에 대한 프로퍼티 값을 지정
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238); // 반드시 세미콜론을 사용
    
    fun rgb() = (r * 256 + g) * 256 + b  // enum 클래스 안에서 메서드를 정의
}
```