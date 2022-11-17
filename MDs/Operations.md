### UNLOAD 
* UNload, replace ' by '' inside query
```
UNLOAD('select * from feeds_v2_usercontentfeedsactivity limit 10')
TO 's3://hme-data-analytics/finance-new-premium-' 
iam_role 'arn:aws:iam::720256604387:role/redshift-s3-access'
parallel off
CSV;
```
### COPY from s3 to redshift table
```
create table scratch.remarketing_test_control_group
(
	email varchar(max),
	first_name varchar(100),
	last_name varchar(100),
	phoneno varchar(100),
	test_group_flag integer
);


COPY scratch.remarketing_test_control_group
FROM
  's3://healthifyme-table-dump/adhoc/file1.csv'
  CREDENTIALS 'aws_access_key_id=<>;aws_secret_access_key=<>'
  CSV
  IGNOREHEADER 1;


GRANT SELECT ON scratch.remarketing_test_control_group TO GROUP readers;
select * from scratch.remarketing_test_control_group limit 4;

```

### Pivot
```
"select * from (select usename, extract(DAY from CONVERT_TIMEZONE('Asia/Kolkata',starttime)) as dayy from stl_query join pg_user on usesysid = userid ) as a
pivot ( count(*) as cnt for a.dayy in  (15,16,17,18,19,20,21));"
```

### Get user with group name
```
SELECT usename, groname 
FROM pg_user, pg_group
WHERE pg_user.usesysid = ANY(pg_group.grolist)
AND pg_group.groname in (SELECT DISTINCT pg_group.groname from pg_group);
```

### Yamraj
```
call cancel_redshift_query_all();
call cancel_anomalisa();
call cancel_metabase_pq();  
```


### Mongo Dump to Redshift
```
CREATE TABLE adhoc_sheets.analytics_apple_asa_campaign_report_stage (LIKE adhoc_sheets.analytics_apple_asa_campaign_report);

COPY adhoc_sheets.analytics_apple_asa_campaign_report_stage
FROM 's3://healthifyme-table-dump/adhoc/asa_history.csv'
  CREDENTIALS 'aws_access_key_id=AKIA2PMVDTDR22SS5VSB;aws_secret_access_key=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'
  CSV
  IGNOREHEADER 1;                                      

BEGIN TRANSACTION;
INSERT INTO adhoc_sheets.analytics_apple_asa_campaign_report
SELECT * FROM adhoc_sheets.analytics_apple_asa_campaign_report_stage;
END TRANSACTION;
DROP TABLE adhoc_sheets.analytics_apple_asa_campaign_report_stage;

```

### Collect
```
-- Kill
call cancel_redshift_query_all();
call cancel_anomalisa();
call cancel_metabase_pq();

-- MV
CREATE MATERIALIZED VIEW as 
REFRESH MATERIALIZED VIEW mv_coach_tl_hot_mapping;
GRANT SELECT ON TABLE mv_fb_google_marketing_spends_master TO  GROUP readers;
show view mv_fb_google_misc_appsflyer_cohort_report;
SELECT * FROM stv_mv_deps limit 10;
select  name from STV_MV_INFO order by name;


-- Governance
create user test password 'Hme@1234567';
CREATE SCHEMA scratch AUTHORIZATION metabase QUOTA 50 GB;
grant create on schema scratch to metabase_priority;
revoke insert on schema public from metabase;
create user jupyter password 'UH`QR>}Xesn]R(1';
grant all on schema scratch to jupyter;
alter group metabase add user jupyter;
grant select on all tables in schema ga_3_web_data to GROUP readers;
GRANT EXECUTE ON public.sp_user_hvc_info(INOUT rs_out refcursor) TO GROUP readers;                                                                                                                                             --user wise group SELECT usename, groname  FROM pg_user, pg_group WHERE pg_user.usesysid = ANY(pg_group.grolist) AND pg_group.groname in (SELECT DISTINCT pg_group.groname from pg_group);
                                               
                         
      
-- Stored Procedure
SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
show procedure sp_user_hvc_info();
show procedure create_mv(varchar(100), varchar(500));
show procedure cancel_redshift_query_all();  
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
             
-- Vaccum
vacuum full adhoc_sheets.appsflyer_cohort_report;
select * from svv_vacuum_summary;
select * from stl_vacuum where userid = 100 and eventtime > '2022-08-15 20:31:48.950516' and eventtime < '2022-08-15 20:34:48.950516' order by eventtime desc;
select * from svv_vacuum_progress;
select * from stl_query where starttime > '2022-08-15 20:31:48.950516' and starttime < '2022-08-15 20:34:48.950516' order by starttime desc;
where starttime between '2022-08-15 20:29' and '2022-08-15 20:35' and querytxt ilike '%vacuum%' order by starttime;                                                                                                                                                
                                                                                          
      
      
```

### System Queries
```
-- System Tables
select * from stl_Wlm_rule_action;
select * from STV_WLM_CLASSIFICATION_CONFIG;
select * from find_depend where refbyschemaname='public' order by name;
select * from STV_MV_DEPS;
select *  from STL_WLM_QUERY;
select * from pg_user;
select * from stl_alert_event_log limit 10;
select * from SVV_TABLE_INFO limit 10;
select "table", unsorted, vacuum_sort_benefit from svv_table_info order by vacuum_sort_benefit desc;
select * from SVV_VACUUM_SUMMARY limit 10;
select distinct schemaname, tablename from pg_table_def where schemaname = 'stitch_cadence_180_v2';
select * from pg_table_def limit 2;
SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
show search_path;
select "table", schema, tbl_rows from SVV_TABLE_INFO where "table" in (select "table" from SVV_TABLE_INFO group by "table" having count(1) = 2) order by "table", tbl_rows desc;
select * from find_depend where refbyschemaname='stitch_cadence_180_2' order by name;
SELECT * FROM stv_mv_deps limit 10;
select * from STL_DDLTEXT where userid = '103' order by starttime desc;
select * from stl_load_errors order by starttime desc limit 10  
select  name from STV_MV_INFO order by name;
select * from STL_WLM_RULE_ACTION where userid = 100 order by recordtime desc;

```
