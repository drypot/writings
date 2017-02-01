---
title: Simple Let's Encrypt
---

<https://letsencrypt.org>{:target="_blank"}

<https://wiki.archlinux.org/index.php/Let’s_Encrypt>{:target="_blank"}

Let's Encrypt 는 무료 웹서버 인증서를 나눠주는 서비스입니다.

다향한 OS, 다향한 웹서버, 다양한 구성이 있을 것이기에 LE 에서도 엄청나게 다양한 시나리오를 제공합니다.

저는 Arch Linux 를 쓰고 있고, nginx 웹서버를 쓰고 있고, 서버 관리자입니다.
아래 내용은 제 경우의 시나리오입니다.


## 원리

www.example.com 이란 웹 사이트를 호스팅하고 있다면
이 사이트의 /.well-known 디렉토리에 LE 에서 쓰라는 것을 적어서 사이트 인증을 받습니다.
LE 에서는 이 사이트에 80 포트로 접근합니다. (더 다양한 시나리오가 있지만 제 시나리오에선 그렇습니다.)
이것만 하면 인증서를 보내줍니다.

받은 인증서를 적당한 폴더에 보관합니다. 프라이빗키, 인증서, 기타 몇몇 파일이 셋트입니다.

nginx 설정파일에서 이 인증서를 사용하도록 링크합니다.

이 작업을 certbot 이란 커맨드가 어느 정도 자동화 해줍니다. 


## certbot 설치

<https://certbot.eff.org>{:target="_blank"}

certbot 을 설치합니다. LE 에서 이게 좋다니 이걸 설치합니다.

    # pacman -S certbot

pacman 은 Arch 에서 쓰는 패키지 매니저입니다.
환경 따라 쓰고 계신 패키지 매니저 쓰시면 됩니다. 우분투에서는 apt 겠죠.


## 공용 /.well-known 의 필요성

nginx 서버가 이미 여러 서비스를 돌리고 있는 상황을 가정합니다.

우리는 LE 가 '항상' 각 사이트의 또는 앞으로 만들 사이트의 /.well-known 에 접근할 수 있게 해야합니다.
처음 인증서를 받기 위해서도 필요하지만 3 개월 마다 자동으로 업데이트하려면 항상 접근할 수 있어야 합니다.

사이트를 계속 추가할 텐데 사이트 마다 이 설정을 넣으려면 귀찮을 겁니다.

각 사이트의 실제 루트 디렉토리에 .well-known 폴더가 생기는 것도 멋진 일은 아닐 겁니다.

해서 모든 사이트가 공용으로 사용할 .well-known 폴더를 만들고 nginx 에서 모든 사이트가 여기를 쓰도록 설정합니다.


## 공용 /.well-known 폴더 생성

다름 명령으로 적당한 위치에 공용 .well-known 을 만듭니다. Arch 환경 기준입니다.

    # mkdir -p /var/lib/letsencrypt/.well-known
    # chgrp http /var/lib/letsencrypt
    # chmod g+s /var/lib/letsencrypt

g+s: 이 디렉토리에 생성되는 하위 파일들은 디렉토리의 그룹을 계승한다는 의미입니다.


## nginx 에 /.well-known 을 링크

서비스 중인 모든 웹사이트가 https 사용하고 있다면
80 포트는 다음과 같은 기본 서버 설정으로 사용하고 계셨을 겁니다.
전 사이트 https 쓰고 계셨죠???

    server {
        listen 80 default_server;
        server_name _;
        return 301 https://$host$request_uri;
    }

위 설정은 이 기계로 들어오는 모든 http 요청을 https 로 리다이렉트하는 겁니다. 영원히;

LE 는 80 포트로 .well-known 을 찾기 때문에 아래와 같이 기본 사이트에 .well-known 을 링크해 두면
앞으로 .well-known 설정을 반복할 필요가 없어집니다.

    server {
        listen 80 default_server;
        server_name _;
        
        location ^~ /.well-known {
          allow all;
          alias /var/lib/letsencrypt/.well-known/;
          default_type "text/plain";
          try_files $uri =404;
        }

        location / {
            return 301 https://$host$request_uri;
        }
    }

