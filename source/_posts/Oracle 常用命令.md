---
title: Oracle 常用命令
date: 2020-05-31
tags: Oracle
categories: Backend
keywords: Oracle
description: Oracle
cover: /img/cover_posts/oracle.jpg
top_img: /img/cover_posts/oracle.jpg
---
## 拉取 oracle 镜像

`$ docker pull akijoey/oracle`

## 构建 oracle 容器

`$ docker run -d -p 1521:1521 --name oracle akijoey/oracle`

oracle 默认端口为 1521

## 查看 sqlplus 版本

`$ sqlplus -v`

## 查看监听器状态

`$ lsnrctl status`

## 启动监听器

`$ lsnrctl start`

## 停止监听器

`$ lsnrctl stop`

## 连接 oracle 服务器

`$ sqlplus <username>/<password>@<servicename>`

以用户名, 密码和服务名登录.

`$ sqlplus / as sysdba`

以操作系统权限认证的 sys 管理员登录.

`$ sqlplus /nolog`

启动 sqlplus 但不进行连接操作.

## 退出 oracle 客户端

`> exit`

## 查看当前用户

`> show user`

## 切换当前用户

`> conn <username>/<password>@<servicename>`

## 查看版本信息

`> select banner from v$version;`

## 查看数据库

`> select name from v$database;`

## 查看实例

`> select instance_name from v$instance;`

## 查看表空间

`> select name from v$tablespace;`

## 启动实例

`> startup nomount`

启动实例, 读取参数文件, 分配 SGA, 启动后台进程.

`> startup mount`

启动并关联实例, 读取控制文件, 读取数据文件和重做日志文件.

`> startup open`

启动并关联实例, 打开数据库, 打开数据文件和重做日志文件.

## 关闭实例

`> shutdown normal`

不允许新的连接, 等待当前 session 结束, 等待当前事务结束, 执行检查点.

`> shutdown transactional`

不允许新的连接, 不等待当前 session 结束, 等待当前事务结束, 执行检查点.

`> shutdown immediate`

不允许新的连接, 不等待当前 session 结束, 不等待当前事务结束, 执行检查点.

`> shutdown abort`

不允许新的连接, 不等待当前 session 结束, 不等待当前事务结束, 不执行检查点.

## 参数文件相关

查看参数文件

`> show parameter pfile`

使用指定文本参数文件 pfile 启动实例

`> startup pfile='<pfile>'`

创建实例时默认会按照如下的顺序查找参数文件:
- `$ORACEL_HOME/dbs/spfile$ORACLE_SID.ora`
- `$ORACEL_HOME/dbs/spfile.ora`
- `$ORACEL_HOME/dbs/init$ORACLE_SID.ora`

其命名规则是固定的, 因此不可显式指定 spfile 作为启动实例的参数文件.

spfile 转 pfile.

`> create pfile='<pfile>' from spfile='<spfile>'`

pfile 转 spfile.

`> create spfile='<spfile>' from pfile='<pfile>'`

## 控制文件相关

查看控制文件

`> show parameter control_files`

备份控制文件

`> alter database backup controlfile to trace as '<controlfile>';`

还原控制文件

`> @<controlfile>`

## 数据文件相关

查看数据文件

`> select name from v$datafile;`

修改数据文件为脱机状态

`> alter database datafile '<datafile>' offline;`

修改数据文件为联机状态

`> alter database datafile '<datafile> online;'`

## 重做日志文件相关

查看重做日志组信息

`> select group#, members, status from v$log;`

重做日志组有四种状态:
- `unused` 表示日志组从末用过
- `current` 表示日志组正在使用
- `active` 表示日志组处于活动状态
- `inactive` 表示日志组处于非活动状态

查看重做日志文件

`> select group#, member from v$logfile;`

查看归档日志文件

`> select destination from v$archive_dest;`

查看归档信息

`> archive log list`

切换为归档模式

`> alter database archivelog;`

切换为非归档模式

`> alter database noarchivelog;`

手动归档日志组

`> alter system archive log current;`

手动切换日志组

`> alter system switch logfile;`

## 创建用户

`> create user <username> identified by <password>;`

## 修改用户密码

`> alter user <username> identified by <password>;`

## 创建角色

`> create role <rolename>`

## 授予权限

授予用户权限

`> grant create session to <username>;`

`with admin option` 用于系统权限授权, 权限回收级联.

`with grant option` 用于对象权限授权, 权限回收不级联.

授予角色权限

`> grant create session to <rolename>;`

授予角色

`> grant <rolename> to <username>;`

## 回收权限

回收用户权限

`> revoke create session from <username>;`

回收角色权限

`> revoke create session from <rolename>;`

回收角色

`> revoke <rolename> from <username>;`

## 查看当前用户权限

`> select * from session_privs;`

## 查看当前用户角色

`> select * from session_roles;`
