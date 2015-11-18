title: 在 CentOS 5.6 上安装 rabbitmq server 3.0.4
---

参考：[http://www.rabbitmq.com/install-rpm.html](http://www.rabbitmq.com/install-rpm.html)
安装 RabbitMQ 之前需要安装 Erlang。可以通过 EPEL (“Extra Packages for Enterprise Linux”) 方式安装 Erlang。EPEL 是 Red Hat / Fedora 组织的一部分，提供了很多额外的包，其中就包括 Erlang。这些是最官方的包，被分成很多小的包，但并不一定都是最新的。为了安装 Erlang 的最新版本，你需要更新 Erlang 的包：
```sh
wget -O /etc/yum.repos.d/epel-erlang.repo http://repos.fedorapeople.org/repos/peter/erlang/epel-erlang.repo 
```
安装 Erlang:
```sh
yum install erlang
...
Updated:
  erlang.x86_64 0:R14B-04.1.el5                                                      

Complete!
```
下载 RabbitMQ-Server:
```sh
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.0.4/rabbitmq-server-3.0.4-1.noarch.rpm
```
安装 RabbitMQ 服务器：
```sh
rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
yum install rabbitmq-server-3.0.4-1.noarch.rpm
...

Transaction Test Succeeded
Running Transaction
  Installing     : rabbitmq-server                                             1/1 
Installed:
  rabbitmq-server.noarch 0:3.0.4-1                                                           

Complete!
```
运行 RabbitMQ Server：
```sh
# /sbin/service rabbitmq-server start
```
出现如下错误：
```sh
Starting rabbitmq-server: FAILED - check /var/log/rabbitmq/startup_{log, _err}
rabbitmq-server.
```
查看错误信息：

```sh
# cat /var/log/rabbitmq/startup_log
ERROR: epmd error for host "ZJ-stg-vps-11": address (cannot connect to host/port)
                [FAILED]
```
解决方法：将主机名指向127.0.0.1
```sh
vim /etc/hosts
127.0.0.1    _hostname_
```
再次启动服务：
```sh
# /sbin/service rabbitmq-server start
Starting rabbitmq-server: SUCCESS
rabbitmq-server.
```
检查启动状态：
```sh
# rabbitmqctl status
Status of node 'rabbit@ZJ-stg-vps-11' ...
[{pid,19421},
 {running_applications,[{rabbit,"RabbitMQ","3.0.4"},
                        {mnesia,"MNESIA  CXC 138 12","4.5"},
                        {os_mon,"CPO  CXC 138 46","2.2.7"},
                        {xmerl,"XML parser","1.2.10"},
                        {sasl,"SASL  CXC 138 11","2.1.10"},
                        {stdlib,"ERTS  CXC 138 10","1.17.5"},
                        {kernel,"ERTS  CXC 138 10","2.14.5"}]},
...
 {run_queue,0},
 {uptime,16}]
...done.
```