#  Ubuntu文件定时备份

> 本教程参考ChatGPT，仅作为笔记，日后复习。
>
> **同时祝愿你的备份永远用不到，但永远在那里！**

随着工位电脑上的重要文件越来越多，同时通过Linux命令行删除文件往往来不及后悔，所以近期萌生了文件定时备份的想法。

该脚本需要具备以下特性：

1. 定时自动执行
2. 增量式备份，仅备份修改了的内容
3. 保留一段时间的备份文件快照，便于恢复
4. 省磁盘空间

## 操作流程

### 创建备份根目录

```bash
SRC=/home/yourname/projects        # 要备份的目录
DEST=/backup/projects              # 备份根目录

sudo mkdir -p "$DEST"							 # 创建目录
sudo chown -R "$(whoami):$(whoami)" "$DEST" # 修改权限。用户名:用户组
```

### 备份脚本

```bash
#!/usr/bin/env bash
set -euo pipefail

SRC="/home/yourname/projects"
DEST="/backup/projects"
KEEP_DAYS=30                       # 快照保留天数

TS=$(date +%F)                    
TODAY="$DEST/$TS"
LAST="$DEST/latest"                

mkdir -p "$TODAY"

rsync -a --delete --link-dest="$LAST" "$SRC/" "$TODAY/"

rm -f "$LAST"
ln -s "$TODAY" "$LAST" # 类似于指针

find "$DEST" -maxdepth 1 -type d -name '????-??-??' -mtime +"$KEEP_DAYS" -exec rm -rf {} +

exit 0
```

#### 讲解

**整体思路：**每一次备份都是一个文件夹，采用硬连接（[软连接和硬连接对比](https://www.cnblogs.com/coder-shane/p/18204202)），不会额外占用磁盘空间。通过`find`命令批量删除过期备份快照。

1. Bash常见三件套

   `set -euo pipefail`：为了让脚本更安全、易排错。让脚本碰到错误命令、变量拼写错误、管道中间失败时立马停下来，而不是继续执行。

   |  **选项**   |   **全写**    |                           **作用**                           |
   | :---------: | :-----------: | :----------------------------------------------------------: |
   |     -e      |  **errexit**  | 任何命令返回非零退出码时，*立即*终止脚本（避免错误被悄悄忽略） |
   |     -u      |  **nounset**  |     访问未定义的变量时立刻报错退出（防止拼写或逻辑错误）     |
   | -o pipefail | **pipe fail** | 在管道 cmd1 \| cmd2 中，只要**任一**命令失败，整个管道返回失败（默认只看最后一个） |

2. 时间格式化

| **格式符** |     **含义**      | **例子（2025 年 7 月 12 日）** |
| :--------: | :---------------: | :----------------------------: |
|     %Y     |     4 位年份      |              2025              |
|     %m     | 2 位月份（01-12） |               07               |
|     %d     |  2 位日（01-31）  |               12               |
|     %F     | 完整日期 %Y-%m-%d |           2025-07-12           |

2. 数据同步工具

`rsync`支持断点续传的数据同步工具。类似的工具还有`scp`，但它不支持断点续传。

`rsync [optional] source destination` —— 源文件（被传输的文件）、目的地址。如果与远程服务器传输，则改为`username@remote_host:path`。

注意：路径中的斜杠很重要。`/source/`表示目录本身；`/source`表示目录内容

常用参数

|       选项        |          全称           |             说明             |
| :---------------: | :---------------------: | :--------------------------: |
|        -a         |        --archive        |  归档模式，保留所有文件属性  |
|        -v         |        --verbose        |       显示详细传输信息       |
|        -z         |       --compress        |        传输时压缩数据        |
|        -r         |       --recursive       |         递归复制目录         |
|        -l         |         --links         |         保留符号链接         |
|        -p         |         --perms         |         保留文件权限         |
|        -t         |         --times         |       保留文件修改时间       |
|        -g         |         --group         |        保留文件所属组        |
|        -o         |         --owner         |        保留文件所有者        |
|        -D         |        --devices        | 保留设备文件（仅限超级用户） |
|        -h         |    --human-readable     |    以人类可读格式输出数字    |
|    --progress     |      显示传输进度       |                              |
|     --delete      | 删除目标中源没有的文件  |                              |
|    --link-dest    |   硬连接复用未变文件    |                              |
|     --partial     |    保留部分传输文件     |           断点续传           |
| --exclude=PATTERN | 排除匹配 PATTERN 的文件 |    两者同时使用，注意顺序    |
| --include=PATTERN | 包含匹配 PATTERN 的文件 |    两者同时使用，注意顺序    |

常用样例：

`rsync -av --progress --partial /source/ /destination/`：保留文件属性，显示进度条，保留部分传输文件（断点续传）。如果是与远程服务器传输，则加上`-z`压缩后传输更快。

3. `find`命令

`find`查找命令十分强大。准确来说，它不止是一个查找命令，更是一个筛选执行命令。

- `-maxdepth`：查找目录深度。1表示当前目录
- `-type`：文件类型。`d`表示目录
- `-name`：文件名字，此处为“glob”模式——Shell通配模式，仅匹配文件名，不含父目录。`?`表示1个任意字母，`*`表示任意多个字符（包括0个）
- `-regrex`：正则表达式，匹配的是整个路径。
- `-mtime`：时间。+号表示XX天以外，即查找XX天以外的文件
- `-exec`：执行命令。{}中包含的是前面查找结果。
- +：表示批量执行，即不是查找到一个就执行一次，而是尽可能多的查找，然后一次性执行命令。

### systemd服务单元

服务单元路径：`/etc/systemd/system/XX.service`

**服务单元：做什么**

```ini
[Unit]
Description=Incremental backup of ~/projects to /backup/projects

[Service]
Type=oneshot
User=yourname
# /usr/local/bin 里存放着用户自定义的脚本
ExecStart=/usr/local/bin/backup_projects.sh 
```

### systemd定时器

定时器路径：`/etc/systemd/system/XX.timer`

**定时器：什么时候做**

惯例：服务单元和定时器同名，这样就无需指明服务名。

```ini
[Unit]
Description=Run backup-projects.service daily at 03:00

[Timer]
OnCalendar=*-*-* 03:00
Persistent=true           # 关机错过 → 开机补跑

[Install]
# 实现开机自动生效
WantedBy=timers.target
```



### 启动并检查

```bash
sudo systemctl daemon-reload			# 重新扫描所有单元文件，把修改过的配置加载进内存
sudo systemctl enable --now backup-projects.timer     # 开机自启并立即计时
systemctl list-timers --all | grep backup-projects    # 下次触发
journalctl -u backup-projects.service -n 50           # 最近日志
```

### 恢复数据

本质是重新传输文件数据。

```bash	
# 恢复最新快照
rsync -a /backup/projects/latest/ /home/yourname/projects/

# 或恢复到历史某天
rsync -a /backup/projects/2025-07-05/important.doc ~/Desktop/
```



## 参考文献

- [Linux rsync 命令](https://www.runoob.com/linux/linux-comm-rsync.html)
- ChatGPT