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

Kotlin의 In-memory Cache에 대해 알아보고 Caffeine 라이브러리 살펴보기 
{:.lead}

- Table of Contents
{:toc .large-only}

## In-memory Cache

In-memory Cache는 Main 서버 DB를 거치지 않고 메모리에 저장해둔 데이터를 가져오는 것을 말합니다. 이때 실시간 데이터가 반영되진 않지만 데이터 전달 속도를 높일 수 있습니다.
변경이 적지만 자주 읽어들여야 하는 데이터나 슬로우 쿼리가 발생하는 데이터들은 캐시로 사용하기 적합합니다.

### Caffeine Library

Java 8 기반의 라이브러리인 Caffeine은 성능이 뛰어나고 캐시 hit-rate 또한 높습니다. Google Guava API와 유사하기도 합니다. *Spring Boot Cache starters*를 쓴다면
classpath에 있는`CaffeineCacheManager`를 읽어 auto-configured 해줍니다.

아래는 예시코드 입니다. 다음 시간엔 실제 DB에 데이터를 가져오는 것과 Cache를 사용하는 방법의 시간 차이를 확인해보겠습니다.


```kotlin
class CachedWebUserRepository(
    val userClient: UserClient
) : UserRepository {

    private val users = Caffeine.newBuilder()
        .expireAfterWrite(1, TimeUnit.MINUTES)
        .buildSuspending<Int, User>()

    override suspend fun getUser(id: Int): User =
        users.get(id) { userClient.fetchUser(id) }
}
```

---
참고: 
- [in-memory-caching-vs-in-memory-data-store](https://dev.to/anurag_vishwakarma/in-memory-caching-vs-in-memory-data-store-leh)
- [Effective-kotlin - Use Caching When Possible](https://kt.academy/article/ek-caching)
- [adding-some-caffeine-to-koltin-springboot](https://medium.com/backyard-programmers/adding-some-caffeine-to-koltin-springboot-2d9ea1f54e98)