# minio-as-s3-alternative

This repository documents how to use MinIO object storage as an alternative of AWS S3 for local and development use cases.

## Steps

- Start **MinIO** server inside docker container with `docker compose up -d`
- Enter inside the `initialize-s3service` container and run following commands

```bash

$ /usr/bin/mc alias set s3service http://s3service:9000 <USER_FROM_MINIO_ENV_FILE> <PASSWORD_FROM_MINIO_ENV_FILE>;
$ /usr/bin/mc mb s3service/"$${BUCKET_NAME}";
$ /usr/bin/mc admin user add s3service "$${ACCESS_KEY}" "$${SECRET_KEY}";
$ /usr/bin/mc admin policy attach s3service readwrite --user "$${ACCESS_KEY}";

```

![Caution]

> I am using NodeJS SDK for AWS S3 `aws-sdk/s3-client` and initializing the `S3Client` imported from the library. Since native AWS S3 endpoints are in the form `https://<BUCKET_NAME>.s3.amazonaws.com` but, **MinIO** uses the format `https://s3.amazonaws.com/<BUCKET_NAME>`. So adding the property `forcePathStyle=true` is a must in order for the setup to work
