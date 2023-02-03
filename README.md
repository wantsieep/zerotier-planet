
# 一分钟自建zerotier-planet

私有部署zeroteir-planet服务
基于 [ztncui](https://github.com/key-networks/ztncui-aio) 整理成 docker-compose.yml 文件.

**特别感谢** <https://github.com/Jonnyan404/zerotier-planet/issues/11#issuecomment-1059961262> 这个issue中各位用户的贡献，基于此issue中 `@jqtmviyu` 的步骤和`kaaass`的 [mkmoonworld](https://github.com/kaaass/ZeroTierOne/releases/tag/mkmoonworld-1.0) 制作成目前的patch（集成planet和moon）。

# 必要条件

- 具有公网ip的服务器
- 安装 docker
- 安装 docker-compose
- 防火墙开放TCP端口 4000/9993/3180 和UDP端口 9993

# 用法

```
git clone https://github.com/Jonnyan404/zerotier-planet
OR
git clone https://gitee.com/Jonnyan404/zerotier-planet

cd zerotier-planet
docker-compose up -d
# 以下步骤为创建planet和moon
docker cp mkmoonworld-x86_64 ztncui:/tmp
docker cp patch.sh ztncui:/tmp
docker exec -it ztncui bash /tmp/patch.sh
docker restart ztncui
```

然后浏览器访问 `http://ip:4000` 打开web控制台界面。

浏览器访问 `http://ip:3180` 打开planet和moon文件下载页面（亦可在项目根目录的`./ztncui/etc/myfs/`里获取）。


- 用户名:admin
- 密码:mrdoc.fun

note: 如果未指定密码,可执行 docker exec -it ztncui cat /var/log/docker-ztncui.log|grep Password 获取密码.

# 各客户端配置planet

### Windows 配置

首先去zerotier官网下载一个zerotier客户端
将 planet 文件覆盖粘贴到 C:\ProgramData\ZeroTier\One 中(这个目录是个隐藏目录，需要运允许查看隐藏目录才行)
Win+S 搜索 服务

找到ZeroTier One，并且重启服务
使用管理员身份打开PowerShell
执行如下命令，看到join ok字样就成功了
```
PS C:\Windows\system32> zerotier-cli.bat join 网络id(就是在网页里面创建的那个网络)
200 join OK
PS C:\Windows\system32>
```
登录管理后台可以看到有个个新的客户端，勾选Authorized就行
执行如下命令：

```
PS C:\Windows\system32> zerotier-cli.bat peers
200 peers
<ztaddr>   <ver>  <role> <lat> <link> <lastTX> <lastRX> <path>
fcbaeb9b6c 1.8.7  PLANET    52 DIRECT 16       8994     1.1.1.1/9993
fe92971aad 1.8.7  LEAF      14 DIRECT -1       4150     2.2.2.2/9993
PS C:\Windows\system32>
```
可以看到有一个 PLANTET 和 LEAF 角色，连接方式均为 DIRECT(直连)
到这里就加入网络成功了

### Linux 客户端
步骤如下：
安装linux客户端软件
进入目录 /var/lib/zerotier-one
替换目录下的 planet 文件
重启 zerotier-one 服务(service zerotier-one restart)
加入网络 zerotier-cli join 网络 id
管理后台同意加入请求
zerotier-cli peers 可以看到 planet 角色


### 私有 zerotier-planet 的优势:
- 解除官方 25 的设备连接数限制
- 提升手机客户端连接的稳定性

# Reference Link

- <https://www.mrdoc.fun/doc/443/>
- <https://github.com/key-networks/ztncui-aio>
