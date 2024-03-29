# git简介

## 1 为什么要有git？

​	对于一个文件反复修改的时候，想查看自己的修改记录，就必须一个文件一个文件查找。有一个好办法就是自己记录每次修改的部分，但这样还是比较麻烦。

​	用git就能解决这个问题，git不仅仅可以记录每次的修改说明，还能保存每一次的修改文件，并且还支持多用户协同作业（相互之间不影响）。

## 2 集中式和分布式（版本管理系统）

​	集中式的版本控制系统前提是有一台中央服务器，每个工作者首先从服务器中下载自己需要的文件，然后修改好之后再上传，这样看上去挺好的，但是中央服务器一旦出问题了，那就是谁都别想干活，并且集中式版本控制系统需要联网，网速不好的时候很可能上传失败。

​	分布式的版本控制系统是将每个工作者的电脑都可以看成一个中央处理器，每个人电脑里都有一个完整的版本库，不用联网也可以工作，所以就不存在电脑坏了就不能工作的情况，git就属于分布式的版本控制系统，接下来还会介绍很多关于git的强大。

## 3 安装git

Linux：

~~~
sudo apt-get install git
~~~

windows:

1. 下载安装程序，注册一个GitHub账号

2. ~~~
   git
   
   git config --global user.name "***"
   git config --global user.email "***"
   ~~~

## 4 创建版本库

a. 创建一个新的文件夹，将其初始化为Git可以管理的仓库

b. 将文件添加到仓库

~~~
git init
git add readme.txt    #add也可以提交多个文件
git commit -m "readme file"    #-m后表示说明
git add file1.txt file2.txt file3.txt
git commit -m "add 3 file"
~~~

c. 对readme文件进行更改，使用git查看不同，提交修改文件

~~~
git status    #查看此时状态
git diff readme.txt    #查看工作区和暂存区（add）之间的差异
git add readme.txt
git commit -m "first modify"
~~~

d. 版本操作

~~~
git log    #查看历史记录，显示各个的版本号和HEAD指向
git log --pretty=oneline    #和上一条命令功能一致，只是显示更简洁
git reset --hard HEAD~1    #回退上一个版本
git reset --hard 1bf20    #根据commit_id来选择回退的版本
git reflog    #查看以往的版本命令
~~~

e. 电脑中的文件夹就是工作区，隐藏文件.git中是Git的版本库，版本库里最重要的就是暂存区，git add就是将文件添加到暂存区，git commit是将暂存区的文件提交到当前分支（master），如果修改了文件但是没有提交到暂存区那么是无法提交到master。可以通过命令查看工作区和版本库之间的区别

~~~
git diff HEAD -- readme.txt
~~~

说明：在工作区修改文件，后提交到暂存区，最后提交到master上，那么此时的状态是没有任何修改的；如果是在工作区修改了文件，后提交到暂存区，在工作区对文件进行再一次修改但不提交，此时执行git commit只能提交第一次修改后的文件，查看状态时会显示文件已被修改

f. 对于修改的文件想要回退有以下操作

​	第一，在工作区修改，还没提交到暂存区

~~~
git checkout -- readme.txt    #将工作区文件回退到最近一次暂存区或者版本区的状态
~~~

​	第二，修改的文件已经提交到了暂存区

~~~
git reset HEAD readme.txt    #将暂存区的修改撤销，但是不改变工作区的修改
~~~

​	第三，已经提交到master

~~~
git reset HEAD~1    #版本回退
~~~

g. 如果在工作区误删文件，可以找回

~~~
git checkout -- test.txt    #相当于将版本区的文件覆盖到工作区
~~~

如果是正常操作，同时将版本区内的文件删除

~~~
git rm test.txt
git commit -m "delete file"
~~~

## 5 远程仓库

### 5.1 添加远程库

查看用户主目录下的.ssh文件中是否具有id_rsa和id_rsa.pub这两个文件，如果没有，需要打开git bash创建

~~~
ssh-keygen -t rsa -C "xieyushunxysh@163.com"
~~~

id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人，登录GitHub添加ssh key（一个电脑对应一个key），这样之后就github就可以识别是你推送的信息了。

在GitHub上创建一个文件夹用于存放推送的数据，将本地文件推送到GitHub

~~~
git remote add origin git@github.com:dumpling-eric/git_learning.git    #关联本地仓库
git push -u origin master    #将当前分支推送到远程，-u会将本地master和远程master关联起来
~~~

远程库的名字就是origin，这是一种默认的叫法，以后如果还需要修改，只需要本地提交即可

~~~
git push origin master
~~~

### 5.2 克隆远程库

对远程库进行克隆到本地

~~~
git clone git@githun.com:dumpling-eric/testgit.git
~~~

补充：

Windows下的一些指令

