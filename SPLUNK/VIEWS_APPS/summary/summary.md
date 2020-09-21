# Developer Views And Apps

## Building Customizations

<table>
<tr><td>API</td><td>Use case</td></tr>
<tr><td><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizDevOverview">Custom visualizations</a></td><td>Create custom visualizations for analyzing data patterns and trends.</a></td></tr>
<tr><td><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/ModAlertsIntro">Custom alert actions</a></td><td>Implement a custom response to alerts.</td></tr>
<tr><td><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/ModInputsIntro">Modular inputs</a></td><td rowspan=2>Index data from unique sources or in non-standard formats.</td></tr>

<tr><td><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/ScriptedInputsIntro">Scripted inputs</a></td></tr>
</table>

<table>
<tr><td width=40%>To learn about</td><td width=60%>See</td></td>
<tr><td><li>Building visualizations and dashboards</li>
<li>Using Simple XML</li></td><td><li><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/Viz/Aboutthismanual">Dashboards and Visualizations</a></li></td></td>
<tr><td><li>Working with alerts</td><td><li><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/Alert/Aboutalerts">Alerting manual</a></li></td></td>
<tr><td><li>General app building guidance</li>
<li>Leveraging Splunk SDKs</li></td><td><li><a href="http://dev.splunk.com/view/SP-CAAAE8T">Get started developing Splunk apps on the Splunk Developer Portal</a></li>
<li><a href="http://dev.splunk.com/sdks">Overview of Splunk SDKs on the Splunk Developer Portal</a></li></td></td>
<tr><td><li>Creating custom search commands</li>
<li>Managing access to custom search commands</li>
<li>Custom search command examples</li></td><td><li><a href="http://dev.splunk.com/view/python-sdk/SP-CAAAEU2">How to create custom search commands on the Splunk Developer Portal</a></li></td></td>
<tr><td><li>Using the Splunk REST API</td><td><li><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/RESTREF/RESTprolog">REST API Reference Manual</a></li>
<li><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/RESTUM/RESTusing">REST API User Manual</a></li>
<li><a href="http://docs.splunk.com/Documentation/Splunk/7.3.1/RESTTUT/RESTconfigurations">REST API Tutorials</a></li></td></td>
</table>

### 개발 리소스

- 튜토리얼
  - [커스터마이징 시각화 구축](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizTutorial)
    - 작동하는 사용자 지정 시각화를 만드는 방법(예제 시각화, 개발자 우수 사례 및 예제 코드 작성 단계가 포함)
- API 세부사항 및 우수사례
  - [커스터마이징 시각화 API 참조](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizApiRef)
    - 사용자지정 시각화 구성 요소 및 앱 디렉토리 구조를 검토
  - [API 업데이트 및 마이그레이션 조언](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizDevMigration)
    - 이전 API 버전을 사용하여 빌드된 앱의 마이그레이션 정보
  - [Formatter API 참조](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizFormatterApiRef)
    - 시각화 형식화를 위해 사용자 인터페이스의 구성 요소를 검토
- 사용자 경험
  - [설계 지침](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizDesign)
    - 맞춤형 시각화 모양 및 동작을 구현
  - [데이터 처리 지침](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizDataBestPractices)
    - 사용자 검색 결과 및 데이터 형식 오류로 작업
