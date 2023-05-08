---
layout: post
read_time: true
show_date: true
title: 위도와 경도를 이용해 거리를 구해보자
img: ":title.png"
date: "2023-05-06 18:23:20"
description: 
tags:
  - math
author: ParkWonyeop
published: true
category: math
---

## 서론

이번에 만들고있는 프로젝트에서 현재 위치와 특정 지점 사이의 거리를 구해야하는 일이 생겼다.  
두 지점의 위경도를 이용해서 거리를 구하는 방법에 대해서 알아보자.  

## Haversine 공식

위경도를 이용해 두 지점을 구하기 위해서는 Haversine 공식을 이용해야한다.  
두 점사이의 거리를 구하면 된다고 생각할지도 모르겠지만 지구는 둥글다.  
그렇기때문에 구 표면의 두 지점 사이의 거리를 구할 수 있는 Harversine 공식을 사용한다.  

우선 Harversine 공식을 구하기 위해서는 지구의 반지름에 대해서 알아야한다.  
지구의 반지름은 약 6371KM 이다.  
그리고 두 지점의 위도 사이의 차이와 경도 사이의 차이를 구해야한다.  

아래는 Harversine 공식으로 위도와 경도 사이의 거리를 구하는 방법을 식으로 나타낸 것이다.  

```
a1 = 첫번째 지점의 위도 * (π/180)
b1 = 첫번째 지점의 경도 * (π/180)
a2 = 두번째 지점의 위도 * (π/180)
b2 = 두번째 지점의 경도 * (π/180)
R = 지구의 반지름(6371km)

(a2 >= a1, b2 >= b1)

c = a2 - a1
d = b2 - b1

x = sin^2(c/2) + cos(a1) * cos(a2) * sin^2(d/2)
y = 2 * atan2(√x, √(1−x))
result = R · y
```

위의 식으로 result를 구하면 km단위로 된 값을 구할 수 있다.  
이것이 두 점 사이의 거리이다.  

## atan란?

위의 식을 이해하려면 우선 삼각함수에 대해서 알아야한다.  
sin, cos, tan 같은 것들은 우리가 고등학교때 배웠던 것들이니 알겠지만 수학에 큰 뜻이 없었다면 atan는 뭘까 싶을 수도 있다.  
atan는 아크탄젠트라고 말하고 쉽게 말하면 탄젠트의 역함수이다.  
두 점 사이의 θ의 절대각을 구할때 사용하는 함수이다.  

그렇다면 위의 공식에서 사용한 atan2는 무엇일까?

atan는 두점 사이의 탄젠트 값을 받아 절대각을 -π/2 ~ π/2의 라디안 값으로 변환한다.  
atan2는 두 점 사이의 상대좌표(x,y) 를 받아 절대각을 -π ~ π의 라디안 값으로 변환한다.  

## 왜 atan2를 사용해야하는가?

atan2는 +와 -가 표시되는 데카르트 좌표계에서 사용할 때 유용하다.  
atan2 함수는 점 A로부터 점 B가 상대적으로 어느 위치에 있는지를 파라미터로 받을 수 있다.  
하지만 atan 함수는 두점 사이의 각도를 구하는 함수이므로 위의 공식에서는 atan2를 사용한 것이다.  


## 계산할줄 알필요는 없다.

대부분의 코드에서는 atan2 값을 구하는 함수를 제공해준다.  

```javascript
Math.atan2(x, y);
```

## Javascript로 구현해본 Haversine 공식

```
function toRadians(degrees) {
  return degrees * (Math.PI / 180);
}

function haversineDistance(lat1, lon1, lat2, lon2) {
  const R = 6371; // 지구의 반지름

  const lat1_rad = toRadians(lat1);
  const lon1_rad = toRadians(lon1);
  const lat2_rad = toRadians(lat2);
  const lon2_rad = toRadians(lon2);

  const dLat = lat2_rad - lat1_rad;
  const dLon = lon2_rad - lon1_rad;

  const a = Math.sin(dLat / 2) ** 2 +
            Math.cos(lat1_rad) * Math.cos(lat2_rad) *
            Math.sin(dLon / 2) ** 2;
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

  const distance = R * c;
  return distance;
}
```

위의 함수를 이용하면 두 개의 위경도 사이의 거리를 구할 수 있다.  

