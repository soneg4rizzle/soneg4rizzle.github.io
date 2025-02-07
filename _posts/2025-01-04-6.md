---
title: File Download Vulnerability
categories: [System Security Vulnerability, Web Application]
tags: [Web Hacking, File Download]
image:
  path: /assets/post/2025/SSV/Web/File Download.jpg
  alt: File Download Vulnerability
published: true
---

## **1\. File Download Vulnerability**

파일 다운로드 취약점은 웹 사이트에 존재하는 파일 다운로드 기능을 이용하여 내부 시스템(ex; WAS)에 저장되어 있는 중요 파일을 다운로드 할 수 있는 취약점입니다.

공격자는 웹 페이지에서 파일 다운로드 요청 시 사용되는 파일명·파일경로 파라미터 값을 조작하여 웹 서버의 특정 경로에 저장된 파일을 다운로드 후 확인할 수 있습니다.

**(취약점 발생조건)**
> 1. 파일 다운로드 시 절대경로/상대경로와 함께 파일명 노출이 노출되는 경우  
> 2. "../" 와 같은 디렉토리 순회 명령어를 통해 웹 루트 상위 디렉토리에 접근 가능한 경우  
> 3. 다운로드 경로가 노출되지 않더라도 파라미터 변조를 통해 특정 파일에 접근 가능한 경우

<br>

**(취약점 예상피해)**
> 파일 다운로드 취약점으로 인해 "/etc/passwd", "/etc/hosts" 등의 중요 파일이 유출되어 2차 피해로 이어질 수 있습니다. 또한, 웹 루트 내에 존재하는 소스파일 혹은 데이터베이스 설정 파일을 다운로드하여 웹 서비스 구조를 분석하고 다양한 공격이 발생하여 추가적인 피해를 입을 수 있습니다.

<br>

**(취약점 진단방법)**
> 업로드 파일 경로를 확인 후 해당 파일의 경로가 노출되었는지 확인하고 파일 다운로드 시 사용되는 파라미터를 조작하거나 "../" 를 통해 다른 파일에 접근 가능한지 여부를 점검해야 합니다.

---

## **2. File Download Defensive Techniques**

파일명과 경로명을 데이터베이스에서 관리하고, 다운로드 시 허용된 경로 이외의 디렉토리와 파일에 접근할 수 없도록 구현하는 것이 바람직합니다. 또한, 웹 투르 폴더 상위로 접근할 수 없도록 권한을 설정하고 경로 관련 문자열("../")을 통해 디렉토리를 순회하지 못하도록 입력 값을 필터링 하는 것이 필요합니다.

### **2-1. 파일 다운로드 - 취약한 소스코드 우회 공격**

> [그림 1] 파일 다운로드 취약한 소스코드 1
{: .prompt-danger}
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdrbA5g%2FbtsK5PTworB%2FCbQkyS3FpyzMHdgBUvFYh0%2Fimg.png" alt="" width=1300>

[그림 1] 은  파일 다운로드 시 사용되는 파라미터 값을 변조하여 웹 서버에 저장된 시스템 중요 파일을 다운로드 하는 과정의 일부입니다. 파일 다운로드 시 파일명과 파일경로에 대한 정보가 노출된다면 공격자는 해당 파라미터를 조작하여 웹 서버에 저장된 중요 정보를 탈취할 수 있게되고, 공격자는 웹 페이지 소스코드나 시스템 중요 파일 정보를 획득하고 2차 공격에 활용할 수 있습니다. 따라서, 보안 점검자는 이를 점검하고 반드시 보안 대책을 적용해야 합니다.

---

## **3. Bypass Defensive Techniques**
<img src="/assets/post/2025/Others/Bypass File Upload Filtering Techniques.png" alt="" width=1300>

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>