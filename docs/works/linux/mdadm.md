---
title: MDADM
---

2011-02-18 ~ 2015-12-24

### 소개

리눅스 소프트웨어 레이드시스템이다.


### 한계

레이드 시스템은 Data Corruption 에 대한 대응은 하지 않는다.
오직 디스크가 고장난 상황에만 대비한다.
dd 같은 명령으로 디스크에 엉뚱한 내용을 쓰더라도 슈퍼블럭이 망가지지 않는한
레이드 시스템은 이것을 감지해 내지 못한다.


### 문서들

인터넷에 리눅스 레이드에 대한 많은 글이 떠돌아 다니고
오라일리 동물책도 있고 우분투 문서도 있는데 다 구버전이다.
아래 문서를 봐야 한다.

<https://raid.wiki.kernel.org/index.php/Linux_Raid>{:target="_blank"}
<https://wiki.archlinux.org/index.php/RAID>{:target="_blank"}


### 디스크 구성

하드 디스크를 통채로 쓸 수도 있고 파티션을 나눠 쓸 수도 있다.
뭐가 좋을까 궁금했는데, 그다지 큰 차이는 없고 위키에 보니 아래 문구가 있다.

"Neil, the md/mdadm author, uses whole disks."


### 설치

커널 지원은 커널에 기본으로 심어져 있다.
필요한 코멘드라인 유틸리티만 설치하면 된다.


우분투에서 설치

    $ apt-get install mdadm --no-install-recommends

    --no-install-recommends 를 안 주면 권장 항목들까지 설치되면서 필요도 없는 postfix 가 설치된다.

아치에서 설치

    $ pacman -S mdadm


### 디스크 확인

사용할 디스크들이 마련되어 있나 먼저 확인한다.
아래 명령들중 좋아하는 것으로.

    $ cat /proc/partitions

    $ blkid

    $ ls -l /dev/disk/by-id/
    $ ls -l /dev/disk/by-uuid/

    $ lshw -class disk


### 레이드 생성

디스크 두 대일 때는 미러하고 여러 대라면 꼭 RAID 6 를 쓴다. RAID 5 는 너무 위험하다.

mdadm 은 디스크에 메타데이터를 기록해야 하는데 버전이 4 가지 정도 있다.
기본이 0.9 로 지정되는데 이게 오래되서 쓰지 말라는 것 같다.
0.9 를 쓰면 2TB 이상 기기를 연결할 수 없는 단점이 있는데 이게 치명적이라 곧 사라질 듯.
최신 버전은 1.2 이다.
버전 별로 차이점은 메타데이터가 기록되는 위치가 다 다르다.
디바이스 뒤에, 앞에, 앞에서 4 킬로 떨어진데 등등.

파티션을 나눈 후 파티션 단위로 묶어도 되는데
예에서는 그냥 디스크들을 통으로 묶겠다.

전에 레이드에 사용되었던 디스크라면 먼저 메타 데이터를 지운다.

    $ mdadm --zero-superblock /dev/sdx

RAID 1 타입 생성.

    $ mdadm --create --verbose /dev/md0 --metadata 1.2 --level=mirror --raid-devices=2 /dev/sd[ab]

RAID 6 타입 생성.

    $ mdadm --create --verbose /dev/md0 --metadata 1.2 --level=6 --raid-devices=4 /dev/sd[abcd]

생성시 스페어 지정하려면 아래 옵션 추가

    $ mdadm ... --spare-devices=1 /dev/sde

    --assume-clean : 레이드 초기 생성시 싱크를 안 하게 할 수 있는 옵션이나 절때 쓰지 말라는 경고가 많다.


### 레이드 상태 확인

    $ cat /proc/mdstat
    $ watch cat /proc/mdstat

Personalities 는 커널이 지원하는 RAID 레벨들 표시.

레이드 빌드가 완료되지 않더라도 재부팅하거나 레이드 디바이스를 사용할 수 있다.


### 인식

재부팅하면 레이드가 자동인식 안 되어있다. 

