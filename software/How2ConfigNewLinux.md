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

之后我解决了此问题：

### 常用的中国大陆镜像源

1. **清华大学镜像源** - `https://pypi.tuna.tsinghua.edu.cn/simple`
2. **阿里云镜像源** - `https://mirrors.aliyun.com/pypi/simple/`
3. **中国科技大学镜像源** - `https://pypi.mirrors.ustc.edu.cn/simple/`
4. **华中科技大学镜像源** - `https://pypi.hustunique.com/simple/`
5. **腾讯云镜像源** - `https://mirrors.cloud.tencent.com/pypi/simple`

使用方法：

 ```bash
 pip install Pillow -i https://pypi.tuna.tsinghua.edu.cn/simple
 ```

如果你想要永久更改镜像源，可以修改`pip`的配置文件

- **Linux和macOS**：在你的家目录下创建或修改`.pip/pip.conf`文件，添加以下内容：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

- **Windows**：在你的用户文件夹下创建或修改`pip.ini`文件（通常路径为`C:\Users\<你的用户名>\pip\pip.ini`），添加同样的内容。

## 基础开发环境

`sudo apt update`

`sudo apt install build-essential`

`gcc --version`

## Miniconda

```bash
sh MinicondaXXXX.sh -b
~/minicondaX/bin/conda init
```



## 虚拟环境

```bash
conda create --name ocr python=3.9
conda activate ocr
conda install jupyter
```

后续可以安装pytorch的GPU版本等深度学习框架。

### Mac远程访问Jupyter

要在Mac上的浏览器中访问位于Linux服务器上的Jupyter Notebook，你需要设置Jupyter Notebook服务器以允许远程访问，并且可能还需要设置SSH隧道来安全地转发通信。下面是如何做到这一点的步骤：

#### 步骤1: 配置Jupyter Notebook以允许远程访问

首先，确保你的Jupyter Notebook配置允许远程访问。在你的Linux服务器上，运行以下命令来生成Jupyter配置文件（如果你还没有）：

```bash
jupyter notebook --generate-config
```

然后，编辑生成的配置文件（通常位于`~/.jupyter/jupyter_notebook_config.py`）。你可能需要设置以下几个选项：

```python
c.NotebookApp.ip = '0.0.0.0'  # 允许任何IP访问
c.NotebookApp.open_browser = False  # 不自动打开浏览器
c.NotebookApp.port = 8888  # 使用8888端口，或者任何你喜欢的端口
# c.NotebookApp.password = ''  # 你可以设置密码（推荐）或使用token认证
```

如果你选择设置密码，可以使用以下命令生成密码：

```bash
jupyter notebook password
```

#### 步骤2: 使用SSH隧道从Mac访问

为了安全地从你的Mac访问Jupyter服务器，你可以设置一个SSH隧道。这样做会将服务器上的端口（比如8888）转发到本地机器上的端口（比如8888），通过SSH的加密通道安全地传输数据。

在你的Mac上，打开一个终端窗口，运行以下命令来建立SSH隧道：

```bash
ssh -N -f -L localhost:8888:localhost:8888 yfy@10.160.215.167
```

这里，`-N`告诉SSH这个连接不会执行任何远程命令，`-f`使SSH进入后台运行，`-L`指定了端口转发设置。`localhost:8888:localhost:8888`意味着把本地的8888端口转发到远程机器的8888端口。

#### 步骤3: 访问Jupyter Notebook

一旦SSH隧道设置完成，你就可以在Mac的浏览器中通过访问`http://localhost:8888`来访问位于Linux服务器上的Jupyter Notebook了。如果你设置了密码，你会被要求输入它；否则，你可能需要输入Jupyter服务器启动时在终端显示的token。

通过这种方式，你可以安全地从你的Mac访问远程Linux服务器上的Jupyter Notebook，而无需在服务器上进行复杂的网络配置。

### Jupyter暗黑模式

在Ubuntu中，要切换Jupyter Notebook到暗黑模式，你可以使用Jupyter Notebook的主题扩展（如`jupyterthemes`）或直接在JupyterLab中更改主题（如果你使用的是JupyterLab）。下面是两种方法的具体步骤：

#### 使用jupyterthemes

1. **安装`jupyterthemes`**：
   首先，确保你已经激活了相应的虚拟环境。然后在终端中安装`jupyterthemes`包：
   ```
   pip install jupyterthemes
   ```

2. **切换到暗黑模式**：
   使用`jt`命令来选择一个暗色主题。例如，使用`chesterish`主题：
   ```
   jt -t chesterish
   ```
   `jupyterthemes`提供了多种主题选项，你可以通过`jt -l`列出所有可用主题，然后选择你喜欢的一个。

