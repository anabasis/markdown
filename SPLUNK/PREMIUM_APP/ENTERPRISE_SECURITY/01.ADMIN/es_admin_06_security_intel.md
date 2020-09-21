# 일반 인텔리전스

## Splunk Enterprise Security에 인텔리전스 추가

ES 관리자는 Splunk Enterprise Security의 위협 인텔리전스 프레임워크를 사용하여 이벤트와 상관하거나 검색을 사용하여 대시보드를 보강하는데 사용할 수 있는 다른 형태의 인텔리전스를 다운로드하고 파싱할 수 있음. 이 일반적인 형태의 인텔리전스를 추가하면 애널리스트의 보안 모니터링 기능이 강화되고 조사에 컨텍스트가 추가됨.
Splunk Enterprise Security에는 몇 가지 인텔리전스 소스가 포함되어 있고, 다른 일반 인텔리전스 소스를 추가할 수도 있음.

ES 관리자는 인터넷에서 피드를 다운로드하여 Splunk Enterprise Security에 일반 인텔리전스를 추가할 수 있음.

1. Splunk Enterprise Security에 포함된 인텔리전스 소스를 설정함.
2. 인터넷에서 인텔리전스 피드를 다운로드함.
3. Splunk Enterprise Security에 인텔리전스를 성공적으로 추가했는지 확인.
4. inputintelligence를 사용한 검색에 일반 인텔리전스를 사용.

## 인터넷에서 Splunk Enterprise Security로 인텔리전스 피드 다운로드

Splunk Enterprise Security는 인터넷에서 제공되는 인텔리전스 피드를 정기적으로 다운로드하고 $SPLUNK_DB/modinput/threatlist 디렉터리에 저장할 수 있음. 그런 다음 inputintelligence 검색 명령어를 사용하여 보고서, 검색 또는 대시보드에 인텔리전스를 사용할 수 있음. 예를 참조. Splunk Enterprise Security에 일반 인텔리전스 소스를 추가함.

1. (선택 사항) 인텔리전스 검색 프록시를 설정함.
2. URL 기반 인텔리전스 소스를 추가함.

### 인텔리전스 검색 프록시 설정

프록시 서버를 사용하여 인텔리전스를 Splunk Enterprise Security로 전송하는 경우, 인텔리전스 소스 프록시 옵션을 설정 함.

사용자는 자격 증명 관리에 Splunk가 안전하게 저장한 자격 증명의 이름과 일치해야 함. 인텔리전스 다운로드 설정 편
집기에서 기존 프록시 사용자 및 암호를 제거하면 다운로드 프로세스에서 저장된 자격 증명을 더 이상 참조하지 않음.
자격 증명 참조를 제거해도 저장된 자격 증명이 자격 증명 관리에서 삭제되지 않음. 자세한 내용은 Splunk Enterprise
Security에서 자격 증명 관리를 참조.

1. Enterprise Security 메뉴 모음에서 설정 > 데이터 보강 > 인텔리전스 다운로드를 선택함.
2. 다운로드 소스를 선택함.
3. 프록시 옵션을 설정함.
    1. 프록시 서버 주소를 입력. 프록시 서버는 URL일 수 없음. 예: 10.10.10.10 또는 server.example.com
    2. 프록시 서버 주소에 액세스하기 위해 사용할 프록시 서버 포트를 입력.
    3. 프록시 서버의 프록시 사용자 자격 증명을 입력. 기본 및 다이제스트 인증 방법만 지원됨.
4. 변경 사항을 저장함.

#### URL 기반 인텔리전스 소스 추가

인터넷에서 URL을 통해 제공되는 TAXII 이외의 인텔리전스 소스를 추가함. URL 기반 일반 인텔리전스 소스를 추가하는 방법은 예: Splunk Enterprise Security에 일반 인텔리전스 소스를 추가함.

