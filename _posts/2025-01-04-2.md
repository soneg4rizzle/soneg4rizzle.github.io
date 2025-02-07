---
title: APKToolkit Usage
categories: [Tools, APKToolkit]
tags: [Mobile App Hacking, Android]
image:
  path: /assets/post/2025/Tools/APKToolkit/APKToolkit.png
  alt: WinSCP
published: true
---

## 1. APK Toolkit

안드로이드 애플리케이션 진단을 하면 APK 파일을 디컴파일 해서 소스코드를 분석하거나, 리패키징을 통해 인증 구간을 우회하는 등 여러가지 시도를 하는데요. CLI(apktool.exe)로 번거롭게 명령어를 입력할 필요없이 GUI 환경에서 동일한 기능을 제공해주는 게 바로 APK Toolkit 입니다.

기본적인 구조는 아래 이미지와 같습니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdqkE6C%2FbtsLCIkVFie%2FgIfsx69IWY7LdBZiq5zTC0%2Fimg.png" alt="" width=1300>

사용법은 간단합니다.
우측의 파일 혹은 폴더 이미지를 클릭하신 후 원하는 APK 파일을 선택하시면 되는데요.
드래그 앤 드롭도 가능하기 때문에 편한 방법을 이용하시면 될 것 같습니다.

번거롭게 디컴파일 , 리빌드 , 서명 등을 위해 명령어를 입력할 필요 없이 화면에 나와있는 "Decompile, Compile, Extract, Zip, Zip Align, Check Align, Sign, Verify Signature" 버튼을 클릭하면 모든 작업이 끝나기 때문에 안드로이드 앱을 진단할 때 정말 유용하게 사용하고 있습니다.

각 버튼을 클릭하면 상단의 "Decompiled, Compiled, Extracted, ..." 폴더에 파일이 저장되기 때문에 해당 폴더에 접근하셔서 실행 결과를 확인하시면 됩니다.

예를 들어, 분석할 APK 파일을 선택하신 후 "Decompile" 버튼을 클릭하면 "Decompiled" 폴더에 디컴파일 된 APK 파일이 저장되고 코드(Smali) 수정 후 "Compile" 버튼을 클릭하면 "Compiled" 폴더에 변경된 내용이 적용된 새로운 APK 파일이 생성됩니다.

각 버튼의 기능에 대한 상세내용은 아래 표로 정리해 두었으니 참고해 주시면 좋을 것 같습니다 :)

| **버튼** | **기능** |
| --- | --- |
| **Decompile** | `APK 파일을 사람이 읽을 수 있는 형식(자바/스말리)으로 변환하여 애플리케이션 구조 분석 [apk -> dex(dlvik) -> smali -> .class -> .java]`  |
| **Compile** | `수정된 리소스 파일과 코드를 다시 APK 파일로 리패키징 [리패키징 후 서명(Signing) 작업 필요]` |
| **Extract** | `APK 파일 내부의 특정 리소스(img, audio, video, xml, ...) 추출` |
| **Zip** | `APK 파일 혹은 디렉토리를 ZIP 형식으로 압축 [APK는 본질적으로 ZIP 파일이기 때문에 디렉토리를 APK 파일로 변환할 때 사용]` |
| **Zip Align** | `APK 파일을 정렬(Algin)하여 안드로이드 시스템 내 메모리 효율 최적화` |
| **Check Align** | `APK 파일이 올바르게 Zip Align 되었는지 확인` |
| **Sign** | `APK 파일에 디지털 서명 추가 [안드로이드 시스템은 서명되지 않은 APK 파일을 설치할 수 없으므로 서명 반드시 필요]` |
| **Verify Signature** | `APK 파일의 서명 유효성을 검증 [APK 서명은 APK 파일의 무결성을 보장하기 때문에 이를 검증]` |

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>