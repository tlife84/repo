---
title: SSL 인증서 교체 작업
tags: [samsung]
keywords: ssl, certificate, change, 인증서, 교체
published: true
private: true
sidebar: private_sidebar
permalink: private_samsung_ssl.html
folder: private
---

SimpleSAMLphp는 메타데이터를 PHP형태로 저장하기 때문에 XML형태의 메타데이터를 PHP형태로 변환해야 한다. 다행히도 SimpleSAMLphp에서 이러한 툴을 제공하지만 몇 가지 설정이 필요하다.

## SimpleSAMLphp를 웹서버 가상디렉토리에 추가하기
SimpleSAMLphp에서 제공하는 메타데이터 변환 툴은 웹페이지로 동작한다. 때문에 이를 사용하기 위해서는 SimpleSAMLphp를 웹서버에 추가해야 한다. 웹서버에 SimpleSAMLphp `가상디렉토리를 추가할 때는 SimpleSAMLphp의 루트디렉토리인 simplesamlphp가 아니라 simplesamlphp/www 디렉토리를 추가`해야 한다. 가상디렉토리 이름은 SimpleSAMLphp에서 기본으로 사용하는 `**simplesaml/**`로 한다 . 이를 수정하고 싶다면 `config/config.php`에서 `baseurlpath` 값을 변경한다. 사용하는 웹서버가 상이할 수 있으므로 이 문서에서는 웹서버에 가상디렉토리를 추가하는 내용은 다루지 않는다.

## SimpleSAMLphp 접속하기
{% include image.html file="simplesaml_welcome_page.jpg" %}

localhost에서 테스트하는 환경이고 SimpleSAMLphp의 가상디렉토리 이름을 Default로 사용했다면 SimpleSAMLphp의 접속주소는 `localhost/simplesaml/index.php`일 것이다. 이 주소로 접속하면 위와 같은 화면이 떠야한다.

{% include image.html file="simplesaml_federation.jpg" %}

`Federation`탭으로 이동하여 `XML to SimpleSAMLphp metadata converter`메뉴로 들어간다. 

{% include image.html file="simplesaml_password.jpg" %}

패스워드는 `config/config.php`의 `auth.adminpassword` 값을 입력한다.