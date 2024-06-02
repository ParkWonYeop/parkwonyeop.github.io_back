---
layout: post
read_time: true
show_date: true
title: Drop Table, Truncate
img: ":title.png"
date: "2024-05-24 18:23:20"
description: 
tags:
  - theory
author: ParkWonyeop
published: true
category: theory
---
## 서론

데이터베이스에서 테이블을 삭제하거나, 초기화하는 경우에 사용되는 sql문으로는 DROP TABLE과 TRUNCATE가 있다.  
이 둘에 대해서 알아보고 차이점을 비교해보자.  

## DROP TABLE

DROP TABLE은 테이블의 구조와 내부 데이터를 모두 제거한다.  
테이블 내부 구조, 즉 연결된 모든 데이터, 인덱스, 제약 조건, 권환 사양 등을 모두 제거하는데 사용된다.  
테이블 구조를 남기지 않고 제거해야하는 경우에는 DROP TABLE을 사용해야한다.  

## TRUNCATE

테이블의 모든 행을 제거하지만, 테이블 구조는 유지한다.  
DELETE 명령어와 유사한데 다른점은 각 행의 삭제를 로깅하지 않고 삭제하므로 더 빠르고, 트랜잭션 로그 리소스를 더 적게 사용한다.  

## 차이점

성능적인 관점에서 보면 TRUNCATE는 각 행을 로깅하지 않으므로 DROP TABLE보다 일반적으로 더 빠르다.  
하지만 테이블 구조를 완전히 제거해야하는 경우에는 DROP TABLE을 사용하는 것이 더 효율적일 수 있다.  
반대로 말하면 테이블 구조를 완전히 제거하지 않고 테이블의 데이터들만 삭제해야하는 경우에는 DROP TABLE로 테이블을 삭제하고 다시 테이블을 생성하는 것보다는 TRUNCATE를 사용하는 것이 더 맞다.  
