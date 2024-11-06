# Chatgpt-web-midjourney-proxy

## 引言

> 这是一站式AI项目网站的部署文档，使用docker镜像部署到云端服务器，需要付费购买API使用相关AI服务，但API的好处是用多少花多少钱，项目官方的文档中也推荐了实惠的中转API，比如拿Midjourney做一张图是2毛钱

### 目标

本文档让读者学会如何从GitHub获取开源项目Chatgpt-web-midjourney-proxy并部署云服务器上，不会对本地部署进行讲解，因为文档中关于本地部署的信息较少，所以也不建议本地部署。如果非要本地部署，也建议部署到docker上。

### 前提

本文档适用于擅长使用搜索引擎、了解基本的Linux命令、有过云服务器的使用经验（部署此项目推荐Ubuntu或Debian系统）的读者尝试部署。docker的使用非常简单，所以没接触过也没关系。

总之，不会你就多问，并为每一步执行过程记录日志。

## 前期准备

### 项目了解

 https://github.com/Dooy/chatgpt-web-midjourney-proxy/releases 有项目的最新版本(选择合适你操作系统的版本)

如果你不清楚你的操作系统请使用搜索引擎或询问大语言模型如GPT——如何查看自己的操作系统？

如果你打算本地部署，请确定好操作系统后，再通过安装包的名字来确定哪一个安装包更适合你的系统，不打算本地部署可跳到环境配置部分

<img src="/Users/yanfeiyu/Library/Application Support/typora-user-images/image-20241106095014372.png" alt="image-20241106095014372" style="zoom:50%;" />

以目前最新版本举例，前三个带有.AppImage的都是给Linux用的，只需要在终端执行

```sh
chmod +x 软件包名.AppImage # 这里是为其添加可执行权限
./软件包名.AppImage
```

当然，你会发现前三个也不太一样，因为后两个是压缩包，你需要使用tar命令先解压，具体操作请自行查询，不想解压请直接用第一个

amd64也代表软件包是为x86_64架构编译的

.deb后缀的是给Debian系列的Linux发行版用的，比如Ubuntu、Debian等

.dmg的就是给Mac用的，带universal标注，所以同时支持Intel版和M系列家族的Mac

如果使用Windows系统，请下载之后的.exe或.msi文件



### 环境配置

#### 开启端口

如果部署在云服务器上，需要在防火墙（有的服务器叫安全组）中开启相关端口，比如80、443、8080等。（这一步主要是熟悉开启端口的方式，后续需要开启多个端口，请一定要找对位置并能熟练开启端口）

#### 安装宝塔面板

接下来在Linux中安装好宝塔面板，这里可以参考这篇有六百多万阅读量的宝塔面板安装教程

https://www.bt.cn/bbs/thread-19376-1-1.html

找到对应系统的命令，在服务器终端内执行即可，如果你想进一步了解宝塔面板的功能，可以看看文章后面的内容。

> 你说的对，但宝塔面板是一款由宝塔网络科技有限公司开发的服务器管理软件，最早于2015年发布，经过多次更新和改进，现已支持Windows和Linux平台。该软件通过图形化界面简化了服务器环境的部署和管理，适用于网站搭建、数据库管理、安全防护等多种场景。在数据方面，它还支持站点、数据库和文件的一键备份与恢复，为用户提供了高效、安全的管理体验。

安装后，终端会显示宝塔面板的内外网地址

例如：

```sh
外网面板地址：http://114514:henhen:aaaa:9090/aaaa
内网面板地址：http://192.168.114.514:8080/henhen
username: 自动生成用户名
password: 自动生成密码
```

上面的内容记得要记住，终端还会让你**开启相应的端口号**

通过外网面板地址访问宝塔面板。

如果外网面板地址进不去，要切换为你的公网IP地址（在服务器管理面板中查询，每个人都不一样，这是你创建实例的时候给你的）

在生成的外网面板地址的基础上修改，最终是这样的，http://114.51.4.0721:端口号/字符串（公网ip+冒号+你刚刚开启的端口号+还可能有个/+一串字母）

你之前创建宝塔面板时，生成的用户名和密码，用在访问宝塔面板成功后登陆。

