## 一、安装包下载上传解压

```shell
cd /usr/local
mkdir activemq
tar -xzvf apache-activemq-5.16.4-bin.tar.gz
```

下载地址 [activemq](http://archive.apache.org/dist/activemq/5.17.1/)

## 二、新建配置文件

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

## 三、对配置文件（activemq）授予权限

```shell
chmod 777 activemq
```

## 四、设置开机启动并启动 activemq

```shell
chkconfig activemq on
service activemq start
```

开启该端口（8161）的防火墙

## 五、安装成功访问地址

http://IP地址:8161/

## 六、其他

#### 查看activemq状态

service activemq status

#### 开启和关闭activemq服务

service activemq start

service activemq stop

#### 设置开机启动或不启动activemq服务

chkconfig activemq on

chkconfig activemq off

## 七、安装错误汇总
#### 降低版本
```shell
[root@gf bin]# ./activemq console
INFO: Loading '/usr/local/activemq/apache-activemq-5.17.1//bin/env'
INFO: Using java '/usr/local/java/jdk1.8.0_291/bin/java'
INFO: Starting in foreground, this is just for debugging purposes (stop process by pressing CTRL+C)
INFO: Creating pidfile /usr/local/activemq/apache-activemq-5.17.1//data/activemq.pid
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" java.lang.UnsupportedClassVersionError: org/apache/activemq/console/Main has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:756)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
        at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
        at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:601)
```



