### 1、Git是什么

Git是目前世界上最先进的分布式版本控制系统。

现实中总会遇到编辑的文件改来改去的问题，如果大家协同作业，编辑一个文件，你自己会修改文件，别人也会修改文件，你可能还想知道自己曾经改过什么，别人改过什么，我的版本和别人的版本有何不同，之前的版本和现在的版本有何不同等等一系列的问题，如果你手动管理，会创建无数个文件，这就很费心费力，这时候如果有一个软件帮你管理，岂不是很爽。

这样的软件当然有，但是在最初的时候，版本控制系统是集中式的，虽然有免费的，但是不好用，然后又因为各种原因，linux系统的开发者linus自己写了一个分布式的版本控制系统——Git，并且迅速发展成最流行的分布式版本控制系统。

##### （1）集中式与分布式版本管理有何不同

集中式需要一个中央服务器来存放版本库，需要联网才能下载最新版本，如果网速不够快的话非常不利于工作

分布式不存在中央服务器，每个人电脑都有一个完整的版本库，然后将自己的修改相互推送就可以看见对方的修改，不过在实际中这种相互推送无法进行，所以一般会有一台充当中央服务器的电脑，但是没这个电脑也不影响大家干活

### 2、安装Git

直接去官网下载对应于自己操作系统的版本，下载完成后安装到自己想安装的文件夹，其他的一些设置直接默认即可，安装完成后可以在开始菜单里面找Git文件夹，打开之后会有Git bash的一项，点击之后弹出命令行对话框，说明安装成功了。

<center class="half">
    <img src="C:\Users\大霖\Desktop\截图\01000100.png" width="250"/>
    <img src="C:\Users\大霖\Desktop\截图\01000101.png" width="300"/>
</center>

安装完成后告诉Git你是谁，怎么联系你，就跟我上面做的那样

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

### 3、创建版本库

创建版本库就是创建自己的仓库，这个仓库里里面的所有文件可以被Git管理，每个文件的修改、删除都能被跟踪，还可以还原状态。

```shell
$ pwd # 查看当前所在的操作目录
$ cd /E # 切换到自己想去的目录
$ mkdir learngit # 创建一个目录
$ cd learngit # 进入创建的目录
$ git init # 将这个目录初始化为仓库
Initialized empty Git repository in E:/learngit/.git/ # 系统回应你仓库建好了，而且这是一个空的仓库
$ ls -ah # 查看隐藏的文件，发现有一个.git文件，这个文件用来跟踪管理仓库，避免手动修改
```

> 虽然可以将有文件的目录初始化为仓库，但是尽量不要这么做

##### （1）将文件添加到仓库

==所有的版本控制系统都只能跟踪文本文件的改动，比如TXT、网页、代码==，像图片、视频的二进制文件虽然可以被管理，但是不可以追踪其变化。因为word是二进制格式，所以也不可以被监控。

==另外就是不要用windows自带的记事本编辑任何文本文件==

当你编辑了一个文件想要提交到仓库的时候，你需要你做以下两步

```shell
$ git add 文件名
$ git commit "对于上面add文件的说明"
```

只有经过了以上两步，文件才会被添加到仓库

==总结命令==

```shell
$ git init 
$ git add
$ git commit
```

##### （2）掌握文件状态

有的时候，我们需要知道一个文件是否被修改了；如果被修改了，是否已经提交了；如果被修改了，被修改了哪些地方，这就需要用到以下命令

```shell
$ git status # 查看文件的状态，是否被修改，三种状态，一是有被修改，但是没有提交；二是有被修改，将要提交；三是当前工作目录干净，没有修改和将要提交
$ git diff 文件名 # 查看被修改了哪些内容
```

##### （3）版本回退

```shell
$ git log
$ git log --pretty=oneline
```

两条命令都是查看修改的日志，也就是修改的记录，不同的是，第二条命令显示的内容更为简洁，为图片所示

