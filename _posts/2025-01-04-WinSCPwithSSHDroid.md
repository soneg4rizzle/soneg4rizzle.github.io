---
title: WinSCP with SSHDroid
categories: [Tools, WinSCP·SSHDroid]
tags: [Mobile App Hacking, SFTP]
image:
  path: /assets/post/2025/Tools/WinSCP/WinSCP.jpg
  alt: WinSCP
published: true
---

안녕하세요. 안드로이드 혹은 iOS 애플리케이션 취약점 진단을 수행하다 보면 단말기에 저장된 파일에 접근하여 내용을 확인하거나 변조된 내용을 적용하여 테스트를 진행해야 할 때가 있습니다.

이럴 때 유용하게 사용 가능한 것이 WinSCP + SSHDroid 의 조합입니다.

WinSCP 는 윈도우즈 운영체제에서 SFTP(SSH + FTP) 기능을 제공해주는 프로그램인데요!

일반적으로 FTP는 데이터를 평문으로 전송하기 때문에 해당 정보가 네트워크에 노출될 위험이 있습니다.

WinSCP는 SSH(Secure Shell)를 통한 암호화 통신으로 리눅스 서버와 안전하게 파일을 주고받을 수 있는 장점이 있기 때문에 많이들 사용하고 계신 것 같습니다.

각설하고, 사용방법에 대해 소개해 드리도록 하겠습니다.

---

## **1\. WinSCP 및 SSHDroid 설치**

[https://apkpure.com/kr/sshdroid/berserker.android.apps.sshdroid](https://apkpure.com/kr/sshdroid/berserker.android.apps.sshdroid)

[https://winscp.net/eng/downloads.php](https://winscp.net/eng/downloads.php)

---

## **2\. WinSCP + SSHDroid 사용방법**

### 2-1. SSHDroid - Activate SSH Server

먼저, SSHDroid APP을 실행하신 뒤에 상단의 "START" 버튼을 눌러주세요.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWfxqX%2FbtsLaJyXhtn%2FHFoQvhAxMRiaF86OxkKs91%2Fimg.png" alt="" width=1300>

SSHDroid 애플리케이션을 확인해 보면 SSH 접속이 가능한 [사용자 계정]@[원격지 IP] 활성화 되었습니다.
추가로 안드로이드 단말에서 # netstat -antp 명령어를 통해 SFTP(22) 포트 활성화된 것을 확인 가능합니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc9EidS%2FbtsLay5weTW%2F2tdgMwnaRzZTRPCOiA22IK%2Fimg.png" alt="" width=1300>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8y1Ho%2FbtsLbwZWCMK%2FuPqVQCQaSKG3btQkwTg410%2Fimg.png" alt="" width=1300>

### 2-2. WinSCP - Connect to SSH Server

WinSCP를 실행시킨 후 활성화된 SFTP 서버로 접속을 시도합니다.

```
# [사용자 계정]@[원격지 호스트 IP]
# SSHDroid는 사용자/패스워드 초기 설정 값으로 (root/admin) 을 사용합니다.
```
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbasAiO%2FbtsLaK5I94u%2FEP1f9uySVdnOIVit3fMl7k%2Fimg.png" alt="" width=1300>

### 2-3. WinSCP - Connection is Completed
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fenjqyq%2FbtsK9YDEh4p%2FEjZ9YA5EtgQWXKrxkCTfV1%2Fimg.png" alt="" width=1300>

정상적으로 SFTP 서버에 연결되었으며, 윈도우즈와 리눅스 운영체제 간 파일 공유가 가능합니다.
/data/data/{application_package}/shared_prefs 경로의 파일을 조회 · 수정 · 삭제하며 진단에 활용할 수 있습니다. 애플리케이션 이용 시 고정된 사용자 식별 값을 사용하고 있는 경우에 이 값을 다른 유저의 값으로 변조해서 권한 탈취 및 인증 우회와 같은 테스트를 해볼 수 있을 것 같습니다. 사용방법은 무궁무진 할 것 같네요.

---

## **3\. Reference**
[https://hackcatml.tistory.com/42](https://hackcatml.tistory.com/42)

[https://medium.com/@leandro.almeida/winscp-3ca451581293](https://medium.com/@leandro.almeida/winscp-3ca451581293)

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>