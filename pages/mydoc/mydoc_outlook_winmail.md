---
title: 아웃룩으로 보낸 메일이 winmail.dat 첨부파일로 오는 현상
tags: [Troubleshooting]
keywords: outlook, mail, winmail, 아웃룩, 메일
sidebar: mydoc_sidebar
permalink: mydoc_outlook_winmail.html
folder: mydoc
---

상대방이 보낸 메일이 winmail.dat 파일만 첨부되어 있고 아무 내용도 없거나(웹메일에서 확인한 경우) 혹은 그냥 아무런 내용도 없는 경우(아웃룩에서 확인한 경우)가 있다. 이는 상대방이 아웃룩으로 메일을 보냈을 때 발생하며, 아웃룩에서 TNEF(Transport Neutral Encapsulation Format) 기능을 사용해서 메일을 보냈기 때문이다. TNEF 기능은 상대방이 의도한 기능은 아니고 메일 내용 중 어떤 서식에 의해 자동으로 활성화 되는 것으로 보인다.

이 기능을 비활성화 하려면 다음의 절차를 따라 레지스트리 값을 추가한다.  
{% include important.html content="이 절차는 메일 수신자가 하는 것이 아니라 송신자측에서 해야함!" %}

* 실행 > regedit  
* HKEY_CURRENT_USER\Software\Microsoft\Office\15.0\Outlook\Preferences 로 이동
* 새로 만들기 > DWORD(32비트) 선택 > DisableTNEF 생성  
* DisableTNEF의 값 데이터를 1로 수정

{% include note.html content="15.0은 오피스 버전에 따라 숫자가 달라질 수 있음.  
오피스2016: 16.0  
오피스2013: 15.0  
오피스2010: 14.0  
오피스2007: 12.0" %}

위처럼 레지스트리를 수정한 후에 PC를 재부팅한다.
레지스트리 편집이 어려운 사람들을 위해 레지스트리를 바로 추가할 수 있는 파일을 첨부한다. **(오른쪽 버튼 클릭 후 다른 이름으로 저장 - 크롬은 확장자를 reg로 변경하여 저장)**  

* [오피스2016용](../../attach/Outlook2016.reg)
* [오피스2013용](../../attach/Outlook2013.reg)
* [오피스2010용](../../attach/Outlook2010.reg)
* [오피스2007용](../../attach/Outlook2007.reg)

## 참고사이트 
[https://support.microsoft.com/ko-kr/help/958012](https://support.microsoft.com/ko-kr/help/958012) 을 토대로 작성함.