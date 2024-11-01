---
layout: post
read_time: true
show_date: true
title: Conventional Commits
img: ":title.png"
date: "2024-10-29 18:23:20"
description: 이 문서는 Conventional Commits 에 대해 설명합니다.
tags:
  - Github
author: ParkWonyeop
published: true
category: theory
---
# Conventional Commits

이 문서는 Conventional Commits 에 대해 설명합니다.

## **Rule**

1. 메시지 구조
    - 제목과 본문은 빈 줄로 명확히 구분
2. 제목 작성 규칙
    - 간결성
        - 최대 50자 이내로 작성하여 변경 사항의 핵심을 명확히 전달
    - 대문자로 시작
        - 첫 글자를 대문자로 작성하여 가독성 증진
    - 명령문
        - 현재 시제 명령문을 사용하여 의도를 명확히 표현
        ***예: “Fix 버그 수정”***
        - 과거형 또는 설명형 시제는 지양
    - 마침표 생략
3. 본문 작성 규칙
    - 상세 설명
        - 제목에서 전달하지 못한 세부적인 변경 사항 및 이유를 명확히 기술
    - 줄 바꿈
        - 각 줄은 72자 이내로 제한하여 가독성을 유지
    - 변경 이유
        - 변경 방법(어떻게)보다는 변경 내용(무엇)과 그 이유(왜)를 중점적으로 설명하여 코드 작성 의도를 명확히 하여야 함
4. 구조
    
    ```
    <type>: <subject>
    
    <body>
    
    <footer>
    ```
    

## **Commit Type**

| **유형** | **설명** |
| --- | --- |
| feat | 새로운 기능 추가 또는 기존 기능을 요구 사항에 맞춰 수정 |
| fix | 버그 수정 |
| build | 빌드 시스템 또는 빌드 도구 관련 수정 |
| chore | 패키지 매니저 설정, `.gitignore` 와 같은 설정 파일 수정 등 기타 자잘한 변경 사항 |
| ci | 지속적 통합(CI) 설정 및 관련 스크립트 수정 |
| docs | 코드 주석, `README` 와 같은 문서 수정 |
| style | 코드 스타일, 포맷팅 수정 (코드 동작 변경 없음) |
| refactor | 코드 리팩토링 **(기능 변경 없음) ***예: 변수 이름 변경, 코드 구조 개선*** |
| test | 테스트 코드 추가 또는 수정 |
| release | 새로운 버전 릴리즈 |

## **예시**

1. 새로운 기능 추가
    
    ```
    feat: 사용자 프로필 페이지 추가
    
    사용자 정보를 확인하고 수정할 수 있는 프로필 페이지를 추가했습니다.
    
    - 프로필 페이지 UI 구현
    - 사용자 정보 API 연동
    - 프로필 이미지 업로드 기능 추가
    ```
    

1. 버그 수정
    
    ```
    fix: 로그인 오류 수정
    
    잘못된 비밀번호 입력 시 발생하는 로그인 오류를 수정했습니다.
    
    - 비밀번호 검증 로직 수정
    - 오류 메시지 표시 기능 개선
    ```
    
2. 빌드 설정 변경
    
    ```
    build: 웹팩 설정 변경
    
    프로덕션 빌드 시 코드 압축 기능을 추가했습니다.
    
    - 웹팩 설정 파일 수정
    - 압축 플러그인 추가
    ```
    

1. 패키지 추가
    
    ```
    chore: axios 패키지 추가
    
    HTTP 요청을 위한 axios 패키지를 추가했습니다.
    
    - package.json 수정
    - axios 설치
    ```
    

1. 테스트 코드 추가
    
    ```
    test: 로그인 기능 테스트 추가
    
    로그인 기능에 대한 단위 테스트를 추가했습니다.
    
    - 로그인 성공/실패 케이스 테스트
    - 테스트 커버리지 증가
    ```
    

1. 문서 업데이트
    
    ```
    docs: README 업데이트
    
    프로젝트 설명 및 사용 방법을 업데이트했습니다.
    
    - README 파일 수정
    - 최신 정보 반영
    ```
    

1. 코드 리팩토링
    
    ```
    refactor: 로그인 컴포넌트 리팩토링
    
    로그인 컴포넌트 코드를 가독성 및 유지보수 용이성을 위해 리팩토링했습니다.
    
    - 코드 구조 개선
    - 변수 및 함수 이름 명확화
    ```
    

1. CI 설정 변경
    
    ```
    ci: 빌드 파이프라인 수정
    
    빌드 파이프라인에 테스트 단계를 추가했습니다.
    
    - CI 설정 파일 수정
    - 테스트 실행 스크립트 추가
    ```
    

1. 릴리즈
    
    ```
    release: v1.2.0 릴리즈
    
    새로운 기능과 버그 수정을 포함한 v1.2.0 버전을 릴리즈했습니다.
    ```