解决无法访问GitHub

Mac版

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