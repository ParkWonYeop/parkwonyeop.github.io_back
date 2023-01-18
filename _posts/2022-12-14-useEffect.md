---
layout: post
read_time: true
show_date: true
title: React- useEffect란?
date: "2022-12-14 18:23:20"
description: useEffect에 대해서 알아보자
tags:
  - React
author: ParkWonyeop
published: true
img: ":title.png"
---

## 서론

우리가 동적인 사이트를 만들때 실시간으로 작업을 처리할 필요성을 느낄 때가 있다.  
이때 필요한 것이 useEffect 이다.  

## useEffect란?

React의 useEffect 함수란 컴포넌트가 렌더링될 때마다 특정 작업을 실행할 수 있도록하는 Hook 이다.  
컴포넌트가 업데이트 되거나, 마운트되거나, 언마운트됐을 때 특정 작업을 처리하도록 할 수 있다.  


## 기본형태

> import {useEffect} from 'react';  

위의 코드로 useEffect를 사용할 준비는 끝난다.  

> useEffect(() => {  
>     ~~처리할코드  
> })  

제일 기본적인 형태로 렌더링될 때 마다 실행되는 형태이다.  

제일 처음 렌더링 될 때만 실행되게 하려면

> useEffect(() => {  
>     ~~처리할코드  
> }, [])  

이렇게 끝부분에 빈 배열을 넣으면 된다.  

## 특정값이 바뀔때만 실행

> useEffect(() => {  
>     ~~처리할코드  
> }, [특정값])  

이렇게 빈배열만 뒀던 부분에 특정값을 넣어두면 저 특정값이 업데이트 될때마다 해당 작업이 실행된다.  


## 컴포넌트가 사라지거나 업데이트되기 직전에 실행

cleanup 함수를 반환하는 방법을 사용한다.  

> useEffect(() => {  
>     ~~처리할코드
>     return () => {  
>       ~~cleanup코드  
>     }  
> }, [])  

다음과 같이 cleanup 코드를 리턴해주면 컴포넌트가 사라질 때 작업을 실행시킬 수 있다.  
위와 같은 cleanup을 사용하는 이유는 컴포넌트가 사라질 때 그 값을 사용하고 정리하는 과정이라고 생각하면 편하다.  

특정값이 업데이트 되기 전에 사용하고싶으면 빈배열이 있는 자리에 값을 넣으면 된다.  

## 마치며

React를 이용해 동적인 사이트를 만들기 위해서는 알아야하는 것들이 많은 것 같다.  
