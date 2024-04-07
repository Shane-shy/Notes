# Linux版本typora更新警告

1. 出现问题

使用`sudo apt update && sudo apt upgrade`的时候，出现报错。内容如下：

![image](./Linux版本typora更新警告.assets/3420247-20240405225937268-730171395.png)


从警告的内容可以看出 **connect (101: Network is unreachable) Could not connect to typora.io:443 (199.59.149.136), connection timed out**，这个typora.io源无法连接，超时。

2.  解决方法

禁用第三方源

```shell
# 使用管理员权限，因为修改的文件需要很高的权限
sudo su
# 前往文件夹，sources.list这个文件存放的是官方的源，不要动它
cd /etc/apt
# 前往第三方源
cd ./sources.list.d
# 注释掉typora源
vim archive_uri-https_typora_io_linux-jammy.list
vim archive_uri-https_typora_io_linux-jammy.list.save
```

其实，我也不知道.save这个文件有什么区别，但是里面也保存了typora.io这个源，所以也给它注释了。之后，正常更新，没有警告了！