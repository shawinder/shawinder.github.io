---
layout: post
lang: SQL
title: AWS Automated Database Backup
comments: true
description : AWS Automated MSSQL and MySQL database backup
date: 2020-10-04
header_desc: AWS Automated DB Backup
---
1. Create a new `Write Only` policy by going to `IAM > Policies > Create Policy` and use following JSON:

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "S3WriteOnly",
                "Effect": "Allow",
                "Action": [
                    "s3:PutObject",
                    "s3:ListBucket"
                ],
                "Resource": [
                    "arn:aws:s3:::bucket-1-name",
                    "arn:aws:s3:::bucket-1-name/*",
                    "arn:aws:s3:::bucket-n-name",
                    "arn:aws:s3:::bucket-n-name/*"
                ]
            }
        ]
    }
    ```

2. Create a new `IAM User` and link it to the already created policy. Remember to note down the Key/Secret.

3. Install AWS CLI V2 using Windows/Linux installer

4. **MSSQL** backup:
    * Take *compressed* DB Backup
        ```
        sqlcmd -S WINDOWS_AUTH_USERNAME -d DATABASE_NAME -E -Q "BACKUP DATABASE [DATABASE_NAME] TO  DISK = N'C:\BackupFolder\BACKUP_FILE_NAME.bak' WITH FORMAT, INIT,  NAME = N'Full Database Backup', SKIP, NOREWIND, NOUNLOAD, COMPRESSION,  STATS = 10"
        ```
    * Upload To S3
        ```
        "C:\Program Files\Amazon\AWSCLIV2\aws" configure set AWS_ACCESS_KEY_ID xxx
        "C:\Program Files\Amazon\AWSCLIV2\aws" configure set AWS_SECRET_ACCESS_KEY xxx
        "C:\Program Files\Amazon\AWSCLIV2\aws" configure set default.region ca-central-1
        "C:\Program Files\Amazon\AWSCLIV2\aws" s3api put-object --bucket AWS_BUCKET_NAME --key BACKUP_FILE_NAME.bak --body C:\BackupFolder\BACKUP_FILE_NAME.bak
        ```
    * Use `SQL Server Agent > Jobs` to automate the backup.

5. **MySQL** Backup
    * Configure AWS Credentials
        ```
        aws configure
        ```
    * Take DB Backup
        ```
        mysqldump -u "MYSQL_USER" -p"MYSQL_PASS" "DB_NAME" > "BACKUP_DIR_PATH/BACKUP_FILE.sql"
        ```
    * Compress Backup
        ```
        gzip -f "BACKUP_DIR_PATH/BACKUP_FILE.sql"
        ```
    * Upload To S3
        ```
        aws s3api put-object --bucket AWS_BUCKET_NAME --key "BACKUP_FILE.sql.gz" --body "BACKUP_DIR_PATH/BACKUP_FILE.sql.gz"
        ```
    * Use `Crontab` to automate the backup
