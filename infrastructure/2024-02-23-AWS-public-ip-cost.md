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
바로 network number(routing prefix)와 rest field(host identifier)입니다. routing prefix는 주소의 첫 번째 부분을 담당하며 *CIDR 표기법으로 작성됩니다.

> 192.50.100.0/24

이 IPv4 주소에서 24는 routing prefix 길이를 나타냅니다. IPv4 주소 한 자리는 bit 입니다. 그리고 `0` ~ `255`까지 숫자를 가지기 때문에 2의 8승인 8 bit를 점유합니다. 따라서 24는(8 * 3), 3번째 자리까지 routing prefix라고 알려주고 있습니다.
따라서 남은 한 자리가 할당될 수 있는 IPv4의 범위를 나타냅니다. `192.50.100.0/24` ~ `192.50.100.255`가 주소 범위인 것입니다.

만약 서로 routing prefix가 다른 두 서브넷이 통신한다면 라우터가 필요합니다. 라우터는 논리적, 물리적 경계를 만들어 줍니다.

subnet은 한정된 자원(IPv4 주소)를 경제적으로 사용하기 위한 방법입니다. 

### Network addresses and routing

IP 네트워크에 참여하는 기기들은 DHCP나 OS에 의해 네트워크 주소를 할당받습니다.
이 주소는 대상 라우팅(destination routing)에서 호스트를 식별하고 네트워크에서 찾는 일을 수행합니다. 

[읽어보기]
https://www.spiceworks.com/tech/networking/articles/what-is-subnet-mask/

https://en.wikipedia.org/wiki/Subnet

### *CIDR 

CIDR(Classless Inter-Domain Routing)은 IP 라우팅을 위해 IP 주소들을 할당하는 기법입니다.  
이름에서 알 수 있듯이 `클래스 없이` 라우팅 하는 기법입니다. 등장한 지는 30년이 넘었습니다.
생긴 목적은 라우터(데이터 패킷을 네트워크들끼리 분배해주는 장)의 라우팅 테이블의 증가속도를 줄이고, IPv4 고갈 속도 또한 늦추기 위함이었습니다.


