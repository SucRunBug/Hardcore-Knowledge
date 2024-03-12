此条目记录对Mac终端的优化升级过程

主要参考的文章：

https://juejin.cn/post/6994453537690222599

https://israynotarray.com/other/20200123/1105303313/

目前我使用的是iterm2软件代替Mac自带的终端软件，可以设置自定义的颜色主题，会有命令提示和自动补全命令的功能，目录会有区别于文件的特殊颜色，可以设置小组件以显示计算机硬件状态（好像不太准确而且不能显示GPU负载），新增一些Linux有但macOS没有的命令，例如`ll`

目前我还需要一个不错的vim编辑器配置，不过这东西还挺复杂的，需要安装各种插件，至少在Ubuntu里我尝试了多次，都无功而返。



在看繁体字的那篇教程时，安装好了一个oh-my-zsh的插件后，每次进终端都有个警告，按照作者的方法不起作用，于是我看了引用的链接里的一个方案，能成功解决：

```bash
compaudit | xargs chown -R "$(whoami)"
compaudit | xargs chmod go-w
```



后日谈

我重装了Mac后再次需要配置此工具，但是我无法使用curl执行位于GitHub上的远程仓库，但我想起了有个GitHub代理网站，叫做GitHub proxy，我最终的命令为：`sh -c "$(curl -fsSL -O https://ghproxy.com/https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

详细的使用方式可以参考https://ghproxy.com

但是我好像还是没有成功安装好oh-my-zsh，因为我在根目录里找不到.oh-mu-zsh的文件夹。

应该是教程的锅，他给curl后面加了个-O，去掉就行了。

然后还是搞了好半天，最终也是成了，太玄学了。

我发现装好这一个就能大大改善终端的实用性了，不需要什么iterm2，总之我就需要路径和文件的高亮，常用命令补全。
