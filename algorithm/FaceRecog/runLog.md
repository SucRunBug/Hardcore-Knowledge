## 前端

下载npm是14.x的版本

在admin路径下执行npm run dev

如果是有关sass的报错，就执行命令 npm rebuild node-sass

## 后端

安装MySQL，测试用的均为8.x版本，Windows有具体的文档，Mac版可以参考之前提到的B站视频

使用IDEA打开door文件夹，修改Project Structure中Project条目下的SDK为1.8，如果首次使用的话，可以选择下载JDK，选择版本1.8，其他的随意。

在IDEA最右侧倒数第二个图标，配置数据库，点击加号选择Data Source（数据源），选择MySQL，输入用户和密码，如果首次使用需要安装驱动，在页面中会有指引的，结束后点击Test Connection测试连接是否成功，然后右键新创建好的@localhost选择New再选择Schema（架构），取名为door，然后右键它选择SQL Scripts，再选择Run SQL Script，这里选择doc文件夹下的door.sql，就可以初始化好数据库了。

再安装Redis，Windows上我是在GitHub上搜的，官网下载的是Linux版的，然后双击下载好的redis-server.exe

macOS使用homebrew安装，然后在终端执行redis-server

然后运行后端代码，打开前端代码链接即可。
