---
title: VMWare Workstation 14 Player에서 포트 포워딩 하기
tags: [Tools]
keywords: vmware, workstation, player, port, forward, 포트, 포워딩
sidebar: mydoc_sidebar
permalink: mydoc_vmplayer_port_forwarding.html
folder: mydoc
---

VM이 설치된 로컬 컴퓨터가 아니라 외부에서 VM으로 접속하기 위해서는 포트 포워딩이 필요하다. 일반적인 VMWare Workstation에는 네트워크 어댑터 NAT편집 메뉴에 포트포워딩 설정이 존재하지만 Player버전에는 포트포워딩 설정이 없는 듯하다. 그러나 C:\ProgramData\VMWare\vmnetnat.conf에서 간단한 편집으로 가능하다.

vmnetnat.conf 파일에 [incomingtcp] 섹션을 찾으면 예제가 잘 나와있다. 간단하게 [incomingtcp] 아래에 다음과 같은 형식으로 한 줄만 적어주면 된다.
```
[incomingtcp]
33890 = 192.168.11.128:3389
```

- 33890: 내 컴퓨터로 들어오는 포트
- 192.168.11.128: VM OS의 IP. 내 컴퓨터에서 VMNet8 네트워크 어댑터의 IP를 확인하는 것이 아니라 VM OS 안에서 ipconfig로 확인했을 때의 IP다.
- 3389: VM OS로 나갈 포트. 즉 VM OS가 받아들이는 포트다.

위와 같이 설정하고 외부에서 내 컴퓨터의 IP와 33890 포트번호로 원격데스크톱을 연결하면 VM OS를 원격제어할 수 있다. 물론 내 컴퓨터와 VM OS의 방화벽에서 해당 포트를 먼저 허용해야 한다.