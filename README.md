# Database Backup and Restore Scripts

## Disclaimer

**WARNING**: These scripts are provided "as is" without any warranties or guarantees. They should be thoroughly tested in a safe, non-production environment before being used in any production setting. The author takes no responsibility for any actions or consequences resulting from the use of these scripts. Use at your own risk and exercise caution when working with database operations, especially in production environments.

It is strongly recommended to:
1. Fully understand the script's functionality before use.
2. Test in a controlled, non-production environment first.
3. Ensure you have proper backups before performing any database operations.
4. Review and adapt the scripts to your specific needs and security requirements.

Remember: Safety first when it comes to your valuable data!

## Description

This repository contains two scripts for managing PostgreSQL and Redis database backups:

1. `db_manager`: For PostgreSQL databases
2. `redis_manager`: For Redis databases

Both scripts support backing up databases from Docker containers and restoring them. They can also interact with AWS S3 for storing and retrieving backups.

## Prerequisites

- Docker
- AWS CLI

## Configuration

1. Copy the `.env.example` file to `.env` and fill in the required values:

```.env.example
AWS_BUCKET_NAME="bucket_name"
AWS_BUCKET_PATH="backups"
```

2. For Redis, set the `REDIS_PASSWORD` environment variable if your Redis instance requires authentication:

```bash
export REDIS_PASSWORD="your_redis_password"
```

## Usage

### PostgreSQL (db_manager)

1. Backup a database:
```bash
./db_manager backup <container_name>
```

2. Restore a database:
```bash
./db_manager restore <container_name> <backup_file> <database_name>
```

3. Download a backup from S3:
```bash
./db_manager download <s3_file>
```

### Redis (redis_manager)

1. Backup a Redis database:
```bash
./redis_manager backup <container_name>
```

2. Restore a Redis database:
```bash
./redis_manager restore <container_name> <backup_file>
```

## Examples

### PostgreSQL

1. Backup the database in the container named "postgres_container":
```bash
./db_manager backup postgres_container
```

2. Restore the "myapp_db" database from a local backup file:
```bash
./db_manager restore postgres_container ./backups/pg_dump_2023-04-15-10-30.sql myapp_db
```

3. Restore the "myapp_db" database from an S3 backup:
```bash
./db_manager restore postgres_container s3://my-bucket/backups/postgres/pg_dump_2023-04-15-10-30.sql myapp_db
```

### Redis

1. Backup the Redis database in the container named "redis_container":
```bash
./redis_manager backup redis_container
```

2. Restore a Redis database from a local backup file:
```bash
./redis_manager restore redis_container ./backups/redis_dump_2023-04-15-10-30.rdb
```

3. Restore a Redis database from an S3 backup:
```bash
./redis_manager restore redis_container s3://my-bucket/backups/redis/redis_dump_2023-04-15-10-30.rdb
```

## Notes

- The scripts will create a `backups` directory in the current working directory if it doesn't exist.
- Always verify the integrity of your backups and test the restore process in a safe environment before using it in production.
