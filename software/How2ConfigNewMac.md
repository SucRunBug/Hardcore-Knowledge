# How2ConfigNewMac

- 此条目记录Mac从头开始配置的重要流程

首先下载必要APP，此步骤较为简单，跳过记录。

## 同步GitHub/Gitee 的代码

此记录以GitHub为例，gitee操作大同小异。

### 下载git

进入终端，输入指令`git --version`，系统就会自动帮你下载git

### 配置git

执行如下命令，设置和GitHub账户同样的昵称和邮箱。

```bash
yanfeiyu@yanfeiyudeMacBook-Pro github % git config --global user.name "SucRunBug"
yanfeiyu@yanfeiyudeMacBook-Pro github % git config --global user.email "yyanfeiyu@163.com"
yanfeiyu@yanfeiyudeMacBook-Pro github % git config --global --list
user.name=SucRunBug
user.email=yyanfeiyu@163.com
```

### 在本地创建SSH Key

进入用户主目录，执行如下命令，注意需要输入和GitHub同样的邮箱。

```bash
ssh-keygen -t rsa -C "yyanfeiyu@163.com"
```

进入`.ssh`路径，用`cat`查看自己的公钥`id_rsa.pub`

### 在GitHub上绑定自己的公钥

进入GitHub个人页面，在左边栏找到「SSH and GPG keys」，选择「New SSH key」，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。

### 克隆远程仓库到本地

复制远程仓库ssh地址，使用git clone命令 + ssh 地址即可开始克隆。

以后不需要用移动硬盘备份了，直接克隆速度更快。

### 测试是否能同步代码

为仓库新增文件，提交测试。



## 安装Homebrew

参考B站UP主Mac云课堂推荐的文章https://zhuanlan.zhihu.com/p/111014448

执行命令

```bash
bash/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

一般选择源为中科大

卸载脚本

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```



## 美化终端

### 安装oh-my-zsh

- 通过GitHub安装

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

如果上述命令不成功，使用curl通过Gitee国内镜像安装oh-my-zsh

```bash
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
```

安装完成就可以命令提示和执行`ll`了

### 设置字体和风格

设置终端启动时为homebrew的风格，字体大小18就很合适了。

## 使用code命令启动vscode

启动vscode，shift+command+p打开命令面板，输入shell command安装code命令到环境变量中。

## Mac翻译不可用

目前系统版本：14.0

直接的原因是因为clashX，所以需要为其创建一个配置文件，用于修改代理规则。

在**~/.config/clash/**路径下创建一个**proxyIgnoreList.plist**文件，输入内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
	<string>192.168.0.0/16</string>
	<string>10.0.0.0/8</string>
	<string>172.16.0.0/12</string>
	<string>127.0.0.1</string>
	<string>localhost</string>
	<string>*.local</string>
        <string>*.crashlytics.com</string>
	<string>sequoia.apple.com</string>
	<string>seed-sequoia.siri.apple.com</string>
</array>
</plist>
```

最后需要在clashX Pro客户端中的Config中Reload Config

## clashX Pro白名单

[参考博客](https://penghh.fun/2023/01/30/2023-1-30-clashx/)，但找不到更多配置的选项，不知道是不是因为系统语言是英文的原因。

该软件开启增强模式的功能是让终端实现全局代理。

自定义.plist过滤网站，[参考博客](https://www.shiqidu.com/d/1023)

但是不清楚apple music的地址，但可以[参考博客](https://blog.butanediol.me/2020/05/07/提升国内-Apple-Music-体验的代理规则/)

我添加了apple music主页和播放的域名到屏蔽列表.plist中。

如果想知道访问的域名是什么，可以抓包。于是我下载了Proxyman，可以看到正在使用的程序的域名，如果谁卡就将谁加入白名单。

## VS Code运行和调试C++代码

使用`code .`是可以创建一个.vscode目录，里面有个文件`tasks.json`，然后给args字段添加上一个选项`-std=c++11`就可以运行C++11标准的代码了。

其余部分只需要跟着官网教程走就行了