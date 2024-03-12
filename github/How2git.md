# 使用git上传代码到GitHub

此文档记录我使用git上传代码到GitHub的流程，以前均使用gitee进行代码托管，但由于gitee文件大小限制问题，所以转向使用GitHub

我已经安装好了git，并且已经科学上网。

------

## 操作流程

我使用给git的目的是让我的三台电脑，包含2个Windows系统，1个Mac系统和2个Linux系统，进行代码同步。

首先，我在桌面创建了一个文件夹，名为`win_mac_linux`

在Windows系统下使用git bash进入上述新文件夹，执行命令 `git init`

此时，本地已经初始化好了名为`win_mac_linux`的git仓库，接下来需要到github中创建一个远程仓库来与本地仓库进行同步。

进入GitHub后，我的账号使用的不是QQ邮箱，而是163邮箱，然后修改了我的头像（原来是默认的，很丑），换成了我使用stable diffusion生成的AI头像，不过非常遗憾的只支持不超过1mb大小的图片。

之后我删除了原本不用的仓库，需要进入仓库，在设置选项的最下面进行删除，删除时还需要你输入相关的路径。

然后就创建了一个新的仓库，也是叫`win_mac_linux`

接下来做的应该是将远程仓库和本地仓库关联起来，并进行提交测试即可。

在git bash中执行命令`git config --global --list`

可以查看配置文件，需要修改名字为GitHub同样的昵称，邮箱也是GitHub的邮箱。

`git config --global user.name "你的名字"`

`git config --global user.email "你的邮箱"`

然后生成公钥，请注意在生成公钥前务必进行正确的邮箱和姓名配置。

在git bash中执行命令`ssh-keygen -t rsa -C "youremail@example.com"`即可创建SSH公钥，位于用户主目录，比如你的电脑名称叫admin，那么位于/c/Users/admin/.ssh下，名为id_rsa.pub，使用cat命令查看，复制该公钥到github网站中个人设置里的SSH keys中，这使得每次上传代码可以免去密码。

之后我再使用git clone命令将远程仓库拉取到本地，我就可以在本地修改该远程仓库，再进行提交。

唯一蛋疼的是，我创建的文件夹里又多了一个文件夹，我只好把我的文件夹改一下名，以免命名冗余。

## 提交失败记录

*此条目用于记录本人实时遇到的冲突情况即解决办法。*

昨天晚上，我在macOS上对swiftUI文档进行了新增，但是由于太晚了，我忘了提交。今天早上我没有立即对其进行提交，而是在Windows上对Hadoop文档进行了新增，并完成了提交。今天下午我使用macOS时，没有立即把最新仓库拉取下来，而是对昨晚的修改进行提交，在执行`git push`的时候出现了错误：

```sh
git push
To github.com:SucRunBug/win_mac_linux.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:SucRunBug/win_mac_linux.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

这时候我才想起，本地仓库还不是最新的，于是执行`git pull`，然后再次出现错误：

```sh
git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 4 (delta 1), reused 4 (delta 1), pack-reused 0
Unpacking objects: 100% (4/4), 1.44 KiB | 294.00 KiB/s, done.
From github.com:SucRunBug/win_mac_linux
   d3b8aa9..32d8269  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

通过询问ChatGPT了解解决方案，需要先将远程代码合并到本地，再进行上传。

合并代码执行`git pull --rebase origin main`

这个命令将使用rebase方式合并远程仓库的更改到本地仓库，这可以使你的提交历史更加线性化。

如果你希望使用merge方式合并更改，可以使用以下命令：

```
git pull --no-rebase origin main
```

执行完`git pull`命令后，你需要处理合并冲突，编辑文件以解决冲突，然后将更改提交到本地仓库。最后，你可以使用以下命令将更改推送到远程仓库：

```
git push origin main
```



2023.5.29

自从使用github一个月以后，我几乎每天都会上传文件，很少出了冲突或者错误。

但是昨天我不小心把paddle安装到我的虚拟环境里了，也就顺带被添加到暂存库了，这个库超过了100mb，就传不上去，我之前就是因为这个错误放弃使用了gitee，没想到GitHub还是会限制，但是好像可以用专门的大文件管理方式。

总之错误信息如下：

```bash
remote: Resolving deltas: 100% (946/946), completed with 13 local objects.
remote: error: Trace: 397ae53f6da38d29b3122f8a2478a738d0add3083ca23c1bcbe7010b6919848c
remote: error: See https://gh.io/lfs for more information.
remote: error: File PythonStudy/pythonProgrammingFromIntro2Practice/learning_log/ll_env/lib/python3.9/site-packages/paddle/fluid/libpaddle.so is 158.30 MB; this exceeds GitHub's file size limit of 100.00 MB
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
To github.com:SucRunBug/win_mac_linux.git
 ! [remote rejected]   main -> main (pre-receive hook declined)
error: failed to push some refs to 'github.com:SucRunBug/win_mac_linux.git'
```

