---
title: SimpleSAMLphp를 이용하여 SAML Service Provider 구축하기
tags: [Web]
keywords: simplesaml, saml
published: true
summary: "Security Assertion Markup Language(이하 SAML) Service Provider(이하 SP)를 개발하는 방법은 여러가지가 있다. 유료 솔루션도 있고 무료로 사용할 수 있는 오픈소스도 존재한다. 본 문서에서는 무료로 사용할 수 있는 오픈소스 중에서 SimpleSAMLphp를 이용하여 SP를 구축하는 방법을 알아본다. SimpleSAMLphp는 몇 가지 SP 환경설정만 하면 특별히 개발공수가 많이 들어가지 않는 유용한 툴이다."
sidebar: mydoc_sidebar
permalink: mydoc_simple_saml.html
folder: mydoc
---

## SimpleSAMLphp 다운로드 및 설치
아래 링크에서 SimpleSAMLphp 소스를 다운로드 받는다.

<https://simplesamlphp.org/download>

적당한 위치에 압축을 푼다. SimpleSAMLphp 소스는 꼭 개발소스 디렉토리 안에 넣지 않아도 무방하다. 이 문서에서는 Certiplanner에서 SAML을 사용하기 위해 다음의 경로에 압축을 풀었다.

```
C:\Workspace\certiplanner3\php\
```

## SP 환경설정
SP 환경설정은 아래 링크의 문서에 상세하게 기술되어 있다. 

<https://simplesamlphp.org/docs/stable/simplesamlphp-sp>

본 문서에서는 위 문서에서 SP환경설정에 필요한 최소한의 핵심만 추려 정리하였다. S사 SAML IDP와 연동하기 위해 필요한 내용만을 정리하였기 때문에 더 자세한 정보가 필요할 경우 위 문서를 참고하면 된다.

### 1. config/authsources.php 에 SP 기본속성 정의하기 
```
/* authsources.php의 Default 내용 */
'default-sp' => array(
    'saml:SP',
    'entityID' => null,
    'idp' => null,
    'discoURL' => null,
),
```

이 항목에서 정의한 일부 속성들이 SP의 메타데이터로 변환된다. 

아래는 위 항목들을 수정하여 S사 SAML IDP와 연동한 SP의 정보다. 이 항목들을 모두 채우기 위해서는 IDP의 메타데이터가 필요하다. IDP에서 메타데이터를 먼저 받았다면 아래 항목들을 그에 맞게 채워주면 되고, 만약 IDP에서 SP의 메타데이터부터 요구할 경우 일단 IDP에 대한 항목은 없이 진행해도 된다. 각 항목에 대한 설명을 /**/ 주석으로 표기하였다.

```
/* SP의 Entity ID이다. 보통 host 이름으로 많이 쓴다. */
'certiplanner.S.net' => array(
    /* SP를 의미 */
    'saml:SP',

    /* acs는 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST'와 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-REDIRECT' 
    형식이 있다. IDP에서 사용하는 방식을 지정한다. */
    'acs.Bindings' => array(
        'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST',
    ),

    /* (추측)만약 IDP가 메시지를 암호화한다면 SP의 공개키로 할 것 같다. 
    복호화는 SP의 개인키로 해야하므로 SP의 개인키를 지정한다. 
    S사에서는 IDP가 메시지를 암호화하지 않지만 추후를 대비해 참고할 만한 옵션이다. */
    //'privatekey' => 'certiplanner_S_net_1_private.pem',

    /* pfx 파일에서 추출한 공개키를 지정한다. 
    certificate 파일은 simplesamlphp/cert에 있어야 한다. 
    SP의 메타데이터를 생성하면 certificate키가 X509Certificate 값으로 생성된다. 
    certificate 키는 SHA256 등의 해시로 변환되어 Request에 실린다. 
    IDP는 Request로 넘어온 해시와 SP로부터 메타데이터로 받았던 X509Certificate 해시를 비교하여 인증한다. */
    'certificate' => 'certiplanner_S_net_1_public.pem',

    /* IDP에서 로그아웃을 Initiate 할 경우 사용할 URL이다. IDP에서 로그아웃을 Initiate 한다는 것은 IDP에서 로그아웃을
    했을 때 SP에서도 자동으로 로그아웃이 된다는 의미이다. 이 기능을 사용하지 않으면 없어도 되는 항목이다. */
    'SingleLogoutService' => 
    array (
        0 => 
        array (
          'Binding' => 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST',
          'Location' => 'https://certiplanner.S.net/simplesaml/module.php/saml/sp/saml2-logout.php/certiplanner.S.net',
        ),
    ),
    
    /* SSO 과정 중 IDP에서 넘겨준 정보를 처리할 URL이다. URL은 "Host 이름/" + "simplesaml/" + 
    "module.php/saml/sp/saml2-acs.php/" + "SP의 Entity ID"로 자동 생성된다. 이 URL에서 IDP가 넘겨준 Assertion 정보를 
    파싱하게 된다. */
    'AssertionConsumerService' =>
    array (
        0 =>
        array (
            'Binding' => 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST',
            'Location' => 'https://certiplanner.S.net/simplesaml/module.php/saml/sp/saml2-acs.php/certiplanner.S.net',
        )
    ),

    /* SP의 Entity ID (꼭 필요한지는 의문) */
    'entityID' => 'certiplanner.S.net',

    /* IDP의 Entity ID */
    'idp' => 'www.S.net',

    /* 이 예제에서는 쓰이지 않으나 기본값이라서 그대로 둠 */
    'discoURL' => null,

    /* signature.algorithm이 주석처리 돼있으면 메시지를 암호화할 경우 SHA1을 사용한다. 
    SHA256을 지정하기 위해서는 아래와 같이 명시한다. 
    단, 이 옵션은 saml20-idp-remote.php에도 넣어야하고 saml20-idp-remote.php에서 'sign.authnrequest'=>'true'일 경우만 동작한다. */
    'signature.algorithm' => 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256',
),
```

