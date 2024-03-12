*注：本文参考https://www.liaoxuefeng.com/wiki/896043488029600*

*原文非常值得一读，作者学识渊博，补充了很多有意思的知识。我仅仅是拾人牙慧。*



**git是最先进的分布式版本控制系统。**



**版本控制系统**——自动记录系统中文件的改动情况，还可以多人协同编辑。



# 一、git诞生历史及背景

git是Linux创始人Linus用C语言编写的，起初用于管理Linux社区提交的代码，后来GitHub网站上线后，为开源软件提供Git存储。



## 1.1 分布式vs集中式

版本控制器有分布式和集中式，集中式是使用一台中央服务器为其他用户进行版本控制，需要联网所以上传速度慢，如果服务器挂了，所有人都没法工作了。分布式是每个用户都有一个完整的版本库，无需联网，多人协同时仅需共享各自修改的部分。但分布式通常也有一台充当中央服务器的电脑。



# 二、git安装教程

*参考文章：https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496*

## 2.1 git配置

```sh
(base) yfy@yfydeMacBook-Pro ~ % git config --global user.name "yfy"
(base) yfy@yfydeMacBook-Pro ~ % git config --global user.email "yyanfeiyu@163.com"
```

查看git配置

```sh
git config --global --list
```



## 2.2 创建版本库

版本库也叫仓库，git会跟踪仓库里的所有文件的修改情况，以便追踪历史或还原。

```sh
(base) yfy@yfydeMacBook-Pro CPP % mkdir learngit
(base) yfy@yfydeMacBook-Pro CPP % cd learngit 
(base) yfy@yfydeMacBook-Pro learngit % git init
Initialized empty Git repository in /Users/yfy/Desktop/CPP/learngit/.git/
(base) yfy@yfydeMacBook-Pro learngit % ls -ah
.	..	.git
```

其中.git目录就是git用来跟踪管理版本库的。



## 2.3 将文件添加到版本库以及其它git命令

*重要提示：所有的版本控制系统，只能跟踪文本文件的改动，无法跟踪二进制文件如图片和视频（包括word）。*

*使用Windows系统不能使用自带的记事本编辑任何文本文件，建议使用vscode编辑。*



- 编写一个txt文件，一定要放在版本库目录下。

- 第一步，用命令git add 告诉Git，把文件添加到仓库。

- 第二部，用命令git commit告诉Git，把文件提交到仓库。-m后的是本次提交的说明。

```sh
(base) yfy@yfydeMacBook-Pro learngit % git add readme.txt 
(base) yfy@yfydeMacBook-Pro learngit % git commit -m"wrote a readme file"
[main (root-commit) 9b82f95] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

如果有**多个文件**需要add：

```sh
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

*笔者个人更偏好使用git add . 进行多个文件的提交*



- 如果文件被修改了，可以使用git status查看状态

```sh
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- 如果想知道哪里被修改了，使用git diff查看

```sh
diff --git a/readme.txt b/readme.txt
index d8036c1..15f01b8 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,3 @@
 Git is a version control system.
-Git is free software.
\ No newline at end of file
+Git is free software.
+Git is easy.
```

- 查看历史记录git log

```sh
commit 3d3d739bf5478af94698d35533dbb2ef8ba10bce (HEAD -> main)
Author: yfy <yyanfeiyu@163.com>
Date:   Wed Feb 1 13:45:52 2023 +0800

    add distributed

commit 9b82f959387583938afebdbcfb091b463e5e803f
Author: yfy <yyanfeiyu@163.com>
Date:   Tue Jan 24 22:01:02 2023 +0800

    wrote a readme file
