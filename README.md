# aws-certified-database-specialty

postgres create extension




postgres=> \dx
                 List of installed extensions
  Name   | Version |   Schema   |         Description
---------+---------+------------+------------------------------
 plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(1 row)

postgres=> create extension pg_stat_statements;
CREATE EXTENSION

postgres=> \dx
                                            List of installed extensions
        Name        | Version |   Schema   |                              Description
--------------------+---------+------------+------------------------------------------------------------------------
 pg_stat_statements | 1.8     | public     | track planning and execution statistics of all SQL statements executed
 plpgsql            | 1.0     | pg_catalog | PL/pgSQL procedural language
(2 rows)



postgres=> SELECT query, calls, total_exec_time, rows, 100.0 * shared_blks_hit /
postgres->                nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent
postgres->           FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 5;
                                query                                | calls |  total_exec_time   |  rows   |     hit_percent
---------------------------------------------------------------------+-------+--------------------+---------+----------------------
 insert into test select * from test                                 |     4 |       30744.826386 | 1346070 | 100.0000000000000000
 Select * from aurora_scale_buffer_cache($1)                         |     5 |       16393.545754 |       5 |
 create table test as select * from ua_offline_event                 |     1 |        8640.894206 |       0 |  12.3230490018148820
 create extension pg_stat_statements                                 |     1 | 1707.5418049999998 |       0 |  94.4307540660423854
 select sum(aurora_stat_get_db_commit_latency(oid)) from pg_database |    38 |  730.3744429999999 |      38 | 100.0000000000000000
(5 rows)
