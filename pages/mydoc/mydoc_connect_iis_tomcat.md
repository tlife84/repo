---
title: IIS와 톰캣 연결하기
tags: [Web]
sidebar: mydoc_sidebar
permalink: mydoc_connect_iis_tomcat.html
folder: mydoc
summary: 톰캣 커넥터는 IIS에서 톰캣을 호출할 때 필요한 라이브러리다. IIS와 톰캣을 한 서버에서 구동하는 경우가 일반적이겠지만 IIS와 톰캣이 분리된 경우도 고려하여 로컬과 원격서버에 연결하는 방법을 모두 살펴본다.
---

## 로컬 톰캣 서버에 커넥터로 연결하기

### Tomcat Connector 다운로드
[***톰캣 커넥터 다운로드***](http://tomcat.apache.org/download-connectors.cgi) 페이지에서 Binary Release for selected versions 다운로드한다. 

### Tomcat Connector 복사
다운로드한 압축파일에서 isapi_redirect.dll 파일만 원하는 경로에 복사한다. 다른 글에서는 톰캣이 설치된 디렉토리 안에 dll파일을 복사하는 경우가 많은데 톰캣 서버가 원격서버에 있을 경우는 톰캣 디렉토리 자체가 없다. 또 톰캣 커넥터는 IIS가 주체가 되는 라이브러리이므로 여기서는 IIS 디렉토리 안에 복사하였다. **C:\inetpub\isapi_redirect\local** 디렉토리를 생성하여 그 안에 dll파일을 복사한다.

같은 경로에 isapi_redirect.properties 파일을 생성하고 아래 내용을 작성한다.
```liquid
# 확장 경로. jakarta는 IIS에서 가상디렉토리로 지정해야하니 잘 기억해둔다.
extension_uri=/jakarta/isapi_redirect.dll

# 로그 파일의 저장 위치 및 로그 파일.
# 로그 파일의 경로는 절대경로로 기입한다.
log_file=C:\inetpub\isapi_redirect\local\logs\isapi_redirect.log

# 로그 레벨. 로그 레벨은 debug, info, warn, error, trace를 쓸 수 있으며, 일반적으로는 설치시 debug로 설정하여 문제 해결에 활용하고, 오픈 후에는 info로 적용해서 사용하는 것이 일반적이다.
log_level=debug

# Tomcat 연동시 사용될 Worker 설정 파일의 경로
worker_file=C:\inetpub\isapi_redirect\local\workers.properties
# Worker와 매핑될 URI의 매핑 설정 파일의 경로
worker_mount_file=C:\inetpub\isapi_redirect\local\uriworkermap.properties
```

### uriworkermap.properties 설정  
isapi_redirect.properties 파일의 worker_mount_file 항목과 매핑되는 파일이다. worker_mount_file에서 지정했던 위치에 uriworkermap.properties 파일을 생성하고 다음 내용을 넣어준다.
```liquid
/certiplanner3_poi/certireport=worker1
```
worker1에 연결된 uri는 호스트 주소를 제외한 나머지 주소다. 위 uri를 직접 자바 클래스에 연결해도 되고 worker에 jsp를 연결한 다음 jsp에서 자바 클래스를 호출해도 된다. 위 uri를 호출하면 worker1이 ajp13타입으로 8009포트를 통하여 톰캣에 연결한다는 의미다. worker1을 설정하는 건 workers.properties에서 한다.

### workers.properties 설정
isapi_redirect.properties 파일의 worker_file 항목과 매핑되는 파일이다. worker_file에서 지정했던 위치에 workers.properties 파일을 생성하고 다음 내용을 넣어준다.
```liquid
worker.list=worker1
worker.worker1.type=ajp13
worker.worker1.host=localhost
worker.worker1.port=8009
```
위와 같이 worker1을 설정한다. worker는 원하는만큼 정의할 수 있다. 나머지 IIS에서 할 설정은 아래에서 다룬다.

## 원격 톰캣 서버에 커넥터로 연결하기
아직 로컬 톰캣 서버에 커넥터로 연결하는 것도 완료되지 않았다면 로컬 톰캣 서버부터 연결한 후 다음 과정을 진행하는 것이 좋다. 한꺼번에 구성하다 꼬일 경우 원인을 찾기가 더욱 어렵기 때문이다.

### Tomcat Connector 복사
다운로드한 isapi_redirect.dll파일을 **C:\inetpub\isapi_redirect\remote** 디렉토리를 생성하여 그 안에 복사한다. 같은 경로에 isapi_redirect.properties 파일을 생성하고 아래 내용을 작성한다. 위 로컬 톰캣 커넥터의 내용과 비교해보면 local->remote 디렉토리 이름만 바뀌었다.
```liquid
# 확장 경로. jakarta는 IIS에서 가상디렉토리로 지정해야하니 잘 기억해둔다.
extension_uri=/jakarta/isapi_redirect.dll

# 로그 파일의 저장 위치 및 로그 파일.
# 로그 파일의 경로는 절대경로로 기입한다.
log_file=C:\inetpub\isapi_redirect\remote\logs\isapi_redirect.log

# 로그 레벨. 로그 레벨은 debug, info, warn, error, trace를 쓸 수 있으며, 일반적으로는 설치시 debug로 설정하여 문제 해결에 활용하고, 오픈 후에는 info로 적용해서 사용하는 것이 일반적이다.
log_level=debug

# Tomcat 연동시 사용될 Worker 설정 파일의 경로
worker_file=C:\inetpub\isapi_redirect\remote\workers.properties
# Worker와 매핑될 URI의 매핑 설정 파일의 경로
worker_mount_file=C:\inetpub\isapi_redirect\remote\uriworkermap.properties
```

### uriworkermap.properties 설정  
로컬 톰캣 커넥터와 동일하나 원격 톰캣 서버에서 호출될 uri는 가상디렉토리를 한 단계 줄여보았다.
```liquid
/certireport=worker1
```

### workers.properties 설정
```liquid
worker.list=worker1
worker.worker1.type=ajp13
worker.worker1.host=192.168.11.128
worker.worker1.port=8009
```
로컬 톰캣 커넥터와 동일하지만 host만 원격 서버 ip로 대체하였다.

## IIS 설정

### 로컬 톰캣
제어판 > 프로그램 및 기능 > Windows 기능 사용/사용 안함 > 인터넷 정보 서비스 > World Wide Web 서비스 > 응용 프로그램 개발 기능 > ISAPI 필터와 ISAPI 확장을 체크하여 설치한다.

{% include image.html file="mydoc_connect_iis_tomcat_0.png" %}


IIS 관리자 > 서버 > ISAPI 및 CGI 제한 진입

{% include image.html file="mydoc_connect_iis_tomcat_1.jpg" %}


우클릭 > 추가 메뉴로 진입하여 *ISAPI 또는 CGI 경로*는 **C:\inetpub\isapi_redirect\local\isapi_redirect.dll** 파일을 선택, 설명은 임의로 입력, 확장 경로 실행 허용을 체크한다.

{% include image.html file="mydoc_connect_iis_tomcat_2.png" %}


Default Web Site에 가상디렉토리를 추가한다. 별칭은 isapi_redirect.properties에서 extension_uri로 지정했던 **jakarta**로 하고 경로는 isapi_redirect.dll이 저장된 경로를 지정한다.

{% include image.html file="mydoc_connect_iis_tomcat_3.png" %}


가상디렉토리 추가 후 Default Web Site > ISAPI 필터

{% include image.html file="mydoc_connect_iis_tomcat_3-1.jpg" %}


우클릭 > 추가 창에서 필터이름은 임의로 지정, 실행파일은 *C:\inetpub\isapi_redirect\local\isapi_redirect.dll* 파일을 선택한다.

{% include image.html file="mydoc_connect_iis_tomcat_4.png" %}


ISAPI 필터를 등록했다면, Default Web Site 또는 하위의 특정 사이트 노드를 클릭한 뒤 처리기 매핑 메뉴로 진입한다. ISAPI-dll이 사용 안함으로 되어있다면, ISAPI-dll을 선택 후 우측의 기능 사용 권한 편집을 클릭한 뒤, 실행 권한을 준다.

{% include image.html file="mydoc_connect_iis_tomcat_5.jpg" %}

### 원격 톰캣
다시 IIS 관리자 > 서버 > ISAPI 및 CGI 제한 진입

{% include image.html file="mydoc_connect_iis_tomcat_1.png" %}


우클릭 > 추가 메뉴로 진입하여 *ISAPI 또는 CGI 경로*는 **C:\inetpub\isapi_redirect\remote\isapi_redirect.dll** 파일을 선택, 설명은 로컬 커텍터와 겹치지 않게 입력, 확장 경로 실행 허용을 체크한다. 

{% include image.html file="mydoc_connect_iis_tomcat_6.png" %}


로컬과 원격 커넥터 두 개를 모두 등록하는 이유는, 커넥터 환경을 구성하는 uriworkermap.properties와 workers.properties 파일이 isapi_redirect.properties 파일에 종속되는데 isapi_redirect.properties 파일은 반드시 isapi_redirect.dll 파일과 같은 디렉토리에 있어야 하기 때문이다. 그러므로 하나의 isapi_redirect.dll 파일을 서로 다른 환경의 커넥터가 공유할 수 없다.

사이트 우클릭 > 웹 사이트 추가

{% include image.html file="mydoc_connect_iis_tomcat_7.jpg" %}


*사이트 이름*은 임의로 지정, *실제 경로*는 원격 톰캣 커넥터와 연결할 사이트 디렉토리를 지정, *포트*는 Default Web Site와 충돌하지 않는 다른 포트를 지정한다.

{% include image.html file="mydoc_connect_iis_tomcat_8.jpg" %}

새로 만든 사이트에도 커넥터 연결을 위한 가상디렉토리를 추가한다. *실제 경로*가 로컬 커넥터와 다른 것을 유의한다.

{% include image.html file="mydoc_connect_iis_tomcat_9.jpg" %}


가상디렉토리 추가 후 새로 만든 사이트 선택 > ISAPI 필터

{% include image.html file="mydoc_connect_iis_tomcat_10.jpg" %}


우클릭 > 추가 창에서 필터이름은 임의로 지정, 실행파일은 *C:\inetpub\isapi_redirect\remote\isapi_redirect.dll* 파일을 선택한다.

{% include image.html file="mydoc_connect_iis_tomcat_11.png" %}


ISAPI 필터를 등록했다면, Default Web Site 또는 하위의 특정 사이트 노드를 클릭한 뒤 처리기 매핑 메뉴로 진입한다. ISAPI-dll이 사용 안함으로 되어있다면, ISAPI-dll을 선택 후 우측의 기능 사용 권한 편집을 클릭한 뒤, 실행 권한을 준다.

{% include image.html file="mydoc_connect_iis_tomcat_5.jpg" %}