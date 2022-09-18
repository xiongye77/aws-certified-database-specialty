# aws-certified-database-specialty

postgres create extension
postgres=> \dx



postgres=> create extension pg_stat_statements;
CREATE EXTENSION

postgres=> \dx


postgres=> select * from pg_available_extensions;

postgres=> SHOW rds.extensions;


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




postgres=> SELECT version();
                                                   version
-------------------------------------------------------------------------------------------------------------
 PostgreSQL 13.7 on aarch64-unknown-linux-gnu, compiled by aarch64-unknown-linux-gnu-gcc (GCC) 7.4.0, 64-bit
(1 row)


postgres=> SHOW server_version;
 server_version
----------------
 13.7
(1 row)
