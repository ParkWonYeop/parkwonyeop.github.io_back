---
layout: post
read_time: true
show_date: true
title: Optional vs Nullable
img: ":title.png"
date: "2024-05-04 18:23:20"
description: 
tags:
  - Kotlin
  - Java
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론

Null을 처리하는 방법에는 여러가지가 있지만 Kotlin에서는 Nullable을 사용하고, 자바에서는 Optional을 사용한다.  
이 두가지의 차이에 대해서 알아보자.  

## Nullable

Nullable은 코틀린 언어 자체의 기능으로, 이 변수가 null이 가능한지 불가능한지를 설정한다.  

```
var nullableValue: String?
```

이런식으로 타입 옆에 ?를 붙여서 선언이 가능하다.  

Nullable의 특징 중 하나는 null일 경우 값이나, 실행할 로직을 정할 수 있다.  

```
val user: User? = userRespository.findByName(name)
  ?: throw RuntimeException() // 혹은 null 일 경우의 값
```

Nullable은 언어 자체의 기능이기때문에 런타임시 오버헤드가 발생할 가능성이 적다.  
또한 안전하지 않은 방법을 사용하면 컴파일시에 에러를 발생시킨다.  

## Optional

자바에서의 Optional은 null 처리를 위한 래퍼 클래스이다.  
자바 자체 기능이 아니라 래퍼 클래스라는 것이 차이인데, 이는 런타임 시에 추가적인 오버헤드가 발생할 수 있다.  
안전하지 않은 방법을 사용하더라도 컴파일시에 오류를 검사하지 않고, 런타임 시에 예외가 발생할 수 있다.  

```Java
Optional<User> optionalUser = userRepository.findByName(name);

User user = optionalUser.get() // 안전하지 않다는 경고 발생

if(optionalUser.isEmpty) {
  throw new RuntimeException()
}

User user2 = optionalUser.get() // 경고 없음
```

위와 같이 사용할 수 있다.  
한가지 차이에 대해서 더 얘기하자면 Nullable은 값 혹은 Null 둘 중 하나를 받는다.  
Optional은 Optional<T> 를 무조건 받는 것이 큰 차이 중 하나이다.  
