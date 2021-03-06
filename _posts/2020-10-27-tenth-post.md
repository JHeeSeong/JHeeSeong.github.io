---
title: "NginX란?"
date: 2020-10-27 09:20:00 -0900
categories: NginX, Apache
---

> Nginx는 웹 서버 소프트웨어로, 가벼움과 높은 성능을 목표로 한다. 웹 서버, 리버스 프록시 및 메일 프록시 기능을 가집니다.

### 특징

- Event-Driven 방식

   → 주기적으로 이벤트가 발생했는지 확인하며, 이벤트가 감지되었을 때 어플리케이션 서버로 이벤트가 있음을 알리는 것입니다.
- Nginx는 요청에 응답하기 위해 비동기 이벤트 기반 구조
- 다수의 연결을 효과적으로 처리 가능
- 코어 모듈이 Apache보다 적은 리소스로 빠르게 동작 가능
- 트래픽이 많은 웹사이트를 위해 확장성을 위하여 설계한 비동기 이벤트  기반 구조의 웹서버

> 웹서버는 클라이어트로 부터 요청이 발생하였을 때, 요청에 알맞는 정적파일을 보내 주는 역할입니다.

### Apache특징

- 스레드/프로세스 기반 구조를 가지는 것
 
   → 요청 하나 당 쓰레드 하나로 처리하는 구조(1 쓰레드 : 1 클라이언트)
- 사용자가 많으면 많은 쓰레드 생성, 메모리 및 CPU 낭비