---
title: PHPUnit 테스트 환경 만들기
tags: [TDD]
keywords: php, unit, phpunit, test, environment, 테스트, 환경
sidebar: mydoc_sidebar
permalink: mydoc_phpunit.html
folder: mydoc
---
아래는 Windows 환경을 기반으로 작성하였다.

## PHPUnit 설치
PHPUnit을 설치하는 방법에는 PHP Archive(PHAR)와 Composer를 이용한 방법이 있다. 두 방법의 정확한 차이는 모르겠지만 PHAR은 글로벌하게 설치할 때 활용하고 Composer를 이용하면 프로젝트에 한정되어 설치가 되는 것 같다. 여기서는 Composer를 이용해서 설치하는 방법을 택했다.

먼저 Composer는 이미 설치가 되어 있고 CMD창에서 Composer 명령어를 바로 실행할 수 있도록 PATH지정도 돼있다고 가정한다.

CMD창을 열고 프로젝트의 루트 디렉토리로 이동한다. 프로젝트 루트 디렉토리에서 다음 명령어를 실행한다.

^5는 phpunit 5를 설치한다는 의미다. 참고로 php버전에 따라 phpunit의 버전을 다르게 설치해야 한다.

| PHP버전 | PHPUnit버전 |
| --- | --- |
| PHP 5.3, 5.4, 5.5 | PHPUnit 4 |
| PHP 5.6 | PHPUnit 5 |
| PHP 7.0 | PHPUnit 6 |
| PHP 7.1이상 | PHPUnit 7 |

```s
> composer require --dev phpunit/phpunit ^5
```
위 명령어를 실행하면 프로젝트 디렉토리에 composer.json과 vendor디렉토리가 생성되고 vendor디렉토리에 PHPUnit에 필요한 파일들이 설치된다.

## PHPUnit 동작 확인
프로젝트 디렉토리에서 다음 명령어가 잘 동작하는지 확인한다.
```s
> vendor\bin\phpunit --version

PHPUnit 5.0.0 by Sebastian Bergmann and contributors.
```

## PHPUnit 소스 디렉토리 지정
PHPUnit의 기본 소스 디렉토리는 프로젝트 루트의 src 디렉토리다. 그러나 테스트할 php파일이 다른 디렉토리에 있다면 이를 변경해주는 것이 좋다.  
프로젝트 루트에 있는 `composer.json`에 `autoload` 항목을 추가하고 `classmap`의 위치를 소스 디렉토리로 변경한다.

```json
{
    "autoload": {
        "classmap": [
            "php/"
        ]
    },
}
```

그 다음은 `autoload.php`를 재생성해야 한다. 아래 명령을 실행하면 `autoload`와 관련된 항목들이 재생성된다.

```s
> composer dump-autoload
```

## 테스트코드 작성
테스트 코드를 작성한다. 여기서는 FileInspector라는 클래스에 getMimeType($filename)이라는 static 메서드를 하나 생성하고 이를 테스트해보기로 한다.

```php
<?php

class FileInspector {
    public static function getMimeType($filename)
    {
        $finfo = finfo_open(FILEINFO_MIME_TYPE);
        $mimeType = finfo_file($finfo, $filename);
        finfo_close($finfo);
        return $mimeType;
    }
}
```

위 코드는 테스트코드가 아니라 **테스트할** 클래스다. getMimeType() 메서드를 테스트하기 위해 다음과 같은 테스트코드를 작성했다.

```php
<?php
//use PHPUnit\Framework\TestCase;

final class FileInspectorTest extends PHPUnit_Framework_TestCase // TestCase
{
    public function testGetMimeType() {
        $this->assertEquals("application/vnd.ms-office", 
            FileInspector::getMimeType("C:\abc.xls"));
    }
}
```

주석처리한 코드는 [PHPUnit 5](https://phpunit.de/getting-started/phpunit-5.html) 공식 홈페이지의 예제에 나와있는 코드를 따라한 것인데 에러가 발생했기 때문에 주석처리 한 것이다. 이유는 모르겠지만 `use`키워드를 이용해서 네임스페이스 기능을 사용할 경우 PHPUnit에서 `TestCase`를 찾지 못한다.

위 코드는 `FileInspector::getMimeType("C:\abc.xls")`의 결과값이 `"application/vnd.ms-office"`이면 성공한다는 뜻이다. 물론 `abc.xls`가 실제 엑셀파일이어야 한다.

## 테스트코드 실행
이제 이 테스트코드를 실행해서 결과를 확인해보자.

```s
> vendor\bin\phpunit --bootstrap vendor\autoload.php tests\FileInspector
PHPUnit 5.0.0 by Sebastian Bergmann and contributors.

.                                                                   1 / 1 (100%)

Time: 173 ms, Memory: 2.50MB

OK (1 test, 1 assertion)
```

성공하면 위와 같이 출력된다.


```s
> vendor\bin\phpunit --bootstrap vendor\autoload.php tests\FileInspector
PHPUnit 5.0.0 by Sebastian Bergmann and contributors.

F                                                                   1 / 1 (100%)

Time: 271 ms, Memory: 2.50MB

There was 1 failure:

1) FileInspectorTest::testGetMimeType
Failed asserting that two strings are equal.
--- Expected
+++ Actual
@@ @@
-'application/vnd.ms-office'
+'text/plain'

C:\SampleProject\tests\FileInspectorTest.php:8

FAILURES!
Tests: 1, Assertions: 1, Failures: 1.
```
위는 테스트에 실패했을 때 출력되는 내용이다. `C:\abc.xls`파일의 mime type이 `'application/vnd.ms-office'`일 거라고 생각했지만 실제로는 `text/plain`이었기 때문에 테스트에 실패했다는 의미다.

## 테스트코드 일괄 실행
만약 여러 테스트케이스 코드를 만들어서 한꺼번에 돌리고 싶을 경우 다음과 같은 명령어를 사용한다.

```s
> vendor\bin\phpunit --bootstrap vendor\autoload.php --testdox tests
```

위 명령어를 실행하면 `tests` 디렉토리에 있는 모든 테스트코드를 실행한다. 단, 테스트코드의 클래스 이름은 `*Test`로 끝나야하며, 메서드 이름은 `test*`로 시작해야한다.

일괄적으로 테스트를 수행하면 다음과 같이 결과가 나온다.

```s
> vendor\bin\phpunit --bootstrap vendor\autoload.php --testdox tests
PHPUnit 5.0.0 by Sebastian Bergmann and contributors.

FileInspector
 [ ] Get mime type
```
빈 대괄호 `[ ]`는 테스트가 실패했다는 뜻이다. 테스트가 성공하면 `[x]`로 표시된다.
