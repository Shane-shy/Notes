# Ubuntu软件安装

## 介绍

Ubuntu的软件安装方式大致分为五种，这五种可以分为两类。

- 图形化界面
  - snap store。Ubuntu的软件商店，使用的是snap包。
  - AppImage。通过官网下载，下载完成后可以直接使用。为不涉及终端命令，所以姑且归类为第一类。
- 终端
  - **`apt`**：Debian系统中的传统包管理系统
    - 资源占用少
    - 更成熟
    - 来自官方软件库，安全性有保障
    - 版本更新慢。如果是LST版本，为了保证稳定性，版本可能比较老
    - 依赖关系复杂，可能出现依赖冲突的问题（目前从未遇到过）
  - **`dpkg`**：Debian包管理系统的底层工具，用于安装、管理和卸载`.deb`软件包。因为apt安装的软件版本可能有点老，所以可以在软件官网下载`.deb`包，通过dpkg安装较新的版本
  - `snap`：Canonical开发的包管理系统，因为是闭源，所以在Linux开源社区内常被嫌弃
    - 独立性强：`snap`包包含了所有的依赖，解决了`apt`可能造成依赖冲突的问题
    - 自动更新：自动保持到最新版本（**其实使用Linux最需要的就是稳定，最新版本很可能会出现奇奇怪怪的问题**）
    - 资源占用多：由于`snap`包包含了所有依赖，所以这个包一般比较大，会占用更多的磁盘空间
    - 启动速度慢：每个包都是独立运行
    - 系统集成性低

## 使用

本节将展示`apt`、`dpkg`以及`snap`常用的安装命令。

### dpkg

在安装日常使用的办公软件的时候，`dpkg`更为常用。当我想安装一个软件的时候，我会找到软件官网，然后下载`.deb`包进行安装。当然，这个看个人习惯，也可能有人更喜欢使用apt安装。

1. 安装流程

```shell
# 下载XXX.deb包

# 安装软件包
sudo dpkg -i XXX.deb

# 安装的时候，可能会报错，这个时候就要看是否缺少依赖
sudo apt install -f
```

2. 软件卸载

[参考以前的博客](https://www.cnblogs.com/coder-shane/p/18110609)复习一下流程。

```shell
# 找到要卸载的包名，两种方式都可以
dpkg --list | grep software_name
apt list | grep software_name # 终端提示apt没有稳定的CLT(Command Line Interface)命令行界面，即命令可能发生变化，要谨慎使用。

# 删除软件
sudo apt remove appName # 删软件，不删配置
sudo apt remove --purge appName # 软件配置都删

# 清除不必要的依赖，如果命令行提醒了，再清除。
sudo apt autoremove
```



3. 软件更新

软件可能会提示更新（如果不提示也没办法），然后去官网下载新的`.deb`包，按照安装流程再次安装。

### apt

`apt`和`apt-get`命令在日常使用中几乎一样，甚至可以**互换着用**（在下面介绍的`apt`，全部可以换成`apt-get`）。它们之间的关系是`apt = apt-get + apt-cache`。不过，`apt`具有更易读和更直观的输出，适合大多数用户。`apt-get`具有更详细的输出，适合高级用户和脚本编写。

> [!NOTE]
>
> `dpkg`是apt的底层工具，apt流程：仓库内找到`.deb`文件 -> 调用`dpkg`安装。所以，两者其实是同根同源的。因此，`dpkg --list`和`apt list`查看到的软件列表相同。

1. 安装流程

```shell
# 更新软件包列表，但不会安装软件
sudo apt update

# 搜索软件包。一：查看apt软件库里有没有该软件。二：往往我们不知道具体的软件名字，这里也是在查询软件名字
apt search software_name

# 安装软件包。软件名字务必准确
sudo apt install software_name
```

2. 软件更新

apt安装的软件，可以通过命令`sudo apt update -y && sudo apt upgrade --yes`直接更新。`-`后面**一般**是大写缩写（这个命令就不是一般情况），比如`-V`。`--`后面**一般**是小写全称（这个命令也不是情况，测试发现大小写全称都可以），比如`--version`

3. 软件卸载

卸载流程与`dpkg`的卸载一样。

### snap

目前，我都尽量没有使用`snap`包，因为启动速度真的有点慢。例如：之前在snap store安装的Typora在双击之后需要等待很长一段时间才能打开。而使用`.deb`包安装的Typora秒开。因为可以直接使用snap store，进行图形化界面安装，所以平常也没有使用过`snap`的命令，下面仅作了解。

```shell
# 更新软件列表
sudo apt update -y

# 搜素软件包
snap find software_name

# 安装
sudo snap install software_name
```

至于更新和卸载，就使用snap store吧。