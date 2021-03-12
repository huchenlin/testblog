### binlog相关操作 

必须设置如下参数:

    log-bin=bin.log
    log-bin-index=bin-log.index
    max_binlog_size=100M
    binlog_format=row
    server-id=1
不要忘了server-id. 重启检查:

```
root@localhost:(test)09:11:58>show variables like '%log_bin%';
+---------------------------------+----------------------------------+
| Variable_name                   | Value                            |
+---------------------------------+----------------------------------+
| log_bin                         | ON                               |
| log_bin_basename                | /data/mysql57/data/bin           |
| log_bin_index                   | /data/mysql57/data/bin-log.index |
| log_bin_trust_function_creators | OFF                              |
| log_bin_use_v1_row_events       | OFF                              |
| sql_log_bin                     | ON                               |
+---------------------------------+----------------------------------+
```

查看binlog format:

```
root@localhost:(test)09:13:21>show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
```

获取bin log 文件列表:

```
root@localhost:(test)09:17:54>show binary logs;
+------------+-----------+
| Log_name   | File_size |
+------------+-----------+
| bin.000001 |       419 |
+------------+-----------+
```

查看当前正在写入的binlog 文件:

```
root@localhost:(test)09:18:31>show master status;
+------------+----------+--------------+------------------+-------------------+
| File       | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------+----------+--------------+------------------+-------------------+
| bin.000001 |      419 |              |                  |                   |
+------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

查看master上的bin log 文件:

```
root@localhost:(test)09:20:39>show master logs;
+------------+-----------+
| Log_name   | File_size |
+------------+-----------+
| bin.000001 |       419 |
+------------+-----------+
1 row in set (0.00 sec)
```

只查看第一个bin log文件的内容:

```
root@localhost:(test)09:20:47>show binlog events;
+------------+-----+----------------+-----------+-------------+---------------------------------------+
| Log_name   | Pos | Event_type     | Server_id | End_log_pos | Info                                  |
+------------+-----+----------------+-----------+-------------+---------------------------------------+
| bin.000001 |   4 | Format_desc    |         1 |         123 | Server ver: 5.7.20-log, Binlog ver: 4 |
| bin.000001 | 123 | Previous_gtids |         1 |         154 |                                       |
| bin.000001 | 154 | Anonymous_Gtid |         1 |         219 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'  |
| bin.000001 | 219 | Query          |         1 |         291 | BEGIN                                 |
| bin.000001 | 291 | Table_map      |         1 |         340 | table_id: 220 (test.tb02)             |
| bin.000001 | 340 | Write_rows     |         1 |         388 | table_id: 220 flags: STMT_END_F       |
| bin.000001 | 388 | Xid            |         1 |         419 | COMMIT /* xid=19 */                   |
+------------+-----+----------------+-----------+-------------+---------------------------------------+
7 rows in set (0.00 sec)
```

查看指定bin log 文件的内容:

```
root@localhost:(test)09:22:19>flush logs;
Query OK, 0 rows affected (0.00 sec)

root@localhost:(test)09:25:16>show binlog events in 'bin.000002';
+------------+-----+----------------+-----------+-------------+---------------------------------------+
| Log_name   | Pos | Event_type     | Server_id | End_log_pos | Info                                  |
+------------+-----+----------------+-----------+-------------+---------------------------------------+
| bin.000002 |   4 | Format_desc    |         1 |         123 | Server ver: 5.7.20-log, Binlog ver: 4 |
| bin.000002 | 123 | Previous_gtids |         1 |         154 |                                       |
+------------+-----+----------------+-----------+-------------+---------------------------------------+
```

用命令查看binlog 文件内容:

```
mysqlbinlog bin.000001
```







ref : [bin log](https://www.cnblogs.com/geaozhang/p/7401416.html "cnblog bin log 相关知识")

