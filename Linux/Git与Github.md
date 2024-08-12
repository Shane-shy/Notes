# Git与Github

> Github是一个托管平台网站，包括但不限于托管代码，只支持Git作为唯一的版本控制方式。其实一切需要版本控制的需求，都可以使用GIthub进行托管。GIthub上的每一个仓库内存上限是1G。
>
> Git是一款开源、免费的分布式版本控制系统。

## Git的安装与部署

### 安装

1. Ubuntu安装 `sudo apt-get install git`
2. Mac安装 `brew install git`。homebrew是mac上的软件管理器，具体安装使用方式，见官网。

### 部署

部署的方式应该都是一样的，可能不同操作系统的文件地址不同。因而，这里只展示**Ubuntu**的部署方式

1. 配置用户名和邮箱

```shell
git config --global user.name yourUserName
git config --global user.email yourtEmail
# 查看配置结果
git config --list
```

2. 配置SSH
   1. 生成SSH密钥 `ssh-keygen -t rsa -C "your email address"`，一路回车。注意看返回的信息，其中会包含生成密钥的地址。
   2. 查看密钥。`cat your keygen address`，之后需要复制密钥，粘贴在Github上。
   3. 在Github上配置SSH。settings -> SSH and GPG keys -> New SSH key。Title自己写，Key type选择Authentication key，然后将步骤2的密钥复制在Key内。点击Add SSH key。
   4. 检验配置结果。`ssh -T git@github.com`

![image-20240407123522630](./Git与Github.assets/image-20240407123522630.png)

## Git的使用