- API 상호 작용
  - [Simple XML로 사용자 정의 시각화](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizXML)
    - 대시보드에 사용자지정 시각화를 추가하고 Simple XML로 구성
  - [SplunkJS의 커스터마이징 시각화](http://docs.splunk.com/Documentation/Splunk/7.3.1/AdvancedDev/CustomVizSplunkJS)
    - SplunkJS에서 커스터마이징 시각화에 액세스하고 인스턴스화

## 개발 모드 세팅

```properties
[settings]
minify_js = False
minify_css = False
js_no_cache = True
cacheEntriesLimit = 0
cacheBytesLimit = 0
enableWebDebug = True
```

## APP 빌드

[custom visualization app](https://docs.splunk.com/images/4/4f/Viz_tutorial_app.zip)
$SPLUNK_HOME/share/splunk/app_templates/

### 디렉토리 구조

```properties
appname
├── appserver
|   └── static
|       └── visualizations
|           └── <visualizations_name>
|               └── src
|                   └── viaulication_source.js
|                   ├── webpack.config.js
|                   ├── visualization.js
|                   ├── visualization.css
|                   ├── formatter.html
|                   ├── package.json
|                   └── preview.png
├── default
|   ├── visualizations.conf
|   └── savedsearches.conf
├── metadata
|   └── default.meta
├── README
|   └── savedsearches.conf.spec
└── index.html
```

<table>
<tr><td>File</td><td>Description</td></tr>
<tr><td>visualization_source.js</td><td><pre>이 파일에는 사용자 지정 시각화를위한 소스 코드가 포함
이 파일에서 소스 코드를 편집
Webpack은 소스 코드 파일을 사용하여 visualisation.js 파일을 빌드</pre></td></tr>
<tr><td>visualization.js</td><td><pre>시각화를 렌더링하기위한 빌드된 파일</pre></td></tr>
<tr><td>formatter.html</td><td><pre>시각화 형식 메뉴를 렌더링하기위한 HTML이 포함
형식 메뉴가 검색 페이지와 대시 보드에 나타남</pre></td></tr>
<tr><td>visualization.css</td><td><pre>시각화를위한 CSS 스타일 및 동작 규칙을 포함
CSS규칙은 가능한 시각화에 특정한 이름</pre></td></tr>
</table>

## Visualization 로직 생성

### visualization source code

1. `viz_tutorial_app/appserver/static/visualizations/standin` radial_meter(적용할 라이브러리명)
2. npm 패키지 인스톨
   - viz_tutorial_app/appserver/static/visualizations/radial_meter 디렉토리에서 $npm install을 실행
   - 이 단계는이 폴더에 /node_modules 하위 디렉토리를 생성합니다.
3. D3 추가
   - Run $npm install --save d3@3.5
   - 이 학습서에서는 특정 버전의 D3을 사용
   - 이 단계는 / node_modules에 D3 디렉토리를 생성
4. 추가적인 Dependency 라이브러리 추가
   - a) Run $npm install --save underscore
   - b) Run $npm install --save jquery
5. viz_tutorial_app/appserver/static/visualizations/radial_meter/src 디렉토리에서 visualisation_source.js 파일
   - visualisation_source.js의 모든 코드를 다음 코드로 변경

    ```js
    define([
            'jquery',
            'underscore',
            'api/SplunkVisualizationBase',
            'api/SplunkVisualizationUtils',
            'd3'
        ],
        function(
            $,
            _,
            SplunkVisualizationBase,
            SplunkVisualizationUtils,
            d3
        ) {
    return SplunkVisualizationBase.extend({
        initialize: function() {
            // Save this.$el for convenience
            this.$el = $(this.el);
            // Add a css selector class
            this.$el.addClass('splunk-radial-meter');
        },
        getInitialDataParams: function() {
            return ({
                outputMode: SplunkVisualizationBase.ROW_MAJOR_OUTPUT_MODE,
                count: 10000
            });
        },
        updateView: function(data, config) {
            // Fill in this part in the next steps.
        }
        });
    });
    ```

### visualisation_source.js Visualization 로직을 관리

- 시각화 로직을 관리하기 위해 visualization_source.js는 중요한 사용자 정의 시각화 프레임 워크 규칙을 사용
- SplunkVisualizationBase 클래스를 확장하는 개체를 반환
  - 이 클래스를 확장하는 과정에서 visualization_source.js는 SplunkVisualizationBase의 두 함수를 재정의

#### updateView

- 이 함수는 검색 결과가 업데이트되거나 시각화 형식이 변경될 때마다 호출
- 다음 두 매개 변수를 사용하여 시각화 렌더링을 처리
  - 데이터. 검색 결과 데이터가 포함된 개체
  - 구성. 시각화 형식 정보가 포함된 개체
