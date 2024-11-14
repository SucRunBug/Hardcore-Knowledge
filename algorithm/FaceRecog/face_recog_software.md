# 人脸识别门禁系统

> 写在前面，括号内批注为此条目作者的批注，其余为课程内容，考虑到口误的可能，记录下的课程内容可能与讲师表达内容有些许偏差

## 系统演示

登录系统

小区管理：添加小区，设置经纬度（这个用百度地图获取），选中小区后可以管理摄像头，添加摄像头（里面有设置IP地址，这里没有提怎么确定IP），IP要唯一

小区地图：根据经纬度在地图上标注出小区图标，右上角有地图放大缩小工具，左上角可以切换地图

居民管理：对居民增删查改，将居民数据导入导出（可节约数据录入时间），导入时仅对小区名称做了判断，如果小区不存在就都不进去，是Excel表格。导入时可以下载Excel模板，以免不能导入。选择居民后，点击人脸采集，可以拍照或者上传人脸（注意！此界面有水印和不相干的文字内容）

人脸识别：检测上述操作中添加的人脸（该界面中也有水印），上传第一次是进小区，第二次是出小区

出入记录：展示界面是按照小区进行排序的，可以查看上一步操作是否成功执行并记录

访客登记：对临时人员的登记，点击新增后填写相关信息，包括增删查改

左上角有个首页，做了居民统计表，对每个小区的男女做统计

系统管理：

- 菜单管理

有三种菜单类型：目录、菜单、按钮。

目录即左边栏，路由地址随意填写，可以自定义菜单图标，也可以隐藏某个功能，比如摄像头管理，是藏在小区管理中的，这是为了选择小区后才能管理摄像头，使用更方便

菜单是目录下级，点击目录后出现的，路由地址不可重复，可随意取名。要注意下面的组件路径，是从sys路径下开始的（视频中演示的路径是src\views\sys），组件路径就是点这个菜单时，展示哪个页面，所以很重要。权限标识是代码中控制权限的地方？

按钮便是菜单的下级，比如小区管理下的新增，只有菜单名称和权限标识

- 角色管理

用于权限设置，添加角色时，设置系统角色或单位角色（普通角色），然后为其授权。这个账号就可以登录到这个系统后台，授权的种类决定他能看到哪些目录和菜单

- 日志管理

系统做的任何一个操作，都要记录下来，这就是日志

### 总结

1、管理员管理：根据不同角色设置不同的管理权限；

2、小区管理，管理多个小区资料，新增、修改、删除、摄像头管理等功能；

3、小区摄像头管理：摄像头的新增、修改及删除功能；

4、居民管理：居民资料新增，修改，删除，Excel批量导入，导出，居民人脸采集；

5、访客登记：访客的新增，修改，删除，进入登记，离开登记，查询等功能；

6、人脸识别：居民出入小区人脸识别功能的实现，使用腾讯AI人脸识别技术实现；

7、出入记录：居民出入小区的人脸识别记录查询；

8、小区地图：所有小区在地图的分布情况，使用百度地图实现；

9、使用Echarts技术实现小区人员分类统计（柱状）图表；

10、菜单管理：新增、修改、删除菜单功能（包括目录，菜单，按钮）

11、角色管理：新增、修改、删除角色（系统角色、普通角色）

12、系统日志：记录了系统中所有操作的日志，方便发现问题，查找原因；

## 技术栈

前后端分离

运行环境：

  1、JDK1.8及以上版本

  2、Tomcat 8.5及以上版本

  3、MySql 5.7及以上版本

  4、Redis

开发工具：

  1、前端开发工具：Visual Studio Code

  2、后端开发工具：Intellij IDEA

使用技术：

  1、前端：Vue2.x（JavaScript框架，做用户界面的）+ElementUI（UI组件是它做的，比如按钮）

  2、后端：Springboot（开发框架，简化Java应用开发）+MyBatisPlus（执行sql的）+Redis（内存级数据库，给前后端通信数据做缓存）+Shiro（负责权限）+Swagger（做API文档给前端看的）

  3、人脸识别技术（腾讯AI）

  4、MySql数据库技术

  5、Redis缓存技术

  6、百度地图

  7、Echarts图表技术

  8、POI Excel导入导出技术

  9、Shiro权限控制：菜单管理，角色管理，权限管理（按钮及用户级别权限）

  10、 Swagger接口配置管理，接口文档管理技术

  11、Token单点技术（一个用户不能同时在多个设备登录使用）

  12、前后端分离跨域设置等技术

![image-20241108152104971](/Users/yanfeiyu/Library/Application Support/typora-user-images/image-20241108152104971.png)

<img src="/Users/yanfeiyu/Library/Application Support/typora-user-images/image-20241108152213630.png" alt="image-20241108152213630" style="zoom:50%;" />