위의 항목들을 모두 채웠다면(IDP항목은 아직 안채웠어도) SP의 메타데이터를 생성할 수 있다. 메타데이터 변환은 [**메타데이터 변환**](mydoc_converting_metadata)에서 따로 다룬다. 

### 2. config/config.php에서 환경설정 변경하기
config.php 는 SimpleSAMLphp를 사용하는데 쓰이는 환경들을 설정하는 파일이다. 역시 다른 항목들은 생략하고 최소한으로 꼭 변경해야 하는 항목만 설명한다.

```
$config = array(
    ...
    /* 패스워드는 원하는 걸로 바꾼다. */
    'auth.adminpassword' => 'certiplanner',

    /* 꼭 필요한 것인지 모르겠으나 반드시 바꿔야하는 항목이라고 설명되어 있으므로 원하는 salt값으로 변경한다. */
    'secretsalt' => 'certiplanner.S.net',

    /* SAML 오류 발생시 에러 리포트를 받을 수 있다고 하니 바꿀 것을 권장한다. */
    'technicalcontact_email' => 'tester@a.co.kr',

    /* 필수는 아니지만 본인의 타임존에 맞게 바꿀 것을 권장한다. */
    'timezone' => 'Asia/Seoul',

    /* SAML 세션이 유지되는 시간이다. 이 시간을 기준으로 IDP도 세션을 유지하는 것 같다. */
    'session.duration' => 24 * (60 * 60),

    /* SSO 로그인 요청과 로그아웃 요청을 하는데 필요한 데이터가 유지되는 시간이다. Default는 session.duration과 
    session.datastore.timeout이 다르지만 같아도 무방할 것 같아 동일하게 설정했다. */
    'session.datastore.timeout' => (24 * 60 * 60),

    /* 인증상태를 저장하는 시간이다. 여기서 말하는 인증상태와 세션의 차이점이 무엇인지는 아직 모르겠으나 역시 같은 시간으로 
    지정하였다. */
    'session.state.timeout' => (24* 60 * 60),

    /* 0으로 설정하면 브라우저가 닫힐 때 쿠키도 사라진다. 쿠키 유지시간도 다른 시간과 동일하게 설정하는 것이 무방하다. S사는 
    12시간만 유지하게 해달라는 요구사항이 있어 12시간으로 설정한 경우다. */
    'session.cookie.lifetime' => 12 * 60 * 60
```

### 3. metadata/saml20-idp-remote.php에 IDP 정보 등록하기
IDP에 대한 메타데이터는 XML형태로 제공받는다. 반면 `saml20-idp-remote.php`는 php형태로 정보가 저장되기 때문에 이를 위한 [**메타데이터 변환**](mydoc_converting_metadata) 작업이 필요하다. 

