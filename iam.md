* create user lambda_user createuser password 'u8{,EwHN,a~{Wn^';
* alter user lambda_user syslog access unrestricted CONNECTION LIMIT 1 SESSION TIMEOUT 60;
* alter group etl add user lambda_user;
* grant select on tables in schema objectives_objectivelog to metabase_pq_user;
* grant USAGE on schema objectives_objectivelog to metabase_pq_user;
* grant select on all tables in schema objectives_objectivelog to metabase_pq_user;


* Query to get which user has access to a certain schema
```SELECT
    u.usename,
    s.schemaname,
    has_schema_privilege(u.usename,s.schemaname,'create') AS user_has_select_permission,
    has_schema_privilege(u.usename,s.schemaname,'usage') AS user_has_usage_permission
FROM
    pg_user u
CROSS JOIN
    (SELECT DISTINCT schemaname FROM pg_tables) s
WHERE
s.schemaname = 'objectives_objectivelog';
```
