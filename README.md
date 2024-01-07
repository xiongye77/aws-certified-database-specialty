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

# RDS parameter group 
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/8a0b23ae-e3d4-4464-806d-deaa154a3121)

When you change a static parameter and save the DB parameter group, the parameter change takes effect after you manually reboot the associated DB instances. For static parameters, the console always uses pending-reboot for the ApplyMethod.

# RDS Option Group
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/27d9fa86-b143-4272-8232-cc8f7ae12e04)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/8e30e9bf-90a9-4842-b871-0e4ea4fca37d)


# RDS Multi-AZ DB Instance/DB Cluster 
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b1045ad9-e44b-4b09-a5d6-d9ec3aeca354)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/4bf4b726-c025-4b08-8102-53d4660738f1)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a13d0253-bbed-4ed4-bf0d-4403b23f6f6d)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/26a2e8e4-dd22-413e-b9c0-e36de5391707)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e714eb5b-e49e-4113-9d11-a7f07665f916)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/21d7e541-03d1-43a8-bc4e-b83d63283401)


# RDS Proxy
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ed36a97f-5162-4cc9-a7d9-f5e1c87f4414)

# RDS Read Replica
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/185fb19d-e9e0-4d82-a8e1-2f3008f42ffc)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6bee7466-ced8-4ae1-bdd2-ddbc6eaccb6a)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/c1d8f702-e6b4-4b93-aaa7-d66eb7ea7f0b)

# RDS Multi-AZ with Read Replica
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a0f337ea-b357-48e2-a277-9bbfdbb80e8a)



# RDS DB Subnet 
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/1f660909-8262-40b9-9729-33cc18458bee)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6bc355f5-9621-4a2c-9d37-77051318f32b)

![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e9244776-19f2-4c14-86dd-8ad695252a18)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/7768ac1e-2403-4e52-81a0-ac25137ea20f)


# AWS Aurora
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/d9a9d699-de96-4a2c-be51-ff2cbcd59f8c)
storage separated from compute
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/41a8e6c5-6de0-4ca2-b2de-c965c877c68c)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/fcc0ef8d-a22a-4ebc-890b-5f2e10875c85)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/4ab1afb2-58d8-41fc-bda7-de59bbbb39d7)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b3e3f7bb-8c62-4971-a9f3-0e70c81cf1f6)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/07e22900-6fda-4fa6-a94c-ab8a3b06be88)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5347f654-e73c-4b20-baba-89f3ad6e6deb)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5ec99a2c-f736-45a1-b641-f72f07b6f20e)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/19fa13ce-3fc5-4308-bfab-68ce9fcb75a8)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/a86ac44f-7c15-468b-b131-6a6154b88581)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/2bd23c0e-de85-4767-9c8c-90fbaae93434)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b186da54-81c2-4368-b237-8e4336f12569)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/717c7c24-5b13-4335-afd0-20d738a763f9)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e97c015d-c329-49be-9a71-8f705b98b75b)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/0506b45c-fe4a-4bea-8592-087f8dd68022)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6ac3be04-a4fb-4ab9-a249-e104c5d74c95)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/128e1f2f-edc0-49c3-bbb5-71330446a32d)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b4223f0c-a41c-41ed-adbb-245ebd61a97d)


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
 

check postgres uptime 
postgres=> select current_timestamp - pg_postmaster_start_time() as uptime;
     uptime
-----------------
 06:22:02.876339
(1 row)
