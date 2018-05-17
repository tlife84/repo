---
title: SQLCMD 커맨드에서 쌍따옴표 사용
keywords: sqlcmd, command, double quotes, 커맨드, 쌍따옴표
sidebar: mydoc_sidebar
permalink: mydoc_sqlcmd_double_quotes.html
folder: mydoc
---

SQLCMD -Q 뒤에 붙는 쿼리가 다음과 같을 경우 "" 때문에 쿼리를 인식하지 못한다.

```
SQLCMD -S Servername -U UserId -P Password -Q SELECT * FROM tablename WHERE column like '%"Name"%'
```

이를 "" 부분을 변수로 처리하여 다음과 같이 치환한다.
```
SQLCMD -S Servername -U UserId -P Password -Q "SELECT * FROM tablename WHERE column like '%$(VAR1)%'" -v VAR1 = "Name"
```

-v 앞의 쿼리를 ""로 감싼 것을 유의해야 한다.