## 腾讯人脸识别申请

链接：https://cloud.tencent.com/product/facerecognition

上边栏的“产品“-》人工智能与机器学习-》人脸识别

登录后，进入页面中间的“产品控制台“，在左边栏的“资源包管理”可以看到你申请的功能，有10000次免费使用

接下来是创建密钥，点击左上角控制台，搜索访问密钥，进入后，点击继续

这时候你应该在左侧看到“API密钥管理”的字样

然后点击“新建密钥”

从而获取相关的secretId和secretKey，注意不要泄露，并且保存下来。

放置的位置：后端代码中src/main/resources/application.yml的44和45行

46行是接口IP，固定的，47行是区域，48是对应人员库名称

关于人员库名称：进入人脸识别控制台，左边栏找到人员库管理，人员管理，点击新建人员库，名称即为48行对应内容（不区分大小写？）

49是人员ID前缀，随便写，50随机字符串随便写（含水印，已改动），52是人脸比对匹配率，80代表80%就合格

人脸识别-》人员库管理-》新建人员组，这里获取人员组名称就是groupId

其他的可以参考腾讯云右上角的API文档

关于API的后端接口位于FaceApi2文件中

## 百度地图开放平台

链接：https://lbsyun.baidu.com/

也是免费的

注册后进入控制台，应用管理-》我的应用-》创建应用-》选择浏览器端

白名单？不部署到云服务器上就填*

最重要：应用AK

此代码文件main.js中61行（位于主目录下的src文件夹内），import BaiduMap from 'vue-baidu-map'，这个需要提前下载

参考教程：https://www.cnblogs.com/WorkingBoy/p/15424905.html

命令如下：npm install vue-baidu-map

admin/src/views/sys/map/index.vue 是使用百度地图的代码

如果有改动，参考API文档

还有拾取坐标系统，可以获取某个地方的经纬度

进入百度地图开放平台——顶栏的开发者频道——开发者工具——坐标拾取器

https://api.map.baidu.com/lbsapi/getpoint/index.html

点击想要的地点后，能在右上角复制到相应的坐标，前面是经度，后面是纬度

## 项目的配置和运行

项目前端代码位于admin文件夹内

（admin/node_modules是项目的依赖库）

安装Node.js，要14或12版本（运行JavaScript的环境）（当前版本过高v22.9.0，还未降级）

安装Vue脚手架（Vue CLI 命令行界面，快速搭建Vue项目）（还未安装）

后端代码位于door文件夹内，使用IDEA打开，配置好Maven（项目管理和构建工具，如果你已经有一个包含 pom.xml 文件的项目，直接在 IDEA 中选择 Open，然后选择项目根目录下的 pom.xml 文件。）

villegePic/face和face里放的是测试用的面部图像

doc/door.sql是数据库的脚本和测试数据，安装Navicat，使用这个脚本创建数据库，设计MySQL和Redis

数据库名叫door，有11张表，camerainfo（摄像头）, community（小区）, inoutrecord（进出记录）, manualrecord（访客登记）, personinfo（居民信息）, sys_log（日志）, sys_menu（菜单）, sys_role（角色-超级管理员、普通角色）, sys_role_menu（权限）, sys_user（系统登录用户）, sys_user_role（登录用户所属角色）

MySQL版本不低于5.7

前端运行：Node.js 14.x版本或12.x版本，建议不要使用最新版本（所以我需要降级）

在前端代码的admin/package.json中可以查看版本（该文件中含有水印）

main.js中主要是3行Cookies, 22-echarts, 27-BaiduMap-61ak

前端接口地址.env.development的2，这是在本地回环上8888端口运行，和后端application.yml的37一致

点击debug开始运行后端项目