1. Enterprise Security 메뉴 모음에서 설정 > 데이터 보강 > 인텔리전스 다운로드를 선택함.
2. 다운로드의 이름을 입력. 이름에는 영숫자, 하이픈, 밑줄만 포함될 수 있음. 이름에 공백이 있으면 안 됨.
3. 새로 추가를 클릭하여 새 인텔리전스 소스를 추가함.
4. 싱크홀 체크박스를 선택하지 않음.
5. 위협 인텔리전스? 체크박스를 선택 해제함.
6. 다운로드 유형을 입력. 유형은 피드에 포함된 정보 유형을 식별함.
7. 설명을 입력. 피드에 있는 정보에 대해 설명.
8. 일반 인텔리전스 소스에는 필드가 중요하지 않으므로 기본 가중치를 놔둠.
9. (선택 사항) 피드의 기본 다운로드 간격을 변경함. 기본값은 43200초, 즉 매 12시간임.
10. (선택 사항) 피드 POST 인수를 입력.
11. 최대 기간 설정을 사용하지 않음.
12. (선택 사항) 환경의 네트워크 보안 컨트롤을 우회하기 위해 사용자 지정 사용자 에이전트 문자열을 지정해야 하는 경우 \<user-agent\>/\<version\> 형식으로 입력. 예: Mozilla/5.0 또는 AppleWebKit/602.3.12 이 필드의 값은 다음 정규식과 일치해야 함. ([A-Za-z0-9_.-]+)/([A-Za-z0-9_.-]+). 여기에 입력하는 문자열이 네트워크 보안 컨트롤에 의해 허용되는지 보안 장치 관리자에게 확인.
13. 리스트가 성공적으로 파싱되도록 파싱 옵션 필드를 작성함. 구분 정규식 또는 추출 정규식을 작성해야 함. 두 필드를 모두 비워둘 수 없음.
    <table>
    <tr><td>필드</td><td>설명</td><td></td></tr>예</td></tr>
    <tr><td>구분정규식</td><td>인텔리전스 소스에서 줄을 분할하거나 구분하는데 사용되는 정규식 문자열임. 구분자가 복잡한 경우 추출 정규식을 사용.</td><td>, 또는 : 또는 \t</td></tr>
    <tr><td>추출정규식</td><td>인텔리전스 소스 문서의 각 줄에서 필드를 추출하는데 사용되는 정규식임. 인텔리전스 소스의 값을 추출하는데 사용.</td><td>^(\S+)\t+(\S+)\t+\S+\t+\S+\t*(\S*)</td></tr>
    <tr><td>필드</td><td>문서의 줄이 구분된 경우에 필요함. 인텔리전스 리스트에서 추출할 필드의 쉼표로 구분된 리스트임. 필드의 이름을 바꾸거나 필드를 결합하는데 사용할 수도 있음. 설명은 필수 필드임. 추가로 위협 인텔리전스에 해당하는 KV 스토어 컬렉션의 필드가 허용되며, 로컬 Lookup 파일 또는 DA-ESS-ThreatIntelligence/collections.conf 파일에서 볼 수 있음. 기본값은 description:$1,ip:$2임.</td><td>&lt;fieldname&gt;:$&lt;number&gt;,&lt;fieldname&gt;.$&lt;number&gt;ip:$1,description:domain_blocklist</td></tr>
    <tr><td>무시정규식</td><td>인텔리전스 소스에서 줄을 무시하는데 사용되는 정규식임. 기본 값은 빈 줄 및 #으로 시작되는 코멘트 무시임.</td><td>^\s*$)</td></tr>
    <tr><td>헤더줄건너뛰기</td><td>인텔리전스 소스를 처리할 때 건너뛸 헤더 줄의 수임.</td><td>0</td></tr>
    <tr><td>인텔리전스파일인코딩</td><td>파일 인코딩이 ASCII 또는 UTF8이 아니면 여기에 인코딩을 지정. 그렇지 않으면 비워 두십시오.</td><td>latin1</td></tr>
    </table>
14. (선택 사항) 리스트가 성공적으로 다운로드되도록 다운로드 옵션 필드를 변경함.
    <table>
    <tr><td>필드</td><td>설명</td><td>예</td></tr>
    <tr><td>재시도 간격</td><td>다운로드 재시도 사이의 대기 시간(초)임. 재시도 간격을 변경하기 전에 권장 인텔리전스 소스 제공자 폴링 간격을 검토함.</td><td>60</td></tr>
    <tr><td>원격 사이트 사용자</td><td>피드를 사용하려면 인증이 필요한 경우, 원격 인증에 사용할 사용자 이름을 입력합니다(필요한 경우). 이 필드에 추가하는 사용자 이름은 자격 증명 관리에 있는 자격 증명 이름과 일치해야 함. Splunk Enterprise Security에서 자격 증명 관리를 참조.</td><td>관리자</td></tr>
    <tr><td>재시도</td><td>최대 재시도 횟수임.</td><td>3</td></tr>
    <tr><td>제한시간</td><td>다운로드 시도를 실패로 표시하기 전 대기 시간(초)임.</td><td>30</td></tr>
    </table>
