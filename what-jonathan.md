# VSCODE Example

## VSCODE MARKDOWN

/*
Title: What is Jonathan?
Sort: 2
ShowOnHome: true
*/

[VisualStudio Code]
Markdown 설정
기본설정 -> 설정

VSCODE App

- Markdown Extension Pack
  - Markdown All in One - All you need for Markdown (keyboard shortcuts, table of contents, auto preview and more).
  - markdownlint - Markdown/CommonMark linting and style checking for Visual Studio Code.
  - Markdown PDF - This extension convert Markdown file to pdf, html, png or jpeg file.
  - Markdown+Math - Mdmath allows to use Visual Studio Code as a markdown editor capable of typesetting and rendering TeX math. In fact it now reuses the built in markdown viewer. KaTeX works inside as a fast math renderer.
  - Markdown Preview Enhanced - Markdown Preview Enhanced is an extension that provides you with many useful functionalities such as automatic scroll sync, math typesetting, mermaid, PlantUML, pandoc, PDF export, code chunk, presentation writer, etc.
  - Markdown TOC - Generate TOC (table of contents) of headlines from parsed markdown file.

- Auto-Open Markdown Preview
- Markdown Shortcuts

- Markdown Nomnoml Support
- Markdown Preview Github Styling
- Markdown Preview with Bitbucket Styles
- Markdown Theme Kit
- Excel to Markdown table

- Atom Keymap
- Notepad++ Keymap

```json
{
    "window.zoomLevel": 0,
    "markdown.preview.breaks": true,
    "git.enabled": true,
    "git.path": "E:/REPO/GIT/SERVER/bin/git.exe",
    //"git.path": "/usr/bin/git",
    "explorer.confirmDelete": false,

    <!-- 절대주소 안됨.
    "markdown.styles": ["D:/DOWNLOAD/GOOGLE_DOC/KMS/vscode_markdown_theme/github-markdown.css"],
    -->
    "markdown.styles": ["https://drive.google.com/file/d/1bh_U4UMZbYHRxW3YbVQ0TtYb2paFEss2/view?usp=sharing"],

    "markdownlint.config": {
        "MD033": {
            "allowed_elements": [
                "h1",
                "h3",
                "HR",
                "p",
                "a",
                "style",
                "table",
                "tr",
                "td",
                "th",
                "thead",
                "br"
            ]
        },
        "MD012": { "maximum": 0 },
        "MD022": false,
        "MD024": false,
        "MD007": { "indent": 2 }
    },
    "workbench.startupEditor": "newUntitledFile",
    "team.showWelcomeMessage": false,
    "markdown.preview.fontSize": 12,
    "markdown.preview.lineHeight": 1.5
}
```

// 마우스로 여러 커서를 추가할 때 사용할 수정자입니다. `ctrlCmd`는 Windows와 Linux에서 `Control`로 매핑되고 macOS에서 `Command`로 매핑됩니다. Go To Definition 및 Open Link 마우스 제스처가 멀티커서 수정자와 충돌하지 않도록 조정됩니다.

```"editor.multiCursorModifier": "alt"```

// Status of Atom Keymap version three features added.

```"atomKeymap.promptV3Features": null```

// 붙여넣은 콘텐츠의 서식을 편집기에서 자동으로 지정할지 여부를 제어합니다. 포맷터는 반드시 사용할 수 있어야 하며 문서에서 범위의 서식을 지정할 수 있어야 합니다.

```"editor.formatOnPaste": false```

Markdown 설정
"markdownlint.config":

Markdown CSS 설정
"markdown.styles" : <https://gist.github.com/BigstickCarpet/5d31c053d0b1d52389eb2723f7550907>


## ATOM NODEJS

### NODE관련 개발 package

- highlight-selected : 단어를 더블클릭했을때 해당 파일에 같은 단어들을 표시해 주는 기능
- linter : linter는 해당 파일에 문법 오류가 있을때 알려주는 기능. 이 package는 linter 기능을 더해주는 package이고, linter 데이터는 따로 설치
- linter-jshint : jshint 데이터를 linter에 추가. javascript파일(.js)의 문법 오류를 알려줌. 위 linter가 설치되어 있어야 동작
- linter-csslint : csslint 데이터를 linter에 추가. css파일(.css)의 문법 오류를 알려줌. 위 linter가 설치되어 있어야 동작
- atom-runner : atom에서 alt+R을 눌러 javascript, ruby파일 등을 바로 실행