<img src="C:\Users\大霖\Desktop\截图\01000110.png" style="zoom: 80%;" />

其中==HEAD==表示当前版本，也就是最新状态，上个版本为==HEAD^==，上上个版本是==HEAD^^==，上一百版本是==HEAD~100==，还可以直接用版本号==commit_ID==

==版本回退==

```shell
$ git reset --hard HEAD^ # 回退到上个版本
```

这时候如果用`git log`查看日志的话，会神奇的发现回退之前的最新状态不存在了，仿佛从来没有发生过改变，但是可以通过更改说明前面的那一串提交ID再变回去，比如

```shell
$ git reset --hard 1dfe7
```

这样就会回到没有回退之前

但是如果你回退了之后关机了，第二天后悔了，然后你还找不到版本号，咋办

```shell
$ git reflog # 查看历史命令，记录你的每一条命令，可以找的版本号
```

==命令总结==

```shell
$ git log
$ git log --pretty=oneline
$ git reset --hard commit_ID
$ git reflog
```

##### （4）工作区和暂存区

我们可以将我们的仓库文件夹看作是工作区

工作区中有一个隐藏目录.git，这个不是工作区，而是Git的版本库，其中包括最重要的==stage==暂存区，还包括自动创建的第一个分支==master==，以及指向master的指针==HEAD==

当我们像仓库中提交文件的时候，有两步操作，第一步`git add xxx`，这一步实际上是把文件提交到暂存区。

第二步`git commit`是把暂存区的文件提交到当前分支

<center class="half">
    <img src="C:\Users\大霖\Desktop\截图\01000111.png" width="300"/>
    <img src="C:\Users\大霖\Desktop\截图\01001000.png" width="300"/>
</center>

##### （5）管理修改

为什么Git比其他的版本控制系统设计的优秀呢，这是因为Git跟踪管理的是修改，而不是文件。

也就是，Git并没有直接管理你的文件，不管你修改了多少次，只要你没有提交到暂存区（提交的是修改，而不是文件），文件就不可能被提交到分支上。

==命令==

```shell
$ git diff HEAD -- filename # 查看版本库中的最新版本和工作区的最新版本的区别
```

##### （6）撤销修改

+ 撤销工作区的修改

```shell
$ git checkout -- filename
```

如果文件修改后还没有被提交到暂存区，执行此命令会令文件回到和当前版本库相同的状态。

如果文件已经被提交到暂存区后又进行了修改，执行此命令会令文件回到当前暂存区的状态。

总结一下，就是这个命令会让文件回到当前版本库状态，或者暂存区状态。

+ 撤销提交到暂存区操作，文件放回工作区

```shell
$ git reset HEAD filename
```

此时可以再撤销工作区的修改

+ 撤销提交到版本库的修改

```shell
$ git reset --hard HEAD^ 
```

回退到上一个版本，但是前提是你没有上传远程版本库

##### （7）删除文件

```shell
$ rm filename # 删除工作区的文件
$ git rm filename # 删除版本库中的文件
$ git checkout -- filename # 工作区文件删除错了，用版本库文件还原
```

#### 4、远程仓库

GitHub提供Git仓库远程托管服务

##### （1）注册一个GitHub账号

官网地址https://github.com/

免费注册

##### （2）建立本地与Github之间的通信

+ 去`C:\用户\用户名\`下找一找有没有`.ssh`文件，或者是找一找有没有后缀名为`.pub`的文件，大概率一开始是没有的，没有的话，打开git bash，直接在用户目录下执行

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

邮箱名字写自己的

执行完成后获得一对公私钥，其中后缀名为`.pub`的文件为公钥

+ 登陆GitHub，打开“Account settings”，“SSH Keys”页面；点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，确认即可。

##### （3）创建一个仓库

<img src="C:\Users\大霖\Desktop\截图\01001001.png" style="zoom:67%;" />

再GitHub网站的右上角点击加号，第一个选项就是创建一个新的仓库

仓库的名字自己起，最好对应项目的名字

其他的默认即可，直接点击创建

##### （4）推送本地仓库到GitHub远程仓库中

+ 将本地仓库与新建的远程仓库连接

```shell
$ git remote add origin https://github.com/LINFALCON/learngit.git # 后面的这个链接因人而异，你需要输入自己的仓库地址