15. (선택 사항) 프록시 서버를 사용하는 경우 피드에 대한 프록시 옵션을 작성함. 인텔리전스 검색 프록시 설정을 참조.
16. 변경 사항을 저장함.

인텔리전스 소스 추가를 완료했으면 Splunk Enterprise Security에 인텔리전스를 성공적으로 추가했는지 확인을 참조.

## inputintelligence를 사용한 검색에 일반 인텔리전스 사용

Splunk Enterprise Security에 일반 인텔리전스를 추가한 후 inputintelligence 명령어를 사용하여 인텔리전스를 활용할 수 있음. Splunk Enterprise Security에 일반 인텔리전스 추가를 참조.

설명

위협리스트 디렉터리의 인텔리전스를 검색 결과에 추가하려면 inputintelligence 명령어를 사용. 일반 인텔리전스는 다운로드되면 파싱되어 $SPLUNK_DB/modinputs/threatlist$ 디렉터리에 저장.

구문

```sql
| inputintelligence <threatlist_stanza_name> [fields=<string>] [delim_regex=<string>] [extract_regex=<string>]
[ignore_regex=<string>] [skip_header_lines=<int>] [include_raw=<bool>] [append=<bool>] [no_parse=<bool>]
```

필수 인수

threatlist_stanza_name

    구문: <string>
    설명: 인텔리전스 다운로드의 스탠자. 인텔리전스 다운로드 페이지의 이름 필드와 일치함. 검색에 여러 스탠자 이름을 포함할 수 있음. 인터넷에서 Splunk Enterprise Security로 인텔리전스 피드 다운로드를 참조.

선택 인수

fields
    구문: \<string\>
    설명: 인텔리전스 다운로드 페이지에서 정의된 인텔리전스 다운로드의 기본 필드 설정을 재정의함. 문서의 줄이 구분된 경우에 필요함. 인텔리전스 리스트에서 추출할 필드의 쉼표로 구분된 리스트임. 필드의 이름을 바꾸거나 필드를 결합하는데 사용할 수도 있음. 설명은 필수 필드임. 추가로 위협 인텔리전스에 해당하는 KV 스토어 컬렉션의 필드가 허용되며, 로컬 Lookup 파일 또는 DA-ESS-ThreatIntelligence/collections.conf 파일에서 볼 수 있음. 기본값은 description:$1,ip:$2임.

delim_regex
    구문: \<string\>
    설명: 인텔리전스 다운로드 페이지에서 정의된 인텔리전스 다운로드의 기본 구분 정규식 설정을 재정의함. 인텔리전스 소스에서 줄을 분할하거나 구분하는데 사용되는 정규식 문자열임. 구분자가 복잡한 경우 추출 정규식을 사용.

extract_regex
    구문: \<string\>
    설명: 인텔리전스 다운로드 페이지에서 정의된 인텔리전스 다운로드의 기본 추출 정규식 설정을 재정의함. 인텔리전스 소스 문서의 각 줄에서 필드를 추출하는데 사용되는 정규식임. 인텔리전스 소스의 값을 추출하는데 사용.

ignore_regex
    구문: \<string\>
    설명: 인텔리전스 다운로드 페이지에서 정의된 인텔리전스 다운로드의 기본 무시 정규식 설정을 재정의함. 인텔리전스 소스에서 줄을 무시하는데 사용되는 정규식임.    기본값은 빈 줄 및 #로 시작되는 코멘트 무시임.

skip_header_lines
    구문: \<int\>
    설명: 인텔리전스 다운로드 페이지에서 정의된 인텔리전스 다운로드의 기본 헤더 줄 건너뛰기 설정을 재정의함.
    인텔리전스 소스를 처리할 때 건너뛸 헤더 줄의 수임.
    기본값: 0

include_raw
    구문: \<bool\>
    설명: 1, t 또는 true이면 원래 줄 콘텐츠를 raw라고 하는 추가 컬럼에 추가함.
    기본값: 0

append
    구문: \<bool\>
    설명: 1, t 또는 true이면 inputintelligence 명령어의 결과를 대체하는 대신 기존 검색 결과 집합에 추가함.
    기본값: 0

