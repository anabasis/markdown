# GUEST BOARD

- appserver
  - controllers : lookup_edit.py
  - static : lookup_edit.css, lookup_edit.js, lookup_list.css, lookup_list.js, lookup_new.css, lookup_new.js
    - css
    - images
    - js : js lib
- bin
  - lookup_editor : \_\_init\_\_.py, lookupfiles.py
- default : app.conf, props.conf, web.conf
  - data
    - ui
      - nav : default.xml
      - views : lookup_edit.xml, lookup_list.xml, lookup_new.xml
- metadata : default.meta, local.meta
- static : appIcon.png, appIcon_2x.png, appIconAlt.png

## default Directory

### [aoo.conf](http://docs.splunk.com/Documentation/Splunk/7.0.2/Admin/Appconf)

```properties
[launcher]
version = 1.0.0
description = Provides a graphical interface for editing lookup files
author = jscho

[package]
id = guest_board

[install]
#state_change_requires_restart = true
#is_configured = false
#state = enabled
#build = 1

[ui]
is_visible = true  # true/false(1/0)
label = Guest Board
# docs_section_override = AddOns:released

[triggers]
#reload.addon_builder = simple
#reload.ta_sample_shell_account = simple
#reload.ta_sample_shell_settings = simple
#reload.passwords = simple
```

### [web.conf](http://docs.splunk.com/Documentation/Splunk/7.0.2/Admin/Webconf)

```properties
[endpoint:guest_board]
```

### [metadata](http://dev.splunk.com/view/webframework-developapps/SP-CAAAE88) Directory

- Permission([default.meta](http://docs.splunk.com/Documentation/Splunk/6.5.3/Admin/Defaultmetaconf))

```properties
[]  # 모든 오브젝트
access = read : [ * ], write : [ admin ]
export = system
```

- higher Permission([local.meta]

```properties
[app/install/install_source_checksum]
version = 7.0.1
modtime = 1516687859.750523000
```

### static Directory

- appIcon.png(36*36*32)
- appIcon_2x.png(72*72*32)
- appIconAlt.png(36*36*32)

### ./default/data/ui/nav/default.xml

```xml
<nav color="#858585" search_view="search">
  <view name="guest_board_list" default="true" />
  <view name="guest_board_new" />
  <collection label="Search">
    <view name="search" />
    <view name="datasets" />
    <view name="reports" />
    <view name="alerts" />
    <view name="dashboards" />
  </collection>
</nav>
```

- URL : guest_board_list, guest_board_new

### ./default/data/ui/views

#### ./default/data/ui/views/guest_board_edit.xml

```xml
<dashboard script="guest_board_edit.js" stylesheet="guest_board_edit.css" isDashboard="false" isVisible="true" hideEdit="true">
    <label>Guest Board Edit</label>
    <row>
        <html>
            <div id="guest_boards_editor"></div>
        </html>
    </row>
</dashboard>
```

#### ./default/data/ui/views/guest_board_list.xml

```xml
<dashboard script="guest_board_list.js" stylesheet="guest_board_list.css" isDashboard="false" isVisible="true" hideEdit="true">
    <label>Guest Boards</label>
    <row>
        <html>
            <div id="guest_boards_list"></div>
        </html>
    </row>
</dashboard>
```

#### ./default/data/ui/views/guest_board_new.xml

```xml
<dashboard stylesheet="lookup_new.css" script="lookup_new.js" isDashboard="false" isVisible="true" hideEdit="true">
    <label>New Lookup</label>
    <row>
      <!-- HTML -->
      <html>
      <div class="lookup-link">
        <a href="lookup_edit?action=new&amp;type=csv">Create CSV Lookup...</a>
      </div>
      <div class="lookup-link">
        <a class="show-kv-supported-only" href="lookup_edit?action=new&amp;type=kv">Create KV Store Lookup...</a>
      </div>
      </html>
      <!-- HTML -->
    </row>
</dashboard>
```

- 추가되는 리소스 위치(Client)
  - ./appserver/static/css
  - ./appserver/static/js
  - ./appserver/static/images

- 추가되는 리소스 위치(Server)
  - ./appserver/controllers : guest_board_edit.py
