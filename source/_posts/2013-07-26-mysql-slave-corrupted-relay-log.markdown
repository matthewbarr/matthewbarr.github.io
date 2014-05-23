---
author: mhbarr
comments: true
date: 2013-07-26 16:22:58+00:00
layout: post
slug: mysql-slave-corrupted-relay-log
title: Mysql slave corrupted relay log
wordpress_id: 46
---

If you have a slave that has a corrupted relay log, it will show:
`Last_Error: Could not parse relay log event entry. The possible reasons are: the master's binary log is corrupted (you can check this by running 'mysqlbinlog' on the binary log), the slave's relay log is corrupted (you can check this by running 'mysqlbinlog' on the relay log), a network problem, or a bug in the master's or slave's MySQL code. If you want to check the master's binary log or slave's relay log, you will be able to know their names by issuing 'SHOW SLAVE STATUS' on this slave.`
in it's mysqld.log & slave status.  


This can happen because of a crash on the mysql slave, or a disk filling up, or something.



You can fix it by executing on the slave in question:

1. STOP SLAVE
2. SHOW SLAVE STATUS
3. Note down ‘Relay_Master_Log_File’ and ‘Exec_Master_Log_Pos’ entries.
4. RESET SLAVE
5. CHANGE MASTER TO ….. (use MASTER_LOG_FILE=relay_master_log_file and MASTER_LOG_POS=exec_master_log_pos from Step 3)
6. START SLAVE


For example, #5 might be something like: 

    change master to master_log_file="mysql-bin.001878",master_log_pos=898514732;
