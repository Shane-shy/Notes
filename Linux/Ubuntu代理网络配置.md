# Ubuntu代理网络配置

> 配置网络代理后，代理流量仍旧会被代理规则过滤，而不是全局代理。

## 终端

### 临时配置

终端临时使用网络代理，关闭终端后，网络代理也将会关闭。

```shell
export http_proxy="http://your-proxy-server:port"
export https_proxy="http://your-proxy-server:port"
```

### 永久配置

```shell
# 编辑 .bashrc
vim ~/.bashrc
# 末端添加
export http_proxy="http://your-proxy-server:port"
export https_proxy="http://your-proxy-server:port"
# 刷新.bashrc
source ~/.bashrc
```

### 验证

若能输出配置的代理地址，则配置成功。也可通过`curl`命令查看网站信息。

**注意：**`ping`命令使用`ICMP`协议，大多数代理服务器不处理`ICMP`流量。

```shell
echo $http_proxy
echo $https_proxy

# 或者curl命令查看某国外网站信息。-I查看网站信息
curl -I XXX.com
```

## APT软件包管理工具

部分软件仓库服务器位于国外，在国内可能连接超时，如[Typora仓库](https://www.cnblogs.com/coder-shane/p/18116492)、spotify仓库等。

如果终端配置了代理，可以使用`sudo -E apt update`临时继承代理环境变量。同样通过`curl`命令检测是否设置成功。

```shell
sudo vim /etc/apt/apt.conf.d/95proxies

# 添加以下内容
Acquire::http::Proxy "http://your-proxy-server:port/";
Acquire::https::Proxy "https://your-proxy-server:port/";
```

## SSH

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

