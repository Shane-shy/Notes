# Git使用教程

> Github是一个托管平台网站，包括但不限于托管代码，只支持Git作为唯一的版本控制方式。其实一切需要版本控制的需求，都可以使用GIthub进行托管。GIthub上的每一个仓库内存上限是1G。
>
> Git是一款开源、免费的分布式版本控制系统。

## Git的安装与部署

### 安装

1. Ubuntu安装 `sudo apt-get install git`
2. Mac安装 `brew install git`。homebrew是mac上的软件管理器，具体安装使用方式，见[官网](https://brew.sh/)。

### 部署

部署的方式应该都是一样的，可能不同操作系统的文件地址不同。因而，这里只展示**Ubuntu**的部署方式

1. 配置用户名和邮箱

```shell
# 用户名和邮箱不需要用引号
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

![image-20240407123522630](./Git使用教程.assets/image-20240407123522630.png)

## Git的使用

> [Git小动画](https://www.bilibili.com/video/BV1Ev4y1L7dh/?spm_id_from=333.337.top_right_bar_window_custom_collection.content.click&vd_source=f5fa465fb229f9bbfa11556291b96b03) 可以帮助形象理解Git操作流程。（推荐）
>
> [learn git branch](https://learngitbranching.js.org/?locale=zh_CN)是一个交互式的git学习在线网站。（推荐）
>
> [猴子都能学懂的git教程](https://nulab.com/zh-cn/learn/software-development/git-tutorial/)是一个记载详细的git工具书，特点就是大且全。

### 日常使用

以下是我最常使用的流程和命令。

#### 整体流程

1.  打开终端，路径切换到需要上传的文件夹。
2.  初始化仓库。这个时候，会出现一个.git的隐藏文件。git的所有操作都保存在这个文件内。**如果删除.git文件，则整个文件夹就跟普通文件夹一样了。** 只需要初始化一次。
3.  将文件添加到暂存区。
4.  将文件推送到远程仓库（即Github）。

![img](./Git使用教程.assets/git-command.jpg)

由图可以很清楚地发现：**如果要影响远程仓库，那么必须push**。

这4个区需要充分理解其含义，否则之后删除文件可能会出错。

- workspace：工作区，即本地文件夹
- staging area：暂存区
- local repository：本地仓库
- remote repository：远程仓库

#### 常用命令

```shell
cd yourPath

# 仓库初始化。只需要初始化一次。
git init
# 关联远程仓库，yourRemoteURL：选择SSH，目前官方不推荐HTTPS。只需要关联一次。
git remote add origin yourRepositoryURL
git remote add your_remote_name yourRepositoryURL # 关联并修改远程仓库名称

# 正常情况，要先拉取一下远程仓库
git pull origin main(your branch)
# 添加文件到暂存区，.表示全部添加，当然可以只添加某个文件
git add .
# 提交文件
git commit # 这个会出现一个vim界面，在里面写注释
git commit -m  '' # 可以在引号内，直接写注释
git commit --amend # 进入vim界面，对之前的注释进行修改
# 推送到远程仓库
git push -u origin main(your branch) # 禁止git push -f origin main 强制推送到远程仓库，因为这会直接覆盖远程仓库内的内容，很危险。-f（force）表示强制执行
# push参数（不常用）。将分支推送到远程仓库（可在HEAD分离状态下进行）
git push your_remote_name your_branch
```

- `git add .`：将当前工作区内的所有更改添加到暂存区。这意味着准备将这些更改添加到本地仓库，**但是这些更改还没有被记录到Git历史中**。即，更改准备提交到本地仓库。
- `git commit`：将暂存区内的更改提交到本地仓库，**记录在Git历史中**。此时，更改才会成为一个版本，才能够被追溯。
- `origin`：是远程仓库的名称。一般都是`origin`，当然可以是其他名称，比如`develop`等。可在`clone`和远程仓库关联的时候修改

##### push参数

- `-u`：全称`--set-upstream`，建立本地分支与远程分支的关联。目的是简化操作，只需要在第一次`push`的时候使用`-u`参数，当本地分支与远程分支建立关联后，可以直接使用`git push`和`git pull`。
- `git push your_remote__branch your_branch`：推送特定分支到特定的仓库。（不常用，通常本地和远程分支都是一致的）
- `git push --force your_remote_branch`：强制推送到并覆盖远程某分支。

### 偶尔使用

这里将会介绍一些不太常用的命令，但不代表不重要。

#### 远程操作

有`remote`都是远程操作

```shell
# 关联远程仓库，上文已经介绍过。
git remote add origin yourRepositoryURL
git remote add your_remote_name yourRepositoryURL # 关联并设置远程仓库名称
# 查看关联远程仓库列表，即该本地仓库关联了哪些远程仓库
git remote -v
# 修改本地仓库关联的远程仓库，常用于关联远程仓库的时候URL弄错了
git remote set-url newYourRepositoryURL
# 修改远程仓库名称，远程仓库名称默认为origin，可以修改为其他名称
git remote rename oldName newName
```

#### 操作日志

```shell
# 查看日志，提交过的版本信息
git log
# 查看所有分支的所有操作记录，包括已经被删除的commit记录和reset记录。（很长一段时间内，应该用不上）
git reflog
```

#### 文件状态

可以查看文件三种的状态，新添加的、已经追踪了的、追踪了但是又被修改了的。

```shell
# 查看当前文件状态，即文件做出了什么修改。详细版
git status

# 简要版
git status -s
git status --short
```

**文件状态简要版标记解释：**

1. ??：新添加的未被追踪的文件前面有红色的??标记，即这是一个全新的文件，没有加入到暂存区，即没有被执行过`git add .`命令的文件。
2. A：新添加到暂存区的文件前面有绿色的标记，即添加到暂存区过的文件 $\rightarrow$ 有被执行过`git add .`命令的文件。
3. AM：A表示该文件已经添加到暂存区了，M表示该文件被修改了，而修改过的文件还没有添加到暂存区，即写了一个文档，添加到暂存区后，又对这个文档进行了修改，且没有再次添加到暂存区。

#### 分支操作

##### 本地仓库

针对本地仓库的分支进行操作。

```shell
# 查看本地分支
git branch
# 查看远程分支，即远程追踪分支
git branch -r
# 查看所有分支。本地 + 远程。可结合正则表达式grep命令
git branch -a
# 切换分支
git checkout your_branch
git switch your_branch # Git2.23版本引入
# 切换HEAD指针的指向，即分离HEAD指针。分离后，HEAD不会关联任何分支。
git checkout commit_hash # commit_hash是某个历史记录点，有一个唯一对应的哈希值。可通过git log查看。（一般用不上，都用相对引用代替，见版本回滚）
# 创建分支
git branch your_branch
# 跟踪分支
git branch -u your_remote_branch your_branch # -u参数见上。
git branch -u your_remote_branch # 如果当前正在your_branch分支上，则可以省略
# 创建并切换分支
git checkout -b your_branch 
git checkout -b your_branch your_remote_branch # 创建分支，并且让该分支跟踪某远程分支
# 修改分支名
git branch -m your_new_branch
git push -u origin your_new_branch # push后才能影响远程仓库。-u参数见上
# 删除分支
git branch -D deleteBranch
```

##### 远程仓库

针对远程仓库的分支进行操作。

```shell
# 跟踪分支
git branch -u your_remote_branch your_branch # -u参数见上。
git branch -u your_remote_branch # 如果当前正在your_branch分支上，则可以省略
# 创建并切换分支，同时跟踪远程分支
git checkout -b your_branch your_remote_branch
# 删除分支
git push origin --delete your_deleted_branch

# 修改分支名 = 修改本地分支名 + 推送 + 删除远程分支名
git branch -m your_new_branch
git push -u origin your_new_branch # push后才能影响远程仓库。-u参数见上
git push origin --delete your_deleted_branch
```

#### 合并版本

```shell
# 从远程仓库中获取最新状态，并且更新在本地仓库上，即同步，但不会合并分支。类似于sudo apt update跟新软件库，sudo apt upgrade才是更新软件。
git fetch
# 合并分支
git merge your_branch
git rebase your_branch # rebase：将该分支的操作在your_branch分支上再执行一遍，并丢弃该分支
# 拉取远程仓库 = 获取 + 合并
git pull origin your_branch # = git fetch + git merge origin your_branch
```

版本的合并有两种，merge和rebase。

- merge：保留提交顺序，但合并多了，会导致提交树很乱。出现冲突需要手动处理冲突。
- rebase：不保留提交顺序，即保持提交树是线性的。

> **推荐使用merge**。因为rebase会破坏提交顺序，不方便版本控制。虽然merge的提交树会很乱，但是可以清清楚楚知道该项目是怎么来的。

#### 删除文件

**建议删除之前，先备份文件，以防错误操作导致丢失文件！**

##### 删除暂存区文件(撤销`git add .`)

当执行了`git add .`后，但是没有进行`git commit`和`git push`，即文件被跟踪（只将文件添加到了暂存区），想撤回`git add .`。

1. `git rm -r --cached`

**取消暂存并停止跟踪（Git跟踪文件的变化，进行版本控制）**。删除后，可重新选择需要添加的文件（可选），重新推送到远程仓库。

```shell
# 删除指定文件或文件夹
git rm (-r 如果有文件夹，则需要添加) --cached delete_file
# 删除所有文件或文件夹
git rm (-r 如果有文件夹，则需要添加) --cached *
```

2. `git reset`

**取消暂存但继续跟踪（仍处于版本控制，传统Git命令）**。具体操作见撤销提交部分。

是否需要重新添加文件：

- `--soft`：不需要重新添加文件，文件仍在暂存区。
- `--mixed`：默认参数，需要重新添加文件，文件不在暂存区。
- `--hard`：没必要，会丢失所有更改。工作区和暂存区都会被重置到指定版本。

3. `git restore`

- **Git 的新语法，Git 2.23+ 才支持。语义化更强，明确针对暂存区操作。**与`git reset`非常类似，但该命令不影响历史记录（每一次`commit`后的记录）。
- **取消暂存，但继续跟踪**

具体操作见文件恢复部分。

##### 撤销本地仓库的文件（撤销`git commit`）

采用`git reset`。具体操作见撤销提交部分。

##### 撤销远程仓库的文件（撤销`git push`）

采用`git revert`。具体操作见撤销提交部分。

##### 仅删除远程仓库的文件，保留工作区的文件

`--cached` 只会影响暂存区，不会影响工作区。

```shell
git rm (-r 如果有文件夹，则需要添加) --cached delete_file
git commit -m 'delete xxx'
git push origin your_branch
```

##### 删除远程仓库和工作区的文件

只能删除已经追踪过的文件，即执行过git add 的文件

```shell
git rm (-r 文件夹使用) delete_file
git commit -m 'delete file'
git push origin your_branch
```

##### 仅删除工作区的文件

正常使用`rm -rf xxx`。

#### 文件恢复

`git restore` 用于帮助恢复工作区/暂存区中的文件或重置一些修改。与`git reset`部分功能重合，但是该命令最大的特点是**不影响提交记录**。

##### 工作区

**注意**：只影响工作区中的文件（即未使用了`git add .`），不会影响暂存区的文件或历史记录，即保留历史记录（保留提交记录）。

```shell
# 恢复某个文件到最新的提交状态（放弃本地修改），默认HEAD指向的版本
git restore file_name

# 恢复所有文件到最新的提交状态
git restore .

# 恢复某个文件到指定版本
git restore --source commit_version file_name
```

##### 暂存区

将已暂存文件从暂存区移除。**注意**：只影响暂存区中的文件（即使用了`git add .`）

```shell
git restore --staged file_name

git restore --staged .

git restore --source commit_version --staged file_name
```

#### 仓库同步

当远程仓库与本地仓库不一致，希望放弃所有更改完成仓库的同步。

##### 本地仓库同步远程仓库

希望放弃本地仓库的所有更改，使得本地仓库与远程仓库一致。

1. `git clone`：最简单的方式，重新克隆远程仓库
2. 本地仓库版本比远程仓库版本新

```shell
# 换言之，撤销提交，恢复至历史提交记录
git fetch origin # 默认远程仓库名origin，可通过 git remote -v 查看
git reset --hard commit_version
```

3. 本地仓库版本比远程仓库版本旧

```shell
# 未发生冲突
git pull
# 若拉取远程仓库会发生冲突，并且希望远程仓库完全覆盖本地仓库，即懒得处理冲突，允许本地文件被覆盖
git restore . # 将工作区文件恢复为最新版本，即丢失工作区文件
git restore --staged . # 将已暂存文件从暂存区移除
git pull
```

##### 远程仓库同步本地仓库

当远程仓库提交记录已经很乱，希望用本地仓库直接覆盖远程仓库。

**以下命令十分危险，请三思而后行，注意备份**

```shell
# 完全覆盖整个仓库，将本地仓库的所有内容完全同步到远程仓库。远程仓库中有的分支，但本地仓库没有，将会被删除
git push --mirror origin

# 等价于
git push --all # 推送所有分支
git push --tags # 推送所有本地标签
git push --prune # 删除远程中本地不存在的引用

# 只覆盖特定分支
git push --force origin branch_name # origin为远程仓库名字，默认为origin，可能会被修改为其他名称
```

#### 撤销提交

恢复到之前的某个版本。本地操作 $\rightarrow$ `reset`；远程操作 $\rightarrow$ `revert`。

**HEAD指针**：指向当前版本号。

##### `reset`——回到过去，抹掉历史

在**本地**分支上进行操作，没有涉及远程仓库。团队协作中，其他人看不到撤销。

**典型用途**：修正**本地**提交错误

```shell
# 查看撤销的版本号
git log

# 软撤销，修改HEAD指针，但之后的版本仍然存在
git reset --soft 版本号
git reset --soft HEAD^ # 撤销到上一个版本，^表示上个版本，^^表示上上个版本（相对引用）
git reset --soft HEAD~N # 撤销到上N个版本，N = 1时，跟HEAD^等价

# 硬撤销，不仅修改HEAD指针，还将该版本之后的版本全部删除。谨慎，谨慎，再谨慎，一般不使用。好像可以通过git reflog 找回，但是不要抱有侥幸心理，数据无价！
git reset --hard 版本号
git reset --hard HEAD^
git reset --hard HEAD~N

# 混合模式，默认参数。
git reset --mixed 版本号
git reset --mixed HEAD^
git reset --mixed HEAD~N
```

三种模式：

- `--soft`：仅重置当前分支的 HEAD 指向的提交，**不会改变工作区和暂存区中的文件**。自该结点以来所有的更改都在**暂存区**中。方便之后操作，比如重新提交。
- `--hard`：重置当前分支的 HEAD 指向的提交，同时**重置工作区和暂存区**。这会**完全抹掉**指定提交后的所有更改，包括未提交的工作内容。**（慎用）**
- `--mixed`（默认）：重置当前分支的 HEAD 指向的提交，**重置暂存区**，但**不重置工作区（本地文件）**。自该结点以来所有的更改都在**工作区**中。

##### `revert`——回到过去，保留历史

创建一个**新提交**，作为当前提交的**逆操作**，从而恢复之前的修改。既然会创建一个新提交，则必然会影响远程仓库。团队协作中，其他人能看到撤销。

**特点**：保留了历史记录，更加**安全**和透明。

```shell
# 查看提交哈希值
git log

# 撤销指定提交
git revert commit_hash
# 撤销多个提交
git revert commit_hash1 commit_hash2
# 撤销一段提交
git revert commit_hash_start..commit_hash_end
```

#### 仓库备份

在执行部分**危险命令**时，建议养成备份的习惯。最简单的方式是在本地手动备份。这里将展示如何创建新的分支，在远程仓库中进行备份。

```shell
# 将远程的remote_branch分支备份到backup_branch，即创建一个新的分支backup_branch，将remote_branch分支中的所有内容备份到backup_branch分支。
git push origin remote_branch:backup_branch

# 以下是相关分支查看，详见分支操作板块
git branch
git branch -r
git branch -a
```

#### 克隆仓库

非常常用的命令：`git clone repositoryURL`，`git clone -o your_remote_name repositoryURL`（设置远程仓库名称，默认为origin）

不需要创建文件夹，该命令会把整个文件夹克隆下来。在克隆之前，需要切换到你想存放文件的文件夹路径上。

#### 仓库覆盖

##### 远程仓库覆盖本地仓库

1. 全部覆盖

```shell
# 获取远程仓库最新信息
git fetch origin
# 硬撤销。重置本地分支为远程分支的状态
git reset --hard origin/<branch_name> # 例如：git reset --hard origin/main
```

2. 覆盖指定文件夹

```shell
git checkout origin/<branch_name> -- <path_to_folder> # 注意 -- 之后有空格 

# 例如
git checkout origin/main -- Notes/ 

# 提交更改（可选，但最好加上。因为也可以随下一次提交一起）
git add <your_files>
git commit -m "your comment"
git push origin <branch_name>
```

##### 本地仓库覆盖远程仓库

1. 全部覆盖

```shell
# 正常添加所有文件夹
git add .
git commit -m 'your comment'
git push origin <branch_name> --force # --force参数，非常危险
```

2. 覆盖指定文件夹

```shell
git fetch # 获取远程仓库的信息
git checkout <branch_name> -- path/to/folder # 可以简单理解为有origin的就是远程仓库
# 例如
git checkout main -- Notes/ 

# 提交更改（可选，但最好加上。因为也可以随下一次提交一起）
git add <your_files>
git commit -m "your comment"
git push origin <branch_name>
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

从上到下，依次匹配。先出现的规则，先生效；后出现的规则，会覆盖前面的规则。

2. 使用方法

   - 基本语法

   ```txt
   #：注释。跟shell一样
   /：表示目录
   *：单星号通配符，匹配多个字符，但是只能匹配同一层目录内的文件或者目录
   **：双星号通配符，匹配零个或多个目录层级，可以匹配任意深度的路径
   ？：匹配单个字符
   !：表示不忽略某文件或者某目录，相当于取反
   
   其他复杂的匹配，可以参考正则表达式
   ```

   **一行写一个规则，否则不会生效**。忽略文件（夹）路径是以`.gitignore`文件位置作为根目录，并且**相对路径不能添加`./`**。

   - **示例**

   相对路径是以**.gitignore所在目录为根目录**，假设目录结构如下：

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

2. 远程仓库与本地仓库版本不统一，产生分歧

```shell
# Git 终端描述
On branch main
Your branch and 'origin/main' have diverged,
and have 2 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```

问题原因及解决方案：本地分支与远程分支存在分歧，各自有1个和2个不同的提交。在终端中，Git也给出了解决方法。使用`git pull`合并远程分支。

```shell
# 使用git pull后的提示
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
```

问题原因及解决方案：存在分歧分支，需要确定合并方式。Git也给出了合并方式选择代码。注意，推荐使用`merge`，即`git config pull.rebase false`。然后正常推送到远程仓库。

过程中可能会遇到冲突，需要手动处理冲突。即，把冲突的文件找到，然后修改冲突部分，最后重新推送到远程仓库。参考[解决冲突——廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

3. `git`远程命令无法使用，使用移动数据又恢复正常（我使用的是**校园网**）

使用`ssh -T -v git@github.com`，`-v`表示详细输出，查看问题所在。发现输出一直卡在`debug1: expecting ssh2_msg_kex_ecdh_reply`，与SSH 密钥交换有关。首先，考虑是否是网络连接存在问题，使用`ping github.com`发现能成功，故“网络没问题”（实际上，确实是网络的问题，但此时，我误排除了该问题）。之后，经过搜索引擎检索，有的人说是网关的MTU的问题，将其调小，小于1500即可。我尝试后发现不起效。由于我对该方面不了解，故不推荐修改MTU。最后，我看到了“Coding的痕迹”的博客[记 SSH 连接不上的问题](https://sunnysab.cn/2022/03/01/A-SSH-Issue/)，其中分析了问题不在MTU的原因，更加坚定了我不修改MTU的想法。最终，根据该博客，选择了最不折腾的方法——SSH使用代理。

[修改方式](https://www.cnblogs.com/coder-shane/p/18410182)：

- SOCKS代理：代理地址以`socks5://` 或者 `socks://` 开头。
  - `-x`：表示后面是SOCKS代理服务器地址
  - `-X 5`：表示SOCKS 5代理。如果是使用SOCKS 4，则改为`-X 4`
  - `%h`和`%p`：分别表示目标主机和端口。SSH 会自动将它们替换为代理服务器地址`your_proxy_address`和端口号`your_port`

```shell
# 修改ssh配置文件。如果没有，则创建
vim ~/.ssh/config
# 在文件内添加
Host example.com
    Hostname example.com
    ProxyCommand nc -x your_proxy_address your_port -X 5 %h %p
```

- HTTP代理：代理地址以`http://`开头。
  - 借助工具`corkscrew`。`sudo apt install corkscrew`

```shell
# 修改ssh配置文件。如果没有，则创建
vim ~/.ssh/config
# 在文件内添加
Host example.com
    Hostname example.com
    ProxyCommand corkscrew your_proxy_address your_port %h %p
```