```



commit后的一长串数字是版本号，用于区分多个人在同一个版本库中的提交信息。

- 查看版本号可以使用git reflog

- 回退一个版本git reset --hard HEAD^

其中HEAD表示当前版本，一个^代表一个版本，回退两个就需要写2个^，回退太多的话，比如回退100个版本，可以是git reset --hard HEAD~100

- 撤销回退git reset --hard 目的版本号





# 三、工作区和暂存区操作

工作区就是创建的learngit目录，其中有一个隐藏目录.git，是Git的版本库，其中有 被称为stage(或index)的暂存区，还有分支master和指向分支的HEAD。

git add是把文件修改添加到暂存区，git commit是将暂存区的所有内容提交到当前分支。



## 3.1 撤销修改

如果文件修改后还未git add，则可以使用git checkout -- 文件名，丢弃工作区的修改。如果已经git add了，则可使用git reset HEAD 文件名，重新放回工作区。（以上命令可以通过git status获得提示），如果已经提交到分支了，就需要进行版本回退了。



## 3.2 删除文件

如果将已经提交到版本库中的文件，用rm命令直接删除，是不能完全删除的，需要用到git rm删除，然后使用git commit命令进行提交。

如果文件已经提交到了版本库，就不用担心误删除，都是可以恢复的。



# 四、远程仓库

我们可以使用git，把代码存储到远程服务器上，例如GitHub网站，这样就不用担心硬盘不足了。但注意git只能管理文本文件。



## 4.1 如何上传代码到GitHub

- 第0步

注意，如果不愿意花额外的费用进行科学上网，建议使用Gitee进行代码托管。如果不介意，就使用邮箱注册GitHub

- **第1步 创建SSH Key**

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key

```sh
ssh-keygen -t rsa -C "youremail@example.com"
```

然后一路回车即可。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

- **第2步 在GitHub上绑定公钥**

进入GitHub个人页面，在左边栏找到「SSH and GPG keys」，选择「New SSH key」，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。

简单讲一下非对称加密算法，你有一个还没上锁的盒子，但锁已经挂在上面了，只需要摁一下就可以上锁，用这个盒子让别人存放东西，他只需要摁一下锁，别人就没法打开，只有盒子所有者有钥匙打开。这个摁一下的动作就相当于公钥，盒子所有者的钥匙相当于私钥。



- **第三步 添加远程库**

我们需要在GitHub上也创建一个git仓库，好与本地仓库同步，可作为备份抑或是多人协作。

注意：参考文章在此处已经不具有操作价值，不过还是有阅读价值。由于GitHub已经不让使用账户密码进行推送代码了，需要使用个人访问令牌。参考https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token



- **第四步 创建个人访问令牌**

进入GitHub，点击头像，找到「setting」选项，进入后在左边栏找到「Developer settings」选项，然后是「Personal access tokens」选项栏中的「token(classic)」。在其中找到「Generate new token」，然后填写相关信息，为了方便可以选择永久使用该token，多选项可以全选。最后会出来一串字符，记得保存下来。



然后在命令行中执行

```sh
git remote set-url origin https://<your_token>@github.com/<USERNAME>/<REPO>.git
```

<your_token>替换为最后出现的字符串，然后后面的部分是仓库的网址。

然后可以执行git remote -v检查一下自己关联的地址是否正确。



*笔者在这里被频繁掉线的梯子和奇葩的git命令报错折磨疯了，所以介绍Gitee了。*



## 4.2 如何上传代码至Gitee

我参考的还是廖老师的文章，和GitHub大同小异。配置好公钥后，上传不再需要使用密码了。

因为之前我关联了GitHub上的，所以还需要git remote rm origin删除一下。

然后我在准备提交的出现了一些不同的报错信息，通过bing检索找到了解决方案。

最终提交的手法可以总结为git三板斧：add+commit+push

也就是提交到了版本库后，执行git push就上传到远程仓库了。



*顺便记录一下我发现在终端里输入命令code .可以直接让vscode打开当前所在文件夹，非常方便。*



## 4.3 从远程仓库克隆

上面的操作部分是介绍了从本地上传至远端仓库，接下来介绍从远端仓库同步到本地，或者叫克隆远端仓库到本地。

只需要找到远程仓库的SSH链接，然后在本地使用git clone + SSH链接即可。当然，HTTPS链接也可以，但缺点是慢。

这个命令只适合于从头开始拷贝一份完整的项目代码，但是如果是想要本地多设备同步，并仅限于自己的分支，修改后使用git pull同步。

比如我的PC修改提交后，想在Mac上继续工作，就需要在Mac上执行git pull先将PC修改的部分同步到Mac。



# 五、分支管理

## 5.1 如何理解分支呢？

假设你需要两周才能完成一个项目，第一周你写了一半的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，你可以创建了一个属于自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上。

git上的分支最大的优点就是快。



*我们各个时间的版本库就像一串链表，分支其实就是指向某个节点的指针，分支的合并就是切换指针的指向。*

## 5.2 分支命令

创建并切换到dev分支：

git checkout -b dev或者git switch -c dev



这相当于两条命令：

git branch dev     

git checkout dev或者git switch dev



git branch命令可以查看所有的分支，当前所在分支前面会有*



此时在dev分支上，进行修改并提交，再切换到主分支上，是看不到dev分支做的修改，需要将dev分支合并到主分支上，使用git merge dev，合并完成后就可以删除dev分支了，使用git branch -d dev



分支汇总命令：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`





