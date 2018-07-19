---
title: IIS에 Local SSL 구성하기
keywords: iis, local, ssl
tags: Web
published: false
sidebar: mydoc_sidebar
permalink: mydoc_local_ssl.html
folder: mydoc
---

* OpenSSL 다운로드 받아 C:\OpenSSL 디렉토리로 압축해제
* bin 디렉토리에 있는 openssl.cnf 파일을 C:\OpenSSL로 이동
* CMD창을 열어 설치된 OpenSSL디렉토리로 이동
```
> cd C:\OpenSSL
```

* openssl 실행
```
> bin\openssl
```

* 개인키 생성
```
OpenSSL> genrsa -out private.key 1024
```

{% include note.html content="  
private.key: 개인키파일명" %}

* 공개키 생성
```
OpenSSL> req -config ./openssl.cnf -new -x509 -days 3650 -key private.key -out public.cer
...
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:
```

{% include note.html content="  
3650: SSL 유효기간  
private.key: 개인키파일명  
public.cer: 공개키파일명" %}
{% include warning.html content="다른값은 별로 유의미하지 않으나 로컬 SSL이 아닐경우 Common Name은 정확한 도메인을 입력해야한다." %}


* `IIS 실행 > 서버 선택 > 서버 인증서 > 인증서 요청 만들기` 에서 일반 이름을 localhost, 나머지는 대충 입력
* 암호화 서비스 공급자 속성은 Default로 놓고 다음
* 파일 이름은 임의로 지정
* OpenSSL이 설치된 디렉토리에 `demoCA`라는 디렉토리 생성
* demoCA 디렉토리에 `newcerts`라는 디렉토리 생성
* demoCA 디렉토리에 `index.txt` 이라는 빈 파일 생성
* demoCA 디렉토리에 `serial` 파일 생성 후 파일내용에 `00`이라고 입력
* 인증키 생성
```
OpenSSL> ca -policy policy_anything -config ./openssl.cnf -cert public.cer -in CSR.txt -keyfile private.key -days 3650 -out ./iis.cer
```