---
title: VirtualBox(Kali Linux) Preset
categories: [Notes, VirtualBox]
tags: [Drag and Drop]
image:
  path: /assets/post/2025/Notes/VirtualBox/virtualbox.jpg
  alt: SQL Cheat Sheet Notes
published: true
---

## Virtualbox Drag & Drop
버추얼박스 최초 설치 후에는 로컬 PC와 버추얼박스(하이퍼바이저) 간의 파일 이동이 불가능합니다.
이를 가능하게 하기 위해서는 버추얼박스 확장팩(Virtualbox Extension Pack) 설치가 필요한데요.

설치 및 설정 방법은 아래와 같습니다.

> Download URL : https://www.virtualbox.org/wiki/Downloads
{: .prompt-info}

### 1. 버추얼박스 공식 홈페이지 접속
<img src="/assets/post/2025/Notes/VirtualBox/1.png" alt="" width=1300>

### 2. 다운로드 파일 실행
<img src="/assets/post/2025/Notes/VirtualBox/2.png" alt="" width=1300>
<img src="/assets/post/2025/Notes/VirtualBox/3.png" alt="" width=1300>
필자는 이미 설치했기 때문에 사진에 "Reinstall"로 표시되지만 최초 설치인 경우에는 "Install"로 표시됩니다.

이후 나오는 화면은 모두 다음~다음~ 하셔서 설치 진행해 주시면 됩니다.

### 3. 로컬 PC에 저장된 파일 복사
<img src="/assets/post/2025/Notes/VirtualBox/4.png" alt="" width=1300>
<img src="/assets/post/2025/Notes/VirtualBox/5.png" alt="" width=1300>

확장팩 설치 후에는 로컬PC에서 칼리리눅스로 파일 이동이 가능합니다.
하지만 칼리리눅스에서 로컬PC로의 파일 이동은 불가능한데요.
이러한 경우에는 버추얼박스에서 제공하는 공유폴더 기능을 통해 하이퍼바이저와 외부(로컬PC)의 파일을 공유할 수 있습니다.

### 4. 공유 폴더 설정
아래와 같이 버추얼박스의 공유 폴더 설정을 통해 로컬PC-버추얼박스 간의 파일 공유가 가능합니다.
<img src="/assets/post/2025/Notes/VirtualBox/6.png" alt="" width=1300>
<img src="/assets/post/2025/Notes/VirtualBox/7.png" alt="" width=1300>
> Reference - https://mpjamong.tistory.com/113
{: .prompt-info}
---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>