no_parse
    구문: \<bool\>
    설명: 1, t 또는 true이면 다른 옵션이 모두 무시되고 인텔리전스 파일의 원시 콘텐츠가 한 행에 한 줄씩 반환됨.
    기본값: 0

사용법

inputintelligence 명령어는 변환 명령어임.

예

1. 상위 100만 개 사이트 보기
    Alexa의 상위 100만 개 사이트를 봄.
    inputintelligence alexa_top_one_million_sites
2. 추가 예
    예를 참조. Splunk Enterprise Security에 일반 인텔리전스 소스를 추가함.

참고 항목은 아래와 같음.

inputlookup

## 예: Splunk Enterprise Security에 일반 인텔리전스 소스 추가

보안 애널리스트는 네트워크에 표시된 호스트를 Spotify 광고와 연결된 호스트와 비교하여 작업일 동안의 Spotify Free 수신이 네트워크에 제기하는 위험을 평가할 수 있음. Spotify 광고와 연결된 호스트는 악성 호스트가 아니며 Splunk
Enterprise Security에 위협 인텔리전스로 추가하지 않아도 됨. 대신 호스트를 일반 인텔리전스로 추가할 수 있음.

### 일반 인텔리전스 다운로드

먼저 리스트에 대한 다운로드 설정을 만듬.

1. 설정 > 데이터 보강 > 인텔리전스 다운로드를 선택함.
2. 새로 만들기를 클릭함.
3. spotify_ads의 이름을 입력.
4. 위협 인텔리전스? 체크박스를 선택 해제함.
5. spotify_ads의 유형을 입력.
6. Spotify 광고를 호스팅하는 컴퓨터의 호스트 이름에 대한 설명을 입력.
7. <https://raw.githubusercontent.com/StevenBlack/hosts/master/data/SpotifyAds/hosts>의 URL을 입력.
8. (선택 사항) 기본 가중치를 변경함.
9. (선택 사항) 기본 간격을 변경함.
10. \s의 구분 정규식을 입력.
11. url:$2의 필드를 입력.
12. (^#|^\s*$)의 무시 정규식을 입력.
13. 저장함.

### 인텔리전스가 성공적으로 다운로드되는지 확인

검색을 사용하여 모듈식 입력이 소스에서 정보를 다운로드하고 있는지 확인.

```sql
| inputintelligence no_parse=1 spotify_ads
```

### 인텔리전스가 올바로 파싱되는지 확인

인텔리전스가 올바로 파싱되는지 확인하려면 사용자 지정 검색 명령어 inputintelligence를 사용.

```sql
| inputintelligence spotify_ads
```

인텔리전스가 올바로 파싱되는 것으로 보이지 않으면 search.log에 오류 메시지가 있는지 확인. 또한 inputintelligence 명령어의 선택 인수를 사용하여 다운로드의 파싱 설정을 변경하여 올바른 설정을 확인할 수 있음.

inputintelligence를 사용한 검색에 일반 인텔리전스 사용을 참조.

### 검색에 새 인텔리전스 소스 사용

검색에서 다양한 방법으로 새 인텔리전스 소스를 사용할 수 있음.

#### 하위 검색에 Spotify 광고 사용

다음 하위 검색을 사용하여 리스트에서 Spotify 광고에 의해 사용되는 URL 100개를 반환하려면:

```sql
| search [| inputintelligence spotify_ads | return 100 url]
```

하위 검색은 다음 항목을 반환함.

```sql
(url="ads.pubmatic.com") OR (url="gads.pubmatic.com") OR (url="pubads.g.doubleclick.net") OR
(url="securepubads.g.doubleclick.net") OR (url="www.googletagservices.com")
```

#### 조인에 Spotify 광고 사용

join을 사용하여 Spotify 광고 인텔리전스 소스의 호스트를 다른 데이터 집합과 조인함.

```sql
... | join url [| inputintelligence spotify_ads | eval spotify_ad="true"] | search spotify_ad="true"
```

#### Lookup 테이블 파일에 Spotify 광고 추가

Lookup 생성 검색을 사용하여 Spotify 광고의 호스트를 Lookup 테이블 파일에 추가함.

```sql
| inputintelligence spotify_ads | eval spotify_ad="true" | outputlookup spotify_ads.csv
```

Lookup을 만든 후 다음 검색 예를 사용하여 검색에 사용.

```sql
... | lookup spotify_ads.csv url OUTPUT spotify_ad | search spotify_ad="true"
```