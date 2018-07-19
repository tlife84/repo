---
title: Ambari 설치
tags: [Big Data & Machine Learning]
keywords: ambari, install, 암바리, 설치
published: false
summary: "추후에 정리하자"
sidebar: mydoc_sidebar
permalink: mydoc_install_ambari.html
folder: mydoc
---

**[Ambari Quick Start Guide](https://cwiki.apache.org/confluence/display/AMBARI/Quick+Start+Guide)** 참고

* Virtual Box 설치
* vagrant 1.9.6 설치: 이보다 상위 버전에서는 ./up.sh 실행 시 Powershell 오류가 발생할 수 있음
* git설치

## Setup
git bash에서 실행
ambari-vagrant가 설치되길 원하는 상위디렉토리에서
git clone https://github.com/u39kun/ambari-vagrant.git

ambari-vagrant/append-to-etc-hosts.txt에 있는 내용을 C:\windows\system32\drivers\etc\hosts에 추가

cd ambari-vagrant
cd centos6.8
./up.sh 3
3이라는 숫자는 원하는 VM 클러스터 갯수

vagrant ssh c6801

Oracle VirtualBox에 login창이 뜨면
vagrant / vagrant로 로그인

여기서부터는 centos 쉘
sudo su -
wget -O /etc/yum.repos.d/ambari.repo http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0/ambari.repo
yum install ambari-server -y
ambari-server setup -s
ambari-server start


## Snapshot
os 디렉토리에서
../snapshot_tool/snapshot.sh take 1 3 init
1은 첫 번째 VM
3은 마지막 VM
init은 snapshot 이름

## 폴더공유
머신선택 > 설정 > 공유폴더 > 폴더 추가

![](image/1.png)

리눅스 쉘에서 아래 명령어 실행

```
mkdir <공유폴더명>  
mount -t vboxsf <VirtualBox에서 지정한 공유폴더이름> <공유폴더명>
```

## Zeppelin interpreter 환경설정
오른쪽 상단 계정 클릭 > Interpreter

* spark 인터프리터의 Properties에 SPARK_HOME=/usr/hdp/current/spark-client 추가

* 최상단 `+Create` 클릭
    - Interpreter Name: 'spark2'
    - Interpreter group: spark
    - Save

* 추가된 spark2 인터프리터의 Properties에 SPARK_HOME=/usr/hdp/current/spark2-client 추가