[Git小动画](https://www.bilibili.com/video/BV1Ev4y1L7dh/?spm_id_from=333.337.top_right_bar_window_custom_collection.content.click&vd_source=f5fa465fb229f9bbfa11556291b96b03) 可以帮助形象理解Git操作流程。

### 日常使用

以下是平常最常使用的流程和命令。

#### 整体流程

1.  打开终端，切换到需要上传的文件夹。
2.  初始化仓库。这个时候，会出现一个.git的隐藏文件。git的所有操作都保存在这个文件内。**如果这个文件删了，那么这个文件夹跟普通文件夹就一样了。** 只需要初始化一次。
3.  将文件添加到缓存区。
4.  将文件推送到远程仓库（即，Github）。

![img](./Git与Github.assets/git-command.jpg)

由图可以很清楚地发现：如果要影响远程仓库，那么必须push。

这4个区需要充分理解其含义，否则之后删除文件可能会出错。

- workspace：工作区，即本地文件夹
- staging area：缓存区
- local repository：本地仓库
- remote repository：远程仓库

#### 常用命令

```shell
cd yourPath

# 仓库初始化。只需要初始化一次。
git init
# 关联远程仓库，yourRemoteURL：选择SSH，目前官方不推荐HTTPS。只需要关联一次。
git remote add origin yourRepositoryURL

# 正常情况，要先拉取一下远程仓库
git pull origin main(your branch)
# 添加文件到缓存区，.表示全部添加，当然可以只添加某个文件
git add .
# 提交文件
git commit # 这个会出现一个vim界面，在里面写注释
git commit -m  '' # 可以在引号内，直接写注释
git commit --amend # 进入vim界面，对之前的注释进行修改
# 添加到缓存区和提交文件缩写方式
git commit -am '' # 引号内写注释
# 推送到远程仓库
git push -u origin main(your branch) # 禁止git push -f origin main 强制推送到远程仓库，因为这会之间覆盖远程仓库内的内容，很危险。-f（force）表示强制执行
```

origin：是远程仓库的别名，用于避免与本地仓库重名。

-u：建立本地分支与远程分支的链接。如果远程仓库为空，那么会同时创建分支。如果远程仓库不为空，可以不用加-u。其实如果分不清，全部加-u，也没关系。

### 偶尔使用

这里将会介绍一些不太常用的命令，但不代表不重要。

#### 远程操作

有`remote`都是远程操作

```shell
# 关联远程仓库，上文已经介绍过
git remote add origin yourRepositoryURL
# 查看关联远程仓库列表，即该本地仓库关联了哪些远程仓库
git remote -v
# 修改本地仓库关联的远程仓库，常用于关联远程仓库的时候URL弄错了
git remote set-url newYourRepositoryURL
```

#### 日志

```shell
# 查看日志，提交过的版本信息
git log
# 查看所以分支的所以操作记录，包括已经被删除的commit记录和reset记录。（很长一段时间内，我应该用不上）
git reflog
```

#### 文件状态

可以查看文件现在的状态，新添加的、已经追踪了的、追踪了但是又被修改了的。

```shell
# 查看当前文件状态，即文件做出了什么修改。详细版
git status

# 简要版
git status -s
git status --short
```

**文件状态简要版标记解释：**

1. ??：新添加的未被追踪的文件前面有红色的??标记，即这是一个全新的文件，没有加入到缓存区过，即没有被执行过git add .命令的文件。
2. A：新添加到缓存区的文件前面有绿色的标即，即添加到缓存区过的文件，即有被执行过git add .命令的文件。
3. AM：A表示该文件已经添加到缓存区了，M表示该文件被修改了，而修改过的文件还没有添加到缓存区，即我写了一个文档，添加到缓存区过，然后我又对这个文档进行了修改，并且还没来得及添加到缓存区。

#### 分支操作

```shell
# 查看分支
git branch
# 切换分支
git checkout branch
git switch branch # Git2.23版本引入
# 创建分支
git checkout -b branch 
# 修改分支名
git branch -m newBranch
git push -u origin newBranch # 修改了之后，要push到远程仓库内
# 删除分支
git branch -D deleteBranch
```

#### 合并版本

```shell
# 从远程仓库中获取最新状态，并且更新在本地仓库上，即同步。类似于sudo apt update。它不会合并分支
git fetch
# 合并分支
git merge branch # branch待被合并的分支
# 拉取远程仓库 = 获取 + 合并
git pull origin branch # = git fetch + git merge origin branch
```

版本的合并有两种，merge和rebase。

- merge：保留提交顺序，但合并多了，会导致提交树很乱。出现冲突需要手动处理冲突。**推荐使用merge**，因为rebase会破坏提交顺序，这样不方便版本控制。虽然提交树会很乱，但是可以清清楚楚知道该项目是怎么来的。
- rebase：不保留提交顺序，即保持提交树是线性的。

#### 删除文件

**建议删除之前，先备份文件，以防错误操作导致丢失文件！**

##### 仅删除远程仓库的文件，保留工作区的文件

`--cached` 只会影响缓存区，不会影响工作区。之后再提交到远程仓库中，就可以实现只删除远程仓库的文件了。

```shell
git rm (-r 如果有文件夹，则需要添加) --cached delete_file
git commit -m 'delete xxx'
git push origin branch
```

##### 删除远程仓库和工作区的文件

只能删除已经追踪过的文件，即执行过git add 的文件

```shell
git rm (-r 文件夹使用) delete_file
git commit -m 'delete file'
git push origin branch
```

##### 仅删除工作区的文件

正常使用`rm -rf xxx`。

#### 克隆仓库

非常常用命令

`git clone repositoryURL`

不需要自己创建文件夹，它会把整个文件夹都克隆下来。在克隆之前，需要切换到你想存放文件的文件夹路径上。

#### 版本回滚

回复到之前的某个版本（在未来可预见的范围内，我应该是用不上的。）

HEAD指针：指向当前版本号。

```shell
# 查看回滚的版本号
git log

# 软回滚，修改HEAD指针，但之后的版本仍然存在
git reset --soft 版本号
git reset --soft HEAD^ # 回滚到上一个版本，^表示上个版本，^^表示上上个版本
git reset --soft HEAD~N # 回滚到上N个版本，N = 1时，跟HEAD^等价

# 硬回滚，不仅修改HEAD指针，还将该版本之后的版本全部删除。谨慎，谨慎，再谨慎，一般不使用。好像可以通过git reflog 找回，但是不要抱有侥幸心理，数据无价！
git reset --hard 版本号
git reset --hard HEAD^
git reset --hard HEAD~N
```

#### 其他操作

```shell
# 查看工作区（本地）的文件变化，即没有执行git add .的文件，执行了后就看不到了。
git diff  

# 查看上n次提交的内容
git show HEAD~n

# 移动或者重命名文件。参考文件删除，可以举一反三知道，如果要对远程仓库的文件进行移动，就需要进行commit和push
git mv oldPath newPath
git commit -m 'move xxx file' # 加了git其实就是为了对远程仓库进行操作。
git push origin branch 
```



## 编写.gitignore

在使用Git上传到Github的过程中，如果一个一个选择需要上传的文件是很麻烦的一件事，而且每次上传都要一个一个写文件名，那就更加麻烦了。因而，可以在本地编写.gitignore文件，忽略不需要上传的文件，直接`git add .`。

1. 生效顺序

从上到下，依次匹配。先出现的规则，先生效；后出现的规则，可能会覆盖前面的规则。

2. 使用方法

   - 基本语法

   ```txt
   #：注释。跟shell一样
   /：表示目录。
   *：单星号通配符，匹配多个字符，但是只能匹配同一层目录内的文件或者目录
   **：双星号通配符，匹配零个或多个目录层级，可以匹配任意深度的路径
   ？：匹配单个字符
   !：表示不忽略某文件或者某目录，相当于取反
   
   其他复杂的匹配，可以参考正则表达式
   ```

   **一行写一个规则，否则不会生效**。如果要忽略某个文件或者目录，直接写起文件名或目录名即可，这样该项目下的所有该文件名或目录名都会被忽略。也可以指定特定的路径下的文件或目录，这样就只会忽略特定路径下的文件或目录。

   - **注意事项**

   相对路径是以**.gitignore所在目录为起点**，假设目录结构如下：

   ```txt
   project/
   ├── folder1/
   │   ├── subfolderA/
   │   └── file1.txt
   ├── folder2/
   │   ├── subfolderA/
   │   └── file2.txt
   └── .gitignore
   ```

   ```shell
   # 忽略folder1目录
   folder1/ # 忽略目录，后面要加 /
   
   # 忽略folder1下的subfolderA
   folder1/subfolderA/
   
   # 忽略所有文件夹下的subfolderA
   **/subfolderA/ # 注意**的作用，见 基本语法部分
   ```

3. 查看规则编写情况

`git check-ignore [<options>] <path>`，<path>中可以直接写被忽略文件或目录的路径，或者直接写其文件名或者目录名。

常用参数：

- `-v`或`--verbose`：详细显示匹配的`.gitignore`文件和对应规则
- `-q`或`--quiet`：静默模式，仅返回退出状态码，不输出任何内容。
- `-n`或`--non-matching`：显示未匹配路径，需要与`-v`一同使用。

## 遇到过的问题

仅展示目前遇到的问题，之后出现了问题会进行补充。

1. 本地分支是master，而远程分支是main。不能推送到main分支。

解决方法：`git fetch`或者`git pull`获取远程仓库状态，更新本地仓库。创建新的main分支，并切换到main分支，正常推送后，再删除master分支。

```shell
git checkout -b main
git push origin main
git checkout -D master
```

2. 当远程仓库内不为空的时候，需要先拉取仓库，才能推送到分支。

解决方法：`git pull origin main` + 正常推送。之后，工作之前，都先pull一次。

3. 使用`git pull`拉取远程仓库，产生合并冲突问题。

解决方法：

法一：默认使用rebase进行合并。修改`pull`的全局配置，只需修改一次。`git config pull.rebase true`。目前不明白为什么这样就能解决冲突，但是实验发现有效果。如果是不参与团队合作的话，采用这种方法比较方便。

法二：手动处理冲突。即，把冲突的文件找到，然后修改为真正想改的样子，提交到main分支后，再和其他分支合并。参考[解决冲突——廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