我貌似知道为什么后面我用SSH推送不成功了，应该问题出在这里，我用的是Http链接的
```

+ 将本地仓库内容推送到远程仓库

```shell
$ git push -u origin master 
```

其中-u表示将本地库与远程库的master分支联系起来，可以简化之后的操作

origin表示远程库

master表示master分支

==注意==：推送时会弹出页面输入GitHub账号密码，按照要求输入即可

推送完成后，GitHub上就多了一个和本地相同的仓库

<img src="C:\Users\大霖\Desktop\截图\01001010.png" style="zoom:67%;" />

以后就可以直接通过下面的命令将本地仓库的最新修改推送到GitHub

```shell
$ git push origin master
```

==命令总结==

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
$ git remote add origin https://github.com/LINFALCON/learngit.git
$ git push -u origin master # 仓库第一次推送
$ git push origin master # 之后的推送
```

##### （5）克隆远程库到本地

+ 先再GitHub新建一个远程仓库，为了不让这个仓库是空的，我们在创建的时候，让仓库自动生成一个md文件
+ 复制仓库的SSH链接

<img src="C:\Users\大霖\Desktop\截图\01001011.png" style="zoom:50%;" />

进入到仓库之后，你会在右上角部分看到一个==code==的按钮，点一下，出来的是==clone with https==，也可以点一下右边的==Use SSH==，就可以复制链接了

+ 在本地git bash上克隆

```shell
$ cd ..
$ git clone git@github.com:LINFALCON/gitskills.git # 使用SSH
$ git clone https://github.com/LINFALCON/gitskills.git # 使用HTTPS
$ cd gitskills
$ ls
README.md
```

表示克隆成功

当然==你也可以克隆大佬的公共仓库，这是非常实用的功能==

#### 5、分支管理

如果你和你的团队公用一个仓库，这时候你突然被要求去做另外一个工作，如果你还在这个仓库里工作会让别人没法干活，但是你不用仓库文件就有丢失的风险，这时候你可以创建一个分支，既可以保证自己的工作，又不影响被人的工作。

最后你还可以将你的分支合并到主分支上

（1）创建、合并、删除分支

```shell
$ git branch name # 创建分支
$ git checkout name # 切换分支
$ git switch name # 切换分支
$ git checkout -b name # 创建并切换分支
$ git switch -c name # 创建并切换分支
$ git branch # 查看分支
$ git merge name # 合并某分支到当前分支
$ git branch -d name # 删除分支
```

##### （2）解决冲突

如果创建分支之后，只有一个分支前进了，另一个分支没变，这种情况下

```shell
$ git merge name 
```

合并分支是一件很容易的事情

但是如果两条分支上的内容都有所改变，当然，这种改变并不相同，拿这种情况下再去合并两个分支的话就会出现问题，因为会有文件之间的冲突，所以这时候需要定下以哪个分支的为准，这时候需要回到文本编辑器中，保留一个分支的内容再重新提交即可。

另外

```shell
$ git log --graph
$ git log --graph --pretty=oneline --abbrev-commit
```

可以看到分支合并图

##### （3）分支管理策略

正确的分支管理策略就是，master作为主分支，需要有一个稳定的状态，它既不是我们的工作区域，也不是我们经常进行合并的分支。

所以我们首先会有一个分支dev，这个分支可以用来作为团队合并工作的分支，另外每一个团队成员都可以有自己的分支，然后将自己的分支合并到dev分支上，形成稳定的版本之后再合并到master分支

