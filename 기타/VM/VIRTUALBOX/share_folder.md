# 공유폴더

<https://everydayminder.wordpress.com/2013/10/23/virtualbox-linux-%ED%98%B8%EC%8A%A4%ED%8A%B8-windows%EA%B0%84-%ED%8F%B4%EB%8D%94-%EA%B3%B5%EC%9C%A0-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/>

## Virtualbox

1. 장치 - 게스트 확장설치 : 게스트 확장 설치
2. 장치 - 공유폴더 : 공유폴더 추가(이름,경로)

## Guest PC

1. (게스트 확장설치 실패시)mount /dev/cdrom /mnt/addons
2. 라이브러리 설치
3. sh VBoxLinuxAdditions.run
4. mount -t vboxsf SPLUNK_APPS /opt/splunk_dev/etc/apps

```bash
# 마운트
mount -t vboxsf SPLUNK_APPS /opt/splunk_dev/etc/apps
```

```bash
# 기동시 자동 mount
./splunk enable boot-start
(./splunk enable boot-start -user 사용자계정)
vi /etc/init.d/splunk
mount -t vboxsf SPLUNK_APPS /opt/splunk_dev/etc/apps

# 기동시 자동 mount 제거
./splunk disable boot-start
```

```bash
# 오류 1
/sbin/mount.vboxsf: mounting failed with the error: No such device

# 조치
yum -y install kernel-headers kernel-devel
yum -y install kernel*
yum install gcc dkms make bzip2
yum install "kernel-devel-uname-r == $(uname -r)"
yum update
```

```bash
# 오류 2
[root@centos7 ~]# sh /mnt/addons/VBoxLinuxAdditions.run
Verifying archive integrity... All good.
Uncompressing VirtualBox 5.2.8 Guest Additions for Linux........
VirtualBox Guest Additions installer
/usr/sbin/vbox-uninstall-guest-additions: line 9: /opt/VBoxGuestAdditions-5.2.2/uninstall.sh: 그런 파일이나 디렉터리가 없습니다
Failed to remove existing installation.  Aborting...

# 조치
# 다음 파일을 삭제
[root@centos7 ~]# ls /usr/sbin/vbo*
/usr/sbin/vbox-greeter  /usr/sbin/vbox-uninstall-guest-additions
[root@centos7 ~]# rm  /usr/sbin/vbox-greeter /usr/sbin/vbox-uninstall-guest-additions

sh VBoxLinuxAdditions.run
```

```bash

# 조치
yum -y remove kernel-devel*
yum -y install kernel-devel*
yum -y remove kernel-headers*
yum -y install kernel-headers*
**재기동**
```

```bash
[root@centos7 etc]# mount -t vboxsf SPLUNK_APPS /opt/splunk_dev/etc/apps
/sbin/mount.vboxsf: mounting failed with the error: Protocol error

# 조치
공유폴더 추가
```
