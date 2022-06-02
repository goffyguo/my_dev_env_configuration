### 解压安装包
```shell
# 解压
[root@iZ2ze9bdnyrkmyvwoabuwxZ etc]# tar -Jxf /root/mysql-test-8.0.24-linux-glibc2.12-x86_64.tar.xz -C /usr/local/
# 移动重命名
[root@iZ2ze9bdnyrkmyvwoabuwxZ local]# mv mysql-8.0.24-linux-glibc2.12-x86_64 mysql
```

### 创建 mysql 和 mysql 用户组
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ mysql]# groupadd mysql
[root@iZ2ze9bdnyrkmyvwoabuwxZ mysql]# useradd -g mysql mysql
```
在/usr/local/mysql/data目录中新建data文件夹，后面会用到

### 修改 mysql 的归属用户
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ mysql]# chown -R mysql:mysql ./
```

### 在 etc/ 目录下新建 my.cnf 文件，配置 mysql
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ etc]# cat my.cnf
[mysql]
# 设置mysql客户端默认字符集 
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock
[mysqld]
skip-name-resolve
# 设置3306端口
port = 3306
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装目录 
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放目录 
datadir=/usr/local/mysql/data
# 允许最大连接数
 max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集 
character-set-server=utf8
# 创建新表时将使用的默认存储引擎 
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16Mk
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid
#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```

### 修改权限
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ etc]# mkdir /var/lib/mysql
[root@iZ2ze9bdnyrkmyvwoabuwxZ etc]# chmod 777 /var/lib/mysql
```
以上都是安装mysql 的事前配置，现在正式安装mysql

### 安装
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ mysql]# ./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```
出现以下信息表示安装成功
```shell
2021-05-19T09:22:41.351869Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: I:oY-i6k_0hT #密码
```

### 复制启动脚本到资源目录
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ mysql]# cp ./support-files/mysql.server /etc/init.d/mysqld
```

### 修改 /etc/init.d/mysqld ，修改其 basedir 和 datadir 为实际对应目录

>basedir=/usr/local/mysql
>datadir=/usr/local/mysql/data

### 设置mysql系统服务开启自启动
#### 增加 mysqld 服务控制脚本执行权限
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# chmod +x /etc/init.d/mysqld
```
#### 将 mysqld 服务加入到系统服务
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# chkconfig --add mysqld
```
#### 检查 mysqld 服务是否已经生效即可
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# chkconfig --list mysqld
```
### 配置mysql的环境变量
```shell
[[root@iZ2ze9bdnyrkmyvwoabuwxZ]vi ~/.bash_profile
#----mysql start-----
export PATH=$PATH:/usr/local/mysql/bin
#----mysql end------
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# cat ~/.bash_profile
```
### 启动 MySQL
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# service mysqld start
. SUCCESS!
```

### 首次登录更改密码
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ init.d]# mysql -u root -p
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.25
mysql> alter user user() identified by "密码";
mysql>flush privileges;
```

### 设置mysql远程主机访问
```shell
mysql> use mysql;
mysql> update user set user.Host='%' where user.User='root';
mysql> flush privileges;
```
