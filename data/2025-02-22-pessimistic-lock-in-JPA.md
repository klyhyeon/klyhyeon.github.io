---
layout: post
title: JPA Pessimistic Lock(비관적락)에 대해 알아보기
image: 
  path: /assets/img/blog/jpa.svg
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

JPA Pessimistic Lock(비관적락)에 대해 알아보기 
{:.lead}

- Table of Contents
{:toc .large-only}

## JPA Pessimistic Lock

동시성 문제를 해결하기 위해 DB Lock을 걸 상황이 생기곤 합니다. Lock은 크게 낙관적/비관적으로 구분되는데 이번엔 비교적 엄격한
비관적 Lock을 사용하고 그에 대해 알아봤습니다. 상황은 동시에 호출된 N개의 요청중 1개의 데이터행만 존재하고 나머지 호출은 데이터행이
존재할때 데이터행을 만들지 않아야 합니다. 하지만 동시에 호출되기 때문에 조회하는 순간 데이터행이 존재하지 않기 때문에 N개의 데이터행이 생겨버립니다.

이를 방지하기 위해 우리는 동시에 조회가 이루어질 수 없도록 해당 DB 테이블을 점유하도록 비관적 Lock을 걸었습니다. 해당 쿼리가 발생되는 메서드에 `@Transaction`을
걸었기 때문에 Lock이 메서드가 끝날때까지 걸려있는 것은 쿼리 로그를 통해 알 수 있습니다. **(로그 공유!!)** 

---
참고: 
- [jpa-pessimistic-locking](https://www.baeldung.com/jpa-pessimistic-locking)