- 학습서의 다음 부분에서는 updateView 기능을 완료하는 방법

#### getInitialDataParams

- 이 기능은 검색에서 데이터를 반환하는데 필요
- 이 기능은 검색 결과의 데이터 출력 형식을 지정
- getInitialDataParams를 사용하여 최대 결과 수를 지정

## css 추가

- appname/appserver/static/visualizations/<visualizations_name>/src/visualization.css

```css
/* Formatting for text element*/
.meter-center-text {
    font-size: 40px;
    font-weight: 200;
    font-family: "Helvetica Neue", Helvetica, sans-serif;
}
/* Center the main SVG in the page */
.splunk-radial-meter svg {
    display: block;
    margin: auto;
}
```

## 설정 추가

### Visualization 등록

1. viz_tutorial_app/default folder, find the visualizations.conf
2. [standin] stanza 삭제
3. stanza 추가

    ```properties
    [radial_meter]
    label = Radial Meter
    ```

<table>
<tr><td>Settings</td><td>Description</td><td>Required</td></tr>
<tr><td>label</td><td>Public label used throughout Splunk Web to refer to the visualization.</td><td>Yes</td></tr>
<tr><td>default_height</td><td>Default visualization height.</td><td>No. Defaults to 250 if unspecified.</td></tr>
<tr><td>description</td><td>Brief description for the visualization, appearing in Splunk Web.</td><td>No</td></tr>
<tr><td>search_fragment</td><td>Brief search portion to indicate how to structure results properly for the visualization. Used in Splunk Web.</td><td>No</td></tr>
<tr><td>allow_user_selection</td><td>Whether the visualization should be available for users to select in Splunk Web.</td><td>No. Defaults to true, meaning the visualization is available.</td></tr>
<tr><td>disabled</td><td>If set to 1, the visualization is not available anywhere in the Splunk platform. In this case, overrides a true setting for allow_user_selection.</td><td>No. Defaults to 0 if unspecified, meaning that the visualization is available.</td></tr>

### Visualization 권한

기본적으로 Custom Visualization은 자체 앱 Context내에서만 사용
 검색 및 보고앱을 포함하여 전 세계에서 사용할 수 있도록 시각화 권한
 Custom Visualization 앱을 내보내려면 다음 단계를 수행

1. viz_tutorial_app/metadata 폴더에서 default.meta 파일
2. 내용을 추가

  ```properties
  [visualizations/radial_meter]
  export = system
  ```

3. stanza 이름 구문은 `visualisation/<visualization_folder_name>` 이고 Visualization 폴더 이름은 radial_meter

## 가시화

필수 시각화 렌더링 코드가 있습니다. 이제 시각화를 구축하고 검색을 실행

### Visualization 재구축

- visualisation_source.js에서 소스 코드를 업데이트할 때마다 Splunk Web의 변경 사항을 보려면 시각화를 다시 작성
- 개발자 모드가 활성화된 경우 Splunk Web에서 재구성된 시각화의 변경 사항을 확인하기 위해 Splunk 플랫폼을 다시 시작할 필요가 없음

### 전제 조건

- `$SPLUNK_HOME` 환경변수가 Splunk 설치 폴더를 가리키는지 확인
- 터미널 창에서 `echo $SPLUNK_HOME`을하여 Splunk 설치 폴더 경로가 인쇄되는지 확인
- 그렇지 않은 경우 `export SPLUNK_HOME =/Applications/Splunk` 명령으로 설정

### 단계

1. `/radial_meter` 디렉토리에서 `$npm run build`를 실행하여 시각화를 빌드, 이렇게하면 동일한 디렉토리에 빌드된 visualization.js 파일이 생성
2. Search And Reporting 홈 페이지에서이 검색을 실행 `index = _internal | stats count`
3. 시각화 탭을 선택
4. 사용 가능한 시각화를 검토하려면 왼쪽에서 시각화 선택기를 선택.
   - 방사형 미터 시각화가 자세히 아래에 나타남
   - 이 튜토리얼에는 시각화 아이콘 추가가 포함되어 있지 않으므로 Radial Meter 시각화에서는이 일반아이콘인 generic icon를 사용
