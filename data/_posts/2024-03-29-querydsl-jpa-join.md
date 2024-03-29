---
layout: post
title: Querydsl - 조인
image: 
  path: /assets/img/blog/querydsl.webp
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

Querydsl에서 join 사용하는 방법을 복습하고자 정리해보았습니다. 김영한님 책에 있는 내용을 그대로 발췌하였습니다.
{:.lead}

- Table of Contents
{:toc .large-only}


## 조인

innerJoin(join), leftJoin, rightJoin, fullJoin을 사용할 수 있고 추가로 JPQL의 on과 성능 최적화를 위한(n+1을 방지할 수 있는) fetch 조인도 사용할
수 있습니다. 조인의 기본 문법은 첫 번째 파라미터에 조인 대상을 지정하고, 두 번째 파라미터에 alias로 사용할 쿼리 타입을 지정하면 됩니다.

`join(조인 대상, 별칭으로 사용할 쿼리 타입)`

```kotlin
// 기본 조인
val order = QOrder.order
val member = QMember.member
val orderItem = QOrderItem.orderItem

query.from(order)
    .join(order.member, member)
    .leftJoin(order.orderItems, orderItem)
    .list(order)

// 조인 on 사용
query.from(order)
    .leftJoin(order.orderItem, orderItem)
    .on(orderItem.count.gt(2))
    .list(order)

// 페치 조인은 join() 뒤에 fetch()를 붙이면 되기 때문에 생략함
```

---
참고: 
- [자바 ORM 표준 JPA 프로그래밍 - 김영한](https://product.kyobobook.co.kr/detail/S000000935744)