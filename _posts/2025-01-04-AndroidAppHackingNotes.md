---
title: Android App Hacking Preset
categories: [System Security Vulnerability, Mobile Application]
tags: [Notes, Android App Hacking]
image:
  path: /assets/post/2025/SSV/Android/thumb.jpg
  alt: Android App Hacking Notes
published: true
---

# Android App Hacking Tips

---

## APKTool Commands
> APKTool를 활용한 APK 디컴파일·리빌드·키 생성·서명 방법
{: .prompt-tip}
```
Decompile
  ㄴ apktool d [ex.apk]

Rebuild
  ㄴ apktool b [decompiled_apk_foldername] -o {ex.apk}  
   
Generate Key
  ㄴ keytool -genkeypair -v -keystore {ex.keystore} -alias {ex} -keyalg RSA -keysize 4096 -validity 10000  

Sign
  ㄴ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore {ex.keystore} {rebuilded.apk} {ex}  
```

---

## ADB(Android Debug Bridge) Commands
> 안드로이드 애플리케이션 진단 시 유용한 ADB 명령어
{: .prompt-tip}
```
(PC -> Android Device) SHELL 권한 획득
  ㄴ adb shell

Install APK to device
  ㄴ adb install {ex.apk}

(PC -> Android Device) 파일 이동
adb push {fileName} {filePath}
  ㄴ ex; adb push ./1.png /data/local/tmp

(Android Device -> PC) 파일 이동
adb pull {filePath}
  ㄴ ex; adb pull /data/local/tmp/1.png
```

---

## Android Device Globval Proxy Settings
> 안드로이드 디바이스 전역 프록시 값 설정
{: .prompt-tip}
```
프록시 수동 설정 등록
  ㄴ adb shell settings put global http_proxy {IP:PORT}

프록시 수동 설정 해제
  ㄴ adb shell settings put global http_proxy :0
```

---

## Android OS Version check
> 안드로이드 OS 버전 확인
{: .prompt-tip}
```
adb shell getprop ro.product.cpu.abi
```

---