---
layout: post
title: Kotlin In-memory Cache & Caffeine
image: 
  path: /assets/img/blog/cache.webp
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

JPA Pessimistic Lock(비관적락)에 대해 알아보기 
{:.lead}

- Table of Contents
{:toc .large-only}

## In-memory Cache

동시성 문제를 해결하기 위해 DB Lock을 걸 상황이 생기곤 합니다. Lock은 크게 낙관적/비관적으로 구분되는데 이번엔 비교적 엄격한
비관적 Lock을 사용하고 그에 대해 알아봤습니다. 상황은 동시에 호출된 N개의 요청중 1개의 데이터행만 존재하고 나머지 호출은 데이터행이
존재할때 데이터행을 만들지 않아야 합니다. 하지만 동시에 호출되기 때문에 조회하는 순간 데이터행이 존재하지 않기 때문에 N개의 데이터행이 생겨버립니다.

이를 방지하기 위해 우리는 동시에 조회가 이루어질 수 없도록 해당 DB 테이블을 점유하도록 비관적 Lock을 걸었습니다. 지금 다시 생각해보니
Transaction 전체에 Lock을 걸거나 저장되는 순간에도 Lock을 걸어야 되지않나 싶은데요, 동시에 일어나다보니 조회시에 걸린 Lock이 끝나고
약간의 시간이 생길때 저장이 이루어지고 다음 요청이 조회할땐 이미 데이터가 생기게 된 것 같습니다. 쿼리 로그를 찍어보면 제일 잘 알 수 있을듯 합니다.

[//]: # (### Caffeine Library)

[//]: # ()
[//]: # (Java 8 기반의 라이브러리인 Caffeine은 성능이 뛰어나고 캐시 hit-rate 또한 높습니다. Google Guava API와 유사하기도 합니다. *Spring Boot Cache starters*를 쓴다면)

[//]: # (classpath에 있는`CaffeineCacheManager`를 읽어 auto-configured 해줍니다.)

[//]: # ()
[//]: # (아래는 예시코드 입니다. 다음 시간엔 실제 DB에 데이터를 가져오는 것과 Cache를 사용하는 방법의 시간 차이를 확인해보겠습니다.)

[//]: # ()
[//]: # ()
[//]: # (```kotlin)

[//]: # (class CachedWebUserRepository&#40;)

[//]: # (    val userClient: UserClient)

[//]: # (&#41; : UserRepository {)

[//]: # ()
[//]: # (    private val users = Caffeine.newBuilder&#40;&#41;)

[//]: # (        .expireAfterWrite&#40;1, TimeUnit.MINUTES&#41;)

[//]: # (        .buildSuspending<Int, User>&#40;&#41;)

[//]: # ()
[//]: # (    override suspend fun getUser&#40;id: Int&#41;: User =)

[//]: # (        users.get&#40;id&#41; { userClient.fetchUser&#40;id&#41; })

[//]: # (})

[//]: # (```)

---
참고: 
- [jpa-pessimistic-locking](https://www.baeldung.com/jpa-pessimistic-locking)