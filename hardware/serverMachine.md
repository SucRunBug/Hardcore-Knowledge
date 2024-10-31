# Server Machine

磁盘管理工具：linux lvm

服务器系统：pve 管理多个虚拟机节点

防爆破，复杂密钥与权限管理

低功耗：C6 参考什么是 C-State？ | Dell 中国](https://www.dell.com/support/kbdoc/zh-cn/000060621/什么-是-c-state)



2024.10.31

## 我们可以部署什么项目？

在线影院，可以随时随地看存储在服务器上的视频，或者采集其它站点的资源

AIGC服务，让你的手机也能AI作图（SD），也能有私人的GPT问答助手（LLM）

搭建一个个人网站，包装一下甚至可以做毕设，比如XX管理系统

部署Docker镜像，使用一些微服务工具，比如word转PDF功能

还有一个awesome-selfhosted开源仓库，上面也有很多项目

再参考一下其他视频

## 项目原理介绍

### 在线影院项目

关于在线影院项目部署：参考BV12Y43ePE4R

安装宝塔面板，教程https://www.bt.cn/bbs/thread-19376-1-1.html

登陆宝塔后安装基础套件

安装CMS后台，爱影CMS，主页http://iycms.com/index.html

内容管理系统（CMS，Content Management System），比如WordPress就是

使用安装脚本在服务器终端中安装，然后开放端口访问，进行相关初始化

新建站点，采集其他资源站的影视资源，相关教程https://www.heimuer.tv/help/index.html#IyCms

最后新增播放器

### 一站式AI项目Docker镜像部署

支持ChatGPT文本对话、Midjourney图像生成、suno音乐生成

参考教程：BV1xJ4m1u7r2

开源项目网址：https://github.com/Dooy/chatgpt-web-midjourney-proxy

可支持无服务器本地部署

推荐的远程连接服务器的工具WindTerm：https://github.com/kingToolbox/WindTerm，支持三种系统

还是按照上面提到的宝塔面板教程安装，宝塔面板可以管理你的云服务器，让安装应用更便捷

进入宝塔面板中安装docker，搜索线上镜像chatgpt-web-midjourney-proxy，拉取镜像到本地（镜像相当于一个软件安装包）

之后需要配置API，推荐使用中转API，网址https://openai-hk.com/open/index

它集成了很多AIGC功能的API，包括大语言模型、AI绘画和音乐生成

最后执行配置好API的命令即可

### 在线博客网站

使用Wordpress一键式部署

开源项目：https://github.com/linhaojun857/aurora（4k star）

### Awesome-Selfhosted开源仓库

超多服务器项目等你发掘——游戏、邮件服务器，无数软件



## 服务器选择

亚马逊云有基于Arm架构的Graviton，宣称比X86架构的更便宜、性能更高

不过注册方式十分蛋疼，尽量选择外服版本

如果你已经在X86平台上搭建了一些项目了，迁移到Graviton上能省一半钱，宣称在同等性能下减少60%能源使用

适用于上面提到的所有项目，特别是针对内存敏感的场景，有优秀的表现，比如多人并发访问自己的网站时；同时对媒体编码也有特殊优化，再也不怕自己的小电影网站出现花屏乱码的场景了。