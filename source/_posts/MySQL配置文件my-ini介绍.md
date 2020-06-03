---
title: MySQL配置文件my.ini介绍
date: 2020-03-09 17:23:18
tags:
- MySQL
- 个人博客
categories:
- MySQL
---

## **my.ini是什么？**

my.ini是MySQL数据库中使用的配置文件，修改这个文件可以达到更新配置的目的。

## **my.ini存放在哪里？**

my.ini存放在MySql安装的根目录，如图所示：

{% asset_img 微信图片_20200309172553.png %}

## **my.ini具体内容介绍**

```java
# CLIENT SECTION
# ----------------------------------------------------------------------
#
# The following options will be read by MySQL client applications.
# Note that only client applications shipped by MySQL are guaranteed
# to read this section. If you want your own MySQL client program to
# honor these values, you need to specify it as an option during the
# MySQL client library initialization.
#
[client]

port=3306

[mysql]

default-character-set=gb2312
```

上面显示的是客户端的参数，[client]和[mysql]都是客户端，下面是参数简介：

　　1.port参数表示的是MySQL数据库的端口，默认的端口是3306，如果你需要更改端口号的话，就可以通过在这里修改。

　　2.default-character-set参数是客户端默认的字符集，如果你希望它支持中文，可以设置成gbk或者utf8。



```java
# SERVER SECTION
# ----------------------------------------------------------------------
#
# The following options will be read by the MySQL Server. Make sure that
# you have installed the server correctly (see above) so it reads this 
# file.
#
[mysqld]

# The TCP/IP Port the MySQL Server will listen on
port=3306


#Path to installation directory. All paths are usually resolved relative to this.
basedir="E:/Java/Mysql/"

#Path to the database root
datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

# The default character set that will be used when a new schema or table is
# created and no character set is defined
character-set-server=gb2312

# The default storage engine that will be used when create new tables when
default-storage-engine=INNODB

# Set the SQL mode to strict
sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"

# The maximum amount of concurrent sessions the MySQL server will
# allow. One of these connections will be reserved for a user with
# SUPER privileges to allow the administrator to login even if the
# connection limit has been reached.
max_connections=100

# Query cache is used to cache SELECT results and later return them
# without actual executing the same query once again. Having the query
# cache enabled may result in significant speed improvements, if your
# have a lot of identical queries and rarely changing tables. See the
# "Qcache_lowmem_prunes" status variable to check if the current value
# is high enough for your load.
# Note: In case your tables change very often or if your queries are
# textually different every time, the query cache may result in a
# slowdown instead of a performance improvement.
query_cache_size=0

# The number of open tables for all threads. Increasing this value
# increases the number of file descriptors that mysqld requires.
# Therefore you have to make sure to set the amount of open files
# allowed to at least 4096 in the variable "open-files-limit" in
# section [mysqld_safe]
table_cache=256

# Maximum size for internal (in-memory) temporary tables. If a table
# grows larger than this value, it is automatically converted to disk
# based table This limitation is for a single table. There can be many
# of them.
tmp_table_size=35M


# How many threads we should keep in a cache for reuse. When a client
# disconnects, the client's threads are put in the cache if there aren't
# more than thread_cache_size threads from before.  This greatly reduces
# the amount of thread creations needed if you have a lot of new
# connections. (Normally this doesn't give a notable performance
# improvement if you have a good thread implementation.)
thread_cache_size=8

#*** MyISAM Specific options

# The maximum size of the temporary file MySQL is allowed to use while
# recreating the index (during REPAIR, ALTER TABLE or LOAD DATA INFILE.
# If the file-size would be bigger than this, the index will be created
# through the key cache (which is slower).
myisam_max_sort_file_size=100G

# If the temporary file used for fast index creation would be bigger
# than using the key cache by the amount specified here, then prefer the
# key cache method.  This is mainly used to force long character keys in
# large tables to use the slower key cache method to create the index.
myisam_sort_buffer_size=69M

# Size of the Key Buffer, used to cache index blocks for MyISAM tables.
# Do not set it larger than 30% of your available memory, as some memory
# is also required by the OS to cache rows. Even if you're not using
# MyISAM tables, you should still set it to 8-64M as it will also be
# used for internal temporary disk tables.
key_buffer_size=55M

# Size of the buffer used for doing full table scans of MyISAM tables.
# Allocated per thread, if a full scan is needed.
read_buffer_size=64K
read_rnd_buffer_size=256K

# This buffer is allocated when MySQL needs to rebuild the index in
# REPAIR, OPTIMZE, ALTER table statements as well as in LOAD DATA INFILE
# into an empty table. It is allocated per thread so be careful with
# large settings.
sort_buffer_size=256K
```