# 六、冲突

*两个设备能通过git上传或拉取远端代码，只有一个主分支，两个设备分别对远端代码进行不同的修改，会造成什么后果。*



如果两个设备对同一个文件的同一行代码进行了不同的修改，并且两个修改都被提交到了远程仓库中的同一个分支，那么这种情况就会导致代码冲突（conflict）。

当你在本地拉取（pull）远程代码时，你会收到一个类似于“冲突”的错误提示。这时你需要手动解决这些代码冲突，即在两个版本之间进行选择或合并。

如果你没有解决这些冲突，而是直接提交了代码，那么就会产生不可预知的后果，如程序崩溃、数据丢失等等。

因此，建议在多人协作开发中，尽量避免同时对同一文件的同一行代码进行修改。如果无法避免，建议采用分支（branch）的方式进行开发，以减少代码冲突的发生。



*如果我不想使用branch的方式避免冲突，只想设备a修改好，然后提交到远程仓库，需要轮到另一台设备b修改时，再pull下来，往复循环，怎么提醒自己在切换设备时马上pull*



你可以设置一个提醒或者定期检查远程仓库，以确保你的代码库是最新的。

设置提醒的方法可以有很多种，例如：

1. 使用邮件提醒：可以设置一个定时任务，每隔一段时间检查一下远程仓库的代码更新情况，并通过邮件提醒你进行 pull 操作。
2. 使用 Git 钩子（Git Hook）：在 Git 中有一种叫做钩子的机制，可以在 Git 执行一些操作之前或之后，自动触发一些脚本。你可以编写一个 Git 钩子脚本，在每次提交代码之前，检查远程仓库是否有新的代码更新，如果有则提示进行 pull 操作。
3. 使用 Git 插件：有些 Git 的客户端工具或者编辑器集成了 Git 插件，可以自动检查远程仓库的更新情况，并在代码库过期时提示你进行 pull 操作。



*那我能不能在同一分支下，不同设备对同一仓库中同一文件的不同行进行修改，然后各自push到远程仓库，这会导致冲突吗？*

如果两个设备在同一分支下，对同一个文件的不同行进行修改，并各自 push 到远程仓库中，这通常不会导致代码冲突（conflict）。

Git 可以在同一个文件中并行处理多个不同的修改，只有当两个设备对同一行代码进行了修改，并且这两个修改都被提交到了远程仓库中的同一个分支时，才会导致代码冲突。

所以，如果你在同一个文件中对不同行进行修改，并分别 push 到远程仓库，Git 可以自动合并这些修改，不会产生代码冲突。

但是，虽然不会产生代码冲突，但是你仍然需要注意代码的一致性和正确性，确保你的修改不会对其他人的修改产生影响，或者不会破坏代码的整体结构。因此，在多人协作开发时，最好遵循一些规范和标准，例如代码风格、命名规范等，以保证代码的整洁和可维护性。





