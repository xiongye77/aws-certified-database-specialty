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

![image](https://user-images.githubusercontent.com/36766101/190889287-2328df0a-6f33-406a-85c7-e73e61d5aa19.png)



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


# Postgres RDS export to S3 use extension

1 AWS RDS Postgres Export to S3: Exporting Data to Amazon S3
CREATE EXTENSION IF NOT EXISTS aws_s3 CASCADE;

postgres=> CREATE EXTENSION IF NOT EXISTS aws_s3 CASCADE;
NOTICE:  installing required extension "aws_commons"
CREATE EXTENSION

2 AWS RDS Postgres Export to S3: PostgreSQL Version Verification

aws rds describe-db-engine-versions   --engine aurora-postgresql --engine-version=13.7  --region=ap-southeast-2
verify s3Export is there 
![image](https://user-images.githubusercontent.com/36766101/190889649-3c6d7db2-a8e1-43c4-ae76-8d813c8dd1ac.png)




3 Create IAM policy/IAM role/Attach Role to RDS

aws iam create-policy  --policy-name rds-s3-export-policy  --policy-document '{
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "s3export",
         "Action": [
           "S3:PutObject"
         ],
         "Effect": "Allow",
         "Resource": [
           "arn:aws:s3:::xero-dgt-test-ap-southeast-2-reporting/*"
         ] 
       }
     ] 
   }'
   

aws iam create-role  --role-name rds-s3-export-role  --assume-role-policy-document '{
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
            "Service": "rds.amazonaws.com"
          },
         "Action": "sts:AssumeRole"
       }
     ] 
   }'
   
aws iam attach-role-policy  --policy-arn arn:aws:iam::328916801733:policy/rds-s3-export-policy  --role-name rds-s3-export-role

aws rds add-role-to-db-cluster    --db-cluster-identifier user-tracking-api-test-cluster   --feature-name s3Export    --role-arn arn:aws:iam::328916801733:role/rds-s3-export-role      --region ap-southeast-2
 
   
![image](https://user-images.githubusercontent.com/36766101/190891695-c50ae14e-134d-4557-9b8d-01a63c4ff1c0.png)

4 Export data to S3 

SELECT aws_commons.create_s3_uri(
   'xero-dgt-test-ap-southeast-2-reporting',
   'ua_offline_event.txt',
   'ap-southeast-2'
) AS s3_uri_1 \gset



select count(*) from "user_tracking_api".public.ua_offline_event;

SELECT * FROM aws_s3.query_export_to_s3(' select * from ua_offline_event;', :'s3_uri_1');


