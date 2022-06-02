### 一、下载安装包并解压
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ redis]# tar zxvf /root/redis-6.2.3.tar.gz 
redis-6.2.3/
redis-6.2.3/.github/
redis-6.2.3/.github/ISSUE_TEMPLATE/
redis-6.2.3/.github/ISSUE_TEMPLATE/bug_report.md
...
redis-6.2.3/utils/whatisdoing.sh
```

### 二、编译安装
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ redis]# cd redis-6.2.3/
[root@iZ2ze9bdnyrkmyvwoabuwxZ redis-6.2.3]# make && make install
cd src && make all
make[1]: 进入目录“/usr/local/redis/redis-6.2.3/src”
    CC Makefile.dep
...
INSTALL redis-server
    INSTALL redis-benchmark
    INSTALL redis-cli
make[1]: 离开目录“/usr/local/redis/redis-6.2.3/src”
```

### 三、将redis安装为系统服务并后台启动
#### 1、进入utils目录，执行如下脚本
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ utils]# ./install_server.sh 
Welcome to the redis service installer
This script will help you easily set up a running redis server

This systems seems to use systemd.
Please take a look at the provided example service unit files in this directory, and adapt and install them. Sorry!
```
此时如果遇到上述错误，进入改文件，注释掉以下内容，重新启动即可
```shell
#bail if this system is managed by systemd
#_pid_1_exe="$(readlink -f /proc/1/exe)"
#if [ "${_pid_1_exe##*/}" = systemd ]
#then
#       echo "This systems seems to use systemd."
#       echo "Please take a look at the provided example service unit files in this directory, and adapt and install them. Sorry!"
#       exit 1
#fi
```
### 四、查看redis服务启动情况
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ ~]# systemctl status redis_6379.service
redis_6379.service - LSB: start and stop redis_6379
   Loaded: loaded (/etc/rc.d/init.d/redis_6379; bad; vendor preset: disabled)
   Active: active (running) since 二 2021-05-25 09:46:58 CST; 2h 10min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 24583 ExecStop=/etc/rc.d/init.d/redis_6379 stop (code=exited, status=0/SUCCESS)
  Process: 24586 ExecStart=/etc/rc.d/init.d/redis_6379 start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/redis_6379.service
           └─24588 /usr/local/bin/redis-server 0.0.0.0:6379

5月 25 09:46:58 iZ2ze9bdnyrkmyvwoabuwxZ systemd[1]: Stopped LSB: start and stop redis_6379.
5月 25 09:46:58 iZ2ze9bdnyrkmyvwoabuwxZ systemd[1]: Starting LSB: start and stop redis_6379...
5月 25 09:46:58 iZ2ze9bdnyrkmyvwoabuwxZ redis_6379[24586]: Starting Redis server...
5月 25 09:46:58 iZ2ze9bdnyrkmyvwoabuwxZ systemd[1]: Started LSB: start and stop redis_6379.
```

### 五、启动redis测试
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ utils]# redis-cli
127.0.0.1:6379> get k1
"v1"
127.0.0.1:6379>
```

### 六、设置远程访问
#### 1、编辑配置文件/etc/redis/6379.conf
```shell
vim /etc/redis/6379.conf
```
#### 2、将bind 127.0.0.1 修改为 0.0.0.0，重新启动服务
```shell
systemctl restart redis_6379.service
```

### 七、设置redis密码
1、编辑Redis配置文件 /etc/redis/6379.conf
2、修改 #requirepass guo ，去掉注释，将guo修改为自己想要的密码，保存重启Redis服务（systemctl restart redis_6379.service）
3、后续则需要密码认证
```shell
[root@iZ2ze9bdnyrkmyvwoabuwxZ utils]# redis-cli
127.0.0.1:6379> get k1
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth ******
OK
127.0.0.1:6379> get k1
"v1"
```

