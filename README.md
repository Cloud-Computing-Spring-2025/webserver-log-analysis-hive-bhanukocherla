## Partitioning Web Server Logs in Hive

To optimize query performance, we implemented **partitioning** on the `web_server_logs` table. 
Partitioning helps in **reducing query execution time** by allowing Hive to scan only relevant partitions.

### Steps to Implement Partitioning:

1. **Created a partitioned table:**
   ```sql
   CREATE TABLE web_server_logs_partitioned (
       ip STRING,
       log_timestamp STRING,
       request STRING,
       user_agent STRING
   )
   PARTITIONED BY (status_code INT)
   ROW FORMAT DELIMITED
   FIELDS TERMINATED BY ',';
