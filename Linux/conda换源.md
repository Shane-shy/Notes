# conda换源

我推荐使用清华源，以下以清华源为例子。因为[清华源的链接](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)比较好找，而且清华源也很快。

## 换源流程

采取编写`.condarc`的方法，适用系统Linux和macos。

1. 找到`.condarc`文件。该文件一般在用户目录下，即`/home/your_user_name`，可以使用`cd ~`直接到用户目录。
2. `vim .condarc`，如果没有该文件，则创建一个。
3. 将清华源复制进去。

```shell
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/
```

## 查看换源是否成功

`conda config --show default_channels`

如果结果返回清华源，则换源成功。