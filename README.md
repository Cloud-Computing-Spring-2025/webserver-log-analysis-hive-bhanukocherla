# Web-Server-Log-Analysis

This project analyzes web server logs using **Apache Hive**. The goal is to extract meaningful insights from log data stored in Hadoop.

## **Steps Followed**
1. **Set up the Hive environment** using Docker.
2. **Created the Hive table** for storing web server logs.
3. **Loaded log data** from a CSV file.
4. **Ran Hive queries** to analyze the logs.

## **Commands Used**
```sql
SHOW TABLES;
SELECT COUNT(*) FROM web_server_logs;
SELECT * FROM web_server_logs LIMIT 10;
SELECT DISTINCT status_code FROM web_server_logs;
SELECT status_code, COUNT(*) AS count FROM web_server_logs WHERE status_code IS NOT NULL GROUP BY status_code ORDER BY count DESC;
SELECT * FROM web_server_logs WHERE status_code = 500 LIMIT 10;
SELECT request, COUNT(*) AS error_count FROM web_server_logs WHERE status_code = 500 GROUP BY request ORDER BY error_count DESC LIMIT 10;
