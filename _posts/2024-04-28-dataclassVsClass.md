---
layout: post
read_time: true
show_date: true
title: Entity, Data class vs class
img: ":title.png"
date: "2024-04-28 18:23:20"
description: 
tags:
  - Kotlin
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론
코틀린 JPA에서 Entity를 만들때 Data class와 일반 class 중 어느것을 고르는 것이 맞을까?  
데이터를 담는 객체이니 Data class를 골라야할까, 데이터를 담는 것 이상의 역할을 하니 일반 class를 골라야할까?  
이 두가지를 비교해보고 무엇이 더 알맞은 방법인지 알아보자.  

## Data class

Data class를 통해 Entity를 만들경우 특징에 대해서 알아보자.  

우선, Data class는 불변 객체를 정의하기 위한 클래스로 여러가지 메소드를 자동으로 생성해준다.  
하지만 Entity는 변경 가능한 객체이므로, 이러한 메서드를 사용했을 경우 문제가 발생할 가능성이 있다.  
또한, equals(), hashCode() 등의 메소드는 Entity에게 불필요하므로 쓸데없는 로직을 가지게 된다.  
그 외에도 toString() 메소드의 경우 순환 참조를 발생시킬 가능성이 있고, Data class는 final 이므로 지연 로딩 기능을 사용할 수 없다.  

## class

일반 class의 경우 필요에 따라 메서드를 재정의할 수 있다.  
위에서 말한 문제가 발생할 가능성도 없기때문에 일반적으로 JPA의 Entity는 class를 통해 사용하는 것이 권장된다.  
