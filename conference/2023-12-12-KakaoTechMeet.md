---
layout: post
title: 2023 GDG 송도 Devfest 참여후기
image: 
  path: /assets/img/blog/gdg_devfest.webp
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

KakaoTech Meet up
{:.lead}


- Table of Contents
{:toc .large-only}

## Virtual Thread란 무엇인가?

JVM 내부 스케줄링을 통해서 수십만개의 스레드를 사용하게 한다.

## Blocking I/O

대기시간이 처리시간보다 길어짐

## Reactive Programming

Webflux는 스레드 대기하지 않고 다른 작업 처리
코드를 작성하고 이해하는 비용이 높음
Reactive 하게 동작하는 라이브러리 지원을 필요
- ex. flatMap {}

## Java Design

스레드 중심 디자인
Exception stack trace, Debugger, Profiling 모두 스레드 기반
Reactive는 여러 스레드를 거쳐 작업되는데, 디버깅이 힘듦

## 해결하고자 하는 문제

Blocking 발생 시 내부 스케줄링을 통해 다른 작업을 처리
기존 스레드 구조 그대로 사용

## 장점

처리량은 Reactive만큼 높고 코드 이해도는 MVC만큼 낮음

## 자원 사용량

Virtual Thread는 Platform Thread보다 자원 사용량이 적음
메모리는 Heap을 사용

## 사용방법

ExcutorService 사용. Thread는 로우레벨
Spring Boot(MVC) 3.2 적용법은 Property로 하면 개꿀
이하 버전은 Tomcat이나 Async Task에 Bean으로 등록하면 됨

## 유의사항

Task들이 Virtual Thread가 할당된다
Thread Local을 남발하면 Virtual Thread는 Heap을 사용하기 때문에 자원 사용량이 크게 늘어남
ReentrantLock으로 pinning을 방지할 수 있음

## 성능테스트

슬로우 쿼리 1초일 때 throughput을 뒤로 넘길 때(DB connection) 타임아웃(Overwhelming)

## 결론 

기존 Thread pool을 사용하던 이유
- Platform Thread가 비쌈
- Throttle 역할도 수행

I/O 인텐시브가 아닌 이미지 작업같은 CPU 인텐시브 작업은 도움되지 않음
Vitual Thread는 기다림(I/O)에 대한 개선, 그리고 Reactive 라이브러리에서 아쉬웠던 플랫폼 디자인과의 조화


## 큐라스 메시지버스

광고효과 예측 모델

