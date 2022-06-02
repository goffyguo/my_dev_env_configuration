### 利用 ssh 工具连接服务器
```shell
guogoffy@Goffys-MacBook ~ % ssh root@47.93.48.xx -p 22
root@47.93.48.xx's password: 

Welcome to Alibaba Cloud Elastic Compute Service !
```

### 下载JDK解压
```shell
[root@username java]# tar -zxvf /root/jdk-8u291-linux-x64.tar.gz
jdk1.8.0_291/
jdk1.8.0_291/COPYRIGHT
jdk1.8.0_291/LICENSE
jdk1.8.0_291/README.html
jdk1.8.0_291/THIRDPARTYLICENSEREADME.txt
jdk1.8.0_291/bin/
jdk1.8.0_291/bin/java-rmi.cgi
...
jdk1.8.0_291/jmc.txt
```

### 更改环境变量配置
```profile
# -----java start-------
JAVA_HOME=/usr/local/java/jdk1.8.0_291
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
# -----java end--------
```

### 重新加载配置文件
```shell
[root@username etc]# source /etc/profile
```

### 查询是否安装成功
```shell
[root@username etc]# java -version
java version "1.8.0_291"
Java(TM) SE Runtime Environment (build 1.8.0_291-b10)
Java HotSpot(TM) 64-Bit Server VM (build 25.291-b10, mixed mode)
[root@username etc]# javac
用法: javac <options> <source files>
其中, 可能的选项包括:
  -g                         生成所有调试信息
  -g:none                    不生成任何调试信息
...
```
