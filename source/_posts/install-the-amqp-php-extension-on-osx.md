title: 在 OSX 10.8.2 上安装 AMQP PHP 扩展
---

参考：[http://www.php.net/manual/zh/amqp.installation.php](http://www.php.net/manual/zh/amqp.installation.php)

### 1，安装 AMQP PHP 扩展，首先要安装 librabbitmq 类库:

#### Download the rabbitmq-c library
```sh
  $ git clone git://github.com/alanxz/rabbitmq-c.git
  Cloning into 'rabbitmq-c'...
  remote: Counting objects: 3285, done.
  remote: Compressing objects: 100% (999/999), done.
  remote: Total 3285 (delta 2347), reused 3145 (delta 2249)
  Receiving objects: 100% (3285/3285), 1.73 MiB | 2 KiB/s, done.
  Resolving deltas: 100% (2347/2347), done.
```
#### Enable and update the codegen git submodule
```sh
  $ cd rabbitmq-c/
  $ git submodule init
  Submodule 'codegen' (https://github.com/rabbitmq/rabbitmq-codegen.git) registered for path    'codegen'
```
```sh
  $ git submodule update
  Cloning into 'codegen'...
  remote: Counting objects: 565, done.
  remote: Compressing objects: 100% (308/308), done.
  remote: Total 565 (delta 306), reused 511 (delta 252)
  Receiving objects: 100% (565/565), 206.24 KiB | 6 KiB/s, done.
  Resolving deltas: 100% (306/306), done.
  Submodule path 'codegen': checked out '9fc9146d057679bc63f152d6c78c3e4adcce3a1e'
```
#### Configure, compile and install
```sh
  $ autoreconf -i && ./configure && make && sudo make install
  ```
### 2，安装 amqp 扩展
```sh
$ sudo pecl install amqp
...
Build process completed successfully
Installing '/usr/lib/php/extensions/no-debug-non-zts-20090626/amqp.so'
install ok: channel://pecl.php.net/amqp-1.0.9
Extension amqp enabled in php.ini
```
### 3，验证 amqp 扩展是否安装成功：
```sh
$ php -m
[PHP Modules]
amqp
bcmath
bz2
calendar
...
```
可以看到 amqp 已经加载。