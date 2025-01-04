---
title: RCity14 Write-Up
categories: [Playground, Red Raccoon]
tags: [Penetration, CTF, Write Up]
image:
  path: /assets/post/2025/Playground/Red Raccoon/redraccoon.png
  alt: Red Raccoon (https://www.redraccoon.kr/)
published: true
---

## 1. sudo 권한 확인

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbtjQu9%2FbtsLBhodGwT%2Fu7n5krf3Qyzv0J6mWKrfKk%2Fimg.png" width=1300>

```
# sudo -l
```

현재 사용자가 실행할 수 있는 sudo 명령 및 권한을 확인하는데 사용하는 명령어입니다.

사용자가 sudo 를 통해 어떤 명령을 실행할 수 있는지, 어떤 조건에서 실행 가능한지를 확인할 수 있습니다.

위 명령어를 입력하면 "rcity15" 계정으로 /usr/bin/find 바이너리를 패스워드 없이 사용할 수 있는 것을 알 수 있습니다.

그렇다면 find 명령어를 rcity15 계정의 권한으로 실행 가능한 것과 플래그를 구하는 것이 어떤 연관성이 있을까요?

문제에서 gtfobins 라는 힌트를 주었는데요 !

gtfobins 는 윈도우/리눅스 운영체제에 기본적으로 탑재되어 있는 바이너리, 스크립트, 그리고 라이브러리들이 어떻게 다양한 공격에 사용될 수 있는지를 모아놓은 프로젝트입니다.

[https://gtfobins.github.io/gtfobins/find/](https://gtfobins.github.io/gtfobins/find/)
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbEhrfl%2FbtsLBuOn7pN%2FrVNkevxtTOW6PDy0KX6ZU0%2Fimg.png" width=1300>

"gtfobins > find" 파트를 살펴보면 유저가 sudo 권한으로 find 바이너리를 실행할 수 있는 환경이 갖춰졌다면, find 명령 시 쉘을 함께 실행하여 다른 유저의 쉘로 진입하여 기능을 사용할 수 있다는 것을 확인할 수 있습니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcRKRbS%2FbtsLAyXZSJ8%2Fs6KLdFE4a5OU6q0AP5tGy1%2Fimg.png" width=1300>

[Step 1] 에서 find 명령어를 실행할 때, rcity15 로는 sudo 명령에 대한 패스워드 필요없이 find 명령어를 실행할 수 있는 것을 확인했습니다. 따라서, -u rcity15 옵션을 통해 sudo 권한으로 find 명령어를 실행하는 유저가 누구인지 명시해 주어야 합니다.

```
# sudo -u rcity15 find . -exec /bin/sh \; -quit 
```

위 명령어를 통해 rcity15 쉘로 진입에 성공한 이후에는 14번 문제(rcity14)에 대한 플래그를 탐색해야 합니다.

rcity15 쉘로 진입하기 전에 현재 위치(/home/rcity14)에서 명령어를 실행했으므로 rcity15 쉘에 진입한 이후에 홈디렉토리(/home/rcity15)로 이동하여 플래그 정보를 확인합니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccKlEe%2FbtsLAy4LW5w%2FC3A8bkiF7jZGHmv9Ok4wU1%2Fimg.png" width=1300>

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>