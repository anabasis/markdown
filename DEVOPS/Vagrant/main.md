# VirtualBox와 Vagrant의 기본 사용법

프로그램의 개발환경을 구축할 때 이제는 VirtualBox나 VMware같은 가상머신을 사용하는 것이 보편화 되었습니다. 가상머신 위에서 개발을 할 경우 심적인 부담없이 자유롭게 개발을 할 수가 있습니다. 예를들어 여러가지 오픈소스를 설치하여 사용 하더라도 다른 프로그램과 충돌이 날 것에 대해 걱정을 하지 않아도 되고 혹시라도 시스템이 죽거나 복구가 불가능할 경우에는 가상머신을 제거하고 다시 설치하여 사용하면 되므로 안심하고 개발을 할 수 가 있습니다.

또한 Vagrant를 활용하여 가상머신을 좀더 편하게 사용할 수 도 있습니다.

가상머신 (VirtualBox)
가상머신은 아무 OS도 설치되어 있지 않은 가상의 데스크탑 이라고 보면 됩니다. 그 가상머신에 windows를 설치하던지 리눅스를 설치하던지 MAC OSX를 설치하던지는 설치하는 사람의 마음입니다.

가상머신의 종류로는 크게 VirtualBox나 VMware가 있으며 여기서는 VirtualBox의 설치에 대해서만 간략히 정리하도록 하겠습니다.

VirtualBox는 오라클에서 관리르 하고 있으며 프리웨어로 제공을 하고 있기 때문에 무료로 설치해서 사용할 수 있습니다.

설치
VirtualBox는 아래의 URL에 접속하여 다운로드를 받으면 됩니다. 설치하는 호스트 머신의 시스템에 따라 Windows hosts, OS X hosts, Linux distributions, Solaris hosts중 하나를 선택하여 다운로드 하면 됩니다.

버츄얼박스 다운로드

VirtualBox 홈페이지

Windows에 설치하는 경우 설치파일을 실행하고 순서대로 진행하여 설치하면 쉽게 설치가 가능합니다.

간단하지만 이것으로 가상머신의 설치는 끝났고 대부분의 설정은 Vagrant에서 진행합니다.

Vagrant
Vagrant는 가상머신을 편리하게 사용할 수 있도록 도와주는 프로그램입니다. 이미 누군가가 Vagrant로 설정해 놓은 가상머신 Box를 간단한 명령어로 손쉽게 설치 할 수 있으며 가상 머신과 호스트 머신과의 환경설정도 쉽게 할 수 있습니다.

설치
Vagrant 다운로드 페이지에서 호스트 머신의 시스템에 따라 Debian Linux*, Centos Linux, Windows, Mac OS X**중 하나를 선택하여 다운로드 하면 됩니다.

Vagrant 다운로드

Vagrant 홈페이지

Windows에 설치하는 경우 설치파일을 실행하고 순서대로 진행하여 설치하면 쉽게 설치가 가능합니다.

가상머신 추가
Vagrant를 설치했다면 Vagrant를 이용하여 Vagrant Box를 추가해야 합니다. Vagrant Box는 누군가가 미리 설정해 놓은 가상머신 Box 파일이며 명령어 한줄로 Vagrant Box를 쉽게 추가 할 수 있습니다.

Vagrant Cloud 홈페이지

누군가가 미리 설정해 놓은 Vagrant Box는 Vagrant Cloud 웹사이트에서 검색을 통해 찾을 수 있습니다.

Vagrant를 이용하여 가상머신을 추가하는 방법입니다.

윈도우에서는 명령 프롬프트(CMD), Mac OS X에서는 터미널에서 명령어를 입력하시면 됩니다.

먼저 vagrant init 명령어로 Vagrantfile를 생성해야 합니다. 가장 기본적인 vagrant의 설정파일이 생성됩니다. 이 설정파일을 변경하면서 나에게 맞는 설정을 하면서 사용하면 됩니다.

> vagrant init
Vagrantfile이 생성되었으면 해당 문서를 열어서 설정을 변경해야 합니다.

Vagrantfile 파일의 15라인 쯤에 있는 config.vm.box = "base"명령어를 아래와 같이 바꾸면 Vagrant를 실행할 때 Vagrant Cloud에서 centos65-x64 가상머신을 자동으로 다운받습니다.

  # config.vm.box = "base"
  config.vm.box = "puphpet/centos65-x64"
가상머신 실행 (Vagrant 실행)
이제 설정한 Vagrant를 실행하면 됩니다. Vagrant 실행이 처음이라면 가상머신을 다운로드받아 설치를 하고 기동을 하고 이미 설치가 되어 있다면 그냥 기동만 하게 됩니다.

> vagrant up --provider virtualbox
가상머신 접속 (로그인)
가상머신이 실행되었으면 가상머신에 접속을 해야 합니다. 접속은 vagrant ssh명령어로 접속을 합니다. Mac OS X는 터미널에서 윈도우에서는 CMD창 이나 Putty(푸티)나 Poderosa(포데로사)같은 별도의 SSH클라이언트 프로그램으로 접속하여 사용하면 됩니다.

터미널이나 CMD창에서의 명령어는 아래와 같습니다.

> vagrant ssh
SSH클라이언트를 사용할 경우 기본적으로 아래의 정보로 접속을 하면 됩니다.

Host : 127.0.0.1
Port : 2222
Username : vagrant
Password : vagrant
Private key : c:/vagrant/.vagrant/machines/default/virtualbox/private_key
웹서버를 사용하기 위한 설정
호스트머신에 띄워져있는 가상머신의 웹서버에 접속을 하기위해서는 Vagrantfile에 몇가지 설정을 추가해야 합니다.

먼저 호스트머신으로 접속한 port(8080)를 가상머신의 port(80)로 전달을 해야 합니다. config.vm.network "forwarded_port", guest: 80, host: 8080

그리고 가상머신의 아이피주소를 설정을 해 주어야 합니다. config.vm.network "private_network", ip: "192.168.33.10"

마지막으로 가상머신과 호스트머신과의 실시간 자동으로 동기화되는 폴더를 설정해야 합니다. 호스트머신에서 소스파일을 수정하게 되면 바로 가상머신의 소스파일도 변경이 되기때문에 개발작업은 호스트머신에서 실행은 가상머신에서 하는 것이 가능해 집니다.

  # 가상머신의 80포트를 호스트머신의 8080 포트에 할당함
  config.vm.network "forwarded_port", guest: 80, host: 8080
  #
  # 가상머신의 IP를 아래 설정한 IP주소에 할당함
  config.vm.network "private_network", ip: "192.168.33.10"
  #
  # 가상머신의 폴더와 호스트 머신의 폴더를 공유함(동기화)
  # 별도로 세팅을 하지 않은경우 호스트머신의 vagrant설정이 있는 폴더와 가상머신의 /vagrant 폴더가 동기화 됨
  # config.vm.synced_folder "호스트머신의 경로", "가상머신의 경로"
폴더공유 에러 발생 시 : 호스트머신에 플러그인을 설치하여 관리하면 편리합니다.
> vagrant plugin install vagrant-vbguest
기본적인 Vagrant 명령어 모음
명령어	설명
vagrant up	가상머신 기동
vagrant status	가상머신 상태 확인
vagrant ssh	가상머신에 접속
vagrant halt	가상머신 정지
vagrant suspend	가상머신 휴면
vagrant resume	가상머신 휴면에서 복원
vagrant reload	가상머신 재시동
vagrant destroy	가상머신 제거