---
title: Burp Suite Preset
categories: [Tools, Burp Suite]
tags: [Web Hacking, Web Proxy, MITM]
image:
  path: /assets/post/2025/Tools/Burp Suite/thumb.jpg
  alt: Burp Suite Professional
published: true
---

# Burp Suite TIPS

---

## Setting > Proxy Settings > HTTP match and replace rules
> 리소스 캐시 유효시간을 0으로 설정하여 리소스에 접근할 때마다 서버에서 신규 할당
{: .prompt-tip }

```
> (Item) Response header  
> (Match) cache-control: max-age={seconds}  
> (Replace) cache-control: max-age=0
```
<br>

---

## Burp Suite Extension
> Useful Extension for PT, VA, BB, etc.
{: .prompt-info }

### Logger++
> Burp Suite Logger 확장 버전으로 필터링 기능을 사용하여 효율적으로 정보 확인이 가능한데요. HTTP History Search 기능과 유사하게 주민등록번호·계좌번호를 정규식으로 탐색하거나 패킷 분석 시 중요하다고 판단되는 포인트를 중점적으로 분석할 수 있어 점검 시 유용하게 활용할 수 있습니다.

```
> Request.URL CONTAINS "{param}"  
> Request.HOST CONTAINS "{param}"  
> Response.Body CONTAINS "{param}"  
> Response.Status == 200 && Request.Host CONTAINS "{param}"  
> Response.Length > 10000 AND Request.Host CONTAINS "{param}"
```
<br>

---
