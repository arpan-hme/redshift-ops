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
