
selinux
방화벽

firewall-cmd --permanent --zone=public --add-port=2222/tcp
firewall-cmd --reload

TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="no"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="bd012aa4-b5ac-43c3-9a24-89b0885407fb"
DEVICE="enp0s3"
ONBOOT="yes"
IPADDR="10.0.2.31"
PREFIX="24"
GATEWAY="10.0.2.1"
DNS1="168.126.63.1"
DOMAIN="168.126.63.2"

yum groupinstall "Development Tools"
yum update

- 오류

```bash
# 오류
Kernel headers not found for target kernel 4.18.0-193.6.3.el8_2.x86_64. Please install them and execute /sbin/rcvboxadd setup
ValueError: File context for /opt/VBoxGuestAdditions-6.0.22/other/mount.vboxsf already defined
modprobe vboxguest failed

# dnf update -y
# dnf install kernel-devel make gcc -y

패키지를 설치했습니다. $ sudo dnf isntall elfutils-libelf-devel 다음을
사용하여 추가 사항을 다시 작성합니다. $ sudo / usr / sbin / rcvboxadd setup
```
