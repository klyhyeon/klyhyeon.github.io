---
layout: post
title: Querydsl - 프로젝션
image: 
  path: /assets/img/blog/querydsl.webp
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

Querydsl에서 프로젝션을 사용해 쿼리 결과를 DTO로 받는 방법을 공부하고자 정리해보았습니다. 김영한님 책에 있는 내용을 그대로 발췌하였습니다.
{:.lead}

- Table of Contents
{:toc .large-only}


## 프로젝션

select 절에 조회 대상을 지정하는 것을 프로젝션이라 합니다.

### 빈 생성

쿼리 결과를 엔티티가 아닌 특정 개겣로 받고 싶으면 빈 생성(Bean population) 기능을 사용합니다.
QueryDSL은 객체를 생성하는 다양한 방법을 제공합니다.

- 프로퍼티 접근
- 필드 직접 접근
- 생성자 사용

`join(조인 대상, 별칭으로 사용할 쿼리 타입)`

```kotlin
class ItemDTO {

    var username: String
    var price: Int
 
    constructor(username: String, price: Int) {
        this.username = username
        this.price = price
    }
    
    constructor()
}

// 프로퍼티 접근(Setter)
val item = QItem.item
val resultByBean = query.from(item)
    .list(
        Projections.bean(
            ItemDTO::class.java,
            item.name.`as`("username"),
            item.price,
        )
    )

// 필드 직접 접근: 필드에 직접 접근해서 값을 채워줌, 필드를 private으로 설정해도 동작함
val resultByField = query.from(item)
    .list(
        Projections.fields(
            ItemDTO::class.java,
            item.name.`as`("username"),
            item.price,
        )
    )

// 생성자 사용: 생성자를 사용해 값을 채워줌. 프로젝션과 파라미터 순서가 같은 생성자가 필요함
val resultByConstructor = query.from(item)
    .list(
        Projections.constructor(
            ItemDTO::class.java,
            item.name,
            item.price,
        )
    )
```

---
참고: 
- [자바 ORM 표준 JPA 프로그래밍 - 김영한](https://product.kyobobook.co.kr/detail/S000000935744)