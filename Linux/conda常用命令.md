# anaconda

anaconda的好处是可以把每个环境给隔离开，避免因为库版本问题导致的冲突。因此，强烈建议，一个项目就开一个虚拟环境，然后需要什么库就安装什么，并且指定版本。

anaconda中的库的数量远小于pip中的库的数量，因此可能会遇见conda内没有的库，但pip内有。这个时候就只能使用pip安装了。

## [anaconda卸载](https://anaconda.org.cn/anaconda/install/uninstall/)

1. 简单删除`rm -rf anacondaPath`，这个会留下一些文件，但是没关系，因为这方法很简单。
2. `conda install anaconda-clean`，使用`anaconda-clean --yes`，然后在使用步骤1。

> 使用了不少命令后，发现命令参数，如果是缩写都是-；如果是全称都是--。我喜欢用全称，这样会让我清楚地知道使用的是什么，用熟悉了可以用简称。

## [miniconda安装](https://docs.anaconda.com/free/miniconda/miniconda-install/)

1. 进入miniconda的官网，选择合适的版本，点击下载.pkg或者.sh
2. 双击.pkg，一直点继续就行。或者bash XXX.sh 就行

我下载的miniconda安装的位置在：`/opt/miniconda3`

在用户目录下的.condarc文件中，可以修改conda的一些配置，可能需要清除一下缓存，`conda clean -i`。

参考[清华镜像网站anaconda配置文件](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)，永远相信清华镜像网站，现在Ubuntu的默认源都换成了清华源。

## 命令操作

### 最常用命令

```shell
# 创建虚拟环境
conda create --name your_env_name # 使用默认的python版本
conda create --name your_env_name python=3.9 numpy# 可以在创建的时候，制定一些包及其版本，中间用空格隔开

# 查看有哪些虚拟环境，以下命令作用是相同的
conda env list # 在mac上只能使用这个命令
conda info -e
conda info --envs

# 激活环境
conda activate your_env_name

# 推出虚拟环境
conda activate (optional base)# 缺失值其实是base环境，相当于激活base
conda deactivate (optional current env name) # 缺失值是当前环境的名字，相当于退出当前虚拟环境

# 删除虚拟环境
conda remove --name delete_env_name (optional --all或者一些包名) # 加上--all，删除虚拟环境以及它所安装的所有包，如果写了具体包名，则只删除虚拟环境及其指定的包。我感觉虚拟环境都删了，不如把它安装的包都删了
```

### 了解命令

```shell
# 更新conda
conda update conda
# 更新anaconda，更新整体
conda update Anaconda
# 更新已安装的软件包
conda update --all
# 从当前环境中，删除一个软件包
conda remove package_name
# 查看conda的所有信息，版本、安装位置等
conda info

# 可以用于分享conda环境，方便同行复现。不过我应该是用不到这个命令了。
# 获得环境中的所有配置
conda env export --name myenv > myenv.yml
# 重新还原环境
conda env create -f  myenv.yml
```

之后，如果有其他命令的需求，将会进行补充。