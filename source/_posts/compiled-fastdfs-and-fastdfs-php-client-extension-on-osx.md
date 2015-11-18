title: 在 OSX 10.8.2 上编译安装 FastDFS 及其 PHP 扩展
---
## Install FastDFS

[下载](https://code.google.com/p/fastdfs/downloads/list) 最新的 FastDFS 安装包：
如我下载的是 FastDFS_v4.06.tar.gz
解压后进入到 FastDFS 目录：

```sh
tar xzvf  FastDFS_v4.06.tar.gz
cd FastDFS
```

打开 FastDFS 文件夹下 INSTALL 可以看到安装方法：


>#step 1\. download libevent 1.4.x and install it
>
>#step 2\. download FastDFS source package and unpack it,
>tar xzf FastDFS_v3.x.tar.gz
>#for example:
>tar xzf FastDFS_v3.05.tar.gz

>#step 3\. enter the FastDFS dir
>cd FastDFS

>#step 4.
>./make.sh

>#step 5\. make install
>./make.sh install


过程中可能遇到这个错误：
```sh
meet error:
ln: /usr/lib64/libfastcommon.so: No such file or directory
ln: /usr/lib64/libfdfsclient.so: No such file or directory
make: *** [install] Error 1
```
解决方法：
参考：[http://bbs.chinaunix.net/thread-4069394-1-1.html](http://bbs.chinaunix.net/thread-4069394-1-1.html)
创建/usr/lib64目录，并在/usr/include/sys/errno.h加以下代码下：
```
#define ENONET 64  /* Machine is not on the network */
```
Install FastDFS PHP Client:
```
cd php_client/
```
按照目录下面的 README 操作：
```sh
phpize
./configure
make
make install
```
成功后会提示：
```sh
Installing shared extensions: /usr/lib/php/extensions/no-debug-non-zts-20090626/
```
拷贝 fastdfs_client.ini 到 php.ini 所在的目录或者直接在 php.ini 里面加上 fastdfs_client.ini 的内容。
查看extension是否加载成功：
```sh
 php -m
 ```
出现如下错误：
```sh
file: /Users/xxx/Downloads/FastDFS/php_client/fastdfs_client.c, 
line: 7204, fastdht_client.ini does not have item "fastdfs_client.base_path", 
set to /tmp!file: /Users/xxx/Downloads/FastDFS/php_client/fastdfs_client.c, 
line: 7320, fastdfs_client.ini: get param fastdfs_client.tracker_group fail!
PHP Fatal error: Unable to start fastdfs_client module in Unknown on line 0 
Fatal error: Unable to start fastdfs_client module in Unknown on line 0
```
说明fastdfs_client module 加载失败
编辑 vim /etc/fdfs/client.conf
编辑 base_path 和 tracker_server 为自己的值。
再执行 php -m ，列表有出现 fastdfs_client，证明 fastdfs_client 模块加载成功。
 
在 phpinfo() 页面能看到如下信息：


>fastdfs_client
>fastdfs_client v4.06 support     enabled