为了达到每次合并都被记录下来的目的，我们在合并分支的时候需要添加额外的参数，以保留分支合并的记录，

```shell
$ git merge --no-off -m"提交说明" dev
```

参数`--no-off`的作用使关闭快速合并，使用普通合并，合并后的历史有分支

最后，正确的分支图为下方所示

<img src="C:\Users\大霖\Desktop\截图\01001100.png" style="zoom:80%;" />

##### （4）bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场==`git stash`==一下，然后去修复bug，修复后，再==`git stash pop`==，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用==`git cherry-pick <commit>`==命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

##### （5）强行删除未合并分支

对于一个还没有被合并过的分支，如果用原来的方法去合并是不成功的，需要强行删除，这时可以用下面的命令

```shell
$ git branch -D name
```

##### （6）怎样多人协作

```shell
$ git remote # 查看远程库
$ git remote -v # 查看更加详细的信息，会显示可以抓取和推送的地址
```

+ 推送分支

```shell
$ git push origin master 
$ git push origin dev # 推送其他的分支
```

一般的话，主分支还有开发分支是要推送的，其他的看需要

+ 抓取分支

考虑这样一种情况，你克隆了大佬的一个项目到本地，你会发现自己克隆下来的只有一个主分支，你也想参与这个项目的开发，于是你

```shell
$ git checkout -b dev origin/dev # 在本地创建和远程分支对应的分支
$ git branch --set-upstream-to=origin/dev dev # 指定本地dev分支与远程的dev分支的链接
```

创建了远程的dev分支到本地，然后你就可以再dev分支做开发

有一天，大佬完成了一个文件，想把这个文件提交dev分支后推送到远程仓库，于是大佬

```shell
$ git add xxx.txt
$ git commit -m "add xxx.txt"
$ git push origin dev
```

然后大佬发现自己推送失败，原来你也向dev分支推送了他的提交，你跟大佬的修改出现了冲突，怎么解决呢？大佬桥下下面的命令

```shell
$ git pull # 抓取远程分支有冲突的提交
```

大佬拿到你的文件后，在不同的地方做出选择，修改后push到远程

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果==`git pull`==提示==`no tracking information`==，则说明本地分支和远程分支的链接关系没有创建，用命令==`git branch --set-upstream-to <branch-name> origin/<branch-name>`==。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

##### （7）变基

git的提交历史很容易搞得很乱，只是只需要一个命令就可以让时间线清晰起来

```shell
$ git rebase 
```

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

#### 6、标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

##### （1）创建标签

```shell
$ git tag v1.0 # 创建标签
$ git tag v1.0 <commit_ID> # 如果忘了给之前的提交打标签，可以通过提交编号打标签
$ git tag # 查看所有标签
$ git show tag_name # 查看标签信息
$ git tag -a v1.0 -m "version 1.0 released" commit_ID # 打标签时添加信息
```

##### （2）操作标签

删除未推送远程的标签

```shell
$ git tag -d v1.0
```

推送标签到远程仓库

```shell
$ git push origin v1.0
```

一次性推送全部尚未推送的标签

```shell
$ git push origin --tags
```

删除远程标签

```shell
$ git tag -d v1.0 # 删除本地标签
$ git push origin :refs/tags/11.0 # 删除远程
```

#### 7、使用GitHub

==`Fork`==可以将项目仓库拷贝到自己的账号下

==`pull request`==可以将自己在本地修改好推送到自己仓库内容推送给原项目，看项目方是不是会接受你的修改

#### 8、使用Gitee

国内的远程仓库托管

具体的使用可以参在GitHub的使用，先注册，再去设置本科可以和Gitee通信，再去创造远程仓库，再去将远程仓库与本地仓库相关联

注意远程仓库的名字不能同时为origin

```shell
$ git remote rm origin
$ git remote add github 链接
$ git remote add gitee 链接 # 这样就可以通过名字推送到不同的远程库
$ git push github master
$ git push gitee master
```













