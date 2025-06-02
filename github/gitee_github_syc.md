提交仓库，同时，到两个远程仓库中，

以GitHub老仓库，同步gitee新仓库为例

第零步，在gitee上建立已有的GitHub仓库，相当于复制过去/

第一步，修改.git目录中的config文件

参考视频：https://www.bilibili.com/video/BV1Dr4y187zx/下面的第一条评论

> 也可以不添加多个源，每个源其实可以有几个url地址，把origin下面的url再复制一行，改成目标url就行了。这样直接git push就能推上去，git pull 只会pull最前面的url

该url代表仓库的SSH链接

第二步，配置公钥和邮箱

然后在gitee上，配置好公钥，以及邮箱，邮箱需要和GitHub上绑定的一致。

第三步，推送测试