（macOS报错信息：
构建进程终止异常: 
nice -n 10 /opt/homebrew/Cellar/openjdk/21.0.3/libexec/openjdk.jdk/Contents/Home/bin/java -Xmx700m -Djava.awt.headless=true "-Djna.boot.library.path=/Applications/IntelliJ IDEA.app/Contents/lib/jna/aarch64" -Djna.nosys=true -Djna.noclasspath=true --add-opens jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.code=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.comp=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.main=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.model=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.processing=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.jvm=ALL-UNNAMED -Dexternal.project.config=/Users/yanfeiyu/Library/Caches/JetBrains/IntelliJIdea2024.2/projects/door.1d96925f/external_build_system -Dcompile.parallel=false -Drebuild.on.dependency.change=true -Didea.IntToIntBtree.page.size=32768 -Djdt.compiler.useSingleThread=true -Daether.connector.resumeDownloads=false -Dio.netty.initialSeedUniquifier=3033586465212712129 -Dfile.encoding=UTF-8 -Duser.language=zh -Duser.country=CN -Didea.paths.selector=IntelliJIdea2024.2 "-Djps.language.bundle=/Applications/IntelliJ IDEA.app/Contents/plugins/localization-zh/lib/localization-zh.jar" "-Didea.home.path=/Applications/IntelliJ IDEA.app/Contents" "-Didea.config.path=/Users/yanfeiyu/Library/Application Support/JetBrains/IntelliJIdea2024.2" "-Didea.plugins.path=/Users/yanfeiyu/Library/Application Support/JetBrains/IntelliJIdea2024.2/plugins" -Djps.log.dir=/Users/yanfeiyu/Library/Logs/JetBrains/IntelliJIdea2024.2/build-log "-Djps.fallback.jdk.home=/Applications/IntelliJ IDEA.app/Contents/jbr/Contents/Home" -Djps.fallback.jdk.version=21.0.4 -Dio.netty.noUnsafe=true -Djava.io.tmpdir=/Users/yanfeiyu/Library/Caches/JetBrains/IntelliJIdea2024.2/compile-server/door_573e4c75/_temp_ -Djps.backward.ref.index.builder=true -Djps.backward.ref.index.builder.fs.case.sensitive=false -Djps.track.ap.dependencies=false "-Djps.kotlin.home=/Applications/IntelliJ IDEA.app/Contents/plugins/Kotlin/kotlinc" -Dkotlin.incremental.compilation=true -Dkotlin.incremental.compilation.js=true -Dkotlin.daemon.enabled -Dkotlin.daemon.client.alive.path=\"/var/folders/4n/sd4ck2wn61bdn23b4cclcn440000gn/T/kotlin-idea-7912806462901765990-is-running\" -Dide.propagate.context=false -classpath "/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jps-launcher.jar" org.jetbrains.jps.cmdline.Launcher "/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jps-builders.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jps-builders-6.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jps-javac-extension.jar:/Applications/IntelliJ IDEA.app/Contents/lib/util-8.jar:/Applications/IntelliJ IDEA.app/Contents/lib/util_rt.jar:/Applications/IntelliJ IDEA.app/Contents/lib/platform-loader.jar:/Applications/IntelliJ IDEA.app/Contents/lib/annotations.jar:/Applications/IntelliJ IDEA.app/Contents/lib/trove.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jetbrains.kotlinx.metadata.jvm.jar:/Applications/IntelliJ IDEA.app/Contents/lib/protobuf.jar:/Applications/IntelliJ IDEA.app/Contents/lib/jps-model.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/javac2.jar:/Applications/IntelliJ IDEA.app/Contents/lib/forms_rt.jar:/Applications/IntelliJ IDEA.app/Contents/lib/util.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/aether-dependency-resolver.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/maven-resolver-connector-basic.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/maven-resolver-transport-file.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/maven-resolver-transport-http.jar:/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/JavaEE/lib/jasper-v2-rt.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/Kotlin/lib/jps/kotlin-jps-plugin.jar:/Applications/IntelliJ IDEA.app/Contents/lib/util-8.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/java/lib/jps/java-compiler-charts-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/eclipse/lib/eclipse-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/eclipse/lib/eclipse-common.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/javaFX/lib/javaFX-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/javaFX/lib/javaFX-common.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/JavaEE/lib/jps/javaee-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/uiDesigner/lib/jps/java-guiForms-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/platform-langInjection/lib/java-langInjection-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/Groovy/lib/groovy-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/Groovy/lib/groovy-constants-rt.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/maven/lib/maven-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/gradle-java/lib/gradle-jps.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/JPA/lib/jps/javaee-jpa-jps.jar" org.jetbrains.jps.cmdline.BuildMain 127.0.0.1 49974 5f31cd0b-0103-491c-8373-9bbb908b7ee9 /Users/yanfeiyu/Library/Caches/JetBrains/IntelliJIdea2024.2/compile-server
nice: /opt/homebrew/Cellar/openjdk/21.0.3/libexec/openjdk.jdk/Contents/Home/bin/java: No such file or directory
）

（解决方案：安装zulu-8，并卸载21版本，然后在项目和Maven中指定zulu-8，参考https://chatgpt.com/share/6734221d-e94c-8000-a151-bcae1938b7ae）

（接下来是数据库连接报错了，因为还没配置数据库，我继续看视频了）

vue.config.js中11行也有个端口，但这不是前端项目的端口（是Vue的端口？）

（我尝试安装了MySQL，按照一个B站视频上操作https://www.bilibili.com/video/BV1jQ4y1G7xP，但发现端口不是默认的3306，代码里还没找，方案1是改代码，但我不太想破坏原始代码，方案2就是改数据库端口到3308，Mac端修改不太方便，另外我还安装了一些工具比如vue和nodejs，另外我还发现后端的application.yml中有文件存储路径，比如32、35、36行，这个也需要修改）

