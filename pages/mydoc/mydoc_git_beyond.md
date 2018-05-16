---
title: Git과 Beyond Compare 4 연동하기
tags: [Tool]
keywords: git, beyond, compare
sidebar: mydoc_sidebar
permalink: mydoc_git_beyond.html
folder: mydoc
---

```bash
git config --global diff.tool bc3
git config --global difftool.bc3.path "c:/Program Files/Beyond Compare 4/BCompare.exe"
git config --global merge.tool bc3
git config --global mergetool.bc3.path "c:/Program Files/Beyond Compare 4/BCcompare.exe"
```
Beyond Compare 버전에 따라 설치 경로가 달라질 수도 있음
