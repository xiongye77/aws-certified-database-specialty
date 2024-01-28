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
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/211cb5ac-809d-4b00-bf4c-4f8471567cbf)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/23e8feb3-c3a5-4adc-9388-cf165a2a55ed)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/fac2ca80-e37f-4888-a127-393977eed71b)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e64f823e-d666-449c-8c9e-216c7507001e)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/bbf0e576-9a64-4df1-83b9-90a171b430d7)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/041d842e-5ad6-4520-b8eb-83b2d711c7ac)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ccb3f753-d485-42ab-ac69-41e04c78162a)

# AWS Aurora COW clone
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/c65c3e33-dd55-4f7e-8d4f-edff9611076d)


# AWS ElasticCache In Memory Database
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6892fcf0-9398-420c-b447-81793ebd5f57)

# RDS monitor using  RDS Event and Cloudwatch Metric and performance insight
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/03bf8a73-ef20-4104-b9c9-d0e9940df774)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/be7f32ae-a32c-4521-ac28-f6e602283fdb)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/b9a62840-b843-4bd8-a53a-f43d0713af0b)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6e92bb2e-ef32-4388-abdf-8c8cd7883520)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5263b09a-ab67-4971-9d80-2f35f063c95a)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/1b1816c0-d3ae-4fbe-a114-c0d5d4829d31)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/aeb5054d-31b5-4af0-895c-683debc7e140)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/0d9e9c00-1753-4da3-9607-d631e07e4a6a)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/32388130-bb80-48ae-aa9f-b2f5e9142ccb)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/48d22919-9a29-4b0e-b90f-7bba7c5239d6)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/400a473f-798d-42db-b915-aa8464332c44)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/2a31778d-e538-4d50-bb97-43484b1bd664)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/26d0bcce-6ba0-457c-bffc-e0b1cad1288f)


# Enhance DynCloudWatch Contributor Insights
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/be2cf9cd-ad81-4bcb-a4cd-5629088567f4)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/32b0818d-c60c-445c-9eab-897633b5ac77)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/6f5eb15f-7fc7-4856-8fad-1a94bf3e73e7)


# Database audit 
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/49e950a4-7dc5-4fcb-b241-d2d250de74db)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/83f39efe-5920-4437-8812-20a7e0ebac51)

# RDS replication issue 
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/1ec4b33b-6baa-4525-817f-5ffe55a8d5ea)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ec12975a-06bf-4d8d-ab42-cb7262d627dd)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/267300bf-bdc0-451d-bf2b-9ca671bdde20)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/85e68297-cc2b-4085-87c8-9214e6eaf59d)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/fa82ee32-27ee-48a2-81e1-18bda3a67347)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/c858fcfa-fa99-4f6a-971b-a9e1465b3b62)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5b708675-621e-4766-b69a-6446983f0515)


# RDS Troubleshooting
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/c117c0d3-20ef-46b8-ae8b-2f5793371364)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/47086221-e947-433f-a02b-9ea277e1699e)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5c06097c-9478-4935-a602-f59d2cb54a2a)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/de3be93d-94c8-4d4c-bcc1-6a323a05af61)


![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/245025bc-f3ee-4955-b4bf-93a4f4becb00)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/653ad0fc-17da-47c1-9ac0-c9d0acd3e9ca)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5d261146-e104-4416-af33-78b1c0f23d20)


# AWS DynamoDB provisioned capacity with autoscale vs on-demand capacity  (2024/01/28)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/50776e6b-fb88-4be1-b866-b8d941e47544)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/d17f279d-b576-443b-9630-171f51707d62)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/30b26d32-e73e-436c-bcaa-cb409718cc7d)

![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/85aa9f99-60ca-445c-8c12-5edbda1a65c8)
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/924ed675-54bd-4f55-a98c-b7af3e23615e)
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/4bfc2d24-f337-4d26-af5d-7beeefb69653)

![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/12ef5020-c8f1-4f60-9655-aa579a132315)

With provisioned capacity you can also use auto scaling to automatically adjust your table’s capacity based on the specified utilization rate to ensure application performance, and also to potentially reduce costs. To configure auto scaling in DynamoDB, set the minimum and maximum levels of read and write capacity in addition to the target utilization percentage.


# AWS DynamoDB local secondary index /global secondary index  
Secondary index allow efficient access to data with attributes other than the primary key.
Local secondary index: An index that has the same partition key as the base table, but a different sort key. Local secondary index can only be created when create the table.
Global secondary index — An index with a partition key and a sort key that can be different from those on the base table.
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/58da0454-6218-44ef-b834-627d47a20ba6)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/55e9a141-89ef-41df-aa05-80808a93807e)

![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ad41def9-481b-43ab-8391-362fddfdfd3f)

# AWS DynamoDB stream to Kinesis data stream 
# AWS DynamoDB transaction
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/b397c7c7-0ca7-4951-bd8f-837c508abc86)
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/9f295555-1b0a-49e2-9e21-19cb5f6c9f1a)

![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/beb235bf-0422-4eca-b8dd-e54fe7c06f18)
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/bac71bbd-12a6-40d1-83c1-d5956598a770)

# AWS DynamoDB stream
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/1a8268b5-650e-493a-b841-a04dbbd40686)
![image](https://github.com/xiongye77/aws_sysops_tasks/assets/36766101/766214fb-c1b9-4add-9919-e73f2acf80bd)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e1574c6e-2fcc-4086-8566-bdeb11bf4a2b)
# DynamoDB Global Table
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ef9596f9-5154-4f20-8b22-3f5f897ec436)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/7253636e-f76a-41b7-8731-892e649f2643)



# AWS Dynamodb Encryption
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/3212363a-8268-4796-bc7b-032cabfbff14)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ea628e3e-2950-45f2-924e-cfdb62f4d9f6)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/564e9dae-a36c-45b4-956d-f75b3bf15b9a)


# Dynamodb scan/query/filter/project expression
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/1013a32c-0965-4143-a738-c4235a80432c)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/4c93bfaa-f9a6-4b95-8e53-c19ba7ffbbca)

![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/0a25b0c8-9a14-4a7f-88e2-5456cb2bb12c)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/e03fe30d-a391-417b-86bd-0bcd3643bf15)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/67c0db5e-11f8-41ca-941d-0fa1d61d8641)

# Dynamodb BatchGetItem
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/bb505ce0-7ab0-4524-bfec-1f60782a5aea)
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/5e6ee9b9-8588-4230-82bf-c05b28ee9d86)

# Dynamodb PITR
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/3472829b-ed36-4369-8399-8ce123177799)

# Dynamodb consistency mode
![image](https://github.com/xiongye77/aws-certified-database-specialty/assets/36766101/ef223061-0b8f-4b03-9ded-5447591072b9)

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
