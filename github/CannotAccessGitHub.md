## 解决无法访问GitHub

### Mac版

参考https://www.jianshu.com/p/e491fa215fb2

1.新建一个访达窗口，同时按住shift command G三个键，进入前往文件夹页面

2.在输入框内输入/etc/hosts

3.找到hosts文件夹

4.由于hosts文件夹不可编辑，所以复制一份hosts文件先保存到本地桌面（这里我是直接拖到桌面）

5.在新的hosts文件夹里添加如下代码：

```
http://github.com 204.232.175.94 http://gist.github.com 107.21.116.220 
http://help.github.com 207.97.227.252 http://nodeload.github.com 199.27.76.130 
http://raw.github.com 107.22.3.110 http://status.github.com 204.232.175.78 
http://training.github.com 207.97.227.243 http://www.github.com
```

6.将/etc/hosts原来的文件删除，删除的时候需要输入你的电脑开机密码

7.再将修改后的保存到桌面的hosts文件拖拽到/etc文件夹下，期间有可能需要你输入开机密码，文件名最好是hosts

8.以上完成后，我们来到终端命令行ping 一下github

### Win

自从我上手了一台NUC后，想要试试不挂代理能否稳定上GitHub，可是难度出乎我的意料了，下面来列举我做过的尝试。

首先是下载了Steam++进行GitHub加速，能上，但如上，经常抽风，你需要卸载证书再安装一遍，总之非常不稳定，但这竟然是我尝试了这么多种方式里最靠谱的一种了。

然后去看了别的UP主的视频，说是网易UU加速器有个学术加速功能，但我加速后依旧没有任何效果，不过能免费连上sci-hub看看论文还是不错的。

之后便是经典的改host操作，有个GitHub520仓库教程非常详细且多样，我全部试了一遍，甚至都没法加载出GitHub的主页。https://github.com/521xueweihan/GitHub520

接下来便是寻找了一些奇怪的开源项目，比如FastGitHub，我还专门编译了项目代码，不知道操作对了没，最后还是没用。

去看了镜像站，kGithub，这镜像站估计倒闭了，搜索功能都寄了。

刚刚提到了520的仓库，我就顺手看了下issue，还是有不少大佬分享其它方法的，比如有用Xbox加速工具上GitHub的，还有直接用一行命令解决的。

首先聊聊Xbox加速工具，因为我下载的版本中，并没有国外服务器，用起来依旧不稳定，不过已经比steam++效果要好了，毕竟可以加载出图片和下载代码了。

不过最让我惊喜的是后面的方法，只需在终端执行下面的命令，就让我顺畅访问，简直和挂梯子没有任何区别

& "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --host-rules="MAP github.com octocaptcha.com, MAP github.githubassets.com yelp.com, MAP *.githubusercontent.com githubusercontent.com" --host-resolver-rules="MAP octocaptcha.com 20.27.177.113, MAP yelp.com 199.232.240.116, MAP githubusercontent.com 199.232.176.133"

下面是相关教程链接：

issue中提到了上述两种方法 https://github.com/521xueweihan/GitHub520/issues/237

Xbox加速器 https://github.com/skydevil88/XboxDownload?tab=readme-ov-file

一行命令的仓库 https://github.com/feng2208/github-hosts



## 原理

### 数据包传输原理

首先，从本地计算机发出一个数据包请求

经过你的本地网络接入互联网骨干网

经过DNS，即域名解析服务，将你输入的这一串字符解析为要连接的服务器的真实IP地址。

例如，当你输入github.com时，DNS会解析出该地址对应的真实服务器地址。

经过解析后，数据包到达国际出口，接入对应的服务器

服务器处理你的请求后返回一个数据包，再发送回你的计算机。

最终你就看到了GitHub的页面。

### GWF拦截原理

那究竟是什么在阻止我们上GitHub呢？

——防火长城GFW

有了GFW之后，当你的本地计算机发出一个数据包给GitHub时

经过本地网络接入骨干网，并经过DNS解析

现在，出现问题了

GFW探测到流量的部分内容，识别到你访问的是GitHub后，返回一个错误的服务器地址，因此你无法访问GitHub。

这就是GFW封锁互联网访问的一种手段：DNS污染。

当然，很多大神想了如何绕开GFW封锁的方法，但GFW也在不断进步，所以也没有一种完美无缺的翻墙方式。

### 我用过哪些热门的方法

Steam++

网易UU学术加速功能

[开源仓库GitHub520](https://github.com/521xueweihan/GitHub520)、[FastGithub](https://github.com/feng2208/github-hosts)

### 为什么热门方法会访问不了

**关于steam++**

- 解析加速（将连接GitHub的请求通过内置DNS解析到国内CDN，不直接走国外网络）
- 代理加速（有代理服务器中转，从代理服务器和GitHub服务器建立连接，减少被GFW影响的几率）

因为依赖第三方代理服务器，会不稳定。

**关于开源仓库改host的操作**

使用脚本定期检测 GitHub 的最新可用 IP，并将其更新到 hosts 文件里。这样在访问 GitHub 时，系统会优先使用这些 IP 地址，绕过不稳定的 DNS 解析路径。

但改host操作失效还是和sni阻断有关，**SNI 阻断不依赖于 IP 地址**，而是基于域名级别的拦截，只要 SNI 中仍然包含被阻断的域名（如 github.com），GFW 依然会中断连接。

### 我用的方法，以及为什么就能访问了

在Xbox加速器或steam++中添加DoH服务器

**DoH（DNS over HTTPS）** 将 DNS 查询封装在 HTTPS 请求中的技术，可以绕过DNS污染

用[这个仓库](https://github.com/feng2208/github-hosts)中提到的神秘命令，原理是通过重定向GitHub域名的解析，从而绕过网络限制
