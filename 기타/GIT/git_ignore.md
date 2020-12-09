# GIT IGNORE 예제

<https://ghwlchlaks.github.io/gitignore-filelist>

```gitignore
# : comments

# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf

*.dll
*.exe
*.o
*.so
*.com
*.class
#=> 컴파일된 결과물입니다. 올릴 이유가 없겠죠?
#=> *은 모두들 아시다시피 전체를 뜻하는 별칭입니다. 앞으로 많이 사용됩니다~

*.log
logs/
#=> 로그파일은 개발자마다 다르니 올릴 이유가 없겠죠?
#=> 폴더를 등록할경우 위에처럼 logs/ 선언하면됩니다. 그러면 logs폴더 내부에 모든 파일이 제외 대상입니다.


*.xlsx
*.xls
*.pdf
*.docs
*.ppt
*.pptx
#=> 문서형식은 올리지 않도록 설정합니다. 위에 형식이 자주 사용하는 파일 형식이죠?
#=> 물론 필요에 따라서 문서를 올리기도 합니다 그때는 적절히 .gitignore파일을 수정해주면 되겠죠?

*.dmg
*.iso
*.tar
*.zip
*.gz
#=> 압축파일 형식입니다. 


.Trashes
.Thumbs.db
.ehthumbs.db
#=> 개발때 사용한 운영체제별 시스템 파일은 제외해야겠죠?

#=> Eclipse
.metadata
bin/
tmp/
*.tmp
*.bak
*.swp
local.properties
.settings/
.loadpath
.recommenders
.project
.externalToolBuilders/
*.launch
.classpath
.factorypath
.buildpath
.target
.springBeans
.recommenders/
-----------------------------------------------------------------------------
#=> VSCode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json
#=> 이클립스를 사용하면 생성되는 설정 파일 혹은 임시 파일들은 제외합니다. 
#=> 해당 정보는 개발자마다 PC환경이 다르기 때문에 설정 정보를 넣어서 push하게 된다면
#=> 해당 내용을 받는 개발자들에 이클립스 환경에 덮어쓰기가되어 동작하지않는 이슈가 생길 수 있습니다. 
#=> 이클립스 뿐만아니라 다른 에디터를 사용하면서 생기는 설정, 임시 파일을 제외해주세요~
```
