---
layout: post
read_time: true
show_date: true
title: @PrePersist와 @PreUpdate
img: ":title.png"
date: "2024-06-03 18:23:20"
description: 
tags:
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

데이터베이스에서 처리하기 어려운 제약 조건을 애플리케이션 로직내에서 처리하려고하면 어떻게 해야할까?  
코드를 일일이 적어서 처리하는 방식은 세련되지 못한다.  
그렇다면 데이터가 저장되기전이나 데이터 변경이 감지되었을때 로직을 실행할 수 있게한다면 좋지않을까?  
그걸 가능하게 해주는게 @PrePersist, @PreUpdate 어노테이션이다.  
오늘은 이 어노테이션에 대해서 알아보자.  

## @PrePersist

@PrePersist 어노테이션은 엔티티가 데이터베이스에 저장되기 전에 호출된다.  
이를 사용하여 엔티티가 영속화되기전에 필요한 초기화나 유효성 검사를 수행할 수 있다.  

```Kotlin
@EntityListeners(AuditingEntityListener::class)
@MappedSuperclass
class BaseEntity(
    @Column(name = "created_at", updatable = false)
    var createdAt: LocalDateTime? = null,

    @Column(name = "updated_at")
    var updatedAt: LocalDateTime? = null,

    @Column(name = "deleted_at")
    var deletedAt: LocalDateTime? = null,
) {
    @PrePersist
    fun prePersist() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }

    fun softDelete() {
        this.deletedAt = LocalDateTime.now()
    }
}
```

위와 같이 데이터베이스가 저장되기전 로직을 실행시켜 createdAt, updatedAt을 실행시킬 수 있다.  

## @PreUpdate

@PreUpdate 어노테이션은 데이터베이스에서 Entity가 업데이트 되기전에 실행된다.  

```Kotlin
@EntityListeners(AuditingEntityListener::class)
@MappedSuperclass
class BaseEntity(
    @Column(name = "created_at", updatable = false)
    var createdAt: LocalDateTime? = null,

    @Column(name = "updated_at")
    var updatedAt: LocalDateTime? = null,

    @Column(name = "deleted_at")
    var deletedAt: LocalDateTime? = null,
) {
    @PrePersist
    fun prePersist() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }

    @PreUpdate
    fun preUpdate() {
        updatedAt = LocalDateTime.now();
    }

    fun softDelete() {
        this.deletedAt = LocalDateTime.now()
    }
}
```

## 장점

1. 데이터 무결성 보장  
- Entity가 영속화되기 전에 실행되므로 데이터의 무결성을 보장할 수 있다.  
  
2. 데이터 처리 효율성 향상  
- 데이터베이스에 저장하기전에 로직을 실행하므로 데이터 처리 효율성이 올라간다.  
  
3. 데이터 보안 강화  
- 데이터베이스에 저장하기전 필드 값을 암호화하거나, 저장하기전 권한을 확인하는 등의 작업을 수행할 수 있다.  
  
4. 데이터 동기화  
- Entity필드값을 다른 Entity와 동기화하거나, 다른 데이터베이스와 동기화하는 작업을 수행할 수 있다.  
  
5. 데이터 일관성 유지  
- 필드값이 변경될 떄 이를 일관성 있게 처리하거나, 데이터베이스에 저장하기전 데이터를 검증하여 일관성을 유지할 수 있다.  

## 단점

1. 제한된 재사용성  
- Entity 클래스와 긴밀하게 결합되기때문에, 다른 Entity에서 동일한 로직을 사용하거나 어플리케이션과 공유하는 것이 어렵다.  
  
2. 감소된 테스트 가능성  
- Entity클래스 내에 정의된 로직을 테스트하는 것은 다른 어플리케이션 로직을 테스트하는 것보다 어려울 수 있다.  
  
3. 잠재된 성능 오버헤드  
- Entity 인스턴스에서 복잡한 계산이나 데이터베이스 상호 작용이 포함된 작업의 경우 성능 오버헤드가 발생할 수 있다.  
