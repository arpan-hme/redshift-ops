select stl_query.querytxt from              
            (select * 
            from STL_WLM_RULE_ACTION 
            where recordtime > dateadd(h,-30,GETDATE()) 
            and userid in (111,124)
            and action = 'abort') as wlm
            inner join
            stl_query
            on                                                              
            stl_query.query = wlm.query
            
