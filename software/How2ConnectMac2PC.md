# Mac连接PC

## 最方便的方式

使用远程桌面软件，例如向日葵、todesk等。但缺点是体验一般，传文件麻烦，清晰度和流畅度需要购买会员服务保障。

但这是最方便、最省力、成本最低的方案。

## 局域网内连接

通过SSH服务，能通过终端直接访问PC所有文件夹，Linux和Windows均可，我觉得这是很好的方案，毕竟只有终端信息，不会获得其它诱惑性的提示，操作过程也不算麻烦，在vscode内也能很好上传下载文件，以及代码编辑。

[参考文章](https://www.jianshu.com/p/1874c7c9d4f4)

```bash
sudo apt-get update  # 更新 apt-get 工具
sudo apt-get install openssh-server
sudo apt-get install openssh-client 
```

```bash
sudo apt-get install vim  # 安装 vim 编辑器，如已安装请掠过
sudo vim /etc/ssh/sshd_config
```

用vim编辑器编辑：

```bash
Port 22                       # 默认22端口，如果有端口占用可以自己修改
PermitRootLogin yes           # 如果配置文件中没有这行内容，需要手动添加
PasswordAuthentication yes    # 密码验证登录
```

```bash
sudo service ssh start
```

```bash
ps -aux | grep ssh
```

上面一个代码检查ssh服务是否开启

使用ifconfig查看ip地址，我使用以太网地址也能连接上。

### 后续优化

#### 配置免密登陆

- 在本地计算机生成SSH密钥对

`ssh-keygen -t rsa -b 4096`

这条命令会生成一对RSA密钥，密钥长度为4096位。它会询问你保存密钥对的位置（默认是`~/.ssh/id_rsa`），以及是否设置密钥短语（一个可选的密码来保护你的私钥）。

- 将公钥上传到远程服务器

`ssh-copy-id user@hostname`

#### 安装独立Linux系统

参考当前目录下的“如何配置新的Linux”

## 局域网外连接

需要购买云服务器以获得公网IP。

涉及购买，受限于经济条件，我可能会货比三家，造成更多时间和精力的浪费，不太愿意选择这种方式。

如果非要连接，就使用远程桌面软件。



2024.5.11

虽然还没有在局域网外适用，但是局域网内的效果出奇的好，显示效果非常清晰。

参考的视频是https://www.bilibili.com/video/BV1YM4y1z7wE/?spm_id_from=333.337.search-card.all.click&vd_source=86646d951d84826a4640314db345f2aa

就是在Mac和win上装了tailscale软件，弄出来虚拟的公网IP，然后再打开win上的远程桌面，Mac上安装Microsoft Remote Desktop，这个下载链接是https://go.microsoft.com/fwlink/?linkid=868963，然后使用win的IP即可，输入win上登录的微软账号密码

2024.06.08

遇到了即使是在局域网内，依旧延迟很高，于是我怀疑是 tailscale 的问题，于是就参考视频https://www.bilibili.com/video/BV1uS4y1k7zx，不创建虚拟局域网，而是直接使用 Windows 的 IP 地址进行链接。