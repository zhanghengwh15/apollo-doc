# oracle与mysql的导入导出

## oracle

- 创建用户和表空间
``` sql
-- 有限制大小的
CREATE TABLESPACE fda_code
DATAFILE 'fda_code.dat' SIZE 10M REUSE
AUTOEXTEND ON NEXT 10M MAXSIZE 2G;
-- 创建用户
CREATE USER fda_user IDENTIFIED BY fda_user DEFAULT TABLESPACE fda_code;
GRANT CONNECT, RESOURCE, DBA TO fda_user;
```
- 数据导入导出

``` bash 
expdp smps/smps  directory=DATA_PUMP_DIR dumpfile=earth.dmp logfile=earth.log

# 注意表空间之类的
impdp earth/earth@47.107.171.54/orclpdb DIRECTORY=DATA_PUMP_DIR DUMPFILE=earth.dmp  logfile=earth.log   remap_tablespace=smps:earth remap_schema=smps:earth schemas=smps table_exists_action=replace transform=segment_attributes:n
```

## MySQL
MariaDB is an open source relational database management system, backward compatible, binary drop-in replacement of MySQL

### MariaDB 用的最为方便
``` bash
yum install mariadb-server
#start the MariaDB service and enable it to start
systemctl start mariadb
systemctl enable mariadb
#Run the mysql_secure_installation script
 mysql_secure_installation
```
对外放开权限

``` sql 
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'  IDENTIFIED BY '123456' WITH GRANT OPTION;
```

### 建立数据库

- 导出数据库

``` bash
mysqldump -h localhost -u root -p  station > /tmp/station.sql
```
- 建立数据库
``` sql
CREATE DATABASE station CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

- 导入数据库
``` bash
mysql -u root -p
mysql -u root -p station < station.sql
```
数据库目录
the data directory location is controlled by the datadir variable. Look at your /etc/mysql/my.cnf file to see where your installation of MariaDB is configured to store data. 
The default is /var/lib/mysql but it is often changed,
