### UNLOAD 
* UNload, replace ' by '' inside query
```
UNLOAD('select * from feeds_v2_usercontentfeedsactivity limit 10')
TO 's3://hme-data-analytics/finance-new-premium-' 
iam_role 'arn:aws:iam::720256604387:role/redshift-s3-access'
parallel off
CSV;
```
