---
title:  톰캣 ClassNotFoundException 발생
tags: [Troubleshooting]
keywords: tomcat, classnotfoundexception, 톰캣
sidebar: mydoc_sidebar
permalink: mydoc_tomcat_classnotfound.html
folder: mydoc
---

## 해결방법 1
자바가 설치된 디렉토리의 `lib`폴더에 해당 jar파일들을 복사한다. 자바가 여러버전이 설치돼있거나 어디에 설치된지 모를 경우 CMD창에서 `echo %JAVA_HOME%`명령어를 실행해보면 자바가 설치된 디렉토리를 알 수 있다. 환경변수에 `JAVA_HOME`이 지정돼있지 않으면 자바가 설치된 디렉토리가 표시되지 않고 `%JAVA_HOME%`이 그대로 리턴된다.

## 해결방법 2
톰캣이 jar파일을 찾을 수 없을 때 ClassNotFoundException이 발생한다. 톰캣에서도 해당 jar파일들을 참조할 수 있도록 `Configure Tomcat > Java 탭 > Java Classpath` 에 추가해준다.

{% include image.html file="tomcat_classpath.jpg" %}

## 해결방법 3
`WEB-INF\lib\` 에 있는 jar파일들은 해당 컨텍스트(IIS의 가상디렉토리 개념)의 자바 서블릿이 호출될 때 자동으로 Classpath에 추가된다. 즉 톰캣이 디폴트 위치에 있으면 `C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT\WEB-INF\lib`가 라이브러리 폴더가 된다. `webapps\ROOT\WEB-INF\lib`에 외부 jar파일들을 모두 복사해놓으면 별도로 톰캣에서 classpath를 추가할 필요가 없어진다.