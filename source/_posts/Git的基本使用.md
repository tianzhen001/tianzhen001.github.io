---
title: Git的基本使用
date: 2020-4-16 20:55:24
tags: git命令
categories: Git
summary:  Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
---
# 1. git基础入门

> Git是一款免费、开源的**分布式** **版本控制系统** ，用于敏捷高效地处理任何或小或大的项目。
>

<img src="./images/reset.gif">

## 1.1 git的安装

[下载地址](https://git-scm.com/download/win)

注意：

1. 不要安装在中文目录
2. 不要使用桌面管理软件

安装很简单，一直下一步即可。在任意的目录下右键，能看到菜单, 就表示安装成功了。



github账号注册 

[github官网](https://github.com/)


## 1.2 git三个区

要对某个项目使用git进行管理，需要使用`git init`命令初始化git仓库
`git init`会在当前目录生成一个隐藏文件夹 .git  不要去修改这个文件夹下的任意东西。

git仓库会分成三个区

**工作区**：我们书写代码的地方，工作的目录就叫工作区。

**暂存区：**暂时存储的区域，在git中，代码无法直接从工作区提交到仓库区，而是需要先从工作区添加到暂存区，然后才能从暂存区提交到仓库区。暂存区的目的是避免误操作。

**仓库区：**将保存在暂存区域的内容永久转储到 Git 仓库中，生成版本号。生成版本号之后，就可以任何的回退到某一个具体的版本。

## 1.3 git基本命令

### git init

+ 作用：初始化git仓库，想要使用git对某个项目进行管理，需要`git init`进行初始化

```bash
# 初始化仓库， 在当前目录下生成一个隐藏文件夹.git
git init
```

### git add

- 作用：将文件由 `工作区` 添加到 `暂存区`，在git中，文件无法直接从工作区直接添加到仓库区，必须先从工作区添加到暂存区，再从暂存区添加到仓库区。
- 命令：`git add 文件名/目录名`

```bash
# 将index.html添加到暂存区
git add index.html

# 将css目录下所有的文件添加到暂存区
git add css

# 添加当前目录下所有的文件
git add .
git add --all
```

### git commit

作用：将文件由 **暂存区** 添加到 **仓库区**，生成版本号（存档）

```bash
# 将文件从暂存区提交到仓库
git commit -m "提交说明"

# 如果不写提交说明，会进入vi编辑器，没有写提交说明，是提交不成功的。 :q!来退出
git commit   # 需要使用vi输入内容  vi编辑器常用操作
```

### git config配置

如果是第一次提交，需要配置提交者信息，推荐和github的账号邮箱一致

```Bash
# git config  user.name 你的目标用户名
# git config  user.email 你的目标邮箱名

# 使用--global参数，配置全局的用户名和邮箱，只需要配置一次即可。推荐配置github的用户名和密码
git config  --global user.name  皮皮虾
git config  --global user.email 2641543334@qq.com
```

如果是之前已经配置了，想清空的话，可以使用如下命令：

~~~js
git config --global user.name '' 
git config --global user.email ''
~~~



### git status

- 作用：查看文件的状态


- 命令：`git status` 
  - 红色表示工作区中的文件需要提交
  - 绿色表示暂存区中的文件需要提交

### git log

- 作用：查看提交日志
- `git log` 查看提交的日志
- `git log --oneline` 简洁的日志信息
- `git reflog ` 查看**所有的**版本信息 ，包括回退的操作

## 1.4 git重置或版本回退

### git reset

- 作用：版本回退，将代码恢复到已经提交的某一个版本中。
- `git reset --hard 版本号` 将代码回退到某个指定的版本(版本号只要有前几位即可)


- 当使用了`git reset`命令后，版本会回退，使用`git log`只能看到**当前版本**之前的信息。使用`git reflog`可以查看所有的版本信息

##  1.5 git忽视文件

> 在仓库中，有些文件是不想被git管理的，比如数据的配置密码、写代码的一些思路，node_modules等。git可以通过配置从而达到忽视掉一些文件，这样这些文件就可以不用提交了。

- 在仓库的根目录创建一个 的文件，文件名是固定的。
- 将不需要被git管理的文件路径添加到`.gitignore`中

```css
# 忽视idea.txt文件
idea.txt

# 忽视css下的index.js文件
css/index.js

# 忽视css下的所有的js文件
css/*.js

# 忽视css文件夹
css
```

# 2. git分支操作

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

![](images/fenzhi.png)

## 2.1 为什么要有分支？

- 如果你要开发一个新的功能，需要2周时间，第一周你只能写50%代码，如果此时立即提交，代码没写完，不完整的代码会影响到别人无法工作。如果等代码写完再提交，代码很容易丢失，风险很大。
- 有了分支，你就可以创建一个属于自己的分支，别人看不到，也不影响别人，你在自己的分支上工作，提交到自己的分支上，等到功能开发完毕，一次性的合并到原来的分支。这样既安全，又不影响他人工作。


## 2.2 git分支命令

> 在git中，分支实质上仅仅是一个指针，每次代码提交后，这个分支指针就会向后移动，保证一直指向最后一次提交的的版本。git中使用HEAD指向当前分支

### 创建分支

- `git branch 分支名称`创建分支，分支中的代码，在创建时与当前分支的内容完全相同。
- git在第一次提交时，就有了一个叫`master`的主分支。
- `git branch dev`，创建了一个叫做dev的分支

### 查看分支

- `git branch`可以查看所有的分支
- 在当前分支的前面会有一个`*`
- 在git中，有一个特殊指针`HEAD`,永远会指向当前分支

### 切换分支

- `git checkout 分支名称`切换分支  HEAD指针指向了另一个分支
- 在当前分支的任何操作，都不会影响到其他的分支，除非进行了分支合并。
- 提交代码时，会生产版本号，当前分支会指向最新的版本号。

### 创建并切换分支



- `git checkout -b 分支名称` 创建并切换分支
- 切换分支会做两件事情
  - 创建一个新分支
  - 把head指针指向当前的分支

### 删除分支

- `git branch -d 分支名称` 可以删除分支
- 注意：不能在当前分支删除当前分支，需要切换到其他分支才能删除。
- 注意：`master`分支是可以删除的，但是不推荐那么做。

### 合并分支

- `git merge 分支名称` 将其他分支的内容合并到当前分支。
- 在`master`分支中执行`git merge dev` 将`dev`分支中的代码合并到`master`分支
- [分支合并](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

## 2.3 git合并冲突

- 对于同一个文件，如果有多个分支需要合并时，容易出现冲突。
- 合并分支时，如果出现冲突，只能手动处理，再次提交，一般的作法，把自己的代码放到冲突代码的后面即可。

# 3. git远程仓库

## 3.1 github与git

git与github没有直接的关系。

- git是一个版本控制工具。
- github是一个**代码托管平台**，开源社区，是git的一个远程代码仓库。

```javascript
//1. gitHub是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名gitHub。
//2. github免费，代码所有人都能看到，但是只有你自己能修改。付费的可以隐藏。
//3. 创建git项目时，不能有中文。
```

[github官网](https://github.com/)

[开源中国-git](https://git.oschina.net/)

![image-20200722113838573](images/image-20200722113838573.png)

## 3.2 git clone

- 作用：克隆远程仓库的代码到本地
- git clone [远程仓库地址]
- `git clone https://github.com/itcastAAA666/myCode68.git`会在本地新建一个`test`文件夹，在test中包含了一个`.git`目录，用于保存所有的版本记录，同时test文件中还有最新的代码，你可以直接进行后续的开发和使用。
- git克隆默认会使用远程仓库的项目名字，也可以自己指定。需要是使用以下命令：`git clone [远程仓库地址] [本地项目名]`

## 3.3 git push

- 作用：将本地仓库中代码提交到远程仓库
- `git push 仓库地址 master` 在代码提交到远程仓库，注意master分支必须写，不能省略
- 例子：`git push https://github.com/itcastAAA666/myCode68.git master` 如果第一次使用，需要填写github的用户名和密码

## 3.4 git pull

- 作用：将远程的代码下载到本地


- 通常在push前，需要先pull一次。

```bash
# 获取远程仓库的更新，并且与本地的分支进行合并
git pull
```


## 3.5 SSH免密码登陆

git支持多种数据传输协议：

- https协议：`https://github.com/你的注册名/test.git`  需要输入用户名和密码
- ssh协议：`git@github.com:你的注册名/test.git`   可以配置免密码登录

每次push或者pull代码，如果使用https协议，那么都需要输入用户名和密码进行身份的确认，非常麻烦。

- github为了账户的安全，需要对每一次push请求都要验证用户的身份，只有合法的用户才可以push
- 使用ssh协议，配置ssh免密码，可以做到免密码往github推送代码

## 3.6 SSH免密码登录配置

注意：这些命令需要在bash中敲

- 1 创建SSH Key：`ssh-keygen -t rsa`  
  - key 钥匙的意思
  - gen-- generate 生成，产生的意思
- 2 在文件路径 `C:\用户\当前用户名\` 找到 `.ssh` 文件夹
- 3 文件夹中有两个文件：
  - 私钥：`id_rsa`
  - 公钥：`id_rsa.pub`   将公钥添加到github当中
- 4 在 `github -> settings -> SSH and GPG keys`页面中，新创建SSH key
- 5 粘贴 公钥 `id_rsa.pub` 内容到对应文本框中
- 5 在github中新建仓库或者使用现在仓库，拿到`git@github.com:用户名/仓库名.git`
- 6 此后，再次SSH方式与github“通信”，不用输入密码确认身份了

## 3.7 git remote

每次push操作都需要带上远程仓库的地址，非常的麻烦，我们可以给仓库地址设置一个别名

```bash
# 给远程仓库设置一个别名
git remote add 仓库别名 仓库地址
git remote add pinyougou git@github.com:你的注册名/test.git

# 使用如下来推送数据
git push pinyougou master

# 删除PPXCode这个别名
git remote remove pinyougou
```