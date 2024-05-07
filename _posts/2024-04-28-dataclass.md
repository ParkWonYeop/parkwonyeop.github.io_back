---
layout: post
read_time: true
show_date: true
title: 데이터 클래스
img: ":title.png"
date: "2024-04-28 18:23:20"
description: 
tags:
  - Kotlin
  - DataClass
author: ParkWonyeop
published: true
category: Kotlin
---
## 서론

자바에서 데이터를 전달하는 객체를 만들때 record를 사용했던 것처럼 코틀린에도 데이터 클래스라는 것이 존재한다.  
오늘은 데이터 클래스에 대해서 알아보자.  

## 데이터 클래스

일반 클래스와 달리 getter, setter, toString 등의 메소드를 자동으로 생성해주는 클래스이다.  
요즘도 자바에서는 record나 lombok의 Data 어노테이션으로 대체가 가능하지만 외부 라이브러리없이 기본 기능으로 제공해준다는 것이 중요하다.  

```Java
class People {
    String name;

    @Override
    public String toString(){
        return "[People] name : " + name + ", age : " + Integer.toString(age);
    }

    public String getName(){
        return name;
    }

    public void setName(String name){
        this.name = name;
    }
}
```

위는 자바에서 데이터를 담는 클래스를 만드는 코드이다.  

```Kotlin
data class People(
    val name: String
)
```

위는 데이터 클래스를 이용해서 만들었다.  
중요한건 위와 아래모두 같은 동작을 한다는 것이다.  
코드를 추가적으로 써줄 필요가 없기때문에 개발에 있어 많은 이득을 가져온다.  

## Record 차이점

데이터클래스는 자바의 record와 비슷한 역할을 하는데 이 둘에도 차이점이 존재한다.  
  
1. copy() 메소드는 데이터 클래스만 제공한다.  
2. 자바의 record는 모든 필드가 private final 인데 반면, 데이터 클래스는 선언 방식에 따라 필드값이 변경 가능하다.  
3. 자바는 Record Class 라는 클래스를 상속받는 방식인데, 코틀린은 데이터 클래스라는 것 자체가 존재한다.  
4. 데이터 클래스는 인스턴스 변수를 만들 수 있지만, record는 인스턴스 변수를 허용하지 않는다.  
