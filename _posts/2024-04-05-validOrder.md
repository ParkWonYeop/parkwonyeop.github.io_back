---
layout: post
read_time: true
show_date: true
title: Valid 순서 정하기
img: ":Spring.png"
date: "2024-04-08 18:23:20"
description: 
tags:
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

Spring에서 @Valid 어노테이션으로 데이터를 검증할때 여러개의 어노테이션을 동시에 붙여서 검증하는 경우가 있다.  
이 경우에 @NotBlank, @Pattern, @Size 등을 사용하는데 어노테이션간의 검증 순서를 정하는 법에 대해서 오늘 알아보자.  

## 검증 순서

예를 들어, 공백의 데이터가 들어왔을때 아래와 같은 검증을 거친다고 생각해보자.  

```Java
@Pattern(regexp = "[0-9]{8,12}", message = "전화번호 형식을 맞춰주세요.")
@NotBlank(message = "빈 문자열 입니다.")
String phoneNumber
```

이 경우 공백이라는 데이터는 두 가지 어노테이션을 모두 위반하기때문에 무엇으로 반환될지 예상할 수 없다.  
이때 순서를 정하는 방법에 대해서 알아보자.  

우선 ValidationGroups 라는 클래스를 만들자.  

```Java
public class ValidationGroups {
    public interface NotBlankGroup {}
    public interface PatternGroup {}
    public interface SizeGroup {}
    public interface FutureGroup {}
    public interface PositiveGroup {}
}
```

이런식으로 순서를 정하고 싶은 어노테이션의 인터페이스를 만들어 클래스에 정리를 해두면 된다.  
물론 따로 인터페이스를 선언해도 상관은 없는데 이렇게 정리하는게 더 깔끔하니 이렇게 하겠다.  

그 후 순서를 정하기 위해 하나의 인터페이스를 만들자.  

```Java
@GroupSequence({NotBlankGroup.class, PatternGroup.class, SizeGroup.class, FutureGroup.class, PositiveGroup.class})
public interface ValidationSequence {
}
```

@GroupSequence 어노테이션 내부에 위와 같이 먼저 검증했으면 하는 어노테이션 그룹부터 순서대로 적으면 된다.  
이제 이걸 어떻게 적용하는지를 알아보자.  

```Java
@Pattern(regexp = "[0-9]{8,12}", message = "전화번호 형식을 맞춰주세요.", groups = PatternGroup.class)
@NotBlank(message = "빈 문자열 입니다.", groups = NotBlankGroup.class)
String phoneNumber,
```

위와 같이 groups 라는 속성을 적고 거기에 순서에 맞춰 클래스를 불러와 적어주면 된다.  
이러면 데이터가 들어왔을때 @NotBlank로 먼저 검증하고, @Pattern을 검증하게 된다.  

하지만 우리가 기존에 사용하던 @Valid 어노테이션으로는 해당 순서가 작동하지 않는다.  
그렇기때문에 기존에 사용하던 컨트롤러에 가서 @Valid 어노테이션을 @Validated(ValidationSequence.class) 으로 교체해줘야한다.  

```Java
@Valid @RequestBody ValidDto validDto
//아래와 같이 교체
@Validated(ValidationSequence.class) @RequestBody ValidDto validDto
```

이제 모든 검증을 거치는 모든 데이터 객체를 원하는 순서에 맞게 교체해주면 검증 순서를 정할 수 있게 된다.  