5. 검색 결과를 렌더링하려면 Radial Meter 시각화를 선택

## 데이터 포멧 에러 헨들링

`visualisation_source.js`가 `SplunkVisualizationBase`에서 상속하는 formatData 메서드를 재정의하는 방법

### formatData 재정의

- `formatData`는 `splunkd`에서 원시 데이터 객체를 가져오고 렌더링 용으로 포맷된 객체를 반환합니다. 이 객체는 데이터 인수로 `updateView`에 전달
- `visualisation_source.js`에 다음 코드를 추가하여 formatData가 원시 데이터의 유효성을 검사하고 처리하는 방법을 지정

표시된대로 정확하게 새 formatData 코드를 추가

```js
getInitialDataParams: function() {
    ...
},
formatData: function(data, config) {
    // Check for an empty data object
    if(data.rows.length < 1){
        return false;
    }
    var datum = SplunkVisualizationUtils.escapeHtml(parseFloat(data.rows[0][0]));
    // Check for invalid data
    if(_.isNaN(datum)){
        throw new SplunkVisualizationBase.VisualizationError(
            'This meter only supports numbers'
        );
    }
    return datum;
},
updateView: function(data, config) {
        ...
}
   ...
```

### 데이터 유효성 검사 및 처리를 위한 모범 사례

formatData에 방금 추가 한 코드는 많은 서식을 지정하지 않습니다. 그러나 데이터 유효성 검사 및 오류 처리를위한 중요한 방법을 보여줍니다. 사용자 지정 시각화를 만들 때 다음 모범 사례를 검토하십시오.

- 빈 데이터 확인
  - 검색 결과가 변경될 때마다 formatData가 호출.
  - 때때로 결과 데이터 오브젝트가 비어 있으며 이 경우를 처리
- 유효하지 않은 데이터 확인
  - 시각화 검색이 렌더링을 위해 올바른 형식으로 데이터를 생성하지 않는 경우를 처리
  - 시각화를 렌더링하기 전에 예상 데이터 형식을 확인(formatData는 숫자를 확인)
- 유용한 오류 전달
  - 시각화에서 `SplunkVisualizationBase.VisualizationError`가 발생하면 Splunk Web에 오류가 표시
  - 오류는 무엇이 잘못되었는지에 대한 정보를 제공하고 사용자가 문제를 해결하는데 도움
- DOM에 추가된 값을 삭제
  - DOM에 추가될 동적값은 보안을 위해 `escapeHtml()`을 통해 전달

### 데이터 형식 오류에서 렌더링 변경

formatData에 대한 이러한 업데이트 외에도 formatData가 렌더링할 항목을 반환하지 않는 경우 아무것도하지 않도록 updateView를 변경

updateView의 모든 코드를 다음 코드로 변경

```js
 updateView: function(data, config) {
        // Return if no data
        if (!data) {
            return;
        }
        // Assign datum to the data object returned from formatData
        var datum = data;

        // Clear the div
        this.$el.empty();

        // Pick a color for now
        var mainColor = 'yellow';

        // Set domain max
        var maxValue = 100;

        // Set height and width
        var height = 220;
        var width = 220;
  
        // Create a radial scale representing part of a circle
        var scale = d3.scale.linear()
            .domain([0, maxValue])
            .range([ - Math.PI * .75, Math.PI * .75])
            .clamp(true);
  
        // Create parameterized arc definition
        var arc = d3.svg.arc()
            .startAngle(function(d){
                return scale(0);
            })
            .endAngle(function(d){
                return scale(d)
            })
            .innerRadius(70)
            .outerRadius(85);

        // SVG setup
        var svg  = d3.select(this.el).append('svg')
            .attr('width', width)
            .attr('height', height)
            .style('background', 'white')
            .append('g')
            .attr('transform', 'translate(' + width / 2 + ',' + height / 2 + ')');

        // Background arc
        svg.append('path')
            .datum(maxValue)
            .attr('d', arc)
            .style('fill', 'lightgray');

        // Fill arc
        svg.append('path')
            .datum(datum)
            .attr('d', arc)
            .style('fill', mainColor);

        // Text
        svg.append('text')
            .datum(datum)
            .attr('class', 'meter-center-text')
            .style('text-anchor', 'middle')
            .style('fill', mainColor)
            .text(function(d){
                return parseFloat(d);
            })
            .attr('transform', 'translate(' + 0 + ',' + 20 + ')');
        }
```