如果你忘了，可以直接拿命令查 sudo bt default（切权限会让你输你的root密码，密码不会回显，这密码是你创建服务器实例的时候设置的，每个人不一样）

#### 安装docker

进入宝塔面板，在左侧边栏找到docker，点击立即安装

安装完成后，顶部中间找到“本地镜像”，进入“线上镜像”，搜索Chatgpt-web-midjourney-proxy

注意开发者名字，所以开头是ydlhero的镜像才是我们要拉取的

找到后点击右侧的拉取，刷新后就可以看到多了一个东西，也就是这个项目被拉取到我们的服务器上了

#### 配置API

接下来是讲解官方文档中“docker部署”标题下的命令

```sh
docker run --name chatgpt-web-midjourney-proxy  -d -p 6015:3002 \
-e OPENAI_API_KEY=sk-xxxxx \
-e OPENAI_API_BASE_URL=https://api.openai.com  \
-e MJ_SERVER=https://your-mj-server:6013  \
-e MJ_API_SECRET=your-mj-api-secret  \
-e LUMA_SERVER=https://your-luma-server:8000  \
-e LUMA_KEY=your-luma-key  \
-e SUNO_SERVER=https://your-suno-server:8000  \
-e SUNO_KEY=you-suno-key  ydlhero/chatgpt-web-midjourney-proxy
```

别着急直接使用它，还需要配置好api的，这时候去中转API购买，我就说下名字，叫openai-hk，这是原项目作者推荐的，就不放链接了，因为不是广告，需要的自行搜索。

然后每一个地方只需填同样的API就行，比如你从中转API里买的密钥是114514henhenaaaa，还要修改网关接口，在你买API的接入说明里查看，那你的命令就该是：

```sh
docker run --name chatgpt-web-midjourney-proxy  -d -p 6015:3002 \
-e OPENAI_API_KEY=114514henhenaaaa \
-e OPENAI_API_BASE_URL=https://api.openai-hk.com  \ # 网关接口在这里换，下面也是换成一样的
-e MJ_SERVER=https://api.openai-hk.com/relax  \ # 这里是休闲模式，你可以改成fast，但会增加消耗
-e MJ_API_SECRET=114514henhenaaaa  \
-e LUMA_SERVER=https://api.openai-hk.com  \
-e LUMA_KEY=114514henhenaaaa  \
-e SUNO_SERVER=https://api.openai-hk.com  \
-e SUNO_KEY=114514henhenaaaa
-e AUTH_SECRET_KEY=1145140721 ydlhero/chatgpt-web-midjourney-proxy # 注意这里添加了一个密码，访问你的网站时就需要密码了，请自己设定一个并牢记
```

注意命令中-p选项后的端口号，**这里也要放开6015端口号**，3002是容器内部的端口号，你不用管，它们是映射关系。

#### 在docker中创建容器

配置好上述命令后，在顶栏稍左侧找到容器，选择创建容器，选择命令创建，复制并执行你的命令即可。

然后使用你的公网ip+冒号+6015，输入命令最后配置的密码，就可以进入了。

## 开始使用

和相关应用的在线网站一样，甚至有更多的功能，比如下载预设提示词，让GPT变成赛博猫娘。

你可以和GPT4聊天，使用Midjourney或Dall.E绘图，使用suno创作音乐

## 后续维护

### 反向代理

如果你只是打算自己用，可以跳过此步骤；如果要开放给别人用，就需要避免暴露自己的公网IP，以免被别人攻击。

首先申请一个域名（请自行寻找方法），并绑定到服务器上。

绑定过程：在宝塔面板的左边栏找到“网站”，添加你的域名。

点击绑定好的网站后找到反向代理，代理名称自己取，目标URL是项目的原始访问地址，也是你的公网IP加上端口号的网址，发送域名填服务器的公网IP，点击确定即可。

然后测试是否能够通过网站域名访问到你的服务。

### 配置防爆破验证

详情参考官方README，有关参数AUTH_SECRET_ERROR_TIME的说明

例如在容器部署命令中添加-e AUTH_SECRET_ERROR_TIME=1，即表示错误尝试后需要等待1分钟。

### 项目新版本更新

删除本地镜像和容器，重新拉取新镜像，并创建容器