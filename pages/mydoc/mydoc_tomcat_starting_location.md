---
title: 톰캣 시작위치 변경
tags: [Web]
keywords: 
sidebar: mydoc_sidebar
permalink: mydoc_tomcat_starting_location.html
folder: mydoc
---

톰캣의 시작위치는 보통 **C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT\WEB-INF**지만 **C:\Program Files\Apache Software Foundation\Tomcat 7.0\conf\server.xml**에서 변경할 수 있다.

```
<Host name="localhost"  appBase="C:\Workspace\poi\webapps"
            unpackWARs="true" autoDeploy="true">
```

**C:\Workspace\poi\webapps**는 **C:\Program Files\Apache Software Foundation\Tomcat 7.0**의 webapps를 그대로 복사하면 된다(docs나 manager 디렉토리는 복사하지 않아도 된다). 