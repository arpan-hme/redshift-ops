* select count(1) as aborted_count from STL_QUERY where starttime >= CURRENT_DATE and aborted = 1;
* select * from STL_WLM_RULE_ACTION where userid = '111' order by recordtime desc;
* select min(recordtime) as min, max(recordtime) as max from STL_WLM_RULE_ACTION;
* select * from STV_WLM_CLASSIFICATION_CONFIG;
* select * from STV_WLM_SERVICE_CLASS_CONFIG;
* select * from STV_WLM_SERVICE_CLASS_STATE;
* select * from pg_user;
* select * from pg_group;
* select * from STL_WLM_RULE_ACTION where extract(hour from CONVERT_TIMEZONE('Asia/Kolkata',recordtime)) = 15 and userid = '111' order by recordtime ;
* select * from stv_wlm_service_class_config;
* SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
* select query, querytxt from stl_query where query in (select query from STL_WLM_RULE_ACTION where recordtime > dateadd(m,-30,GETDATE()) and userid = '111' or userid = '124' and action = 'abort')
* select pid from stv_recents where status = 'Running' and trim(user_name) = 'metabase' and duration > 100000000;
  
                                                                         

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

