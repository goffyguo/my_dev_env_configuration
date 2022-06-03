## 安装包下载上传解压

```shell
cd /usr/local
mkdir activemq
tar -xzvf apache-activemq-5.16.4-bin.tar.gz
```

下载地址 [activemq](http://archive.apache.org/dist/activemq/5.17.1/)

## 新建配置文件

```shell
cd /etc/init.d/
vi activemq
```

配置文件(activemq)

```shell
#!/bin/sh
#
# /etc/init.d/activemq
# chkconfig: 345 63 37
# description: activemq servlet container.
# processname: activemq 5.16.4
# Source function library.
#. /etc/init.d/functions
# source networking configuration.
#. /etc/sysconfig/network
export JAVA_HOME=/usr/local/jdk1.8.0_144
export CATALINA_HOME=/usr/local/activemq/apache-activemq-5.16.4
case  $1 in
     start)
         sh $CATALINA_HOME/bin/activemq start
     ;;
     stop)
         sh $CATALINA_HOME/bin/activemq stop
     ;;
     restart)
         sh $CATALINA_HOME/bin/activemq stop
         sleep 1
         sh $CATALINA_HOME/bin/activemq start
     ;;
esac
exit 0
```

### 对配置文件（activemq）授予权限

```shell
chmod 777 activemq
```

### 设置开机启动并启动 activemq

```shell
chkconfig activemq on
service activemq start
```

开启该端口（8161）的防火墙

### 安装成功访问地址

http://IP地址:8161/

### 其他

#### 查看activemq状态

service activemq status

#### 开启和关闭activemq服务

service activemq start

service activemq stop

#### 设置开机启动或不启动activemq服务

chkconfig activemq on

chkconfig activemq off