上面是服务器断参数，以下是参数的简介：

　　1.port参数也是表示数据库的端口，默认3306。

　　2.basedir参数表示MySQL的安装路径。

　　3.datadir参数表示MySQL数据文件的存储位置，也是数据库表的存放位置。

　　4.default-character-set参数表示默认的字符集，这个字符集是服务器端的。

　　5.default-storage-engine参数默认的存储引擎。

　　6.sql-mode参数表示SQL模式的参数，通过这个参数可以设置检验SQL语句的严格程度。

　　7.max_connections参数表示允许同时访问MySQL服务器的最大连接数，其中一个连接是保留的，留给管理员专用的。

　　8.query_cache_size参数表示查询时的缓存大小，缓存中可以存储以前通过select语句查询过的信息，再次查询时就可以直接从缓存中拿出信息。

　　9.table_cache参数表示所有进程打开表的总数。

　　10.tmp_table_size参数表示内存中临时表的总数。

　　11.thread_cache_size参数表示保留客户端线程的缓存。

　　12.myisam_max_sort_file_size参数表示MySQL重建索引时所允许的最大临时文件的大小。

　　13.myisam_sort_buffer_size参数表示重建索引时的缓存大小。

　　14.key_buffer_size参数表示关键词的缓存大小。

　　15.read_buffer_size参数表示MyISAM表全表扫描的缓存大小。

　　16.read_rnd_buffer_size参数表示将排序好的数据存入该缓存中。

　　17.sort_buffer_size参数表示用于排序的缓存大小



```java
#*** INNODB Specific options ***


# Use this option if you have a MySQL server with InnoDB support enabled
# but you do not plan to use it. This will save memory and disk space
# and speed up some things.
#skip-innodb

# Additional memory pool that is used by InnoDB to store metadata
# information.  If InnoDB requires more memory for this purpose it will
# start to allocate it from the OS.  As this is fast enough on most
# recent operating systems, you normally do not need to change this
# value. SHOW INNODB STATUS will display the current amount used.
innodb_additional_mem_pool_size=3M

# If set to 1, InnoDB will flush (fsync) the transaction logs to the
# disk at each commit, which offers full ACID behavior. If you are
# willing to compromise this safety, and you are running small
# transactions, you may set this to 0 or 2 to reduce disk I/O to the
# logs. Value 0 means that the log is only written to the log file and
# the log file flushed to disk approximately once per second. Value 2
# means the log is written to the log file at each commit, but the log
# file is only flushed to disk approximately once per second.
innodb_flush_log_at_trx_commit=1

# The size of the buffer InnoDB uses for buffering log data. As soon as
# it is full, InnoDB will have to flush it to disk. As it is flushed
# once per second anyway, it does not make sense to have it very large
# (even with long transactions).
innodb_log_buffer_size=2M

# InnoDB, unlike MyISAM, uses a buffer pool to cache both indexes and
# row data. The bigger you set this the less disk I/O is needed to
# access data in tables. On a dedicated database server you may set this
# parameter up to 80% of the machine physical memory size. Do not set it
# too large, though, because competition of the physical memory may
# cause paging in the operating system.  Note that on 32bit systems you
# might be limited to 2-3.5G of user level memory per process, so do not
# set it too high.
innodb_buffer_pool_size=107M

# Size of each log file in a log group. You should set the combined size
# of log files to about 25%-100% of your buffer pool size to avoid
# unneeded buffer pool flush activity on log file overwrite. However,
# note that a larger logfile size will increase the time needed for the
# recovery process.
innodb_log_file_size=54M

# Number of threads allowed inside the InnoDB kernel. The optimal value
# depends highly on the application, hardware as well as the OS
# scheduler properties. A too high value may lead to thread thrashing.
innodb_thread_concurrency=18
```

上面是InnoDB存储引擎使用的参数，一下是参数的简介：

　　1.innodb_additional_mem_pool_size参数表示附加的内存池，用来存储InnoDB表的内容。

　　2.innodb_flush_log_at_trx_commit参数是设置提交日志的时机，若设置为1，InnoDB会在每次提交后将事务日志写到磁盘上。

　　3.innodb_log_buffer_size参数表示用来存储日志数据的缓存区的大小。

　　4.innodb_buffer_pool_size参数表示缓存的大小，InnoDB使用一个缓冲池类保存索引和原始数据。

　　5.innodb_log_file_size参数表示日志文件的大小。

　　6.innodb_thread_concurrency参数表示在InnoDB存储引擎允许的线程最大数。



**注意：每次修改参数后，必须重新启动MySQL服务才会有效。**操作如下：

```java
net stop mysql80
net start mysql80
```