解决方案

```bash
git filter-branch --tree-filter 'rm -f PythonStudy/pythonProgrammingFromIntro2Practice/learning_log/ll_env/lib/python3.9/site-packages/paddle/fluid/libpaddle.so' HEAD
```

```bash
git push --force origin main
```

参考ChatGPT的回答：

这个错误提示是因为您尝试将一个超过GitHub文件大小限制的文件（158.30 MB）推送到您的GitHub仓库中。 GitHub的文件大小限制为100 MB，因此您需要采取适当的措施来解决此问题。

您可以使用Git Large File Storage (LFS)来管理大型文件。 

如果您已经将大文件添加到暂存区（也称为索引），但尚未提交更改，您可以使用以下命令从git存储库中完全删除它：

```bash
git rm --cached path/to/your/large/file
```

这将从您的git存储库中删除该文件的历史记录，并将其从暂存区中删除。请注意，这不会从您的本地文件系统中删除该文件。

如果您已经提交了更改，并且要从git存储库中完全删除大文件，您需要执行以下步骤：

1. 从您的本地文件系统中删除大文件。

2. 使用以下命令来删除存储库中的大文件的历史记录：

```bash
git filter-branch --tree-filter 'rm -f path/to/your/large/file' HEAD
```

这将删除存储库中所有提交的大文件的历史记录。这可能需要一些时间，具体取决于您的存储库的大小和提交历史记录的数量。

3. 将存储库强制推送到GitHub：

```bash
git push --force origin your-branch
```

请注意，这会覆盖GitHub上存储库上的任何更改，并且可能会影响其他人的工作。因此，您应该谨慎使用`git push --force`命令，并确保在执行之前与您的团队成员进行沟通。

最后，请注意，删除存储库中的历史记录可能会导致某些提交的更改丢失。因此，在执行此操作之前，您应该仔细检查您的存储库，并确保您不会丢失任何重要的更改。

### 提交失败的终极解决方案

我发现有时候这种情况是一环扣一环的，比如上面我传了个大文件，我直接删了本地仓库再同步，但还是有问题，那么终极解决方案就是如果涉及大型工程，只能把代码上传，而不是上传一些复杂的工程文件，毕竟git主要就是为文本文件同步服务的。

# git协同工作

## 喜欢的命令

`git stash` 可以把当前没有完成的事情先暂存一下

`git cherry-pick` 可以让你有选择地合并提交

`git add -p` 可以让你挑选改动提交

`git grep $regexp $(git rev-list --all)` 可以用来在所有的提交中找代码。

## 中心式协同工作流

### 一般过程

1. 从服务器上做`git pull origin master`把代码同步下来。
2. 改完后，`git commit`到本地仓库中。
3. 然后`git push origin master`到远程仓库中，这样其他同学就可以得到你的代码了。

**注意，这里有我上面冲突记录的解决方案。**

如果在第 3 步发现 push 失败，因为别人已经提交了，那么你需要先把服务器上的代码给 pull 下来，为了避免有 merge 动作，你可以使用 `git pull --rebase` 。这样就可以把服务器上的提交直接合并到你的代码中，对此，Git 的操作是这样的。

1. 先把你本地提交的代码放到一边。
2. 然后把服务器上的改动下载下来。
3. 然后在本地把你之前的改动再重新一个一个地做 commit，直到全部成功。

如果有冲突，那么你要先解决冲突，然后做 `git rebase --continue` 。git 在做 pull --rebase 时，会一个一个地应用（apply）本地提交的代码，如果有冲突就会停下来，等你解决冲突。

上面这种方式，是我目前主要在用的，但如果开发的功能一旦多起来了，这样会干扰严重，于是有了下面的方案。



## 功能分支协同工作流

我们引入“功能分支”。这个协同工作流的开发过程如下。

1. 首先使用 `git checkout -b new-feature` 创建 “new-feature”分支。
2. 然后共同开发这个功能的程序员就在这个分支上工作，进行 add、commit 等操作。
3. 然后通过 `git push -u origin new-feature` 把分支代码 push 到服务器上。
4. 其他程序员可以通过`git pull --rebase`来拿到最新的这个分支的代码。
5. 最后通过 Pull Request 的方式做完 Code Review 后合并到 Master 分支上。



## 无法连接到GitHub

家用的广电网络，即使翻墙了，依旧有概率无法pull代码到GitHub，只需要连接手机热点即可解决。



## 错误提交大文件后高效的解决办法

直接把代码从远端拉下来，再把该提交的提交了，老的本地仓库直接删掉，反正也没有提交上去什么新东西。
