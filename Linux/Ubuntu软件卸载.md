# Ubuntu软件卸载

apt 包含了apt-get，因此以下命令可直接使用apt代替。

**整体流程**

1. 找到软件包的名字
2. 使用命令卸载
3. 清除不必要的依赖（如果内存多，不建议清除，以防删掉重要的东西）

```shell
# 过滤包名
dpkg --list | grep appName
# 删除
sudo apt-get remove appName #只删除软件，不删除配置
sudo apt-get --purge remove appName #删除软件和配置
# 或者简写
sudo apt-get purge appName
# 清除不必要的依赖
sudo apt-get autoremove 
```
之后用到了其他删除方法，会进行补充。