레이드를 만들 때는 자동으로 이름이 주어진다. ‘Name’ 필드 확인.

    $ mdadm --examine /dev/sda

해당 이름의 레이드를 다시 조립한다.

    $ mdadm --assemble /dev/md0 --name "uglyduck:1"

다시 레이드가 보인다.

    $ cat /proc/mdstat


### 부팅시 레이드 자동 인식

재부팅시 레이드를 인식시키기 위해서는 mdadm.conf 파일에 ARRAY 행을 넣어줘야 한다.
넣어줄 값들은 아래 명령으로 덤프할 수 있다.

    $ mdadm --detail --brief /dev/md0

아래 명령으로 설정파일 끝에 추가한다.

    $ mdadm --detail --brief /dev/md0 >> /etc/mdadm/mdadm.conf

sudo 를 써야할 경우

    $ sudo bash -c "mdadm --detail --brief /dev/md0 >> /etc/mdadm.conf"

재부팅하면 레이드가 인식되어 있다.


### 포멧

    $ sudo mkfs.ext4 -m 0 /dev/md0

마운트

    $ sudo mkdir /data
    $ sudo chown drypot:wheel /data
    $ sudo mount /dev/md0 /data

    $ sudo lsblk -f


### /etc/fstab

    UUID=... /data ext4 defaults 0 2


### 부팅 파티션 미러

안 하는 것이 좋다. 여러 가지 문제가 발생.

부팅 파티션 자체를 미러 레이드 잡는 경우 initrd 이미지 안의 mdadm.conf 를 업데이트 해야한다.
자세한 방법은 아치 위키 참고.

    $ update-initramfs -u


### 어레이 정보

어레이 정보.

    $ mdadm --detail /dev/md1

디스크 슈퍼블럭 정보.

    $ mdadm --examine /dev/sda1

관련 로그.

    /var/log/messages


### 메일 알림

mdadm 을 데몬으로 띄워서 레이드 상태를 모니터링 하는 기능이 있다.

/etc/mdadm.conf 파일의 MAILADDR 필드를 수정한다.

postfix / ssmtp / msmtp 중에 하나를 설치해 둔다. 메일 보내는 서비스.
아치 위키에 dma 가 몇 줄 언급되어 있는데 메일을 시스템 외부로는 못 보낸다니 패스.

우분투에서 mdadm 패키지를 설치하면 init.d 스크립트 통해서 모니터가 자동으로 뜨게 되어 있다.
현재 우분투 머신이 없어서 자세한 내용은 생략.

아치도 마찬가지로 mdmonitor.service 가 systemd 에 등록되어 있다.
disabled 되어 있으면 가동.

    $ systemctl status mdmonitor.service
    $ systemctl start mdmonitor.service

Arch 에서 mdadm.service 와 mdmonitor.service 가 중복되서 하나가 fail 나는 경우가 있다.
하나는 Arch 문화에서 하나는 Opensuse 문화에서 왔다는 것 같음. 논의중.
큰 문제는 없어 보임.

테스트 메일 발송.

    $ mdadm --monitor --scan -1 --test

메일이 와야 한다.


### 이상 발생시

고장난 디스크명을 확인한다.

    $ cat /proc/mdstat

고장난 디스크에 대한 시리얼을 확인한다.

    $ lshw -class disk

어레이에서 고장난 디스크 분리

    $ mdadm --remove /dev/md0 /dev/sda

본체 열고 시리얼 확인해서 디스크 물리적으로 교환.

어레이에 새 디스크 추가

    $ mdadm --add /dev/md1 /dev/sda


### 레이드 고장 시뮬레이션

메일 알림이 설정되어 있다면. 레이드를 고장내 본다.

    $ mdadm --manage --set-faulty /dev/md0 /dev/sdc

메일이 바로 온다.

디스크를 분리했다가.

    $ mdadm --remove /dev/md0 /dev/sdc

다시 추가해 주면.

    $ mdadm --add /dev/md0 /dev/sdc

리빌드를 시작한다.


### 레이드 강제 해산

레이드 강제 해산

    $ mdadm --stop /dev/md0



