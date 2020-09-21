Vagrant는 https://www.vagrantup.com/downloads.html에서 받아서 설치하면 Oracle VM VIrtualBox까지 한 번에 설치가 된다. Vagrant를 통해서 개발환경 운영체제가 VirtualBox에 가상화되어 설치, 동작하게된다.

https://github.com/lekdw/book-dev-env에 Vagrant와 Ansible 설정파일을 공개해 놓았으니 그대로 이용하자. 실무에서도 배포와 관련된 설정파일은 git과 같은 SCM을 통해 관리하고 공유하면 매우 편리하다. 여기서는 git을 사용하므로 https://msysgit.github.io에서 Windows 콘솔에서 사용할 수 있는 msysgit를 받아서 설치한다. msysgit를 설치하면 따라오는 MinGW와 MSYS는 덤으로 Windows 콘솔에서 ssh 같은 POSIX 도구들을 사용할 수 있다.


적당한 위치에서 콘솔창을 열어 다음과 같이 book-dev-env 프로젝트를 clone 한다.

git clone https://github.com/lekdw/book-dev-env

위의 결과로 다음과 같은 book-dev-env 프로젝트 디렉토리와 설정 파일들이 생성된다.

