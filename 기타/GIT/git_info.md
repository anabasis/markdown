# GIT

<https://huusz.github.io/2017/Git/GIT/01.git-connect-github/#2-원격-저장소-생성하기>

## Git vs Github

- Git은 분산 버전 관리 시스템이다.버전관리 시스템
- GitHub는 Git 저장소(repository)를 업로드 할 수 있는 웹사이트, 웹기반 인터페이스

## Git & Github 주요개념

- commit : 하나 또는 다수의 파일에 변경 내용을 저장할 때마다 새로운 commit 생성(이 변경 내용을 commit하고 이를 GitHub로 push)
- commit message : commit을 생서할 때 마다 왜 변결을 했는지에 대한 이유를 설명하는 메시지를 제공
- branch : 테스트를 해보거나 새로운 기능을 개발하기 위해 사용할 수 있는 따로 떨어진 독립적인 commit들(새로운 검색기능을 구현하기 위해 branch를 생성)
- master branch : 새로운 Git 프로젝트를 만들 때마다 ‘master’라고 불리는 기본 branch가 생성(배포할 준비가 되면 작업이 최종적으로 마무리되는 branch,master로 바로 commit 하면 안 됨)
- feature or topic branch : 새로운 기능을 개발할 때마다 작업할 브랜치를 만드는데, 이를 feature branch
- release branch : 직접 QA(품질보증) 작업을 하거나 고객의 요구로 구 버전의 소프트웨어를 지원해야 한다면 모든 수정이나 업데이트를 위한 장소로 release branch가 필요(feature branch와 release branch는 기능적 차이는 없지만, 팀월들과 프로젝트에 대해 이야기할 때 확연히 구별할 수 있어 유용)
- Merge : 병합(merge)은 한 branch에서 완성된 작업을 가져와 다른 branch에 포함하는 방법.(흔히 feature branch를 master branch로 merge)
- Tag : 특정 이력이 있는 commit에 대한 참조. 어떤 버전의 코드가 언제 배포(release)되었는지 정확히 알 수 있도록 제품 배포 기록에 가장 자주 사용
- Check out : 프로젝트 history의 다른 버전으로 이동해 해당 시점의 파일을 보기 위해 ‘checkout’(가장 일반적으로 branch에서 완료된 작업을 모두 보기 위해 branch를 checkout하지만, commit도 checkout)
- Pull request : 원래 pull request는 branch에서 완료된 작업을 다른 사람이 리뷰하고 master로 merge요청을 하기 위해 사용
- Issue : GitHub는 기능에 대해 논의하거나 버그를 추적하거나 혹은 두 가지 경우 모두 사용될 수 있는 Issue라는 기능
- Wiki : Ward Cunningham이 최초로 개발하였다. wiki는 링크들 간을 연결해 간단하게 웹페이지를 만드는 방법(GitHub 프로젝트는 문서 작성에 자주 Wiki를 사용)
- Clone : 종종 로컬로 작업하기 위해 프로젝트 복사본을 GitHub에서 다운로드(repository를 사용자의 컴퓨터로 복사하는 과정을 복제(cloning), “repository를 clone하고, 버그를 수정한 다음 오늘 밤 수정본을 다시 GitHub로 push)
- Fork : 때로는 프로젝트를 직접 변경하는 데 필요한 권한을 보유하지 못할 때가 있다. 잘 알지 못하는 사람이 작성한 오픈소스 프로젝트이거나, 회사에서 작업을 같이 많이 하지 않는 다른 그룹이 작성한 프로젝트일 수도 있다. 그러한 프로젝트를 변경하고 싶다면 먼저 GitHub의 사용자 계정에 프로젝트 복사본을 만들어야한다. 그 과정을 repository를 fork(그런다음 복제(clone)하고, 변경하고, pull request를 이용해 원본 프로젝트에 변경 내용을 반영, repository를 fork하고 수정안과 함께 pull request를 제출)

## Git :: 주요 명령어

### 1. Git 작업환경 시작하기 (see also: git help tutorial)

- clone 새로운 디렉터리에 Repository를 복제한다.
- init 새로운 빈 Repository를 생성하고, 기존 존재하던 것들을 초기화한다.

### 2. 현재 변경작업 (see also: git help everyday)

- add 인덱스에 파일 내용을 추가 Add file contents to the index
- mv 파일 이동 또는 파일 명 변경 또는 심볼릭 링크의 이름변경 (Move or rename a file, a directory, or a symlink)
- reset 지정된 상태로현재 HEAD 리셋 (Reset current HEAD to the specified state)
- rm 작업 트리(Working tree)에서 인덱스 파일을 제거(Remove files from the working tree and from the index)

### 3. 작업이력과 현재 상태를 검사 (see also: git help revisions)

- bisect 이진 검색으로 버그 변경을? 찾는다.( Find by binary search the change that introduced a bug)
- grep Print lines matching a pattern
- log Commit 로그를 보이게한다. (Show commit logs)
- show 다양한 종류의 객체를 보이게 한다.(Show various types of objects)
- status 작업트리(Working tree)를 보이게한다.(Show the working tree status)

### 4. 성장, 표시, 당신의 최근 변경 이력

- branch 브런치를 생성하거나 목록을 만든다. (List, create, or delete branches)
- checkout ??Switch branches or restore working tree files
- commit 저장소(Repository)에 변경사항을 기록한다.(Record changes to the repository)
- diff 두 개 이상의 변경사항 (Commit과 작업트리의 Commit등)을 표시 변경? (Show changes between commits, commit and working tree, etc)
- merge 두 개 이상의 개발 과정을 병합 (Join two or more development histories together)
- rebase Forward-port 지역 저장소의 Commit을 업데이트 된 상위 HEAD에 전달. (Forward-port local commits to the updated upstream head)
- tag 다양한 태그 객체를 생성, 삭제 또는 확인한다. (Create, list, delete or verify a tag object signed with GPG)

### 5. 협업 관련 (see also: git help workflows)

- fetch 다른 저장소에서 개체를 다운로드하고 바로 병합하지는 않는 상태(Download objects and refs from another repository)
- pull 원격 저장소(Repository)에서 로컬저장소로 가져와 로컬지점과 통합한다.)Fetch from and integrate with another repository or a local branch)
- push 관련 객체를 가지고 원격저장소에 업데이트 위해 보낸다. 바로업데이트는 되지않는다.(Update remote refs along with associated objects)