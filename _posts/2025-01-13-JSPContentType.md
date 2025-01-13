---
title: JSP content-type configuration
categories: [Notes, Cheat Sheet]
tags: [Cheat Sheet, JSP Content-Type]
image:
  path: /assets/post/2025/Notes/Cheat Sheet/JSP.png
  alt: JSP Content-Type Configuration
published: true
---

## JSP Content-Type Configuration
웹 애플리케이션 취약점 진단을 수행할 때 반드시 확인해야 할 취약점으로 파일 업로드 취약점이 있습니다.
파일 업로드 취약점은 웹 서버에 악성 파일(웹 쉘)을 업로드하고 해당 경로로 접근하여 실행할 수 있어야 하는데요.
이를 위해서는 아래와 같은 조건이 필요합니다.

> 1. 서버 사이드 스크립트 확장자(PHP, JSP, ASP) 업로드가 가능해야 합니다.
> 2. 파일의 업로드 경로 및 파일명을 확인 가능해야 하며 해당 경로로 접근이 가능해야 합니다.
> 3. 업로드 파일 폴더에 파일 실행 권한이 설정되어 있어야 합니다.

파일이 업로드 되더라도 해당 파일이 실행되지 않는다면 삽입된 공격 페이로드/데이터로부터 안전하다고 볼 수 있습니다.
때문에 일반적으로 웹서버/WAS에서 자체적인 설정을 통해 파일을 텍스트 타입으로 인식하도록 해서 공격자가 삽입한 공격 구문이 실행될 수 없도록 하고 있습니다.

```apache
[Apache - .httaccess]
AddType text/plain .html .js .css .txt .xml

[Apache - httpd.conf]
<Directory "/path/to/your/folder">
    AddType text/plain .html .js .css .txt .xml
</Directory>
```

---

```nginx
[Nginx - nginx.conf]
server {
    location /your-folder/ {
        types {
            text/plain html js css txt xml;
        }
    }
}
```

---

```apache
[Apache Tomcat - web.xml]
<mime-mapping>
    <extension>html</extension>
    <mime-type>text/plain</mime-type>
</mime-mapping>
<mime-mapping>
    <extension>js</extension>
    <mime-type>text/plain</mime-type>
</mime-mapping>
```

스프링 환경으로 구축된 웹 사이트에 JSP 확장자를 업로드하고 해당 경로에 접근해보면 응답 패킷에서 Content-Type 이 text/plain 으로 설정된 경우가 있는데요. 이는 웹 서버에서 자체적으로 해당 파일을 텍스트(평문)으로 인식해서 실행될 수 없도록 처리하기 때문일 가능성이 있습니다.

이를 우회하기 위해서 파일 업로드 시 아래와 같은 코드를 추가한 후 스크립트를 작성해주면
JSP 파일 내의 서버 쪽 스크립트는 실행되지 않더라도, DOM 내부에 삽입한 스크립트 구문을 실행시킬 수 있습니다.

```jsp
<%@ page contentType="text/html; charset=UTF-8"; pageEncoding="UTF-8" %>
<% out.println("convert content-type plain to html") %>
<scripT>alert(1)</script>
```

SOP(Same Origin Policy) 정책이 적용되어서 CSRF(Cross-Site Request Forgery)나 XSS(Cross-Site Scripting)와 같은 연계공격이 불가능한 경우에 웹 서버의 파일이 클라이언트에게 전달될 때 사용하는 MIME 타입을 "text/html"로 설정한다면 다른 출처의 스크립트를 사용하는 것이 아니라 자신의 웹 서버에 저장된 파일을 불러와 스크립트를 실행하는 것이기 때문에 잘 활용한다면 또 다른 연계공격도 가능할 것 같습니다.

---

## Reference
1. https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F
2. https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-CORS-%EB%B3%B4%EC%95%88-%EC%B7%A8%EC%95%BD%EC%A0%90-%EC%98%88%EB%B0%A9-%EA%B0%80%EC%9D%B4%EB%93%9C
3. https://h0-0cat.tistory.com/entry/6%EC%9B%947%EC%9D%BC-%EC%98%A4%EC%A0%84-%EC%8B%9C

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>