updateView는 이제 널 데이터를 확인하고이 경우 렌더링하지 않습니다.

이러한 업데이트 후 검색에서 숫자가 반환되지 않으면 유용한 오류 메시지가 나타납니다.

이 시점에서 시각화를 시도하려면 $ npm run build를 실행하여 visualisation.js 파일을 다시 빌드하십시오.

## user-configurable properties 추가

사용자 지정 시각화에는 사용자 구성 가능 속성 및 사용자가 설정을 지정할 수있는 인터페이스가 포함될 수 있습니다. 학습서의이 부분에서는 visualisation_source.js에서 두 개의 구성 가능한 특성을 선언하고 특성 설정을 처리하는 방법을 보여줍니다. 이 단계는 사용자 인터페이스와 작동하도록 시각화를 설정합니다.

속성 이름
속성 이름 간격은 구성 파일 선언을 위해이 구문을 따릅니다.

display.visualizations.custom. <app_name>. <visualization_ name>. <property_name>
formatter.html 사용자 인터페이스를 빌드할 때 개발자는 특성 참조에 단축 네임 스페이스 구문을 사용할 수 있습니다. 이 구문 옵션은이 자습서의 뒷부분에 나와 있습니다.

속성 이름
구성 파일에서 특성 이름은 영향을주는 시각화의 특정 부분을 나타냅니다. 예를 들어 방사형 미터의 기본 색상을 결정하는 속성에이 이름을 사용하십시오.
display.visualizations.custom.viz_tutorial_app.radial_meter.mainColor
미터 최대 카운트 값을 결정하는 특성에이 이름을 사용하십시오.
display.visualizations.custom.viz_tutorial_app.radial_meter.maxValue
속성 정보 선언
구성 가능한 속성의 이름을 결정하면 다음 단계는 속성 이름, 유형 및 기본값을 선언하는 것입니다.

방사형 미터 앱은 다이얼 색상 및 최대 값에 대한 속성이 필요합니다. viz_tutorial_app / README / savedsearches.conf.spec에 다음 특성 이름 및 유형을 추가하십시오.

```properties
display.visualizations.custom.viz_tutorial_app.radial_meter.mainColor = <string>
display.visualizations.custom.viz_tutorial_app.radial_meter.maxValue = <float>
```

그런 다음 viz_tutorial_app / default / savedsearches.conf에 다음 컨텐츠를 추가하여 특성 기본값을 지정하십시오.

[태만]
display.visualizations.custom.viz_tutorial_app.radial_meter.mainColor = # f7bc38
display.visualizations.custom.viz_tutorial_app.radial_meter.maxValue = 100

이제 속성 구성을 처리하기 위해 visualization_source.js에 코드를 추가할 수 있습니다.

속성 설정 처리
학습서의이 부분에서는 다음 두 특성을 사용하여 전경색 및 최대 방사형 미터 값에 대한 사용자 설정을 얻는 방법을 보여줍니다.

display.visualizations.custom.viz_tutorial_app.radial_meter.mainColor
display.visualizations.custom.viz_tutorial_app.radial_meter.maxValue

updateView를 변경하여 특성을 확인하고 특성이 설정되지 않은 경우 기본값을 사용하십시오.

updateView에서이 줄을 바꾸십시오.

 
    // 지금 색상을 선택하십시오
    var mainColor = '노란색';

    // 도메인 최대 값 설정
    var maxValue = 100;
이 코드로.

