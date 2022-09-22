### UNLOAD 
* UNload, replace ' by '' inside query
```
UNLOAD('select * from feeds_v2_usercontentfeedsactivity limit 10')
TO 's3://hme-data-analytics/finance-new-premium-' 
iam_role 'arn:aws:iam::720256604387:role/redshift-s3-access'
parallel off
CSV;
```

### Pivot
```
"select * from (select usename, extract(DAY from CONVERT_TIMEZONE('Asia/Kolkata',starttime)) as dayy from stl_query join pg_user on usesysid = userid ) as a
pivot ( count(*) as cnt for a.dayy in  (15,16,17,18,19,20,21));"
```
