# Ubuntu软件仓库GPG公钥缺失

## 问题描述

```shell
sudo apt update
...

# 出现以下警告
W: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: http://repository.spotify.com stable InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY C85668DF69375001
W: Failed to fetch http://repository.spotify.com/dists/stable/InRelease  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY C85668DF69375001
W: Some index files failed to download. They have been ignored, or old ones used instead.
```

由于每次更新软件仓库都会出现此警告，因此我下定决心解决这个问题。

问题的原因：缺少用于验证Spotify软件仓库的GPG公钥，导致`apt`不信任该仓库。

## 解决方法

### 允许未授权的仓库

简单粗暴，但是存在安全隐患，**不建议使用**。

```shell
sudo apt update --allow-unauthenticated
```

### 添加公钥

从`keyserver.ubuntu.com`公开的GPG密钥服务器中下载密钥。Ubuntu软件仓库和许多第三方仓库的密钥都可以从这里获取。

`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <KEY-ID>`，其中`<KEY-ID>`就是报错中的公钥`C85668DF69375001`。

到这一步，可以试试`sudo apt update`，看是否会有警告或者报错。我在这一步时遇到了以下报错：

```shell
W: http://repository.spotify.com/dists/stable/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
```

简而言之，传统的方式将所有的GPG公钥都存储在单一的全局密钥环文件内，即`/etc/apt/trusted.gpg`，不安全。建议将公钥放到 `/usr/share/keyrings` 目录，并使用 `signed-by` 参数进行绑定。

### 修改公钥存放目录

1. 查看现有公钥

```shell
sudo apt-key list
# 以下为终端输出值
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
/etc/apt/trusted.gpg
--------------------
pub   rsa4096 2024-11-13 [SC] [expires: 2026-02-06]
      B420 FD37 77CC E3A7 F007  6B55 C856 68DF 6937 5001
uid           [ unknown] Spotify Public Repository Signing Key <tux@spotify.com>
... # 后面的省略
```

2. 导出密钥

`sudo apt-key export 69375001 | sudo gpg --dearmor -o /usr/share/keyrings/spotify.gpg`

注意：XXXXXXXX为pub值的后八个字符

3. 配置仓库

`echo "deb [signed-by=/usr/share/keyrings/spotify.gpg] http://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list`

使用 `signed-by` 参数绑定仓库与公钥文件。也可以使用`vim`在`/etc/apt/sources.list.d/spotify.list`中进行添加。

4. 更新软件包列表

   `sudo apt update`。此时，应该就没有任何报错或者警告了。

## 参考文献

[apt-key deprecation warning when updating system: "Key is stored in legacy trusted.gpg keyring"](https://askubuntu.com/questions/1398344/apt-key-deprecation-warning-when-updating-system-key-is-stored-in-legacy-trust)