~~~
type readme.txt    #查看文件内容
dir    #相当于Linux中的ls
~~~

## 6 分支管理

​	创建一个只属于自己的分支，随时提交随时更改。

### 6.1 创建合并分支

在原本的主分支中，master指向最后提交的内容，HEAD指向master，每次提交就相当于master向前移动。创建一个分支就是新建一个dev，并且HEAD是指向dev，此时提交文件dev向前移动，而master原地不动。

~~~
git checkout -b dev    #新建并切换到dev指针
git branch    #查看当前分支，当前分支前有一个“*”
git add readme.txt
git commit -m "branch test"    #提交后master上并不会显示
git checkout master    #切换回master
~~~

将dev与master合并，删除dev分支

~~~
git merge dev    #合并分支
git branch -d dev    #删除分支，如果没有合并则会报错不能删除
git branch -D dev    #强行删除分支
~~~

合并时使用的是fast forward模式，直接将master移动到dev处

### 6.2 解决冲突

假设新建一个分支dev并在该分支上提交了文件，后返回到分支master上也提交了修改文件，此时两个分支合并就会发生问题，只能手动解除冲突。

~~~
type readme.txt    #查看文件会显示两个分支上的内容
git log --graph --pretty=oneline --abbrev-commit    #查看分支的合并情况
~~~

### 6.3 分支管理

fast forward模式在删除分支后就会丢失分支信息，所以一般使用普通模式

~~~
git merge --no-ff -m "merge with no-ff" dev
~~~

no-ff表示禁用，-m后是说明commit的说明，所以普通模式就是master向前移动一位，同时dev也指向那个位置。普通模式是实际开发中常用的，在实际开发中master不轻易更改，先建一个dev分支，然后每个人新建属于自己的分支，合并内容时将自己的分支和dev分支合并，最后确定后再将dev和master合并。

### 6.4 bug分支

假设一个场景，目前处于dev分支上，并且文件还不能提交，这个时候master分支上有一个bug必须马上处理，所以需要将dev分支存储起来，等处理好了bug之后再恢复，这就利用的Git里面的stash功能。

~~~
git stash    #将dev分支存储起来，接着查看status是干净的
git checkout master
git checkout -b bug-1
git add readme.txt
git commit -m "..."    #处理完bug
git checkout master
git merge --no-ff -m "..." bug-1
git branch -d bug-1    #合并到master分支上
~~~

之后开始恢复dev分支

~~~
git checkout dev
git stash list    #查看储存的分支
git stash pop    #恢复最近储存的分支并删除
git stash apply stash@{0}    #恢复最近储存的分支
git stash drop atash@{0}    #删除最近储存的分支
~~~

### 6.5 多人协作

对于远程仓库可以查看

~~~
git remote -v
~~~

**推送分支**是也可以任意选择：master分支是主分支，时刻保持同步；dev分支是开发分支，所以小伙伴都在上面工作；bug分支是修复本地bug，没有必要推送到远程；feature分支根据具体情况推送。

~~~
git push origin dev
~~~

假设一个小伙伴要在dev分支上工作，可是在默认情况下只能查看远程库的master分支，此时他就必须在本地创建和远程origin/dev分支相对应的分支

~~~
git checkout -b dev origin/dev
~~~

当你和小伙伴都在dev分支上推送相同文件，那么会报错，解决方式是先将本地的dev和远程origin/dev分支链接起来，这样才可以将最新的提交从origin/dev上抓取下来，在本地完成合并上再推送

~~~
git branch --set-upstream-to=origin/dev dev
git pull    #抓取最新的提交，方便在本地进行合并
~~~

总而言之，多人协作的工作模式如下：

首先，推送自己的修改git push origin branch-name；

推送失败时，表明有小伙伴已经推送了相同的文件，此时git pull抓取最新的提交文件；

如果抓取不了，显示no trackiing information则说明本地分支和远程分支没有链接，使用git branch --set-upstream-to branch-name origin/branch-name;

抓取下来在本地解决冲突，最后git push origin branch-name。

## 7 标签管理

标签类似于快照，可以保存某个时刻的版本库

~~~
git checkout master    #切换到需要打标签的分支上
git tag v1.0
git tag    #查看标签

git tag v0.9 e5e5533    #给特定的commit_id打标签
git tag -a tagname -m "..."    #给标签附上说明
~~~

本地标签可以删除

~~~
git tag -d v0.9
~~~

标签可以推送到远程并且也可以删除

~~~
git push origin v1.0    #推送一个标签
git push origin --tag    #推送全部标签

git tag -d v1.0    #删除本地标签
git push origin :refs/tags/v1.0    #删除远程标签（先删除本地标签再删除远程标签）
~~~

## 8 码云和搭建git服务器

待补充，目前用不到

