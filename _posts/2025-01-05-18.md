---
title: How URLs work
categories: [CS, Network]
tags: [Web, URL, DNS, TCP/IP, HTTP]
image:
  path: /assets/post/2025/CS/Web/web concept.gif
  alt: The Journey of URL
published: true
---

안녕하세요. 오늘은 URL이 어떻게 동작하는지 원리에 대해서 알아보도록 하겠습니다.
URL의 동작원리에 대해 공부하던 중 이와 관련된 내용이 잘 정리된 게시글이 있는 것 같아 이 글을 기반으로 공부한 내용을 공유하도록 하겠습니다.

---

<img src="/assets/post/2025/CS/Web/web concept.gif" alt="" width=1300>

## **1. What is IP Address ?**
우리는 현대사회에서 컴퓨터나 휴대폰을 통해 언제 어디서든 원하는 정보를 획득할 수 있습니다. 이를 위해서는 외부 네트워크와 연결되어 통신할 수 있는 기기가 필요한데요.
이를 일반적으로 호스트(Host)라고 말합니다.

호스트 간의 통신을 위해서는 이 호스트가 누구인지 확인할 수 있는 식별자가 필요한데, 우리는 이 식별자를 IP(Internet Protocol) 주소라고 합니다.

다른 사람과 통화할 때 우리는 그 사람이 누구인지 알고 있어야 더 편하게 대화를 할 수 있겠죠. 누구인지도 모르는 사람과 대화하는 건 아무도 좋아하지 않을테니까요.
그런 맥락에서 IP 주소는 네트워크 통신에 있어서 없어서는 안 될 존재입니다.

### 1-1. Public/Private IP, NAT
IP 주소는 공인IP(Public IP), 사설IP(Private IP)로 나뉘는데
공인IP는 주민등록번호와 같이 전세계에서 유일하게 자신을 식별할 수 있는 IP인데요.
IP 주소는 무한한 것이 아니라 사용할 수 있는 대역이 한정되어 있습니다.
그것이 바로 IPv4(32bit), IPv6(128bit) 입니다.

```
IPv4 (32bit) - ********.********.********.********
IPv6 (128bit) - ********.********.********.******** x4
```

공인 IP는 주민등록번호와 같이 전세계에서 유일하게 식별되는 IP주소를 말합니다.
우리는 공인IP 주소를 사용하기 위해서 ISP(인터넷 서비스 제공자)로부터 사용 가능한 IP를 할당받아 사용해야 합니다.
하지만, 공인IP 주소는 앞서 말씀드린 것처럼 사용할 수 있는 주소가 제한되어 있기 때문에 누군가는 IP 주소를 할당받지 못할 수 있었고 한정된 자원으로 인해 비용 문제 또한 존재했습니다. 이러한 문제를 완화시키고자 사설IP와 NAT라는 개념이 등장했습니다.

<img src="/assets/post/2025/Others/NAT.jpg" alt="" width=1300>

간단히 요약하면, 외부 네트워크와 연결되는 출입구는 하나의 공인IP를 할당하여 통신하고 이를 기준으로 내부 네트워크는 임의의 IP를 할당해서 사용하도록 하는 것입니다.
내부 네트워크끼리 통신할 때는 동일한 네트워크(IP) 대역에서 사설 IP끼리 통신하도록 하고 외부 네트워크와 통신이 필요할 때는 NAT(Network Address Translation) 기술을 통해 외부와도 통신할 수 있도록 한 것이죠.


## **2. DNS**
DNS(Domain Name Server)는 도메인(google.com) 주소를 IP주소로 변환해주는 7계층(애플리케이션) 프로토콜 중 하나입니다.
앞서 모든 호스트들은 IP주소를 기반으로 상대방을 식별하고 통신을 진행할 수 있다고 말씀드렸는데요. IP주소는 숫자로만 구성되어 있기 떄문에 사용자가 IP주소만 보고는 이게 어떤 사이트인지 구별하기 쉽지 않습니다. DNS는 사용자의 편의성을 위해 만들어진 프로토콜로 도메인 주소로 접근 시 자동으로 도메인에 할당된 IP주소를 찾아 변환해준 후 해당 IP주소로 접근할 수 있도록 도와주는 기능을 제공해 주고 있습니다.

DNS는 도메인에 맵핑되는 IP주소를 찾기 위해 재귀적 질의(Recursive Querying), 반복적 질의(Iterative Querying) 작업을 수행합니다.

> DNS 구성요소
{: .prompt-info}

