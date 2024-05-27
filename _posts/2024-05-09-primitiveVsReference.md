---
layout: post
read_time: true
show_date: true
title: Primitive vs Reference
img: ":title.png"
date: "2024-05-09 18:23:20"
description: 
tags:
  - Kotlin
  - Java
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론

Validation에 대해서 스터디하던 중 @NotNull 어노테이션에 대해서 알아봤다.  
NotNull 어노테이션에서 Primitive 타입은 기본값이 존재하기때문에 validation이 작동하지 않는다는 이슈가 있었다.  
그래서 오늘은 Primitive타입은 무엇인지, 그리고 nullable인 Reference 타입에 대해서 알아보고, 코틀린에서는 이 타입들이 어떻게 쓰이는지 알아보자.  

## Primitive Type

원시 타입이라고도 부르는 Primitive 타입은 정수, 실수, 문자 등 실제 값을 저장하는 타입이다.  
Reference 타입과의 차이는 Reference타입은 메모리 주소 값을 저장해서 그 메모리에서 값을 가져오는 것이고, 원시타입은 실제로 값을 저장하는 타입이다.  

int, long, double, float, boolean, byte, short, char 총 8가지의 Primitive 타입이 Java에서는 존재한다.  
Kotlin은 Null을 허용하는 언어라고 했는데, 그렇기때문에 이 원시타입을 기본타입으로 제공하지 않는다.  
이 부분에 대해서는 뒤에서 참조타입에 대해 설명할때 추가적으로 얘기하겠다.  

## Reference Type

참조 타입이라고 부르는 Reference Type은 위에서 말한 원시타입의 8가지를 제외한 타입들을 말한다.  
참조 타입은 자바의 최상위 클래스인 java.lang.Object 클래스를 상속하는 모든 클래스를 말한다.  

Java에서 실제 객체는 힙 메모리에 저장되며, 참조 타입 변수는 스택 메모리에 실제 객체들의 주소를 저장하여, 객체를 사용할때 마다 객체의 주소를 불러와 사용하는 방식이다.  

원시 타입과의 가장 큰 차이라고 한다면 Null을 담을 수 있다는 것이다.  
참조타입은 null을 입력값으로 받을 수 있는 반면, 원시타입은 null을 담을 수 없고, 기본값이 존재한다.  
그리고 List<T> 와 같은 제네릭 타입도 사용할 수 없다.  
성능적으로는 당연히 메모리 주소값을 가지는 참조타입보다 원시타입이 유리하다.  

## Kotlin?

그렇다면 코틀린에서는 이 두가지 타입에 대해서 어떻게 처리할까?  

코틀린에서는 이 원시타입을 boxed Type으로 사용한다.  
원시타입을 객체에 감싸서 사용하는 것이다.  

그렇다면 참조타입과 유사하지만 한가지 다른점은 Null 값을 담기 위해서는 Nullable을 표시해줘야한다.  

```
val id: Long = null // 에러
val nullId = Long? = null
```

위의 코드와 같이 nullable을 사용해야 null값이 담기게 되고 처음에 말한 @NotNull 에 대한 validation 또한 작동하는 것이다.  