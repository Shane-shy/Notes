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

## Git使用操作

### 整体流程和常用命令

#### 整体流程

1.  打开终端，切换到需要上传的文件夹。
2. 初始化仓库。这个时候，会出现一个.git的隐藏文件。git的所有操作都保存在这个文件内。如果这个文件删了，那么这个文件夹跟普通文件夹就一样了。
3. 将文件添加到缓存区。
4. 将文件推送到远程仓库（即，Github）。

#### 常用命令

```shell
cd yourPath

git init
# 添加文件到缓存区
git add .
# 提交文件
git commit # 这个会出现一个界面，让你写评论，类似于简介或者提醒
git commit -m  '' # 可以在引号内，直接写评论
# 推送到远程仓库
git push -u origin main(your branch) # 禁止git push -f origin main 强制推送到远程仓库，因为这会之间覆盖远程仓库内的内容。
```

origin：是远程仓库的别名。用于避免与本地仓库重名。

-u：建立本地分支与远程分支的链接。如果远程仓库为空，那么会同时创建分支。如果远程仓库不为空，可以不用加-u。其实如果分不清，全部加-u，也没关系。

### Git命令操作

这里将会介绍一些不太常用的命令。

总结一下：如果要对远程仓库进行增删、重命名等。在本地仓库上修改了，还需要提交并推送到远程仓库里。

1. `git remote add origin yourRepositoryURL`

关联本地仓库与远程仓库。URL选择SSH，而不是HTTPS，目前HTTPS不被官方推荐。**每个仓库只需要关联一次。**

![image-20240407130313595](./Git与Github.assets/image-20240407130313595.png)

2. `git remote -v`

查看当前本地仓库关联的远程仓库列表。

3. `git remote set-url origin newUrl`

修改关联的远程仓库地址。

4. `git status`

可以查看仓库当前状态。

5. `git log`

查看提交日志。

6. `git checkout branch`

切换分支。`git checkout -b branch`创建新的分支。`git checkout -b branch origin/branch`创建一个与远程分支相同的分支，并且远程分支与这个新建的本地分支关联。（这个还不太确定）

7. `git branch`

查看当前有哪些分支

8. `git branch -m newBranch`

修改分支的名字。之后记得推送到远程仓库内，`git push -u origin newBranch`

9. `git fetch`

从远程仓库中获取最新状态，并且更新在本地仓库上。一个同步的感觉。与`sudo apt update`有点像。它并不会合并，只是更新个状态。

10. `git merge branch`

当前分支与branch分支进行合并。

11. `git branch -D deleteBranch `

删除分支。

12. `git pull origin branch`

拉取origin/branch分支，并且与当前分支合并。相当于`git fetch + git merge branch`

13. `git rm file`  OR `git rm -r file` ，前者是文件，后者是文件夹。

删除远程仓库中的文件或者文件夹。之后，需要提交到远程仓库内。即`git commit -m 'deleteFile'`  + `git push origin branch`。还是需要慎重。

14. `git clone yourURL`

不需要自己创建文件夹，它会把整个文件夹都克隆下来。在克隆之前，需要切换到你想存放文件的地址上。

15. `git rm --cached fileName`

清楚缓存区内的文件。

### 出现的问题

仅展示目前遇到的问题，之后出现了问题会进行补充。

1. 本地分支是master，而远程分支是main。不能推送到main分支。

解决方法：`git fetch`获取远程仓库状态，更新本地仓库。之后，按照正常流程进行。