```
1. Recursive/Cache DNS Server
    ㄴ 사용자에게 도메인 질의 받으면 자신의 캐시에 저장된 정보확인(Domain<->IP)
    ㄴ 캐시에 저장된 정보가 없다면 Authoritative DNS Server 에게 질의

2. Authoritative DNS Server
    ㄴ 관리하는 도메인이 있어 해당 도메인에 대한 질의에만 응답하는 서버
        ㄴ Zone : 관리하는 도메인 영역
        ㄴ Zone File : 관리하는 도메인 정보가 저장된 파일

3. Query
    ㄴ Recursive Query
        ㄴ 사용자가 Recursive DNS Server 에 질의할 때 사용
        ㄴ 질의한 도메인의 레코드 정보 조회 후 응답을 요청
        ㄴ RDNS Server 서버는 자신의 캐시에 저장된 정보 없다면 Iterative Query 전송

    ㄴ Iterative Query
        ㄴ Recursive DNS Server 가 각 Authoritative DNS Server 로 질의할 때 사용
```

사용자가 URL 주소창에 특정 도메인을 입력하면 먼저 Recursive DNS Server 에 요청 도메인에 대한 IP 정보가 저장되어 있는지 캐시를 확인합니다. 이건 운영체제 별 동작원리가 상이합니다.

```
Windows
    ㄴ "Local DNS Cache" 확인 > hosts.ics 확인 > hosts 파일 확인
        ㄴ 위 과정에서 발견된 정보 없으면 Authoritative DNS Server 에게 질의

Linux
    ㄴ hosts 파일 확인
        ㄴ 위 과정에서 발견된 정보 없으면 Authoritative DNS Server 에게 질의
            ㄴ /etc/resolv.conf : 시스템 기본 네임서버 설정파일
            ㄴ /etc/host.conf : 도메인 질의 순서가 정의된 파일
            ㄴ /etc/hosts : 도메인과 IP 주소 맵핑정보가 저장된 파일
```

> 사용자에게 DNS 질의를 받은 Recursive DNS 서버는 해당 도메인 정보가 자신의 캐시가 저장되어 있다면 사용자에게 응답을 전달하며 저장된 정보가 없으면 Authoritative DNS 서버에게 루트 도메인부터 TLD(Top-level domain), SLD(Second-level domain), TLD(Third-level domain)까지 순차적으로 질의하며 해당 도메인에 대한 IP주소를 찾아 사용자에게 응답
{: .prompt-info}

<img src="/assets/post/2025/Others/Domain Type.png" alt="" width=1300>


## **3. TCP/IP**
프로토콜의 종류는 여러가지가 있겠지만 웹 사이트의 HTTP 통신의 경우에는 TCP를 기반으로 합니다.

클라이언트와 서버가 통신하기 위해서는 먼저 TCP Connection(3-Way HandShaking) 작업이 선행되어야 합니다. 이 작업은 아래의 순서로 진행됩니다.

```
1. 클라이언트(A)가 서버(B)에게 SYN 패킷 전송
    ㄴ A : B야 나 너랑 대화하고 싶어
2. 서버(B)는 클라이언트(A)에게 SYN+ACK 패킷 전송
    ㄴ B : 그래, 나도 너랑 대화할게. 연락처 보냈으니까 이쪽(포트)으로 연락줘
3. 클라이언트(A)는 서버(B)에게 ACK 패킷 전송
    ㄴ A : 답장 잘 받았어. 이제부터 할 말 있으면 너가 준 연락처로 연락할게
```

3-Way HandShake 과정이 정상적으로 완료되면, 클라이언트는 이제 서버에게 데이터를 전송할 수 있는데요. 클라이언트/서버 간의 데이터를 요청하고 응답하는 과정을 HTTP Request, HTTP Response 라고 말합니다.

## **4. HTTP Request/Response**
클라이언트는 RFC 7231(HTTP)에 정의된 HTTP Method 를 사용하여 서버에게 요청을 보낼 수 있습니다. 메소드로는 (GET, POST, OPTIONS, ...) 등이 있습니다. 서버는 클라이언트의 요청에 따라 "1xx, 2xx, 3xx, 4xx, 5xx"와 같은 HTTP 상태코드와 함께 HTTP Request 에 따른 응답 값을 반환해 줍니다.

서버도 웹서버(Apache, Nginx, ...)와 웹애플리케이션서버(WAS; Tomcat, WebLogic, WebSphere, ...)로 나뉘는데 간단히 구분하자면 웹서버는 "HTML, CSS, JS, 이미지" 등의 정적인 데이터를 제공하는 서버이고 WAS는 동적인 컨텐츠(Server side script, Database data)들을 처리하고 제공하는 서버로서 클라이언트와 웹 서버 간의 중계역으로 웹 애플리케이션의 실행 및 데이터 처리를 담당합니다.

생략된 부분이 정말 많지만 URL 동작 원리에 대해서 아주 간략...하게 알아보았습니다.
공부를 하면 할수록 네트워크 쪽은 정말 재밌는 것 같네요. 도움이 되셨으면 좋겠습니다.
---

### 5. Reference
1. [The Journey of URL](https://www.linkedin.com/feed/update/urn:li:activity:7104787767296942081/)
2. https://devjin-blog.com/what-happen-browser-search/
3. https://onlinecomputertips.com/support-categories/networking/601-network-address-translation-nat/

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>