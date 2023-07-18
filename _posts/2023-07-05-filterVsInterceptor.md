---
layout: post
read_time: true
show_date: true
title: Filter vs Interceptor
img: ":Spring.png"
date: "2023-07-05 18:23:20"
description: 
tags:
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

웹 개발 영역에서는 Filter 및 Interceptor와 같은 개념을 알아야한다.  
두 용어 모두 개발자가 요청 및 응답을 처리하고 활용하는 기술이다.  
오늘은 Filter와 Interceptor 의 개념과 차이에 대해서 알아보자.  

## Filter

Filter는 응용프로그램에서 요청 또는 응답을 가로채는데 사용된다.  
HTTP 요청 및 응답을 처리하는 웹 애플리케이션에서 주로 사용되는 기술이다.  

Filter의 전형적으로 인증, 로깅, 압축, 암호화 등에 사용된다.  
이는 정확히 비즈니스 로직의 일부는 아니지만 애플리케이션의 기능 및 보안에 필수적인 작업들이다.  

Filter는 일반적으로 순서대로 수행되는 작업의 Filter Chain이라는 순서로 실행된다.  
각 Filter는 요청을 처리한 다음 최종 대상 (servlet 또는 웹페이지)에 도달할 때까지 체인의 다음으로 전달한다.  
반환되는 응답은 동일하거나 다른 Filter Chain을 통과할 수 있으므로 사용자에게 반환되기 전에 사후 처리가 가능하다.  

## Intercepter

Intercepter는 개발자가 개체에서 작업을 호출하기 전이나 추가 동작을 삽입할 수 있도록 하는 소프트웨어 개발에 사용되는 디자인 패턴이다.  
Spring 에서는 MVC패턴이 이에 해당된다.  

Intercepter는 주로 로깅, 보안, 트랜잭션 관리 및 캐싱과 같은 계층화된 아키텍처를 가로지르는 프로그램의 측면인 cross-cutting concerns 을 해결하는데 사용된다.  
Intercepter를 사용하면 이러한 문제를 쉽게 모듈화하고 애플리케이션 전체에 적용할 수 있다.  

## Filter vs InterCepter

Filter와 Intercepter 모두 유사한 목적을 수행하지만 차이점이 존재한다.  

### 범위

Filter는 일반적으로 HTTP 요청 및 응답 처리로 제한되므로 웹 애플리케이션 환경에 집중되어있다.  
Intercepter는 더 넓은 범위를 가지며 애플리케이션의 모든 작업을 처리하는데 사용될 수 있다.  

### 기능성

Filter는 주로 요청 및 응답의 전처리 및 후처리에 중점을 둔다.  
Intercepter는 좀 더 동적이며 메소드 실행 전후에 실행될 수 있다.  

### 호출

MVC 패턴에서 요청이 servlet 또는 Controller에 도달하기 전에 Filter가 호출된다.  
반면 Intercepter는 일반적으로 메소드 실행전 또는 메소드 실행후에 호출된다.  

### 유용성

Filter는 일반적으로 들어오는 모든 요청에 적용된다.  
Intercepter는 보다 더 세분화된 제어를 제공할 수 있다.  

## 마치며

Filter와 Intercepter는 얼핏보기에는 비슷해보일 수도 있다.  
하지만 둘은 애플리케이션내에서 서로 다른 역할을 수행한다.  
이러한 개념을 이해하고 적절히 활룡한다면 더 좋은 애플리케이션을 구축하고 개발하는데 도움이 될 것이다.  