# Linux文件搜索

> 推荐使用顺序：
>
> whereis $\rightarrow$ locate $\rightarrow$ find

## 可执行文件查找

查找`PATH`目录下的可执行文件，常常是命令

`which [-a] command`

```shell
# 示例
which ls # ls命令的位置，但只打印第一个被找到符合要求的指令

which -a ls # 打印出PATH目录下所有匹配的命令位置
```

## 文件查找

1. `whereis`：特定目录中，查找文件。一般用于查找二进制文件、源代码文件和manual手册等系统级别的文件。**速度快。**

```shell
# 语法
whereis [-option] fileName
# 参数介绍
-l：查找的主要目录
-b：只查找二进制文件
-m：只查找在说明文档manual路径下的文件
-s：只查找source来源文件
-u: 查找不在上述三个类别的其他特殊文件
```

**示例：**

```shell
whereis -l
# 输出太长，部分结果
bin: /usr/bin
bin: /usr/sbin
bin: /usr/lib/x86_64-linux-gnu
...
man: /usr/share/man/man6
man: /usr/share/man/man1
man: /usr/share/man/man7
...
src: /usr/src/linux-headers-6.5.0-45-generic
src: /usr/src/linux-headers-6.8.0-40-generic
src: /usr/src/linux-hwe-6.5-headers-6.5.0-45

whereis passwd # 全部文件

whereis -m passwd # 在manual手册中的文件
```

2. `locate`：一般文件查找的**首选方式**

   - **速度快**，创建文件数据库，而不是直接读取硬盘
   - 无法查找到**最新**文件。数据库每天自动更新，更新前的新文件无法被查找到。可`sudo updatedb`手动更新数据库，可能花费几分钟
   - 查找功能不如`find`强大，不过日常查找够用了


有可能没有安装，可通过`sudo apt install mlocate`进行安装。

```shell
# 语法
locate [-option] keyword # 只需要关键词就可以了，不需要全称
locate [-option] keyword | less # 有可能输出很多，可用less、more进行分页
# 参数介绍
-i：--ignore-case，常见参数，忽略大小写
-c：不输出文件名，而是输出找到的文件数量
-l：控制输出的行数，例如-l 5 输出5行
-S：输出locate所使用的数据库文件的相关信息
-r：后接正则表达式
```

   **示例：**

   ```shell
   locate passwd
   # 为了看出差别，改为输出结果数量
   locate -c passwd # 214
   
   locate -ic passwd # 214
   
   locate -rc passwd* # 895
   
   locate -l 5 passwd
   # /etc/passwd
   # /etc/passwd-
   # /etc/pam.d/chpasswd
   # /etc/pam.d/passwd
   # /etc/security/opasswd
   ```

3. `find`：非常强大的查找方法，可以支持多种方式进行查找，如时间、权限、名称等
   - **速度慢，费硬盘**。每次都在硬盘中进行查找，而不是在数据库中进行查找。
   - **查找方式多**

   ```shell
   # 查找方式过多，以下仅展示使用过的方式。若之后用到新的方式，会对其进行补充
   # 基础查找
   find PATH -name fileName
   
   PATH：查找某路径下的文件，如/，查找根目录下的文件
   fileName：文件名称，支持正则表达式。使用正则表达式的时候，需要加引号，如"*passwd*"
   
   # 额外操作
   find PATH -name fileName -exec command {} \; # 查找后，接着执行其他命令。\;对分号进行转义
   
   -exec：执行命令，直到\;
   {}：表示find查找的结果，类似于变量。或者类似于python中的print("{}".format(value))
   ```

## 参考文献

- 《鸟哥的Linux私房菜：基础学习篇 第四版》