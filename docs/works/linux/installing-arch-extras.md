---
title: Installing Arch Extras
---

개인적으로 추가 설치하는 모듈들.

## MDADM

MDADM 레이드 생성


## Data directory

    # mkdir /data
    # chown drypot:wheel /data
    # mount <device> /data

볼륨의 uuid 확인

    $ lsblk -f

    $ ls -l /dev/disk/by-uuid

fstab 에 uuid 추가

    /etc/fstab

    UUID=.... /data2 ext4 defaults 0 2
    UUID=.... /data3 ext4 defaults 0 2


## Bash

    ~/.bashrc

    alias lla='ls -alFv'
    alias ll='ls -lFv'
    alias la='ls -CFav'
    alias l='ls -CFv'

## Base-devel

C, make 등 기본 컴파일 도구 설치

    # pacman -S base-devel

## Git

    # pacman -S git


## Postfix

    # pacman -S postfix
    # systemctl enable postfix

설정.

    /etc/postfix/main.cf

    myhostname = ... <-- hostname 에서 자동으로 가져온다.
    mydomain = ... <-- hostname 에서 자동으로 가져온다.

    mydestination = $mydomain <-- 찾아서 언코멘트
    myorigin = $mydomain <-- 찾아서 언코멘트
    mynetworks_style = host <-- 찾아서 언코멘트

메일 발송 테스트. raysoda 프로젝트 루트에서.

    $ node server/mailer/mailer2-test-send.js

메일을 발송하려면 DNS SPF 레코드에 메일 서버 IP를 등록해놔야한다.

    v=spf1 include:aspmx.googlemail.com ip4:219.254.35.24 ip4:219.254.35.25 ip4:219.254.35.26 ip4:219.254.35.27 ~all

## Nginx

    # pacman -S nginx
    # systemctl enable nginx

기본 설정 파일 수정

    /etc/nginx/nginx.conf

    include /data/nginx/nginx.conf;

로컬 설정 파일 수정

    /data/nginx/nginx.conf

    ...

## certbot

Let's Encrypt 클라이언트 설치.

    # pacman -S certbot
    

## MonogoDB

    # pacman -S mongodb
    # systemctl enable mongodb

    # mkdir /data/mongodb
    # chown mongodb:daemon /data/mongodb

설정.

    /etc/mongodb.conf

    dbpath=/data/mongodb

테스트.

    # systemctl start mongodb

    ...

mongo 에서 transparent_hugepage/enabled, transparent_hugepage/defrag 를 never 로 하라고 경고 뜨면, 
/etc/tmpfiles.d/local.conf 파일에 아래 내용 추가하고 재부팅

    w /sys/kernel/mm/transparent_hugepage/enabled - - - - madvise
    w /sys/kernel/mm/transparent_hugepage/defrag - - - - madvise
    w /sys/kernel/mm/transparent_hugepage/khugepaged/defrag - - - - 0


## Redis

    # pacman -S redis
    # systemctl enable redis

설정.

    /etc/redis.conf

    save 로 시작하는 부분 코멘트해서 디스크 저장을 못하게 한다.


## Node

    # pacman -S nodejs
    # pacman -S npm

    # pacman -S python2 <— npm gyp 등에서 사용.

    # npm config set prefix ~/npm <- sudo 없이 npm install -g 를 쓸 수 있다.

.bashrc 에 다음을 추가

    PATH=$PATH:~/npm/bin
    export PYTHON=python2

mocha, bower 등을 설치

    # npm install -g mocha

    ...

## Installing App Servers

해당 프로젝트의 INSTALL.md 참고.

