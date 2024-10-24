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
