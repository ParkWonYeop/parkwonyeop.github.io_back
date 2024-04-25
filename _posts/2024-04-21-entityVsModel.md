---
layout: post
read_time: true
show_date: true
title: Model과 Entity 차이
img: ":title.png"
date: "2024-04-21 18:23:20"
description: 
tags:
  - theory
author: ParkWonyeop
published: true
category: theory
---
## 서론

DTO, 엔티티, 모델은 데이터베이스에 관련된 용어들이다.  
이중에서 DTO는 역할이 분명한데 엔티티와 모델은 명확하게 구분하기 어렵다.  
오늘은 이 둘의 역할에 대해서 정확하게 알아보자.  

## Entity

Entity는 데이터베이스의 특정 테이블과 매핑되는 클래스를 나타낸다.  
주로 데이터베이스의 테이블 스키마를 정의하며, 해당 테이블의 레코드를 나타내는 객체이다.  

예를 들어, JPA에서 Entity는 아래와 같이 적을 수 있다.  

```JAVA
@Data
@Setter
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Entity(name = "users")
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long userId;

    @Column(name = "phone_number", nullable = false)
    private String phoneNumber;

    @Column(name = "password", nullable = false)
    private String password;

    @Column(name = "name", nullable = false)
    private String name;

    @CreationTimestamp
    private LocalDateTime createAt = LocalDateTime.now();

    @UpdateTimestamp
    private LocalDateTime updateAt = LocalDateTime.now();
}
```

이렇게 유저와 관련된 데이터베이스 스키마를 객체의 형태로 나타낸게 Entity이다.  


## Model

일반적으로 데이터를 처리하는 일련의 규칙과 메소드를 정의하는 부분을 Model이라고 한다.  
데이터베이스의 CRUD 작업을 처리하는 메서드와 기능을 가지고 있다.  

자바에서는 Repository로 모델의 기능을 구현한다.  

```Java
public interface UserRepository extends JpaRepository<UserEntity, Long> {
    Optional<UserModel> findUserModelByPhoneNumber(String phoneNumber);
}
```
