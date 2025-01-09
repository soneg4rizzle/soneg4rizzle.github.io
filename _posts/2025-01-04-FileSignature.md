---
title: File Extension, Signature·Magic Number
categories: [Notes, Cheat Sheet]
tags: [Cheat Sheet, File Signature]
image:
  path: /assets/post/2025/Notes/Cheat Sheet/File Extension.jpg
  alt: File Extension Signature, Content-Type
published: true
---

## 파일 확장자 시그니처(File Extension Signature)
파일 시그니처 혹은 파일 매직 넘버라고 하는 이 값은 해당 파일이 어떤 파일인지를 인식하도록 하는 식별자입니다.
파일 업로드 시 HEX 값을 살펴보면 헤더와 푸터 부분에 아래와 같은 식별 값이 존재하는 것을 확인할 수 있는데요.
만약 파일 업로드 시 화이트 리스트 필터링 방식이 아니라 Content 값으로 서버 측에서 검증을 하고 있다면
파일을 식별하는 시그니처의 헤더 및 푸터 부분을 허용된 파일 확장자로 변조하여 우회할 수 있습니다.
식별 값을 변조하기 위해서는 `MITM(Burp Suite, Fiddler)` 혹은 `HxD` 툴을 사용할 수 있습니다.

### 파일 확장자 별 시그니처(Header, Footer)

| File Type | Header Signature (Hex) | Footer Signature (Hex) |
| --- | --- | --- |
| `JPEG` | `FF D8 FF E0 FF D8 FF E8` | `FF D9` |
| `GIF` | `47 49 46 38 37 61 47 49 46 38 39 61` | `00 3B` |
| `PNG` | `89 50 4E 47 0D 0A 1A 0A` | `49 45 4E 44 AE 42 60 82` |
| `PDF` | `25 50 44 46 2D 31 2E` | `25 25 45 4F 46` |
| `ZIP` | `50 4B 03 04` | `50 4B 05 06` |
| `ALX` | `41 4C 5A 01` | `43 4C 5A 02` |
| `RAR` | `52 61 72 21 1A 07` | `3D 7B 00 40 07 00` |

---

## 파일 확장자 및 컨텐트 타입(File Extension, Content-Type)
파일 업로드 시 Content-Type 으로 검증하고 있다면 요청 패킷의 Content-Type 을 허용된 값으로 변조하여 쉽게 우회가 가능하며, 확장자 검증 시 블랙 리스트 필터링이 적용되어 있다면 금지된 확장자와 동일한 의미를 갖는 확장자 중 차단되지 않은 확장자를 사용하는 방법으로 우회를 시도해 볼 수 있습니다.
> 예시 - jsp 파일 업로드 차단된 경우에 jSp, jspx, jspf, JsP 등으로 우회 시도

| 파일 확장자        | Content-Type              | 동일한 의미를 갖는 확장자      |
|--------------------|---------------------------|--------------------------------|
| `.php`             | `text/php`                | `.phtml`, `.php3`, `.php4`     |
| `.jsp`             | `text/html`               | `.jspx`, `.jspf`               |
| `.asp`             | `text/html`               | `.aspx`, `.ascx`               |
| `.bat`             | `application/x-msdos-program` | `.cmd`                         |
| `.html`            | `text/html`               | `.htm`, `.xhtml`               |
| `.css`             | `text/css`                |                                |
| `.js`              | `application/javascript`  | `.mjs`                         |
| `.json`            | `application/json`        |                                |
| `.xml`             | `application/xml`         | `.xsl`, `.xsd`                 |
| `.txt`             | `text/plain`              |                                |
| `.csv`             | `text/csv`                |                                |
| `.jpg`, `.jpeg`    | `image/jpeg`              | `.jpe`, `.jfif`                |
| `.png`             | `image/png`               |                                |
| `.gif`             | `image/gif`               |                                |
| `.bmp`             | `image/bmp`               |                                |
| `.svg`             | `image/svg+xml`           |                                |
| `.mp3`             | `audio/mpeg`              | `.mpeg3`, `.mpg3`              |
| `.wav`             | `audio/wav`               | `.wave`                        |
| `.mp4`             | `video/mp4`               | `.m4v`                         |
| `.avi`             | `video/x-msvideo`         |                                |
| `.mov`             | `video/quicktime`         |                                |
| `.zip`             | `application/zip`         | `.tar`, `.tar.gz`, `.tar.bz2`   |
| `.pdf`             | `application/pdf`         |                                |
| `.doc`, `.docx`    | `application/msword`      | `.dot`, `.dotx`                |
| `.ppt`, `.pptx`    | `application/vnd.ms-powerpoint` | `.pot`, `.potx`            |
| `.xls`, `.xlsx`    | `application/vnd.ms-excel`| `.xlt`, `.xltx`                |

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>