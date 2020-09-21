# iBatis 연동

- conf/config.properties

```properties
jdbc.db.driver=org.sqlite.JDBC
jdbc.db.url=jdbc:sqlite:db/schedule.db
```

- com.splunk.siemdb.util/sqlmap-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE sqlMapConfig PUBLIC "-//iBATIS.com//DTD SQL Map Config 2.0//EN"
    "http://ibatis.apache.org/dtd/sql-map-config-2.dtd">

<!-- ================================================================== -->
<!-- iBATIS SqlMap definition                                           -->
<!-- ================================================================== -->
<sqlMapConfig>
    <properties resource="conf/config.properties" />

    <!-- useStatementNamespaces true 임으로 queryForList에서 namespaces 꼭 넣을것 -->
    <settings
        maxRequests="512"
        maxSessions="256"
        maxTransactions="64"
        cacheModelsEnabled="true"
        enhancementEnabled="true"
        lazyLoadingEnabled="true"
        useStatementNamespaces="true"/>

    <transactionManager type="JDBC">
        <dataSource type="SIMPLE">
            <property name="JDBC.Driver" value="${jdbc.db.driver}" />
            <property name="JDBC.ConnectionURL" value="${jdbc.db.url}" />
            <property name="JDBC.Username" value="" />
            <property name="JDBC.Password" value="" />
        </dataSource>
    </transactionManager>
    <sqlMap resource="com/splunk/siemdb/db/xml/sql.xml"/>
</sqlMapConfig>
```

- sql.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE sqlMap
    PUBLIC "-//iBATIS.com//DTD SQL Map 2.0//EN"
    "http://ibatis.apache.org/dtd/sql-map-2.dtd">

<sqlMap namespace="config">

    <select id="getLastLogNotRealTime" parameterClass="java.util.HashMap" resultClass="java.util.HashMap">
    SELECT idx
        ,daemon_type
        ,db_type
        ,proc_type
        ,write_date
        ,delay_minute
        ,success_yn
        ,error_msg
        ,task_sdate
        ,task_edate
        ,regist_date
        ,update_date
    FROM siem_agent_log
    WHERE daemon_type = #daemon_type#
    AND proc_type = #proc_type#
    AND db_type = #db_type#
    order by regist_date desc
    LIMIT 1
    </select>

    <update id="updateAgentLog" parameterClass="java.util.HashMap">
    UPDATE siem_agent_log SET
        success_yn = #success_yn#
        ,error_msg = #error_msg#
        ,task_edate =   datetime('now')
        ,update_date =  datetime('now')
    WHERE daemon_type = #daemon_type#
    AND proc_type = #proc_type#
    AND db_type = #db_type#
    </update>
</sqlMap>
```

- MySqlMapClient.java

```java
public class MySqlMapClient {

    private static volatile SqlMapClient sqlMap;
    private static final Logger logger = LoggerFactory.getLogger(MySqlMapClient.class);
    private static int no;
    private final static String resource = "com/splunk/siemdb/db/xml/sqlmap-config.xml";

    public static SqlMapClient getSqlMapInstance() {
        return getSqlMapInstance(resource);
    }

    public static SqlMapClient getSqlMapInstance(String resource) {
        Reader reader = null;
        try {
            if (sqlMap == null) {
                synchronized (SqlMapClient.class) {
                    if (sqlMap == null) {
                        reader = Resources.getResourceAsReader(resource);
                        /*
                        int readData;
                        String change=""; 
                        while((readData = reader.read()) != -1) 
                            change+= Character.toString((char) readData);
                        logger.debug("SqlMapClientBuilder # : " + change);
                        */
                        sqlMap = SqlMapClientBuilder.buildSqlMapClient(reader);
                        reader.close();
                        no++;
                        logger.debug("SqlMapClientBuilder New Instance Count[" + no + "]");
                    }
                }
            } else {
                logger.debug("SqlMapClientBuilder Old Instance Count[" + no + "]");
            }
        } catch (Exception e) {
            logger.error("MySqlMapClient.getSqlMapInstance Exception : " + e.getMessage());
            e.printStackTrace();
        }
        return sqlMap;
    }
}
```

IBATIS