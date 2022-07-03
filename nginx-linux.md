## 1、上传安装包

```shell
mv nginx-1.23.0.tar.gz /usr/local/nginx
```

## 2、安装帮助依赖

```shell
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
```


## 3、解压

```shell
tar -zxvf nginx-1.23.tar.gz 
```

## 4、构建
```shell
./configure
```
```shell
make && make install
```

## 5、启动
```shell
cd /usr/local/nginx/sbin
```

## 6、检查
```shell
[root@iZ2zedu1r5dgld4ka8sgklZ sbin]# ps -ef|grep nginx
root      245631       1  0 23:26 ?        00:00:00 nginx: master process ./nginx
nobody    245632  245631  0 23:26 ?        00:00:00 nginx: worker process
```

## 7、防火墙
```shell
[root@iZ2zedu1r5dgld4ka8sgklZ sbin]# systemctl restart firewalld.service
[root@iZ2zedu1r5dgld4ka8sgklZ sbin]# firewall-cmd --permanent --zone=public --add-port=80/tcp
success
[root@iZ2zedu1r5dgld4ka8sgklZ sbin]# systemctl restart firewalld.service
[root@iZ2zedu1r5dgld4ka8sgklZ sbin]# firewall-cmd --list-ports
80/tcp
```

