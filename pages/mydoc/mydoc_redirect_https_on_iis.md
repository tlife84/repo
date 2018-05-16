---
title: IIS 7.0에서 http를 https로 강제 변환하기
tags: [Web]
keywords: iis, http, https, 변환
sidebar: mydoc_sidebar
permalink: mydoc_redirect_https_on_iis.html
folder: mydoc
---

* IIS 관리자 실행
* 웹사이트 전체를 적용하고 싶으면 **Default Web Site를 선택 또는 특정 가상디렉토리**만 적용하고 싶으면 해당 가상디렉토리를 선택한 후 **SSL 설정** (여기서는 Default Web Site로 가정한다.)
* **SSL 필요** 체크

![](../../images/403.4-error.jpg)
여기까지 진행했다면 http로 접속했을 때 위와 같이 403 에러가 발생한다. 위 에러코드는 실제로는 403.4 에러다. 다음 해줘야 하는 것은 403.4 에러페이지를 만들어서 http로 접속했던 주소를 https로 변경하는 일이다.

* 403.4 에러페이지 작성 