前端文件.env.development是开发环境的接口地址，即本机地址，另外一个是.env.production，这是打包以后的程序放在服务器上的接口，如果不放在服务器上，那就改成本机地址

正常运行后端项目后，会有四个地址，分别是本机地址，网络地址，swagger做的接口文档地址，用于给前端开发查看，最后一个地址没说

运行前端，在admin路径下 npm run dev

在door路径下，WebMvcConfig.java文件中，最后有重要的路径，和application.yml的upload字段里的一致，不能说完全一致，后缀能匹配上



## 项目结构

我看都是用的IDEA开的后端代码，可能还需要介绍软件下载方式

我安装了破解的版本，使用的是2024一月的版本，最新版本不支持破解，比较简单

破解方法是一个个人网站上的。放在收藏夹栏中了。

但是默认字体很难看，我后面打算修改一下字体

讲到后端中sys文件夹里的Java文件时，项目和视频中演示的文件有出入，比如没有AgentInfoMapper

要记住的是一个表对应一个java bin对应一个service对应一个service的实现

## mybatis-plus

mysql端口，项目中开的是3308，用户root，密码1234（我是12345678）

第五、六、七较为重要

## 小区管理功能

使用数据库的软件是Navicat Premium

百度地图-坐标拾取器

## 摄像头

前后端都是放好了相关的代码文件

## 小区地图

和小区管理有关，比如经纬度

地图一出现就有确定位置，是设置了中心位置

## 居民管理

### 应用层

实现模块：居民增删差改的功能

居民表：列名有很多，都是居民的基本信息

对居民表进行添加、编辑、人脸采集、删除、导入、导出的功能

支持搜索功能

导入时对小区名称做了限制，修改了小区名再导入，比如添加一个1，会自动删除掉1

采集人脸相片时，提示框内有水印和不合场景的提示

人脸采集上传照片后能识别但不加载图片，这是因为数据库中，图片路径没有拼接正确，视频中展现的是少了door，在后端的application.yml的37行urlPredix位置，在路径最后添加了door

原始代码中也没有door，视频中展示的是直接在IDEA中运行后端，写到数据库里的路径中没有项目名

然后打开了Navicat（也叫Tomcat？不是的，这俩完全不是一个东西），然后在door数据库personinfo表内，可以直接用faceUrl去浏览器测试是否上传成功

### 后端代码

后端代码文件涉及：PersonInfoController, PersonInfoVo（放居民的基本信息）, PersonInfoMapper, PersonInfoService, PersonInfoServiceImpl

在讲PersonInfoVo时，再次提及到@TableField(exist=false)，它是给小区名称用的，因为表中没有这个字段，防止报错才加的。

然后跳到PersonInfoMapper中的22行，指出是personInfo和community这两个表进行关联（它获取personInfo表中所有列的内容以及community表中的communityName列，并根据两表的communityId进行匹配）

35行的函数中传入了Page对象，分页查询导入导出的区别是有没有这个page对象，没有就是查所有，有就是分页（但分页查询一般是有limit字段，我搜不到他用了limit。分页查询就是防止查询结果过多所以做成了一页一页的查询结果，不知道视频里是不是这个意思）

56行便是查询所有小区，之后便是小区有关的其他操作，均有注释

PersonInfoService对应的是接口（类似于C++中.h文件，只有函数的声明）

PersonInfoServiceImpl则是接口的实现（类似于不含有main函数的.cpp文件

PersonInfoController是控制器，有导出的路径和前缀，54-65行是分页【personService.getPageList(params)，其中params可能带有分页参数，从而实现分页】，其余都有中文标注或注释

### 前端代码

person.js：放对应的接口，比如居民的增删差改，对应调用后端的接口

（这里看.vue文件安装了vue的VS插件）

/person/index.vue: 相关页面组件，比如4-13行三个查询条件，15行有绑定listQuery对象，98行的carNo是没用的，可以删除，85-90行是对应的对象，分页查询107，多选106，初始化105，初始化是查询分页列表127，搜索功能中的查询小区列表141，查询结果绑定到代码文件最上面的下拉框？（有个communityId，对应的一个是7-10），155是实现的多选，159是实现添加和编辑，175人脸采集（这章不讲），206和213分别是导入导出，222是删除

/person/add-or-update.vue: 都是常见组件，84行有居民性质，被绑定到47行的下拉框里，53行是备注的下拉框，66行是导入相关增删查改函数？91-104是合法性验证，116是初始化，143是表单提交，175是获得小区列表，189是关闭当前页面

进度——3:26:59