# Databases Cheatsheet

## PostgreSQL

```bash

```

```sql
-- Inspection
SELECT datname FROM pg_database;
SELECT tablename FROM pg_tables WHERE schemaname = 'public';
SELECT column_name, data_type FROM information_schema.columns WHERE table_name = '<table-name>';
SELECT pg_size_pretty(pg_total_relation_size('<table-name>'));
SELECT pg_size_pretty(pg_database_size('<database-name>'));

-- Monitoring
SELECT * FROM pg_stat_activity;
-- WARNING: Terminates the specified query/process
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE pid = <process_id>; 
```

## MySQL/MariaDB

```bash
# Connection
mysql -u <username> -p
mysql -u <username> -p -h <host> <database>

# Execute & backup
mysql -u <username> -p <database> < file.sql
mysqldump -u <username> -p <database> > backup.sql
mysqldump -u <username> -p --all-databases > all_backup.sql
mysqldump -u <username> -p <database> | gzip > backup.sql.gz
# WARNING: Will overwrite existing data in the database
gunzip < backup.sql.gz | mysql -u <username> -p <database>
```

```sql
-- Inspection
SHOW DATABASES;
USE <database-name>;
SHOW TABLES;
DESCRIBE <table-name>;
SHOW COLUMNS FROM <table-name>;
SHOW TABLE STATUS LIKE '<table-name>';

-- Monitoring
SHOW PROCESSLIST;
-- WARNING: Terminates the specified query/process
KILL <process-id>;
SHOW VARIABLES LIKE '%max_connections%';
SHOW STATUS LIKE '%connections%';
```

## MongoDB

```bash
# Connection
mongo <database-name>
mongo mongodb://host:port/database

# Backup & restore
mongodump --db <database> --out /backup/path
mongorestore --db <database> /backup/path/<database>
mongoexport --db <database> --collection <collection> --out file.json
mongoimport --db <database> --collection <collection> --file file.json
```

```javascript
// Shell commands
use <database-name>
show dbs
show collections

// CRUD
db.<collection>.find()
db.<collection>.find({field: "value"})
db.<collection>.findOne()
db.<collection>.insertOne({key: "value"})
db.<collection>.updateOne({filter}, {update})
db.<collection>.deleteOne({filter})

// Inspection
db.<collection>.countDocuments({filter})
db.<collection>.getIndexes()
db.<collection>.find({field: "value"}).explain("executionStats")

// Indexes
db.<collection>.createIndex({field: 1})
```

## Redis

```bash
# Connection
redis-cli
redis-cli -h <host> -p <port>
redis-cli -h <host> -p <port> -a <password>
```

```bash
# Keys
SET key value
GET key
DEL key
EXISTS key
# WARNING: KEYS can block Redis on large datasets
KEYS pattern
TTL key
EXPIRE key seconds

# Lists
LPUSH list value
RPUSH list value
LPOP list
RPOP list
LRANGE list start stop

# Hashes
HSET hash field value
HGET hash field
HGETALL hash
HDEL hash field

# Sets
SADD set member
SMEMBERS set
SREM set member

# Database
SELECT <db-number>
FLUSHDB
FLUSHALL
INFO
MONITOR
```

## SQLite

```bash
# Connection & operations
sqlite3 <database.db>
sqlite3 <database.db> < file.sql
sqlite3 <database.db> .dump > backup.sql
sqlite3 <database.db> -header -csv "SELECT * FROM table;" > output.csv

# Shell commands
.tables
.schema
.schema <table-name>
.databases
```

```sql
-- Inspection
SELECT name FROM sqlite_master WHERE type='table';
PRAGMA table_info(table_name);
SELECT page_count * page_size as size FROM pragma_page_count(), pragma_page_size();
```

## Common SQL Patterns

```sql
-- Joins
SELECT * FROM table1 JOIN table2 ON table1.id = table2.id;
SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.id;

-- Aggregates
SELECT COUNT(*) FROM table;
SELECT SUM(column), AVG(column), MAX(column), MIN(column) FROM table;
SELECT column, COUNT(*) FROM table GROUP BY column;

-- Filtering
SELECT * FROM table WHERE column LIKE 'pattern%';
SELECT * FROM table WHERE column IN (value1, value2);
SELECT * FROM table WHERE column IN (SELECT column FROM other_table);

-- Sorting & limiting
SELECT * FROM table ORDER BY column ASC;
SELECT * FROM table ORDER BY column DESC;
SELECT * FROM table LIMIT 10 OFFSET 20;
SELECT DISTINCT column FROM table;
```
