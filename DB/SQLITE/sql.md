# SQL

- SELECT
- LIMIT `<int>`

```sql
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
```

- UPDATE
- datetime('now')

```sql
UPDATE siem_agent_log SET
            success_yn = #success_yn#
            ,error_msg = #error_msg#
            ,task_edate =   datetime('now')
            ,update_date =  datetime('now')
        WHERE daemon_type = #daemon_type#
        AND proc_type = #proc_type#
        AND db_type = #db_type#
```

- INSERT

```sql
INSERT INTO siem_agent_log
(
    daemon_type
    ,db_type
    ,proc_type
    ,write_date
    ,delay_minute
    ,success_yn
    ,task_sdate
    ,regist_date
)
VALUES
(
    #daemon_type#
    ,#db_type#
    ,#proc_type#
    ,#write_date#
    ,#delay_minute#
    ,#success_yn#
    ,datetime('now')
    ,datetime('now')
)
```

SQLSQL