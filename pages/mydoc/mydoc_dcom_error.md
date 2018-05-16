---
title: DCOM 오류
keywords: DCOM, error, 오류
tags: [Troubleshooting]
summary: "Error: Retrieving the COM class factory for component with CLSID {XXX-XXX}"
sidebar: mydoc_sidebar
permalink: mydoc_dcom_error.html
---

## 오류 메시지
Error: Retrieving the COM class factory for component with CLSID {00024500-0000-0000-C000-000000000046} failed due to the following error: 80070005 액세스가 거부되었습니다. (Exception from HRESULT: 0x80070005 (E_ACCESSDENIED)).

## 원인
DCOM 컴포넌트에 접근하기 위한 권한이 없어서 발생한다. 

## 해결방법
1. 실행창 > DCOMCNFG
2. 콘솔루트 > 구성 요소 서비스 > 컴퓨터 > 내 컴퓨터(우클릭 속성) > COM 보안 탭
3. 액세스 권한 > 기본값 편집  
3.1 추가 > 고급 > 지금 찾기 > NETWORK SERVICE 추가 > 로컬 액세스 허용 체크  
3.2 추가 > 고급 > 지금 찾기 > Users 추가 > 로컬 액세스 허용 체크  
4. 시작 및 활성화 권한 > 기본값 편집  
4.1 추가 > 고급 > 지금 찾기 > NETWORK SERVICE 추가 > 로컬 시작 허용 체크, 로컬 활성화 허용 체크  
4.2 추가 > 고급 > 지금 찾기 > User 추가 > 로컬 시작 허용 체크, 로컬 활성화 허용 체크