---
layout: post
read_time: true
show_date: true
title: 테스트할때 Request 객체를 어떻게 구성해야할까?
img: ":Spring.png"
date: "2024-04-18 18:23:20"
description: 
tags:
  - JUnit
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

JUnit에서 MockMVC를 이용해 테스트를 진행할 때 Reqeuest 객체를 구성해서 보내야한다.  
이때, DTO로 구성하여 보내는 방법이 있고, HashMap을 이용해 직접 객체를 만들어 보내는 방법이 있다.  
이 게시글은 두가지 방법 중 어느 방법이 더 좋은 방법일까 고민해보고 작성한 글이다.  

## 본론

MockMVC에서 Body에 객체를 실어보낼때 이미 구성되어 있는 DTO 클래스를 불러와 보내는 방법이 있다.  
이 방법은 이미 구성되어 있는 DTO를 통해 객체를 구성할 수 있으므로, 코드가 간결해지고 데이터의 무결성을 보장할 수 있다.  
하지만 이 데이터의 무결성이 보장되는 부분이 내가 HashMap을 사용해야한다고 생각한 부분이다.  

## TEST?

우리가 테스트를 하는 이유는 무엇일까?  
여러가지 변수가 있는 상황에서 개발자가 예상한대로 어플리케이션이 작동하는지를 검증하기 위해서이다.  
이러한 변수중에서는 잘못된 매개변수가 들어왔을때 어플리케이션이 어떻게 동작하는지도 있을 것이다.  
이런 부분에서 DTO를 통해 Body를 구성한다면 예외적인 데이터를 만들기가 매우 까다롭다.  
하지만 직접 객체를 만들어 HashMap으로 Body를 구성해 보내준다면 예외적인 데이터를 만들기 쉽다.  
그러면 잘못된 데이터가 들어왔을때 어플리케이션이 어떻게 동작하는지를 알 수 있는 테스트를 만들 수 있는 것이다.  

## HashMap

```JAVA
public class ReservationFixtures {
    public static Map<String, String> createReservationRoomType(String roomType) throws JsonProcessingException {
        Map<String, String> body = new HashMap<>();

        LocalDateTime startTime = LocalDateTime.now().plusHours(1).withMinute(0).withSecond(0).withNano(0);

        body.put("roomType", roomType);
        body.put("seatNumber", "1");
        body.put("startTime", startTime.toString());
        body.put("endTime", startTime.plusHours(1).toString());

        return body;
    }
}
```

다음은 내가 테스트를 위해 만든 Fixture 중 하나다.  
HashMap을 사용해 직접 바디를 구성해 리턴해주는 코드이다.  

```Java
public class ReservationFixtures {
    public static ReservationDto createReservationRoomType(String roomType) {
        LocalDateTime startTime = LocalDateTime.now().plusHours(1).withMinute(0).withSecond(0).withNano(0);

        ReservationDto reservationDto = new ReservationDto(roomType, 1, startTime, startTime.plusHours(1));

        return reservationDto;
    }
}
```

위 코드는 ReservationDto를 이용해 데이터를 만드는 코드이다.  
이 두 코드에서 roomType은 Enum인데 그렇다면 아래 코드에서 Enum형과 맞지않는 데이터가 들어간다면 예외를 발생시킬 것이다.  
이러한 경우 잘못된 데이터를 테스트하기 어려워진다.  
그렇기때문에 위와 같이 HashMap을 통해 직접 Json을 만들어 예외적인 데이터가 들어왔을때의 상황을 테스트한 것이다.  

## 마치며

테스트를 할때 중요한 것은 기능이 성공적으로 작동하는 것이 아니다.  
문제가 생겼을 때, 혹은 예외적인 상황이 발생했을때 어플리케이션이 내가 구성한대로 잘 대응되는지를 테스트해야한다.  
그렇기때문에 거의 모든 상황에 대한 테스트 케이스를 구성해둔다면 더 완성도 높은 어플리케이션을 만들 수 있을 것이다.  