---
title: 톰캣에 자바 서블릿 맵핑하기
tags: [Web]
permalink: mydoc_mapping_tomcat_java_servlet.html
keywords: tomcat, java, servlet, mapping, 톰캣, 자바, 서블릿, 맵핑
sidebar: mydoc_sidebar
folder: mydoc
---

톰캣이 서비스하는 호스트 디렉토리의 WEB-INF에는 web.xml이 있다. 예를 들면 기본으로 설치했을 때 호스트 디렉토리는 **C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT**가 되고 web.xml은 **C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT\WEB-INF\web.xml**이다.  
자바서블릿을 매핑하기 위해서는 web.xml에 다음과 같이 서블릿에 관련된 엘리먼트를 추가한다.
```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                      http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
  version="3.0"
  metadata-complete="true">
...중략
  <servlet-mapping>
    <servlet-name>POIExample</servlet-name>
    <url-pattern>/poiexample</url-pattern>
  </servlet-mapping>

  <servlet>
    <servlet-name>POIExample</servlet-name>
    <servlet-class>POIExample</servlet-class>
  </servlet>
...중략
</web-app>
```

servlet-mapping의 **url-pattern** 사이에 들어가는 url는 서블릿을 호출하기 위한 url이다. 앞에는 호스트명이 생략돼있다. 즉, 호스트가 localhost일 때 위 url은 http://localhost/poiexample를 말한다. 위 url로 접속하면 **servlet-mapping의 servlet-name과 servlet의 servlet-name이 맵핑**되어 **servlet의 servlet-class가 호출**된다. 즉 POIExample.class 파일을 찾아서 호출하는 것이다.  

_유의할 점: 만약 POIExample.java에 "package com.mycompany.poi;"처럼 패키지명이 선언돼 있었다면 servlet-class는 com.mycompany.poi.POIExample 로 명시해야 한다. 실제로 위처럼 패키지명이 선언됐을 경우 class파일도 C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT\WEB-INF\classes\com\mycompany\poi 에 생성된다._

그러나 아직 아파치 웹서버는 /poiexample이라는 url을 모른다. 그러므로 아파치 웹서버와 톰캣을 커넥터로 연결해야 한다. 이는 [아파치 웹서버와 톰캣 커넥터 연결하기](connect_tomcat_with_apache_webserver)를 참고한다.