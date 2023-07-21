---
layout: post
read_time: true
show_date: true
title: ControllerAdvice란?
img: ":Spring.png"
date: "2023-07-03 18:23:20"
description: 
tags:
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

Spring으로 웹 애플리케이션을 구축할 때 여러 다른 컨트롤러에서 일관되게 예외를 처리하거나 모델에 공통 속성을 추가해야하는 상황이 생긴다.  
이를 쉽게 구현하는 방법이 ControllerAdvice를 이용하는 것이다.  
오늘은 ControllerAdvice가 무엇인지 알아보고 어떻게 작동하는지를 알아보자.  

## ControllerAdivce란?

Spring MVC 프레임워크에서 제공하는 주석으로, 애플리케이션 전체에서 예외를 처리해준다.  
'@RequestMapping' 주석이 달린 메소드에서 발생하는 예외를 처리하는 인터셉터로 생각하면 쉽다.  
또한 예외처리뿐만이 아니라 모든 컨트롤러에서 모델에 데이터를 추가하거나, 요청 및 응답 객체를 수정하는 등의 공통 동작을 사용자 정의하는 데도 사용할 수 있다.  

## 예외처리 방법

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = Exception.class)
    public ResponseEntity<Object> handleGenericException(Exception ex, WebRequest request) {
        Map<String, Object> body = new HashMap<>();
        body.put("timestamp", new Date());
        body.put("message", ex.getMessage());

        return new ResponseEntity<>(body, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

위의 코드를 보고 설명해주겠다.  

'@ControllerAdivce' 주석은 이 클래스를 애플리케이션의 모든 컨트롤러에서 사용 가능할 수 있도록 한다.  

'@ExceptionHandler' 주석은 메소드가 처리할 예외의 유형을 지정하는데 사용된다.  
위의 코드의 경우 모든 유형의 예외를 처리한다.  

'handleGenericException' 메소드는 애플리케이션 내에서 처리되지 않은 예외가 발생할 때마다 호출된다.  
이 메소드는 예외의 세부 사항이 포함된 맵을 생성하고 '500 Internal server error' 를 응답 본문에 반환한다.  

## 공통 속성 추가

'@ControllerAdvice'를 사용하여 애플리케이션의 모든 컨트롤러에 대한 모든 모델에 공통 속성을 추가할 수도 있다.  
이는 '@ModelAttribute' 주석을 사용하여 구현할 수 있다.  

```java
@ControllerAdvice
public class GlobalAdvice {

    @ModelAttribute("commonAttribute")
    public String commonAttribute() {
        return "Common Attribute Value";
    }
}
```

위의 코드에서는 'commonAttribute'라는 공통 속성을 'Common Attribute Value'와 함께 애플리케이션의 모든 컨트롤러에 대한 모든 모델에 추가하였다.  

## 마치며

ControllerAdvice는 Spring 애플리케이션에서 컨트롤러의 예외처리, 모델의 공통 속성 추가 등이 필요한 경우 중앙 집중화되고 일관된 해결법을 제시한다.  
이를 잘 활용하면 구조적으로도 유지보수적인 측면에서도 더 좋은 애플리케이션을 만드는데 도움이 될 것 이다.  