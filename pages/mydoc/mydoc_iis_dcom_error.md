---
title: IIS DCOM 오류
tags: [Web]
keywords: iis, dcom, application, exception
summary: "Application has thrown an uncaught exception and is terminated: Error: 'aaa.xlsx' 파일을 사용할 수 없습니다. 원인은 다음과 같습니다."
sidebar: mydoc_sidebar
permalink: mydoc_iis_dcom_error.html
folder: mydoc
---

IIS에서 Node.js의 edge모듈을 사용하던 중 Microsoft.Office.Interop.Excel 객체를 사용하다보면 Workbooks.Open()과정중에 파일을 사용할 수 없다는 오류가 발생할 수 있다.

~~~
Application has thrown an uncaught exception and is terminated:
Error: 'aaa.xlsx' 파일을 사용할 수 없습니다. 원인은 다음과 같습니다.

• 해당 파일 이름이나 경로가 없습니다.
• 다른 프로그램에서 파일을 사용하고 있습니다.
• 저장하려고 하는 통합 문서 이름이 현재 열려 있는 통합 문서의 이름과 같습니다.
    at Error (native)
    at <anonymous>:1:55
    ...
    at emitNone (events.js:86:13)
    at IncomingMessage.emit (events.js:185:7)
    at endReadableNT (_stream_readable.js:974:12)
    at _combinedTickCallback (internal/process/next_tick.js:80:11)
    at process._tickCallback (internal/process/next_tick.js:104:9)
~~~

aaa.xlsx 를 절대경로이고 경로명도 틀림없는데 문제가 발생한다면 DCOM 구성을 변경해야 한다.

* 실행창 > **DCOMCNFG**
* 콘솔 루트 > 구성 요소 서비스 > 내 컴퓨터 > DCOM 구성 > **Microsoft Excel Application** (우클릭 속성)
* **보안** 탭 > **액세스 권한** > **사용자 지정** 선택 > **편집** > **추가** > **IIS_IUSRS** 추가 > **로컬 액세스 허용, 원격 액세스 허용** 체크
* **ID** 탭 > **대화형 사용자** 선택