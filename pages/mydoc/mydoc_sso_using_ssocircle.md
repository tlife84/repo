---
title: SSOCircle을 이용하여 무료로 SAML IDP 구축하기
tags: [Web]
keywords: saml, ssocircle, idp, 무료, 구축
published: true
sidebar: mydoc_sidebar
permalink: mydoc_sso_using_ssocircle.html
folder: mydoc
---

Security Assertion Markup Language(이하 SAML) 로 Single Sign On(이하 SSO) 시스템을 구축하는 경우는 대부분 Service Provider(이하 SP)의 입장일 것이다. 만약 Identity Provider(이하 IDP)가 공개된 경우라면 테스트할 때 어려움이 없겠지만 IDP가 특정 회사의 내부에서만 사용할 경우에는 그 회사 내부가 아니면 테스트를 할 수 없다. 그렇다고 IDP를 직접 구축하자니 시간과 비용이 만만치 않을 것이다. [**SSOCircle**](https://www.ssocircle.com)에서는 이런 사용자들을 위해 무료로(1년간) IDP를 발급해준다. 이 문서에서는 SSOCircle에서 IDP를 구축하는 과정을 알아본다.

SSOCircle 사이트에 접속하여 `Sign In / Register > Register` 메뉴를 통해 회원가입을 한다. 회원가입 절차는 생략한다.

{% include image.html file="user_profile.jpg" %}

회원가입을 완료하고 로그인하면 다음과 같은 화면이 뜬다. User Profile에서는 따로 수정할 것이 없다.

{% include image.html file="manage_metadata.jpg" %}

**_Manage Metadata_** 메뉴로 이동한다. SP의 메타데이터를 등록하기 위해 **_Add new Service Provider_**를 클릭한다.

{% include image.html file="metadata_import.jpg" %}

입력할 항목은 3가지다.

* **Enter the FQDN of the ServiceProvider:** FQDN은 Fully Qualified Domain Name의 약자다. 쉽게 말해 SP의 호스트 이름이다. SP 메타데이터의 ACS 바인딩 주소 호스트가 localhost라면 여기에도 localhost를 입력한다.
* **Attributes sent in assertion (optional)**: Reponse로 받을 데이터를 선택한다. optional이므로 선택을 안하면 모든 정보가 넘어온다.
* **Insert the SAML Metadata information of your SP:** SP의 XML 메타데이터를 입력한다. SP의 메타데이터를 얻는 방법은 [**메타데이터 변환하기**](mydoc_converting_metadata)를 참고한다.

모두 입력한 후 Submit을 눌러 등록을 완료한다.

{% include image.html file="manage_metadata2.jpg" %}

SP 메타데이터 등록이 잘 됐다면 그림과 같이 SP 목록에 SP의 Entity ID가 보인다. SP 메타데이터를 등록했으면 IDP의 메타데이터를 얻기 위해 **_SSOCircle Public IDP Metadata_** 를 클릭한다. 

{% include image.html file="idp_metadata.jpg" %}

빨간박스 영역에 IDP의 메타데이터가 보인다. 이를 SP에 등록하면 IDP 구축은 끝이다. 다행히도 테스트로 사용할 사람을 위한 배려인지 별도의 Signing과 Encryption이 없어도 잘 동작한다. SP 환경설정과 메타데이터 등록은 [**SimpleSAMLphp를 이용하여 SAML Service Provider 구축하기**](mydoc_simple_saml)을 참고한다.