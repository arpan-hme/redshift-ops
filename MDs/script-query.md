### Redshift Aborted Queries
```select stl_query.querytxt from              
            (select * 
            from STL_WLM_RULE_ACTION 
            where recordtime > dateadd(h,-30,GETDATE()) 
            and userid in (111,124)
            and action = 'abort') as wlm
            inner join
            stl_query
            on                                                              
            stl_query.query = wlm.query;
```

### Heavy Dashboard
```
        SELECT rd.id as dashboard_id, rd.name as DashboardName, count(*) as count
         FROM report_dashboard rd
         INNER JOIN report_dashboardcard rdc on rd.id = rdc.dashboard_id
         INNER JOIN report_card q on q.id = rdc.card_id
         where rd.archived = false
         group by DashboardName,rd.id
         having count(*) > 30
         order by count desc;
```         