updateView : 함수 (데이터, 구성) {
        ...
 
        // 색상 구성을 얻거나 기본 노란색 음영을 사용합니다.
        var mainColor = config [this.getPropertyNamespaceInfo (). propertyNamespace + 'mainColor'] || '# f7bc38';
 
        // 미터 최대 값을 설정하거나 기본값을 사용합니다
        var maxValue = parseFloat (config [this.getPropertyNamespaceInfo (). propertyNamespace + 'maxValue']) || 100;
        ...
이러한 업데이트 후에 / radial_meter 디렉토리에서 $ npm run build를 실행하여 시각화를 다시 작성하십시오. 이제 시각화는 구성 사용자 인터페이스를위한 준비가되었습니다.

## 포멧메뉴 구현

이 코드는 이제 두 가지 시각화 속성을 확인하고 사용자 설정을 처리합니다. 다음 단계는 이러한 속성을 설정하기위한 사용자 인터페이스를 사용자에게 제공하는 것입니다. 다음 단계는 HTML에서 Splunk Web 구성 요소를 사용하여 형식 메뉴를 정의하는 방법을 보여줍니다. 메뉴가 검색 페이지와 대시 보드 메뉴에 나타납니다.

형식 메뉴 정의
여기에 정의된 형식 메뉴에는 두 개의 섹션이 있습니다. 첫 번째는 사용자가 최대 미터 값을 지정할 수있게합니다. 두 번째 섹션에서는 Splunk Web 구성 요소를 사용하여 미터 전경의 색상 선택기를 정의합니다.

단계
viz_tutorial_app / appserver / static / visualizations / radial_meter / formatter.html 파일을 찾으십시오.

파일을 열고 다음 내용을 추가하십시오.
<form class = "splunk-formatter-section"section-label = "최대 값">
    <splunk-control-group label = "최대 다이얼 값">
        <splunk-text-input name = "{{VIZ_NAMESPACE}}. maxValue"value = "100">
        </ splunk-text-input>
    </ splunk-control-group>
</ form>
<form class = "splunk-formatter-section"section-label = "다이얼 컬러">
    <splunk-control-group label = "Color">
        <splunk-color-picker name = "{{VIZ_NAMESPACE}}. mainColor"value = "# f7bc38">
        </ splunk-color-picker>
    </ splunk-control-group>
</ form>

formatter.html 작업
방금 추가 한 코드는 형식 메뉴 구현의 몇 가지 중요한 측면을 보여줍니다.

각 메뉴 섹션에는 자체 양식이 필요합니다.
각 섹션을 개별적으로 렌더링하려면 각 섹션에 splunk-formatter-section 클래스를할당해야합니다.
각 입력 요소는 영향을받는 특성에 따라 이름을 지정해야합니다.
팔레트 유형 속성이 지정되지 않은 경우 splunk-color-picker 구성 요소는 기본적으로 splunkCategorical 색상 팔레트를 사용합니다.
사용자가 형식 메뉴에서 설정을 변경하면 구성 사전이 업데이트된 특성 값을 가져옵니다. udpateView는 config의 새 값을 사용하여 호출됩니다.
또한 방금 추가 한 코드는 formatter.html에서 사용 가능한 단축 속성 네임 스페이스 구문을 사용하여 시연합니다.

포맷터 속성 이름
formatter.html에서 다음 단축 네임 스페이스 구문을 사용하여 시각화 속성을 참조할 수 있습니다.

{{VIZ_NAMESPACE}}. <property_name>
이 단축된 구문은 savedsearches.conf에서 속성을 선언하는데 사용되는 정규화된 이름과 같습니다.

display.visualizations.custom. <app_name>. <viz_name>. <property_name>
형식 메뉴를 정의하는 다음 절차에는 단축 구문이 포함됩니다.

참고 : 단축된 속성 이름 구문은 formatter.html 파일에서만 사용할 수 있습니다. 다른 시각화 앱 파일에서 정규화된 속성 이름을 사용하십시오.


형식 메뉴 인터페이스 구현 및 구성에 대한 자세한 정보는 Formatter API 참조를 참조하십시오.