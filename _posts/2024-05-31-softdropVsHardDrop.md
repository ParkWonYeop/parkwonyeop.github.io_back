---
layout: post
read_time: true
show_date: true
title: soft delete, hard delete
img: ":title.png"
date: "2024-05-31 18:23:20"
description: 
tags:
  - theory
author: ParkWonyeop
published: true
category: theory
---
## 서론

데이터베이스에서 데이터를 삭제할 때 크게 두가지 방법이 있다.  
물리적으로 데이터베이스에서 삭제하는 Hard Delete, 데이터는 유지하되 삭제 된 것처럼 보이게 하는 Soft Delete  
오늘은 이 두가지에 대해서 알아보자.  

## Hard Delete

Hard Delete는 보통 Delete, Truncate 등을 이용해 테이블에서 물리적으로 데이터를 삭제하는 것을 말한다.  

```SQL
DELETE FROM user WHERE userId = ?;
```

위와 같은 SQL문을 이용해 삭제하는 것을 Hard Delete라고 한다.  

## Soft Delete

Soft Delete는 데이터를 직접 삭제하는 대신 데이터가 삭제되었다는 흔적을 남겨서 데이터를 사용하지 않게하는 방법이다.  
여기서는 보통 데이터가 삭제되었다는 컬럼을 하나 추가해 체크하는데 필자같은 경우는 데이터가 삭제된 날짜를 표시하는 방식으로 Soft Delete를 구현한다.  

```SQL
UPDATE user SET deletedAt = currentdate where userId = ?
```

위와같이 구현하면 기존에는 null인 deletedAt을 현재 시간을 기록하며 언제 삭제되었는지를 표시할 수 있다.  
그렇다면 해당 데이터를 조회할때는 어떻게 조회해야할까?  

```SQL
SELETE * FROM user WHERE deletedAt IS NULL
```

이렇게하면 Soft Delete된 데이터를 빼고 조회하는 것이 가능하다.  
