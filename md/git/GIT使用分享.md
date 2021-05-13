**目录**
- [分支命名规范:](#分支命名规范)
- [GIT 使用分享](#git-使用分享)
  - [一、基础命令](#一基础命令)
  - [二、添加别名](#二添加别名)
  - [三、分支管理](#三分支管理)
    - [1. 功能开发](#1-功能开发)
    - [2. 单独新功能开发](#2-单独新功能开发)
    - [3. 版本发布](#3-版本发布)
    - [4. 线上bug修复](#4-线上bug修复)
  - [四、仓库迁移](#四仓库迁移)
  - [五、子模块](#五子模块)
    - [1. 使用前提](#1-使用前提)
    - [2. 添加子模块](#2-添加子模块)
    - [3. 查看子模块](#3-查看子模块)
    - [4. 更新子模块](#4-更新子模块)
    - [5. 克隆包含子模块的项目](#5-克隆包含子模块的项目)
    - [6. 修改子模块](#6-修改子模块)
    - [7. 删除子模块](#7-删除子模块)

> 资料：  
> 1. https://git-scm.com/book/zh/v2/起步-关于版本控制    
> 2. https://www.ibm.com/developerworks/cn/java/j-lo-git-mange/index.html


# 分支命名规范:
```

feature_版本号      ---> 功能分支

develop             ---> 开发分支 

release_版本号      ---> 测试待发布分支 (SIT - UAT - PUAT)

master              ---> PRO分支

hotfix_版本号      ---> 线上bug修复分支

```




|分支类型|	命名规范|	创建自|	合并到|	说明|
|:-----:|:------:|:------:|:---:|:---:|
feature|	feature_*|	develop|	develop|	新功能
release|	release_*|	develop|	develop 和 master|	一次新版本的发布
hotfix|	hotfix_*|	master|	develop 和 master 可能会有release分支|	生产环境中发现的紧急 bug 的修复



# GIT 使用分享
![md-git.png](../../source/res/md-git.png)


## 一、基础命令
```
git init //初始化本地git环境
git clone XXX//克隆一份代码到本地仓库
git pull //把远程库的代码更新到工作台
git pull --rebase origin master //强制把远程库的代码跟新到当前分支上面
git fetch //把远程库的代码更新到本地库
git add . //把本地的修改加到stage中
git commit -m 'comments here' //把stage中的修改提交到本地库
git push //把本地库的修改提交到远程库中
git branch -r/-a //查看远程分支/全部分支
git checkout master/branch //切换到某个分支
git checkout -b test //新建test分支
git checkout -d test //删除test分支
git merge master //假设当前在test分支上面，把master分支上的修改同步到test分支上
git merge tool //调用merge工具
git stash   //把未完成的修改缓存到栈容器中
git stash list //查看所有的缓存
git stash pop //恢复本地分支到缓存状态
git blame someFile //查看某个文件的每一行的修改记录（）谁在什么时候修改的）
git status //查看当前分支有哪些修改
git log //查看当前分支上面的日志信息
git diff //查看当前没有add的内容
git diff --cache //查看已经add但是没有commit的内容
git diff HEAD //上面两个内容的合并
git reset --hard HEAD //撤销本地修改

```

## 二、添加别名

```
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.unstage 'reset HEAD'
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
```

## 三、分支管理

### 1. 功能开发
![md-git_develop.png](../../source/res/md-git_develop.png)

### 2. 单独新功能开发
![md-git_feature.png](../../source/res/md-git_feature.png)

### 3. 版本发布
! [20181228-203713.png](WEBRESOURCE03d20ebf8a286411f916abde06978092)

### 4. 线上bug修复
![md-git_bugfix.png](../../source/res/md-git_bugfix.png)


## 四、仓库迁移

```
git clone --bare http://域名/分组/仓库名称.git

cd 仓库名称.git

git push --mirror http://新域名/新分组/新仓库名称.git

```


## 五、子模块
> 参考资料: https://git-scm.com/book/zh/v2/Git-工具-子模块

### 1. 使用前提

经常碰到这种情况：当你在一个Git项目上工作时,你需要在其中使用另外一个Git 项目.
也许它是一个第三方开发的Git库或者是你独立开发和并在多个父项目中使用的.
这个情况下一个常见的问题产生了：你想将两个项目单独处理但是又需要在其中一个中使用另外一个.
在Git中你可以用子模块submodule来管理这些项目，submodule允许你将一个Git 仓库当作另外一个Git仓库的子目录.
这允许你克隆另外一个仓库到你的项目中并且保持你的提交相对独立.


### 2. 添加子模块
```
此文中统一将远程项目https://github.com/erlyshare/gitsub.git克隆到本地gitsub文件夹。

$ git clone https://github.com/erlyshare/gitsub.git gitsub
$ cd gitsub
$ git submodule add https://github.com/erlyshare/gitsub_common.git common

添加子模块后运行git status, 可以看到目录有增加1个文件.gitmodules, 这个文件用来保存子模块的信息。
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   .gitmodules
    new file:   common
```


### 3. 查看子模块
```
$ git submodule
 50b237cd34fc7c978ad8dac3d049adee7fd9e0f4 common  (heads/master)
```

### 4. 更新子模块

```
更新项目内子模块到最新版本
$ git submodule update

更新子模块为远程项目的最新版本
$ git submodule update --remote

```



### 5. 克隆包含子模块的项目
```
克隆包含子模块的项目有二种方法：一种是先克隆父项目，再更新子模块；另一种是直接递归克隆整个项目。

<一> 克隆父项目，再更新子模块

a. 克隆父项目
$ git clone https://github.com/erlyshare/gitsub.git gitsub

b. 查看子模块
$ git submodule
 -50b237cd34fc7c978ad8dac3d049adee7fd9e0f4 common
子模块前面有一个-，说明子模块文件还未检入（空文件夹）

c. 初始化子模块
$ git submodule init
Submodule 'common' (https://github.com/erlyshare/gitsub_common.git) registered for path 'common'

d. 更新子模块 初始化模块只需在克隆父项目后运行一次
$ git submodule update
Cloning into 'gitsub/common'...
Submodule path 'common': checked out '50b237cd34fc7c978ad8dac3d049adee7fd9e0f4'


<二> 递归克隆整个项目
$ git clone https://github.com/erlyshare/gitsub.git gitsub --recursive

Cloning into 'gitsub1'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
Submodule 'common' (https://github.com/erlyshare/gitsub_common.git) registered for path 'common'
Cloning into 'gitsub/common'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Submodule path 'common': checked out '50b237cd34fc7c978ad8dac3d049adee7fd9e0f4'

递归克隆整个项目，子模块已经同时更新了，一步到位。

<三> 一个命令
$ git submodule update --init --recursive

```

### 6. 修改子模块
```
在子模块中修改文件后，直接提交到远程项目分支。
$ git add .
$ git ci -m "commit"
$ git push origin master

```


### 7. 删除子模块
```
删除子模块比较麻烦，需要手动删除相关的文件，否则在添加子模块时有可能出现错误
同样以删除assets文件夹为例

a. 删除子模块文件夹
$ git rm --cached common
$ rm -rf common

b. 删除.gitmodules文件中相关子模块信息
[submodule "common"]
  path = common
  url = https://github.com/erlyshare/gitsub_common.git

c. 删除.git/config中的相关子模块信息
[submodule "common"]
  url = https://github.com/erlyshare/gitsub_common.git

d. 删除.git文件夹中的相关子模块文件
$ rm -rf .git/modules/common


#### 8. 迁移子仓库
a.先做整体的仓库迁移(目录第四大点)

b. 拉取新地址的仓库修改master分支子模块
$ git checkout master
修改.gitmodules
[submodule "common"]
	path = common
	url = https://..正确的路径../common.git
	
c. 设置子模块的url
$ cd common
$ git remote set-url origin https://..正确的路径../common.git

d. 更新一下 
$ git submodule update --init --recursive

e. 然后推送一下

$ git add . && git commit -m "modify submodule" 
$ git push origin
```

迁移例子：
```
$ game_name>git remote set-url origin https://git.company.com/APP/Game/Kind/game_name.git

检查一下地址
$ game_name>git remote -v
origin  https://git.company.com/APP/Game/Kind/game_name.git (fetch)
origin  https://git.company.com/APP/Game/Kind/game_name.git (push)

推送所有分支
$ game_name>git push origin --all
Counting objects: 11346, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (10753/10753), done.
Writing objects: 100% (11346/11346), 12.87 MiB | 1.27 MiB/s, done.
Total 11346 (delta 2742), reused 0 (delta 0)
remote: Resolving deltas: 100% (2742/2742), done.
To https://git.company.com/APP/Game/Kind/game_name.git
 * [new branch]      master -> master

处理master分支
$ game_name>git checkout master
Already on 'master'
Your branch is up-to-date with 'origin/master'.


修改
$ git checkout master
修改.gitmodules
[submodule "common"]
	path = common
	url = https://git.company.com/APP/Game/Kind/common.git

$ game_name>cd common

修改子模块的地址
$ game_name\common>git remote set-url origin https://git.company.com/APP/Game/Kind/common.git

$ game_name\common>git remote -v
origin  https://git.company.com/APP/Game/Kind/common.git (fetch)
origin  https://git.company.com/APP/Game/Kind/common.git (push)

$ game_name\common>cd ..

$ game_name>git remote -v
origin  https://git.company.com/APP/Game/Kind/game_name.git (fetch)
origin  https://git.company.com/APP/Game/Kind/game_name.git (push)

$ game_name>git status
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   .gitmodules

no changes added to commit (use "git add" and/or "git commit -a")

$ game_name>git add . && git commit -m "modify submodule"
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory.
[master 9881bd9] modify submodule
 1 file changed, 1 insertion(+), 1 deletion(-)

$ game_name>git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 385 bytes | 385.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://git.company.com/APP/Game/Kind/game_name.git
   dcc302d..9881bd9  master -> master
   
```   

