---
layout: post
title:  "풀스택 웹개발(6) - In memory database - Redis"
date:   2023-11-04 12:40:48 +0900
categories: webdev
---


## 소개
[풀스택 웹개발] 시리즈는 Spring을 활용하여 풀스택 웹사이트를 만드는 것을 목표로 합니다. 빠르고 직관적인 이해를 돕기 위해 전문적인 용어보다는 비유적인 표현을 많이 사용했습니다. 웹사이트를 처음 만들어보는 분도 쉽게 배우고 따라하는 웹개발 설명서가 되었으면 합니다.

본 시리즈가 다루는 주제는 크게 다음과 같습니다:
> - [API, REST API](#https://minisemin.github.io/webdev/2023/09/09/webdev1.html)
> - [Spring Framework](#https://minisemin.github.io/webdev/2023/09/14/webdev2.html)
> - [Spring Sequrity](#https://minisemin.github.io/webdev/2023/10/07/webdev3_spring_security.html)
> - [Git, Git Action과 Docker](#https://minisemin.github.io/webdev/2023/10/14/webdev4_git_gitaction_docker.html)
> - [AWS 클라우드 컴퓨팅](#https://minisemin.github.io/webdev/2023/10/14/webdev5_AWS.html)
> - [In Memory Database - REDIS](#)
> - 마이크로서비스 아키텍처

다음과 같은 내용을 학습하시면 본 시리즈를 이해하는 데 도움이 됩니다:
> - 객체지향(Opject-oriented-programming)
> - Java
> - SQL

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### Table of Contents
---
1. [Introduction](#소개)
2. [In Memory Database](#in-memory-database)
3. [REDIS](#redis)

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## In Memory Database

인 메모리 데이터베이스 (In-memory database, IMDB)는 데이터를 디스크가 아닌 메모리(RAM)에 저장하여 운영하는 데이터베이스 시스템입니다. 컴퓨터는 프로그램을 실행시키기 위해 데이터를 저장소로부터 가져오고, 업데이트해서 다시 저장하는 작업을 반복해야 하는데, 디스크는 용량이 크지만 접근 시간이 너무 오래 걸린다는 단점이 있습니다. 그래서 현대 컴퓨터는 자주 사용하는 정보를 CPU에 가까운 메모리에 옮겨놓고 사용합니다.

> Disk > DRAM > SRAM > CPU

![Memory Hierarchy](https://media.geeksforgeeks.org/wp-content/uploads/20230609020524/Memory-Hierarchy-Design-768.png)

### 특징

1. 고성능: RAM에 데이터를 저장하기 때문에 디스크 I/O(input/output)에 비해 훨씬 빠른 데이터 액세스가 가능합니다.

2. 데이터 복구와 지속성: 시스템 장애 시 많은 종류의 인 메모리 데이터베이스는 데이터를 복구할 수 있는 메커니즘을 가지고 있습니다. 일부 데이터를 디스크에 스냅샷으로 주기적으로 저장하면 데이터가 손실되는 사고를 방지할 수 있습니다.

3. 확장성: 대규모 분산 시스템에 사용할 때도 확장성이 뛰어나기 때문에 클러스터 환경에서 데이터를 공유하고 처리하기 용이합니다.

4. 유연성: 구조화된 데이터 뿐만 아니라 비구조화된 데이터도 처리가 가능합니다.

### 사용

- 실시간 분석: 금융 거래, 네트워크 모니터링, 실시간 데이터 분석 등 빠른 데이터 처리가 필요한 경우

- 캐싱: 웹 애플리케이션에서 자주 사용되는 데이터를 빠르게 접근하기 위해 캐시로 사용할 경우

- 세션 스토리지: 웹 서버의 세션 정보를 저장하고 빠르게 접근하는 용도

&nbsp;

## REDIS

Remote Dictionary Server, REDIS는 오픈 소스 인 메모리 데이터 저장소입니다. 주로 캐싱, 세션 관리, 게임 리더보드, 실시간 분석 등에 사용합니다. REDIS는 Key-Value Store로서 작동하기 때문에 Key값을 통해 Value에 access할 수 있습니다.

> REDIS의 가장 큰 특징은 데이터를 메모리에 저장하기 때문에 매우 빠른 Read & Write가 가능하다는 점입니다.

![REDIS 개요](https://redis.com/wp-content/uploads/2022/08/durable-redis-1.svg?&auto=webp&quality=85,75&width=1200)

### 특징

1. 다양한 Data Structure: 단순한 Key-Value pair 외에도 list, set, hash, ordered set, bitmap같은 다양한 자료구조를 지원합니다.

2. 지속성: REDIS는 In-memory database이지만, 데이터를 디스크에 저장하는 옵션이 존재합니다. RDB(Redis Database)와 AOF(Append Only File) 방식이 있으며, 덕분에 시스템을 재시작하더라도 데이터가 휘발되지 않습니다.

3. 복제 및 고가용성: REDIS는 Master-Slave (주인-노예) 복제 기능을 제공하여 데이터의 고가용성을 보장합니다. 마스터에 장애가 발생할 경우, 슬레이브 노드가 자동으로 마스터 역할을 수행할 수 있습니다.

4. 트랜잭션 처리: REDIS는 기본적인 트랜잭션 기능을 지원하여, 여러 명령어를 한 번에 실행할 수 있습니다.

### 사용

- 캐싱: 데이터베이스 쿼리 결과나 자주 요청되는 페이지의 콘텐츠를 캐싱하여 빠른 응답 시간을 달성할 수 있습니다.

- 세션 스토리지: 웹 애플리케이션에서 사용자 세션 정보를 REDIS에 저장하여 빠르게 접근하고 관리할 수 있습니다.

- 메시징 큐: REDIS의 리스트와 펍/서브 기능을 활용하여 메시징 시스템이나 작업 큐를 구현할 수 있습니다.


---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### 자료 출처
- YC Tech Academy 6주차