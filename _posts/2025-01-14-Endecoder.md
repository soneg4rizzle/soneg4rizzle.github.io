---
title: Endecoder
categories: [Tools, Endecoder]
tags: [Web Hacking, URL, base64, HEX, Unicode]
image:
  path: /assets/post/2025/Tools/Endecoder/endecoder.png
  alt: Endecoder for base64·hex·html·utf7·url·unicode
published: true
---

## Endecoder
BASE64, URL, HEX, HTML, Unicode, UTF-7bit
웹·모바일 애플리케이션 취약점 진단 시 필터링 우회를 위한 인코더·디코더입니다.

<h3>Encoder</h3>
<div><textarea id="inputText" placeholder="Enter your plain-text here." style="width: 100%"></textarea></div>
<div style="text-align: center;"><button onclick="encodeBase64()">BASE64</button>　　　<button onclick="encodeHEX()">HEX</button>　　　<button onclick="encodeHTML()">HTML</button>　　　<button onclick="encodeUTF7()">UTF-7</button>　　　<button onclick="encodeURL()">URL</button>　　　<button onclick="encodeUnicode()">Unicode</button></div>

<h3>Decoder</h3>
<div><textarea id="outputText" placeholder="Enter your encoded-text here." style="width: 100%"></textarea></div>
<div style="text-align: center;"><button onclick="decodeBase64()">BASE64</button>　　　<button onclick="decodeHEX()">HEX</button>　　　<button onclick="decodeHTML()">HTML</button>　　　<button onclick="decodeUTF7()">UTF-7</button>　　　<button onclick="decodeURL()">URL</button>　　　<button onclick="decodeUnicode()">Unicode</button></div>

<script>
    function encodeBase64() {
        const input = document.getElementById('inputText').value;
        const encoded = btoa(input);
        document.getElementById('outputText').value = encoded;
    }

    function decodeBase64() {
        const input = document.getElementById('outputText').value;
        try {
            const decoded = atob(input);
            document.getElementById('inputText').value = decoded;
        } catch (e) {
            document.getElementById('inputText').value = "디코딩 오류: 올바른 Base64 문자열이 아닙니다.";
        }
    }

    function encodeHEX() {
        const input = document.getElementById('inputText').value;
        let encoded = '';
        for (let i = 0; i < input.length; i++) {
            encoded += '\&#x' + input.charCodeAt(i).toString(16).padStart(2, '0');
        }
        document.getElementById('outputText').value = encoded;
    }

    function decodeHEX() {
        const input = document.getElementById('outputText').value;
        const regex = /\&#x([0-9a-fA-F]+)/g;
        let decoded = input.replace(regex, (match, p1) => String.fromCharCode(parseInt(p1, 16)));
        document.getElementById('inputText').value = decoded;
    }

    function encodeHTML() {
        const input = document.getElementById('inputText').value;
        let encoded = '';
        for (let i = 0; i < input.length; i++) {
            encoded += '\&#x' + input.charCodeAt(i).toString(10).padStart(2, '0') + ';';
        }
        document.getElementById('outputText').value = encoded;
    }

    function decodeHTML() {
        const input = document.getElementById('outputText').value;
        const regex = /\&#x([0-9a-fA-F]+);/g;
        let decoded = input.replace(regex, (match, p1) => {
            return String.fromCharCode(parseInt(p1, 10))
        });

        document.getElementById('inputText').value = decoded;
    }
    
    function encodeUTF7() {
        const input = document.getElementById('inputText').value;
        let encoded = '';
        for (let i = 0; i < input.length; i++) {
            encoded += '\&#x' + input.charCodeAt(i).toString(10).padStart(7, '0') + ';';
        }
        document.getElementById('outputText').value = encoded;
    }

    function decodeUTF7() {
        const input = document.getElementById('outputText').value;
        const regex = /\&#x([0-9a-fA-F]+);/g;
        let decoded = input.replace(regex, (match, p1) => String.fromCharCode(parseInt(p1, 10)));
        document.getElementById('inputText').value = decoded;
    }
    
    function encodeURL() {
        const input = document.getElementById('inputText').value;
        const encoded = encodeURIComponent(input).replace(/'/g,"%27").replace(/"/g,"%22");;
        document.getElementById('outputText').value = encoded;
    }
    
    function decodeURL() {
        const input = document.getElementById('outputText').value;
        const decoded = decodeURIComponent(input);
        document.getElementById('inputText').value = decoded;
    }

    function encodeUnicode() {
        const input = document.getElementById('inputText').value;
        const encoded = Array.from(input).map(char => '\\u' + ('0000' + char.charCodeAt(0).toString(16)).slice(-4)).join('');
        document.getElementById('outputText').value = encoded;
    }

    function decodeUnicode() {
        const input = document.getElementById('outputText').value;
        const decoded = input.replace(/\\u([a-fA-F0-9]{4})/g, (match, grp) => String.fromCharCode(parseInt(grp, 16)));
        document.getElementById('inputText').value = decoded;
    }
</script>

<br>

---

## ASCII Table
<img src="/assets/post/2025/Tools/Endecoder/ascii.png" alt="" width=333>

---

### Reference
1. [Regular Expression](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)
2. [ASCII Table](https://shaeod.tistory.com/228)

--- 

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>