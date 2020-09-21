# UBUNTU

## VM

## NETWORK

<https://unabated.tistory.com/entry/VMware-%EC%9D%98-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B5%AC%EC%84%B1%EA%B3%BC-%EC%97%B0%EA%B2%B0%EC%9D%98-%EC%9D%B4%ED%95%B4>
<https://m.blog.naver.com/PostView.nhn?blogId=urasima76&logNo=110106676351&proxyReferer=https%3A%2F%2Fwww.google.com%2F>

1. Minimal 설치
2. 관리자 권한 구동
3. Window 10 문제 해결하기 : <https://freeprog.tistory.com/347>
4. Edit > Vistual Network Editor
    - <https://zrungee.tistory.com/207>
    - <https://aeac.tistory.com/27>
    - <https://inmile.tistory.com/23>
5. `sudo passwd root`
6. `sudo apt update`
7. 업데이트
8. `sudo apt install net-tools`
9. `sudo apt install curl`

## 통신확인

<https://ko.wikipedia.org/wiki/TCP/UDP%EC%9D%98_%ED%8F%AC%ED%8A%B8_%EB%AA%A9%EB%A1%9D>
<https://www.lesstif.com/pages/viewpage.action?pageId=24445571>
<https://brownbears.tistory.com/194>

53 PORT : DNS
631 PORT :

## 보안설정

<https://cyantai.tistory.com/32>
<https://blog.shako.net/ubuntu-server-16-04-initial-setup-guide/>

## 방화벽

<https://webdir.tistory.com/206>

## Shared VMs

<https://bimmermac.com/2747>

## NAT ubuntu 설정

<https://all-record.tistory.com/179>

## 복제(Clone)

1. 관리 > CLONE
2. 사용자삭제

## uBuntu SSH 설치

### SSH 서버 설치

```bash
# 서버만 설치
sudo apt-get install openssh-server
# 서버/클라이언트 설치
sudo apt-get install ssh
```

### SSH 서버 설정파일

/etc/ssh/sshd_config

```properties
Port 22
#ListenAddress 0.0.0.0
Protocol 2
#Protocol 1,2
```

## trouble shooting

문제발생

- VMware Workstation 10 으로 Ubuntu 12.04를 설치하려고 가상머신을 시작하는데 다음 에러메세지 출력.
- "Could not get vmci driver version: The handle is invalid.  You have an incorrect version of driver "vmci.sys". 
   Try reinstalling VMware workstation.

문제해결

- 현재 설치하려는 가상운영체제의 설치폴더로 이동(VMware를 통해서 지정한 폴더)
- [(내가지정한)가상머신이름].vmx 파일을 메모장으로 열기(예: Ubuntu - 64bit.vmx)
- vmci0.present 항목을 찾아서 값을 FALSE로 변경