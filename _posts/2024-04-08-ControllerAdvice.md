---
layout: post
read_time: true
show_date: true
title: ControllerAdive 에러 핸들링
img: ":Spring.png"
date: "2024-04-08 18:23:20"
description: 
tags:
  - Java
author: ParkWonyeop
published: true
category: Java
---
## 서론

Spring에서 예외 상황이 발생했을때, 에러를 발생시켜 처리해야한다.  
하지만 기존의 RuntimeException은 핸들링하기 쉽지가 않다.  
이럴때 ControllerAdvice를 이용해 에러를 핸들링 해보자.  

## CustomException

일단 에러를 원하는대로 핸들링하기 위해서는 기존의 RuntimeException의 형태를 변형시켜서 원하는대로 바꿔야한다.  
필자는 다음과 같은 형대로 에러를 바꿨다.  

```Java
@Getter
public class CustomException extends RuntimeException{
    private final String message;
    private final HttpStatus httpStatus;

    public CustomException(CommunalResponse communalResponse) {
        this.httpStatus = communalResponse.getHttpStatus();
        this.message = communalResponse.getMessage();
    }
}
```

RuntimeException을 상속받아 CustomException을 만들었다.  
@Getter 어노테이션으로 Getter를 생성하고, CommunalResponse라는 Enum을 만들어 원하는 httpStatus와 메세지를 담을 수 있게 커스텀했다.  
Enum은 다음과 같은 형태를 가지고 있다.  

```Java
@AllArgsConstructor
@Getter
public enum CommunalResponse {
    //Auth
    USER_NOT_FOUND(HttpStatus.UNAUTHORIZED, "유저를 찾을 수 없습니다."),
    USER_NOT_CORRECT(HttpStatus.BAD_REQUEST, "유저가 일치 하지 않습니다."),
    ALREADY_SIGNUP_PHONENUMBER(HttpStatus.BAD_REQUEST, "이미 가입한 전화번호입니다."),
    //RentalWare
    ALREADY_RENTAL(HttpStatus.BAD_REQUEST, "이미 예약한 유저입니다."),
    RENTAL_NOT_FOUND(HttpStatus.BAD_REQUEST, "해당 유저의 예약을 찾을 수 없습니다."),
    BANED_USER(HttpStatus.BAD_REQUEST,"이용이 정지 된 유저입니다."),
    //Admin
    WARE_ALREADY_ADD(HttpStatus.BAD_REQUEST,"이미 존재하는 물품입니다."),
    WARE_COUNT_MINUS(HttpStatus.BAD_REQUEST, "물품의 최대 개수가 마이너스 입니다."),
    FILE_IS_EMPTY(HttpStatus.BAD_REQUEST,"파일이 비어있습니다.");

    final HttpStatus httpStatus;
    final String message;
}
```

위는 예시로 원하는 httpStatus와 message를 담아 예외를 발생시켜주면 된다.  

```Java
userRepository.findUserModelByPhoneNumber(phoneNumber);

if (userRepository.findUserModelByPhoneNumber(phoneNumber).isPresent()) {
    throw new SportException(CommunalResponse.ALREADY_SIGNUP_PHONENUMBER);
}
```

이렇게 사용해주면 된다.  

## ControllerAdvice

이제 이렇게 만든 예외를 원하는대로 처리하기 위해 필요한 것이 ControllerAdvice 이다.  
ControllerAdvice는 컨트롤러 전역에서 발생하는 로직에 대해 처리할 수 있게 해준다.  

우선 우리가 원하는 예외를 반환해주기 위해 아래와 같이 코드를 짜주면 된다.  

```Java
@ControllerAdvice
@Slf4j
public class GlobalControllerAdvice {
  @ExceptionHandler(CustomException.class)
    public ResponseEntity<Object> handleLibraryReservationException(final CustomException CustomException) {
        log.error(CustomException.getMessage());
        return ResponseEntity.status(CustomException.getHttpStatus()).body(CustomException.getMessage());
    }
}
```

코드를 위와 같이 만들어주면 열거형으로 만들었던 데이터를 예외가 발생하면 컨트롤러 어드바이스에서 처리해서 클라이언트에게 반환해줄 수 있다.  

```Java
@ControllerAdvice
@Slf4j
public class GlobalControllerAdvice {
  @ExceptionHandler(CustomException.class)
    public ResponseEntity<Object> handleLibraryReservationException(final CustomException CustomException) {
        log.error(CustomException.getMessage());
        return ResponseEntity.status(CustomException.getHttpStatus()).body(CustomException.getMessage());
    }

    @ExceptionHandler({RuntimeException.class,})
    public ResponseEntity<Object> handleBadRequestException(final RuntimeException runtimeException) {
        log.error(runtimeException.getMessage());
        return ResponseEntity.badRequest().body(runtimeException.getMessage());
    }

    @ExceptionHandler(AccessDeniedException.class)
    public ResponseEntity<Object> handleAccessDeniedException(final AccessDeniedException accessDeniedException) {
        log.error(accessDeniedException.getMessage());
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body(accessDeniedException.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<Object> handleException(final Exception exception) {
        log.error(exception.getMessage());
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(exception.getMessage());
    }
}
```

꼭 커스텀한 에러 외에도 위와 같이 여러가지 예외를 처리할 수 있는 로직을 만들 수 있으니 활용하면 좋을 것이다.  