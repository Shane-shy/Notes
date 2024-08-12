# Ubuntu软件非`.deb`包安装及软件桌面快捷方式配置

## 问题描述

我们常用`apt`、`dpkg`命令安装`.deb`包来安装软件，不需要了解软件具体被安装的具体地址，并且安装完后，常常就能直接在应用内看到所安装的软件。但是，有一些软件，比如pycharm、Zotero等软件，官方并没有给`.deb`包而是zip或者tar的压缩包，这时软件安装在哪里，如何配置桌面快捷方式就都需要靠我们自己配置。

==注意==：安装流程应参照官方的安装指南，本博客仅展示目前遇到的情况。

## 操作流程

1. 准备阶段

下载官方压缩包，并将压缩包解压至`\opt`。一般软件是存放在**`\opt\`目录**下，必要时需要自己新建待安装软件目录。这个时候，往往可以直接通过命令行`./appName`或者`bash appName.sh`执行软件，但是这种方法始终还是不方便，所以需要创建快捷方式。

2. 编写`.desktop`

在下载的压缩包内，往往会有一个`appName.desktop`（如果没有，则参照其他软件的`.desktop`内容自行编写），这是创建快捷方式的关键。主要修改其中的Exec和Icon，分别是对应可执行文件的位置和图标的位置。**注意：采用绝对路径编写。**

对于Exec部分，pycharm是`Exec=/opt/pycharm/bin/pycharm.sh `，Zotero是`Exec=bash /opt/zotero/zotero %U`。如果可执行文件的地址确定正确，可以考虑在地址前加上`bash`。

3. 复制`.desktop`到`/usr/share/applications`

`sudo cp /opt/appName/appName.desktop` `/usr/share/applications`，稍等一会儿就能在应用中查看到安装的软件。也可以采用软链接的方式处理本操作。

4. 错误查询

如果安装了之后，能查看到软件，但是软件无法使用。可以查看系统日志`journalctl -xe | grep appName`，查看具体报错信息。