www.example.com:80/.well-known 도 이리로 올 것이요, www.future-cool.com:80/.well-known 도 이리로 올 겁니다.

혹시 80 포트로 실 서비스 중인 사이트가 있다면 해당 사이트의 인증서를 받기 위해선
위 location ... well-known 을 사이트마다 복사 또는 인클루드해야 합니다.

nginx 설정을 reload 해서 LE 가 우리 사이트 .well-known 에 접근할 수 있도록 합니다.

    # systemctl reload nginx


## 인증서 받기

certbot 으로 인증서를 받는 굉장히 다양한 방법들이 있는데 전 옛날 사람이라 제일 원시적이고 이해하기 쉬운 방법을 택했습니다.

    certbot certonly --email drypot@gmail.com --webroot -w /var/lib/letsencrypt/ -d raysoda.com -d www.raysoda.com -d file.raysoda.com

--webroot 로 certbot 이 받아쓰기할 곳을 알려줍니다. 우리의 공용 .well-known 웹루트입니다.

-d 는 인증서에 한꺼번에 담을 도메인 이름들입니다.
관련된 도메인들은 한꺼번에 담고 전혀 관련 없는 그룹은 certbot 명령을 여러번 실행하면 됩니다.

이미 nginx 에서 기본 80 사이트에 .well-known 을 연결해 놨기 때문에 LE 에서 들어와서 확인 잘 하고 인증서 줄 겁니다.

받은 인증서들은 /etc/letsencrypt/live/raysoda.com/* 에 링크가 됩니다.

## nginx ssl 사이트 설정

ssl 설정중 꽤 많은 양이 중복되므로 인크루드할 파일을 하나 만듭니다. 제 경우는 /data/nginx/ssl.inc 로 생성.
자세한 의미는 저도 모릅니다. 하라는 대로; 좋은 게 좋은 거;

    /data/nginx/ssl.inc

    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:50m;
    ssl_session_tickets off;
    ssl_protocols TLSv1.2;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
    ssl_prefer_server_ciphers on;
    add_header Strict-Transport-Security max-age=15768000;
    ssl_stapling on;
    ssl_stapling_verify on;

메인 사이트.

    server {
      listen 443 ssl http2;
      ssl_certificate /etc/letsencrypt/live/raysoda.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/raysoda.com/privkey.pem;
      ssl_trusted_certificate /etc/letsencrypt/live/raysoda.com/chain.pem;
      include /data/nginx/ssl.inc;

      server_name raysoda.com;

      ...
    }

같은 인증서를 사용하는 보조 사이트.

    server {
      listen 443 ssl http2;
      ssl_certificate /etc/letsencrypt/live/raysoda.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/raysoda.com/privkey.pem;
      ssl_trusted_certificate /etc/letsencrypt/live/raysoda.com/chain.pem;
      include /data/nginx/ssl.inc;

      server_name file.raysoda.com;

      ...
    }

여러분이 신이 아니라면 설정 파일 신텍스가 다 맞는지 한번 검사.

    # nginx -c /etc/nginx/nginx.conf -t

nginx 재기동.

    # systemctl reload nginx

여기까지 하면 https 로 사이트에 접속할 수 있습니다.

## 인증서 갱신

환경마다 서비스 관리하는 법이 다를 것인데 아래는 systemd 기준입니다.

인증서 업데이트 자동화 1 단계.

    /etc/systemd/system/certbot.service

    [Unit]
    Description=Let's Encrypt renewal

    [Service]
    Type=oneshot
    ExecStart=/usr/bin/certbot renew --quiet --agree-tos
    ExecStartPost=/bin/systemctl reload nginx.service

인증서 업데이트 자동화 2 단계.

    /etc/systemd/system/certbot.timer

    [Unit]
    Description=Daily renewal of Let's Encrypt's certificates

    [Timer]
    OnCalendar=daily
    RandomizedDelaySec=1day
    Persistent=true

    [Install]
    WantedBy=timers.target

스타트.

    # systemctl enable certbot.timer
    # systemctl start certbot.timer

별 문제가 없다면 이제 인증서는 3 개월 마다 영원히 자동 업데이트됩니다.
