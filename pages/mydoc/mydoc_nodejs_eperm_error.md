---
title: Nodejs spawn EPERM 오류
tags: [Troubleshooting]
keywords: nodejs, spawn, eperm, error
sidebar: mydoc_sidebar
permalink: mydoc_nodejs_eperm_error.html
folder: mydoc
---

~~~
Application has thrown an uncaught exception and is terminated:
Error: spawn EPERM
    at _errnoException (util.js:1022:11)
    at ChildProcess.spawn (internal/child_process.js:323:11)
    at exports.spawn (child_process.js:502:9)
    at Object.exports.execFile (child_process.js:212:15)
    at exports.exec (child_process.js:142:18)
    at asyncEachObject 
    ...
~~~

`asyncEachObject` 내부에서 `exec()`를 실행하던 도중 발생한 에러다. 자식 프로세스를 spawn 하려는데 권한이 없을 경우 발생할 수 있다. (spawn 할 수 없는 원인은 여러가지가 있을 수 있으니 이 방법대로 해도 안되면 다른 방법을 참고하길)

`exec()` 함수는 커맨드 명령어를 실행하여 자식 프로세스를 만들기 때문에 커맨드 명령어를 실행 할 수 있는 `C:\windows\system32\cmd.exe`에 대한 권한을 갖고 있어야 한다.  보통 `cmd.exe`는 `Users` 그룹권한을 가지고 있지만 서버의 경우 관리자가 cmd창을 아무나 실행할 수 없도록 `Users` 권한을 제거했을 수도 있다. 이럴 경우 위와 같은 에러가 발생한다.

해결방법은 `C:\windows\system32\cmd.exe`에 `Users` 권한을 주는 것이다. 만약 nodejs를 IIS의 응용프로그램으로 연결하여 실행하는 것이라면 `Users` 대신 `IIS_USER` 권한을 줘도 된다.

{% include note.html content="서버 관리자에게 cmd.exe에 대한 권한을 요청할 때 특정 사용자에게 권한을 부여해달라고 하면 안된다. 이 경우 cmd창을 실행할 수는 있지만 exec()함수를 사용하여 자식 프로세스를 실행하진 못한다. 왜냐하면 exec커맨드는 해당 사용자의 권한으로 실행되는 것이 아니기 때문이다." %}

