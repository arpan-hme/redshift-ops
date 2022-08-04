select count(1) as aborted_count from STL_QUERY where starttime >= CURRENT_DATE and aborted = 1;
select * from STL_WLM_RULE_ACTION where userid = '111' order by recordtime desc;
select min(recordtime) as min, max(recordtime) as max from STL_WLM_RULE_ACTION;
select * from STV_WLM_CLASSIFICATION_CONFIG;
select * from STV_WLM_SERVICE_CLASS_CONFIG;
select * from STV_WLM_SERVICE_CLASS_STATE;
select * from pg_user;
select * from pg_group;
select * from STL_WLM_RULE_ACTION where extract(hour from CONVERT_TIMEZONE('Asia/Kolkata',recordtime)) = 15 and userid = '111' order by recordtime ;
select * from stv_wlm_service_class_config;
SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
--
select query, querytxt from stl_query where query in (select query
from STL_WLM_RULE_ACTION 
where recordtime > dateadd(m,-30,GETDATE()) 
and userid = '111' or userid = '124' 
and action = 'abort')
--
select pid from stv_recents where status = 'Running' and trim(user_name) = 'metabase' and duration > 100000000;
 Stored Procedure
CREATE OR REPLACE PROCEDURE cancel_redshift_query() AS $$
DECLARE
  pids RECORD;
  counter INT;
BEGIN
	RAISE INFO 'Killing queries..';
    counter:=0;
    FOR pids IN select pid from stv_recents where status = 'Running' and trim(user_name) = 'metabase' and duration > 100000000   LOOP
    EXECUTE 'CANCEL ' || pids.pid;
    counter:= counter+1;
  END LOOP;
  RAISE INFO 'Killed Queries %', counter;
  RETURN;
END;
$$ LANGUAGE plpgsql;    
--
show procedure create_mv(varchar(100), varchar(500));
SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
--   
CREATE OR REPLACE PROCEDURE create_mv(name character varying(100),query character varying(10000)) AS $$
DECLARE 

BEGIN 
  RAISE INFO 'Creating MV %',name;
  EXECUTE 'DROP MATERIALIZED VIEW IF EXISTS ' || name ;
  EXECUTE 'CREATE MATERIALIZED VIEW ' || name || ' AS ' || query;
  EXECUTE 'GRANT SELECT ON TABLE ' || name || ' TO GROUP readers';
  RAISE INFO 'Created MV %', name;
  RETURN;
END;
$$ LANGUAGE plpgsql;
--                                                                                          
--                                                                                          
create user lambda_user createuser password 'u8{,EwHN,a~{Wn^';
alter user lambda_user syslog access unrestricted CONNECTION LIMIT 1 SESSION TIMEOUT 60;
alter group etl add user lambda_user;
--
grant select on tables in schema objectives_objectivelog to metabase_pq_user;
grant USAGE on schema objectives_objectivelog to metabase_pq_user;
grant select on all tables in schema objectives_objectivelog to metabase_pq_user;
--
--
Query to get which user has access to a certain schema
--SELECT
--    u.usename,
--    s.schemaname,
--    has_schema_privilege(u.usename,s.schemaname,'create') AS user_has_select_permission,
--    has_schema_privilege(u.usename,s.schemaname,'usage') AS user_has_usage_permission
--FROM
--    pg_user u
--CROSS JOIN
--    (SELECT DISTINCT schemaname FROM pg_tables) s
--WHERE
--s.schemaname = 'objectives_objectivelog'
--;
--
--
UNload, replace ' by '' inside query
--UNLOAD('select * from feeds_v2_usercontentfeedsactivity limit 10')
--TO 's3://hme-data-analytics/finance-new-premium-' 
--iam_role 'arn:aws:iam::720256604387:role/redshift-s3-access'
--parallel off
--CSV;


select pid from stv_recents where status = 'Running' and trim(user_name) = 'metabase' and duration > 100000000;

SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;

select sysdate;

