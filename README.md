## **Partitioning Web Server Logs in Hive**

To optimize query performance, we implemented **partitioning** on the `web_server_logs` table.  
Partitioning helps in **reducing query execution time** by allowing Hive to scan only relevant partitions.

---

### ** Steps to Implement Partitioning:**

### 1️⃣ **Created a Partitioned Table**
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


SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;

INSERT INTO TABLE web_server_logs_partitioned PARTITION (status_code=200)
SELECT ip, log_timestamp, request, user_agent
FROM web_server_logs WHERE status_code = 200;

INSERT INTO TABLE web_server_logs_partitioned PARTITION (status_code=404)
SELECT ip, log_timestamp, request, user_agent
FROM web_server_logs WHERE status_code = 404;

INSERT INTO TABLE web_server_logs_partitioned PARTITION (status_code=500)
SELECT ip, log_timestamp, request, user_agent
FROM web_server_logs WHERE status_code = 500;

SHOW PARTITIONS web_server_logs_partitioned;

SELECT * FROM web_server_logs_partitioned WHERE status_code = 200 LIMIT 5;
SELECT * FROM web_server_logs_partitioned WHERE status_code = 404 LIMIT 5;
SELECT * FROM web_server_logs_partitioned WHERE status_code = 500 LIMIT 5;

SELECT COUNT(*) FROM web_server_logs;
SELECT COUNT(*) FROM web_server_logs_partitioned;

INSERT OVERWRITE LOCAL DIRECTORY '/tmp/webserver_logs_output'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
SELECT * FROM web_server_logs_partitioned;
