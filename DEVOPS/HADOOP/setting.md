# HADOOP 설치

## CentOS 설치

10.10.10.21
10.10.10.22
10.10.10.23

```properties
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="no"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="ccf8941c-221c-4562-9b69-b4097b19d56c"
DEVICE="enp0s3"
ONBOOT="yes"
IPADDR="10.10.10.11"
PREFIX="24"
GATEWAY="10.10.10.1"
DNS1="168.126.63.1"
DOMAIN="168.126.63.2"
```

## Virtualbox Guest Additions

```bash
//gcc, make, kernel-dvel 등
yum groupinstall "Development Tools"
yum update
reboot
# Guest Addition 설치하기

#//cdrom 에 있는 내용들 모두(-r) /media 디렉토리에 연결
mount -r /dev/cdrom /media

/media/VBoxLinuxAdditions.run
reboot

```

```bash
# yum install openssh-server openssh-clients openssh-askpass
vi /etc/ssh/sshd_config

# firewall-cmd --permanent --zone=public --add-port=22/tcp
```
