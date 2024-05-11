---
layout: post
read_time: true
show_date: true
title: mutable, immutable
img: ":title.png"
date: "2024-05-01 18:23:20"
description: 
tags:
  - Kotlin
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론
값이 변하지 않는 것을 불변성(Immutability)이라고하고, 값이 변하는 것을 가변성(Mutability)이라고 한다.  
코틀린의 var는 가변성이고, val는 불변성이다.  

## mutable의 문제성

코틀린을 포함한 다수의 언어에서는 불변성을 권장한다.  
그 이유가 무엇일까?  
흔히들 생각하기로는 값을 유동적으로 바꿀 수 있다면 활용도가 늘어날 것도 같은데 왜 가변성을 지양해야하는 것일까.  

그 이유는 값을 메모리에 저장하기 때문이다.  
메모리에 저장한 값을 변경하는 행위는 예상하지 못한 문제를 발생시킬 수 있다.  
그러한 문제를 해결하기 위해서 여러가지 추가적인 처리를 해줘야한다.  
멀티스레드에서도 문제가 발생할 가능성이 생기고, 테스트와 디버깅이 어렵다는 문제도 있다.  

## Immutable의 중요성

불변성을 지닌 값은 선언한 이후로 값을 변경할 수 없다.  
그렇기에 생기는 불편함도 있겠지만, 위에서 말한 것처럼 메모리의 안정성에 중요한 역할을 한다.  

스레드 간의 안정성이 보장되고, 한번 생성된 값은 변하지 않으므로 캐시도 수월하다.  
값이 변하였을때의 처리를 따로 해주지 않아도 된다는 장점도 있다.  

## Kotlin에서의 가변성

코틀린에서는 가변성을 어떻게 제한하고 있을까?  
크게 세가지로 나뉜다.  

1. 읽기 전용 프로퍼티 val  
2. Mutable 컬렌션과 read-only 컬렉션 구분  
3. data class의 copy()  

이중에서 Mutable List와 ReadOnly 리스트에 대한 예만 써보겠다.  

```Kotlin
val readOnlyList = listOF("a","b") // readOnly List
val arrMutable = mutableListOf() // 수정가능한 List

arrMutalbe.add("c")
readOnlyList.add("c") // 컴파일 에러
```

위 코드처럼 listOf로 선언한 리스트는 값을 변경할 수 없고, mutableListOf로 선언한 리스트는 내부 값을 이후에도 수정할 수 있다.  