3. **重启Jupyter Notebook**：
   如果Jupyter Notebook已经在运行，需要重启它以应用新的主题设置。

4. **恢复默认主题**：
   如果你想要恢复到默认的Jupyter Notebook主题，可以使用以下命令：
   ```
   jt -r
   ```
   然后重启Jupyter Notebook。

#### 在JupyterLab中切换暗黑模式

如果你使用的是JupyterLab，切换到暗黑模式更为简单：

1. **打开JupyterLab**：
   在终端中启动JupyterLab：
   ```
   jupyter lab
   ```

2. **切换主题**：
   在JupyterLab界面中，点击顶部菜单栏的`Settings` > `JupyterLab Theme` > `JupyterLab Dark`来切换到暗黑模式。

JupyterLab的暗黑模式可以直接通过菜单栏进行切换，不需要安装额外的扩展或主题。这使得在JupyterLab中切换主题变得非常方便。

## D2L

参考书上的方式，创建好d2l虚拟环境。

## 显卡驱动

在“附加驱动”中选择即可。

## cuda

cuda-toolkit？需要再装

## Docker

*此步骤并未参考官方文档，而是直接询问的GPT4，内容可能存疑。*

在Ubuntu中安装Docker环境的步骤如下，这些步骤适用于大多数Ubuntu版本，包括Ubuntu 20.04 LTS和Ubuntu 18.04 LTS。请按照下列步骤操作：

1. **更新软件包索引**：
   打开终端并执行以下命令，以确保您的Ubuntu系统的软件包索引是最新的。
   ```bash
   sudo apt-get update
   ```

2. **安装所需的软件包**：
   安装`apt-transport-https`、`ca-certificates`、`curl`、`software-properties-common`这些软件包，这些是添加外部HTTPS源所需要的。
   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **添加Docker的官方GPG密钥**：*【运行后警告：apt-key已弃用。改为在trusted.gpg.d中管理密匙环文件(参见apt-key(8))】*
   使用curl将Docker的官方GPG密钥添加到您的系统中，以确保下载的软件包的安全性。
   
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

*重新询问：*

收到`apt-key is deprecated`的警告消息表明`apt-key`命令已经被废弃，这是因为Ubuntu和Debian等Linux发行版正在向更安全的软件包签名验证机制过渡。虽然这个警告不会阻止您添加Docker的GPG密钥，也不会立即影响Docker的安装和使用，但为了遵循最佳实践，建议采用新的方法来管理APT的密钥。

以下是根据警告信息更新的步骤，使用更现代的方法来添加Docker的官方GPG密钥，并确保APT能够验证下载软件包的签名：

1. **下载Docker的GPG密钥**：
   首先，使用`curl`命令下载密钥：
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o docker.gpg
   ```

2. **将密钥添加到APT的信任密钥库中**：
   然后，将下载的密钥添加到`/usr/share/keyrings`目录下的一个密钥环文件中。这是一个更安全的方法，因为它不会将密钥添加到全局的`trusted.gpg`或`trusted.gpg.d`目录中，而是将密钥存储在一个单独的文件中。
   ```bash
   sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg docker.gpg
   ```

3. **设置Docker的APT仓库**：
   接下来，您需要添加Docker的APT仓库到您的系统中。不同于之前，现在您应该指定密钥环文件来验证仓库签名。
   ```bash
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. **更新软件包索引并安装Docker**：
   最后，更新APT索引并安装Docker：
   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

这些步骤遵循了新的安全实践，确保了您在添加外部仓库和密钥时的系统安全性。即使在收到`apt-key`已被废弃的警告后，您仍然可以通过这种方式安全地添加Docker的GPG密钥并继续安装。

*测试：*

1. **验证Docker安装**：
   安装完成后，您可以运行一个测试镜像来验证Docker是否正确安装。
   ```bash
   sudo docker run hello-world
   ```

   这个命令会从Docker Hub下载一个测试镜像，并在容器中运行。如果安装成功，它将在终端中打印一条欢迎消息。

2. **（可选）将当前用户添加到Docker组**：
   默认情况下，运行`docker`命令需要`sudo`权限。为了避免每次运行`docker`命令都需要输入`sudo`，可以将您的用户添加到`docker`组中。
   ```bash
   sudo usermod -aG docker $USER
   ```
   完成这一步后，您需要注销并重新登录，或者重启系统，以使这一改变生效。

这就是在Ubuntu系统中安装Docker环境的全部步骤。安装完成后，您就可以开始使用Docker来运行容器化的应用程序了。

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