```
/* saml20-idp-remote.php의 Default 내용 */
$metadata['https://openidp.feide.no'] = array(
    'name' => array(
        'en' => 'Feide OpenIdP - guest users',
        'no' => 'Feide Gjestebrukere',
    ),
    'description'          => 'Here you can login with your account on Feide RnD OpenID. If you do not already have an account on this identity provider, you can create a new one by following the create new account link and follow the instructions.',

    'SingleSignOnService'  => 'https://openidp.feide.no/simplesaml/saml2/idp/SSOService.php',
    'SingleLogoutService'  => 'https://openidp.feide.no/simplesaml/saml2/idp/SingleLogoutService.php',
    'certFingerprint'      => 'c9ed4dfb07caf13fc21e0fec1572047eb8a7a4cb'
);
```

아래는 S사의 IDP의 메타데이터 내용을 변환한 것이다. Default 내용을 지우고 변환된 메타데이터 내용을 붙여넣기 한다.
```
$metadata['www.S.net'] = array (
  'entityid' => 'www.S.net',
  'contacts' => 
  array (
  ),
  'metadata-set' => 'saml20-idp-remote',

  /* sign.authnrequest는 Request/Response 메시지를 인증하는지를 결정한다. */
  'sign.authnrequest' => true,
  'SingleSignOnService' => 
  array (
    0 => 
    array (
      'Binding' => 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST',
      'Location' => 'https://S.net/sso/saml/SingleSignOnService',
    ),
  ),
  'SingleLogoutService' => 
  array (
     'Binding' => 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST',
     'Location' => 'https://S.net/sso/saml/SingleLogoutService',
  ),
  'ArtifactResolutionService' => 
  array (
  ),
  'NameIDFormats' => 
  array (
    0 => 'urn:oasis:names:tc:SAML:2.0:nameid-format:entity',
  ),
  'keys' => 
  array (
    0 => 
    array (
      'encryption' => false,
      'signing' => true,
      'type' => 'X509Certificate',
      'X509Certificate' => 'MIIEdzCCA1+gAw...DMjIYue5V/PZ6J3o6DjUJaw==',
    ),
    1 => 
    array (
      'encryption' => true,
      'signing' => false,
      'type' => 'X509Certificate',
      'X509Certificate' => 'MIIEdzCCA1+gAw...DMjIYue5V/PZ6J3o6DjUJaw==',
    ),
  ),

  /* signature.algorithm이 주석처리 돼있으면 메시지를 암호화할 경우 SHA1을 사용한다. 
    SHA256을 지정하기 위해서는 아래와 같이 명시한다. 
    단, 이 옵션은 authsources.php에도 넣어야하고 saml20-idp-remote.php에서 'sign.authnrequest'=>'true'일 경우만 동작한다. */
    'signature.algorithm' => 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256',
);
```

환경설정은 이것으로 끝이다.

## SSO 테스트
SimpleSAMLphp의 장점은 소스에 통합하기 전에 SimpleSAMLphp에서 제공하는 툴로 SSO를 테스트 해볼 수 있다는 점이다. 여기서 말하는 툴이란 메타데이터 변환 때 사용했던 웹페이지 형태의 툴이다. 이 툴을 이용할 환경이 아직 구성되지 않았다면 [**메타데이터 변환**](mydoc_converting_metadata)의 **SimpleSAMLphp를 웹서버 가상디렉토리에 추가하기** 절을 참조한다.

{% include image.html file="simplesaml_welcome_page.jpg" %}

`https://호스트이름/simplesaml/index.php`에 접속한다. 실제 운영환경이라면 SimpleSAMLphp가 설치된 서버에서 직접 테스트할 수가 없다. 서버에서 직접 테스트하기 위해서는 호스트이름에 localhost를 넣어야 한다. SimpleSAMLphp는 IDP에게 **REQUEST를 보낼 때 ACS 바인딩 주소를 현재 접속중인 URL을 기반으로 생성**한다. 그런데 IDP에게 전달한 메타데이터의 ACS 바인딩주소가 `https://certiplanner.S.net/simplesaml/module.php/saml/sp/saml2-acs.php/certiplanner.S.net`와 같은 형태일 것이므로 REQUEST로 보낸 ACS 바인딩 주소와 IDP에게 보낸 메타데이터의 ACS 바인딩 주소가 일치하지 않아 IDP에서 이 요청을 받아들이지 않는다. 그러므로 SimpleSAMLphp이 설치된 서버가 아닌 제3의 클라이언트에서 실제 서버의 도메인주소로 접속해서 테스트해야 한다.

