### 安装JDK

#### 解压安装包

解压安装包到`/usr/local`目录

`tar -zxvf jdk-8u171-linux-x64.tar.gz -C /usr/local`

#### 配置环境变量

修改/etc/profile文件，在文件末尾加入如下配置

```
JAVA_HOME=/usr/local/jdk1.8.0_171
PATH=$JAVA_HOME/bin:$PATH
```

#### 重新加载profile文件

>注意谨慎修改此文件，有可能造成关键环境变量丢失，致使部分linux命令无法使用

```
 source /etc/profile
```

#### 检查是否安装成功

```
java --verison
```



### 安装Tomcat

#### 解压安装包

安装包解压到指定目录/usr/local下

```
tar -zxvf apache-tomcat-7.0.57.tar.gz -C /usr/local
```

#### 启动Tomcat

进入Tomcat的bin目录启动服务。

```
sh startup.sh或者./startup.sh
```

####  Tomcat进程查看

上述我们将Tomcat启动完成之后，并不能知道Tomcat是否正常运行，那么我们验证Tomcat启动是否成功，有多种方式，我们这里主要介绍常见的两种方式： 

**1). 查看启动日志**

Tomcat的启动日志输出在Tomcat的安装目录下的logs目录中，Tomcat的启动及运行日志文件名为 catalina.out，所以我们查看Tomcat启动日志，主要可以通过两条指令，如下： 

```
tail -50 /usr/local/apache-tomcat-7.0.57/logs/catalina.out
```

只要Tomcat在启动的过程中，日志输出没有报错，基本可以判定Tomcat启动成功了。

**2). 查询系统进程**

我们也可以通过Linux系统的查看系统进程的指令，来判定Tomcat进程是否存在，从而判定Tomcat是否启动。执行如下指令： 

```
ps -ef|grep tomcat
```



### **防火墙操作**

| 操作                         | 指令                                                         | 备注                 |
| ---------------------------- | ------------------------------------------------------------ | -------------------- |
| 查看防火墙状态               | systemctl status firewalld / firewall-cmd --state            |                      |
| 暂时关闭防火墙               | systemctl stop firewalld                                     |                      |
| 永久关闭防火墙(禁用开机自启) | systemctl disable firewalld                                  | ==下次启动,才生效==  |
| 暂时开启防火墙               | systemctl start firewalld                                    |                      |
| 永久开启防火墙(启用开机自启) | systemctl enable firewalld                                   | ==下次启动,才生效==  |
| 开放指定端口                 | firewall-cmd --zone=public --add-port=8080/tcp --permanent   | ==需要重新加载生效== |
| 关闭指定端口                 | firewall-cmd --zone=public --remove-port=8080/tcp --permanent | ==需要重新加载生效== |
| 立即生效(重新加载)           | firewall-cmd --reload                                        |                      |
| 查看开放端口                 | firewall-cmd --zone=public --list-ports                      |                      |



### 安装mysql

#### 检测当前系统是否安装过MySQL相关数据库

```
rpm -qa | grep mysql	
```

centos默认可能安装了mariadb，mariadb与mysql依赖冲突，需要卸载

**rpm卸载**

```
rpm -e --nodeps  软件名称
```

**解压**

**安装**

```
rpm -ivh mysql-community-common-5.7.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-devel-5.7.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-compat-5.7.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.25-1.el7.x86_64.rpm
yum install net-tools
rpm -ivh mysql-community-server-5.7.25-1.el7.x86_64.rpm
```

**查看初始密码**

```
 cat /var/log/mysqld.log | grep password
```

**修改密码**

```
①. 登录mysql（复制日志中的临时密码登录）
	mysql -uroot -p								

②. 修改密码
    set global validate_password_length=4;			设置密码长度最低位数
    set global validate_password_policy=LOW;		设置密码安全等级低，便于密码可以修改成root
    set password = password('root');				设置密码为root
    
③. 开启访问权限
    grant all on *.* to 'root'@'%' identified by 'root';
    flush privileges;
```







### 网络相关信息

```
netstat -tunlp		
```

> ​	<font color='red'>netstat命令用来打印Linux中网络系统的状态信息，可让你得知整个Linux系统的网络情况。</font>
>
> ​		参数说明: 
>
> ​		-l或--listening：显示监控中的服务器的Socket；
> ​		-n或--numeric：直接使用ip地址，而不通过域名服务器；
> ​		-p或--programs：显示正在使用Socket的程序识别码和程序名称；
> ​		-t或--tcp：显示TCP传输协议的连线状况；
> ​		-u或--udp：显示UDP传输协议的连线状况；



### centos网络设置

网络配置目录

``` 
/etc/sysconfig/network-scripts/
```

配置项

```properties
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
IPADDR="192.168.138.100"        # 设置的静态IP地址
NETMASK="255.255.255.0"         # 子网掩码
GATEWAY="192.168.138.2"         # 网关地址
DNS1="192.168.138.2"            # DNS服务器
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=afd0baa3-8bf4-4e26-8d20-5bc426b75fd6
DEVICE=ens33
ONBOOT=yes
ZONE=public
```





















