---
layout: post
title: AWS 서브넷(Subnet) & CIDR(Classless Inter-Domain Routing)
image: 
  path: 
#description: >
#  Version 9 is the most complete version of Hydejack yet.
#  Modernized design, big headlines, and big new features.
sitemap: false
---

AWS private 서브넷 생성하기
{:.lead}

AWS는 2024년 2월부터 public IP에 대해 [과금](https://aws.amazon.com/ko/blogs/aws/new-aws-public-ipv4-address-charge-public-ip-insights/)을 시작했습니다.
시간 당 $0.005이 발생하기 때문에 만약 100개의 `public IP`를 24시간 사용한다면 하루에 `$12`가 청구됩니다.
과금폭탄을 피하기 위해 private IP로 전환하는 과정에서 private 서브넷을 생성해보며 알게된 개념과 CIDR(Classless Inter-Domain Routing)을 기술해보겠습니다.

- Table of Contents
{:toc .large-only}

### Subnet

IP의 논리적 분배입니다. 보통 네트워크를 두 개 이상의 네트워크로 나눌 때 Subnetting 한다고 부릅니다.
동일한 서브넷에 있는 기기는 IP 주소에서 고유의 identical group을 가집니다. 이로써 IP는 두 가지 부분으로 나눌 수 있는데요.
바로 network number(routing prefix)와 rest field(host identifier)입니다. routing prefix는 주소의 첫 번째 부분을 담당하며 CIDR 표기법으로 작성됩니다.


https://en.wikipedia.org/wiki/Subnet

### CIDR 

CIDR(Classless Inter-Domain Routing)은 IP 라우팅을 위해 IP 주소들을 할당하는 기법입니다.  
이름에서 알 수 있듯이 `클래스 없이` 라우팅 하는 기법입니다. 등장한 지는 30년이 넘었습니다.
생긴 목적은 라우터(데이터 패킷을 네트워크들끼리 분배해주는 장)의 라우팅 테이블의 증가속도를 줄이고, IPv4 고갈 속도 또한 늦추기 위함이었습니다.