또, IDP에서 signing과 encryption을 사용하지 않는다면 http로 접속해도 테스트가 가능하지만 그렇지 않다면 반드시 https로 접속해야 SSO 로그인이 가능하다. 아직 웹서버에 인증서가 설치되지 않았다면 [**윈도우2008서버 iis 7.x버전 ssl보안인증서 설치 방법**](https://m.blog.naver.com/PostView.nhn?blogId=73053860&logNo=220673775075&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)를 참조하여 발급받은 SSL인증서를 설치한다. 만약 실제 IDP가 없어서 테스트 할 환경이 없다면 [**SSOCircle**](https://www.ssocircle.com)에서 무료로 개인 IDP를 제공받아 SSO테스트를 할 수 있다. SSOCircle을 이용한 IDP 테스트는 [**SSOCircle을 이용하여 무료로 IDP 구축하기**](testing_sso_using_ssocircle)에서 따로 다룬다.
  
{% include image.html file="simplesaml_authentication.jpg" %}    

Authentication 탭에서 Test configured authentication sources 메뉴로 들어간다.

{% include image.html file="simplesaml_authentication_sources.jpg" %}

config/config.php에서 추가했던 SP의 Entity ID가 보일 것이다. 이를 클릭하면 해당 IDP의 SSO 로그인 창이 뜬다.

{% include image.html file="simplesaml_sso_login.jpg" %}

실제로는 S사의 SSO로그인 창이 떠야하지만 S사의 SSO 로그인 창을 캡쳐할 수가 없어 SSOCircle의 로그인 화면으로 대체하였다. 

{% include image.html file="simplesaml_sso_login_complete.jpg" %}

이 그림 역시 S사의 SSO로그인 화면은 아니지만 로그인이 완료되면 이와 같은 페이지가 떠야한다. Your attributes는 IDP에서 어떤 값을 넘겨주느냐에 따라 달라진다.

## 소스에 통합하기
여기까지 진행되었다면 별도의 소스 개발없이 SimpleSAMLphp의 환경설정만으로도 SSO로그인이 가능하다는 것을 알 수 있을 것이다. 그렇다면 소스에는 어떻게 적용해야 할까? 이 역시 생각보다 간단하다.

```
<?php
    /* SimpleSAMLphp 모듈을 로드한다. */
    require_once('simplesamlphp/lib/_autoload.php');
    
    $as = new SimpleSAML_Auth_Simple($asId);
    
    if (!$as->isAuthenticated()) /* SSO 로그인이 안돼있는 경우 */
    {
        /* Return URL을 지정해준다. SSO 로그인이 완료되면 복귀할 URL이다. 보통은 ReturnURL에서 Response를 파싱한다. */
        $url = 'https://certiplanner.S.net/index.html';
        $params = array(
            'ErrorURL' => $url,
            'ReturnTo' => $url,
        );

        /* SSO 로그인 페이지로 이동한다. */
        $as->login($params);
    }
    else /* SSO 로그인이 돼있는 경우 */
        /* Return URL로 이동한다. Certiplanner는 로그인이 완료되면 항상 아래 URL로 이동해야 하지만 경우에 따라서는 로그인 후 
        Return URL과 로그인이 돼있을 때의 URL이 다를 수도 있다. */
        header('Location: '.'https://certiplanner.S.net/index.html');
    
    /* SimpleSAMLphp의 세션을 정리한다. 만약 SP에서 별도의 PHP세션을 사용중이라면 SimpleSAMLphp의 세션은 정리해야 
    PHP세션이 꼬이지 않는다. */
    $session = SimpleSAML_Session::getSessionFromRequest();
    $session->cleanUp();
?>
```

위 소스만으로 로그인이 완료된다. 그러나 아직 IDP로부터 받은 Response를 파싱하는 과정이 없다. Certiplanner는 "https://certiplanner.S.net/index.html" 주소로 접속했을 때 SSO로그인이 됐는지 한번 더 확인한 후 로그인이 돼있다면 이 때 Response를 파싱한다.

```
    $as = new SimpleSAML_Auth_Simple($asId);
    if ($as->isAuthenticated()) 
    {
        $attributes = $as->getAttributes();
        $authData = $as->getAuthDataArray();
        $nameIdArray = $authData['saml:sp:NameID'];
        $name = $nameIdArray['Value'];

        if (ENTITY_ID == 'certiplanner.S.net') // S
            $response = [$name];
        else if (ENTITY_ID == 'certiplanner')            // ssocicle test
            $response = [$attributes['FirstName'][0]];
        return $response;
    }
```

IDP에 따라 Attributes정보가 달라지는 것에 주목해야 한다. Attributes 정보는 SimpleSAMLphp 툴로 SSO테스트를 해봤다면 어떤 정보가 넘어오는지 확인할 수 있다.
이제 이 정보를 가지고 SP에서 로그인 처리를 하는 것은 SP의 몫이다.