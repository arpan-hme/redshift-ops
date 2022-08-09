## Code for Stored Procedures

* show procedure create_mv(varchar(100), varchar(500));
* SELECT * FROM svl_stored_proc_messages ORDER BY recordtime desc;
* create_mv
```
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
```


* cancel_redshift_query

```
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
```

* vacuum
```
CREATE OR REPLACE Procedure vacuumTables(upcList IN varchar(max))
AS 'DECLARE
  idx int;
  slice varchar(8000);
  upcListVar varchar(max);
BEGIN
  idx = 1;
  upcListVar = upcList;
  WHILE idx != 0 LOOP
    idx = charindex('','', upcListVar);
    IF idx != 0 THEN
      slice = left(upcListVar, idx - 1);
    END IF;
    IF idx = 0 THEN
      slice = upcListVar;
    END IF;
    IF len(slice) > 0 THEN
      RAISE INFO ''vacuum FULL % '', slice;
      EXECUTE ''VACUUM FULL '' || slice ;
    END IF;
    upcListVar = right(upcListVar, len(upcListVar) - idx);
  END LOOP;
END;
' LANGUAGE plpgsql;

```
