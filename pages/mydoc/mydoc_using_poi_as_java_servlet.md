---
title: Java Servlet으로 POI 사용하기
keywords: java, servlet, poi, 자바, 서블릿
summary: "POI는 아파치 재단에서 진행하는 프로젝트다. Open Office XML을 기반으로 한 자바 API를 제공하여 워드, 엑셀, 파워포인트, 아웃룻, Visio, 퍼블리셔 파일을 생성할 수 있게 해준다. 여기서는 자바 서블릿에서 POI를 사용하기 위해 환경설정하는 방법 위주로 알아본다."
sidebar: mydoc_sidebar
permalink: mydoc_using_poi_as_java_servlet.html
folder: mydoc
---

## POI 다운로드
[https://poi.apache.org/download.html](https://poi.apache.org/download.html) 링크로 접속하면 bin과 src 두 가지를 받을 수 있다. 라이브러리를 사용하기만 할 거라면 bin을 받는다.

## 소스코드 작성
아래는 POI에서 워드 파일을 생성하는 매우 간단한 예제코드이다. 아무 텍스트에디터로나 작성한다. 
```liquid
import static org.apache.poi.POIXMLTypeLoader.DEFAULT_XML_OPTIONS;
import org.openxmlformats.schemas.wordprocessingml.x2006.main.*;
import org.apache.xmlbeans.XmlToken;

public class POIExample extends HttpServlet {
    private XWPFDocument document;

    public void doGet(HttpServletRequest req,HttpServletResponse res) throws ServletException, IOException {
        document = new XWPFDocument();
        XWPFParagraph paragraph = document.createParagraph();
        XWPFRun run = paragraph.createRun();
        run.setText("Hello World!");

        FileOutputStream out = new FileOutputStream(new File("C:\\helloWord.docx"));
        document.write(out);
        out.close();
    }
}
```

## 소스코드 빌드
이클립스를 사용하면 분명 빌드하긴 편하다. 하지만 처음부터 이클립스로만 쓰다보면 이클립스가 없는 환경에서는 수정하고 빌드하는 방법을 모르는 불상사가 발생한다. 여기서는 이클립스가 없는 환경에서 작업한다고 가정한다.  

소스코드는 어느 위치에 있어도 상관없다. 소스코드가 빌드되어 생성되는 클래스 파일의 위치가 중요하다. 소스코드의 위치는 `C:\Workspace\poi\src\` 로 가정한다. 위 소소코드는 `C:\Workspace\poi\src\POIExample.java`가 된다. cmd창을 열고 `POIExample.java`파일이 있는 위치로 가서 `javac` 명령어로 빌드한다. `javac` 명령어를 사용하기 위해서는 자바가 설치돼있어야 한다. 자바 설치는 여기서 다루지 않는다. 

```
> javac POIExample.java
```

위 명령어를 실행하면 XWPFDocumnet 등을 찾을 수 없다면서 에러가 뜬다. 위 소스에서 import한 패키지들을 찾을 수 없기 때문이다. 다운받았던 POI 라이브러리 파일을 압축해제한다. 압축해제하면 여러파일이 있는데 이 중에서 필요한 파일은 아래 파일들이다.

```
poi-3.16.jar
poi-ooxml-3.16.jar
poi-ooxml-schemas-3.16.jar
ooxml-lib\curvesapi-1.04.jar
ooxml-lib\xmlbeans-2.6.0.jar
lib\commons-codec-1.10.jar
lib\commons-collections4-4.1.jar
```

### CLASSPATH 지정

빌드할 때 위 라이브러리를 참조할 수 있는 방법은 두 가지다.  

* **설치된 자바 디렉토리의 `lib` 또는 `lib\ext` 에 복사:**  
자바디렉토리 안에서 라이브러리를 찾기 때문에 `javac` 옵션이 간단해진다.  자바는 하나의 OS안에 여러버전이 설치될 수 있는데 path 설정을 제대로 해두지 않았다면 시스템이 어떤 자바버전을 참조하는지 헷갈릴 수도 있다. 최악의 경우 모든 자바버전의 lib디렉토리에 라이브러리 파일을 모두 복사해야할 수도 있다.  

1번 방법은 자바디렉토리 안에 위 라이브러리 파일을 복사만 하면 되므로 생략한다. (복사할 때 curvesapi-1.04.jar과 같이 디렉토리 안에 있는 파일은 디렉토리없이 복사한다.)  

* **javac 명령어에서 `-classpath` 옵션으로 경로 직접 지정:**  
라이브러리 파일이 많을수록 빌드명령어가 복잡해진다. 대신 라이브러리 파일을 관리하기가 쉽다. 빌드명령어는 배치파일로 한번 잘 만들어두면 편하다.  

2번 방법은 `javac` 옵션에 `-classpath` 또는 `-cp` 옵션을 사용한다. `-classpath` 뒤에는 사용자 클래스 파일의 위치가 따라와야 한다. 여기서는 lib파일이 `C:\Workspace\poi\lib`에 있다고 가정한다.

{% include note.html content="사용자 클래스 파일이 .class파일이라면 class파일이 위치한 디렉토리명만 적어도 모든 class파일이 인식되지만 jar파일은 파일 이름을 각각 명시해야 한다. 파일이나 디렉토리가 여러개일 땐 파일명 사이에 ;을 붙인다." %}

```
> javac -cp C:\Workspace\poi\lib\poi-ooxml-schemas-3.16.jar;C:\Workspace\poi\lib\poi-3.16.jar;C:\Workspace\poi\lib\poi-ooxml-3.16.jar;C:\Workspace\poi\lib\xmlbeans-2.6.0.jar;C:\Workspace\poi\lib\curvesapi.jar POIExample.java
```

`-classpath` 뒤에 오는 인자가 너무 길어서 알아보기도 힘들다. 이 인자들을 환경변수에 등록해서 간단하게 변수처럼 사용할 수도 있지만 빌드와 환경변수가 종속성이 생겨서 복잡해지는 단점이 있다. 선택은 본인 몫이다.  

### Class 생성 디렉토리 지정

위 명령어를 실행하면 빌드가 되면서 `C:\Workspace\poi\src\POIExample.class` 파일이 생성된다. 그러나 톰캣은 이 파일의 존재를 모른다. 일반적인 옵션으로 톰캣을 설치했다면 `C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT`가 localhost의 시작위치다. 톰캣은 이 하위에 위치한 `WEB-INF\web.xml`에서 맵핑되는 서블릿이 있는지 확인하여 `WEB-INF\classes`디렉토리에서 class파일을 찾는다(web.xml에 자바 서블릿을 맵핑하는 방법은 [Tomcat에 자바 서블릿 맵핑하기](mydoc_mapping_tomcat_java_servlet.html)을 참고한다.). `WEB-INF\classes` 디렉토리에 class파일을 복사하거나 아니면 다음과 같이 빌드할 때 `classes` 폴더에 class파일이 생성되도록 `-d` 옵션을 사용한다.

```
> javac -d C:\Program Files\Apache Software Foundation\Tomcat 7.0\webapps\ROOT\WEB-INF\classes -cp C:\Workspace\poi\lib\poi-ooxml-schemas-3.16.jar;C:\Workspace\poi\lib\poi-3.16.jar;C:\Workspace\poi\lib\poi-ooxml-3.16.jar;C:\Workspace\poi\lib\xmlbeans-2.6.0.jar;C:\Workspace\poi\lib\curvesapi.jar POIExample.java
```

만약 클래스 파일을 쉽게 관리하기 위해 톰캣 시작위치를 바꾸고 싶다면 [톰캣 시작위치 변경](mydoc_tomcat_starting_location.html)을 참고한다.

## 톰캣 Servlet 설정
톰캣 서블릿 설정은 [톰캣에 자바 서블릿 맵핑하기](mydoc_mapping_tomcat_java_servlet.html)를 참고한다.

## 톰캣 Classpath 설정
빌드까지 됐으면 톰캣 서버를 재시작한다. 그런데 `http://localhost/poiexample`로 접속하면 에러가 발생한다. 톰캣이 poi의 jar파일을 찾을 수 없어 ClassNotFoundException이 발생한 것이다. 이 경우 [톰캣 ClassNotFoundException 발생](mydoc_tomcat_classnotfound.html)을 참고한다.

## 실행
여기까지 문제없이 진행됐다면 `http://localhost/poiexample`로 접속했을 때 웹페이지에는 아무것도 뜨지 않지만 `C:\HelloWorld.docx`가 생성되는 것을 확인할 수 있다.