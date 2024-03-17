# 系统安装

下载好ISO镜像，烧录至U盘，从U盘启动安装Linux。

详细可以参考安装教学视频。

# 网络环境

安装clash，按照ikuuu教程即可。

后续可以配置dashboard可视化和节点切换。

我使用了端口映射，即可在Mac上的浏览器访问到。

1. 打开你的Clash配置文件（通常名为`config.yaml`），这个文件应该在你的Clash目录中
2. 确保文件中有指定`external-controller`和`secret`字段。这通常看起来像是：

```yaml
external-controller: '0.0.0.0:9090'
secret: 'your_secret'
```

但是我没有设置密码。

3. **选择一个Web UI项目**：有几个不同的Web UI项目可供Clash使用。Yacd (Yet Another Clash Dashboard) 是一个流行的选择。下载的yacd是.tar.xz，需要单独解压

```bash
tar -xvf yacd.tar.xz
```

将解压好的文件夹放入以下目录：

```bash
sudo mv yacd-directory /etc/clash/dashboard
```

4. 修改clash配置文件，新增字段：

```bash
external-ui: "/etc/clash/dashboard"
```

5. 重启clash，然后从Mac浏览器输入你的Ubuntu的IP:9090/ui，会有一个链接，打开即可。



# 开发环境

## 换源

换了清华源，但是有问题。

## 基础开发环境

`sudo apt update`

`sudo apt install build-essential`

`gcc --version`

## D2L

参考书上的方式，创建好d2l虚拟环境。

## 显卡驱动

在“附加驱动”中选择即可。

## cuda

cuda-toolkit？需要再装

# 后期优化

## 输入法

直接搜搜狗输入法，按照官方教程来做，但别跟最上面的教程，整个教程很混乱，下面的才是最完整的流程。

## 开启自启动程序

比如自动启动clash，需要将脚本添加到开机自启动里。

- 启动clash的脚本

```bash
#!/bin/bash
cd ~/Desktop/clash
./clash -d .
```

```bash
chmod +x ~/start_clash.sh
```

- 检查是否能成功运行

```bash
ps aux | grep clash
```

- gnome-terminal来自动启用clash服务

```bash
#!/bin/bash
gnome-terminal -- bash -c '~/start_clash.sh; exec bash'
```

```bash
chmod +x ~/start_clash_in_terminal.sh
```

1. 打开`启动应用程序`首选项。
2. 点击`添加`来创建一个新的启动项。
3. 在`名称`字段中输入一个描述性的名称，比如`Start Clash in Terminal`。
4. 在`命令`字段中输入脚本的完整路径，例如`/home/你的用户名/start_clash_in_terminal.sh`（记得替换`你的用户名`为你的实际用户名）。
5. 点击`添加`保存你的更改。

## 局域网连接

参考当前目录下的文档，如何使用Mac连接PC

## 内网穿透