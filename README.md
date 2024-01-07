# aws-certified-database-specialty

# Encrypt an unencrypted RDS using snapshot
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/72ed6e5e-0b7c-4a11-9346-2c4a680d5aeb)

# RDS backup using backup VS snapshot
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a7b88ced-7360-441d-be39-89ce6fa1874d)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/1ee35bb9-54c7-480a-9837-25b415539f8f)

# RDS PITR
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a85a7411-0736-4b77-80e2-9cd4cb31718c)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ee74bef4-c6df-4f28-9d96-1b70dd31436f)

# RDS Restore
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e980aa5a-545e-483a-8add-2c155d5cafd6)

# check postgres uptime 

postgres=> select current_timestamp - pg_postmaster_start_time() as uptime;
     uptime
-----------------
 06:22:02.876339
(1 row)



# DB parameter group 
1 Create new db parameter group and make changes to parameter.
![image](https://user-images.githubusercontent.com/36766101/190900783-4339eb3c-47eb-46f6-8c43-c5bcfc693abf.png)

2 Associate parameter group with db instance

3 Apply immediately and reboot db instance if needed. 
![image](https://user-images.githubusercontent.com/36766101/190900862-c313d9d3-ceec-4d35-97a3-42be9ab38d42.png)





postgres create extension
postgres=> \dx



postgres=> create extension pg_stat_statements;


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

# RDS Automatic Backup
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b0eb2397-6212-4756-b75b-03988d673086)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a600e52d-44bf-4711-aba1-3304e9a79ebe)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/aa390dfa-c07c-4357-8805-bf780bff09e3)




SELECT * FROM aws_s3.query_export_to_s3(' select * from ua_offline_event;', :'s3_uri_1');


