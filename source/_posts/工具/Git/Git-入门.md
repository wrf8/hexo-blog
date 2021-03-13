---
title: Git --- 入门
date: 2021-03-08 21:38:39
categories:
- 工具
- Git
tags:
---

## 1.关于版本控制
### 1.1 什么是版本控制
版本控制是一种记录一个或若干文件(可以是任意类型的文件)内容变化，以便将来查阅特定版本修订情况的系统。

### 1.2 为什么要用版本控制
在我们的实际开发过程中，经常会有这样的问题：
1. 多人开发同一个项目，代码合并的问题
2. 检查某个文件的修改记录（修改人、修改时间、修改内容）
3. 将项目代码恢复到之前的某个版本

### 1.3 历史分类
纵观版本控制系统的发展历史，[《Version Control By Example》](https://ericsink.com/vcbe/index.html)一书的作者 `Eric Sink` 在他的书中对版本控制进行了分类，广义上讲，版本控制工具的历史可以分为三代：

| 代 | 名称 | 网络 | 操作 | 并发性 | 示例 |
| :-------: | :-------: | :-----: | :--------: | :--------: | :---------: |
| 第一代 | 本地版本控制系统 | 无 | 仅一个文件 | 锁定的 | RCS |
| 第二代 | 集中式版本控制系统 | 集中式 | 多文件 | 提交之前合并 | CVS, SourceSafe, Subversion, Team Foundation Server |
| 第三代 | 分布式版本控制系统 | 分布式 | 变更的集合 | 合并之前提交 | Bazaar, Git, Mercurial |

#### 1.3.1 本地版本控制系统
本地版本控制系统，一次只能有一个人处理文件，大多都是采用某种简单的数据库来记录文件的历次更新差异。

#### 1.3.2 集中式版本控制系统
集中式版本控制系统，版本库是集中存放在中央服务器的，协同工作的人都是通过客户端连接到这台服务器，取出最新的文件或者提交更新。
![](/images/工具/Git/git_start_category_cvcs.png)
由上图可看到，在集中式版本控制系统中：
- 优点: 不同的开发者可以在不同的电脑上进行协同开发，对同步修改更加宽容
- 缺点: 中央服务器的单点故障。如果宕机一小时，那么在这一个小时内，谁都无法提交更新。如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，那么将丢失所有数据——包括项目的整个变更历史，只剩下各自机器上保留的单独快照。

#### 1.3.3 分布式版本控制系统
分布式版本控制系统，允许合并和提交分开，在每个使用者电脑上都有一个完整的版本库，包括完整的历史记录，没有网络依然可以使用。
![](/images/工具/Git/git_start_category_dvcs.png)
由上图可看到，分布式式版本控制系统也可以有个服务器端的仓库，用来同步各开发者的私有仓库！在分布式版本控制系统中，每个参与者的本地也会有一个完整的仓库。即使服务器端崩溃，我们仍然可以使用 Git（仅在本地仓库管理我们的代码），在网络具备时，再和服务器进行同步即可！

## 2.Git
### 2.1 Git起源
Linus 在1991年创建了开源的 `Linux`，Linus 虽然创建了 Linux，但 Linux 的壮大是靠全世界热心的志愿者参与的，初期世界各地的志愿者把源代码文件通过 `diff` 的方式发给Linus，然后由linus 本人通过 `linux` 命令 `diff` 和 `patch` 两条命令手动完成。随着 Linux 代码越来越壮大，靠 Linus 一个人来手动合并已经不现实。2002 年，Linus 选择了一个商业版本控制系统 `BitKeeper` 作为 Linux 内核的代码管理工具（`BitKeeper` 的开发商 `BitMover` 授权 linux 社区免费使用）。但是，免费使用是有很多的限制的，因此 linux 社区的大佬开始破解 `BitKeeper`。其中，`samba` 的作者 `andrew` 破解成功了。但是被 `BitMover` 公司发现，收回免费使用权。

迫不得已，Linus 选择了自己开发一个分布式版本控制工具以替代 `BitKeeper`。linus 闭关一个月，用C写出了 `Git`。在一个月后，`Git` 成功接管了 `Linux` 社区的版本控制工作，并且开始开源。

### 2.2 Git简介
`Git` 是目前世界上最先进的分布式版本控制系统。

#### 2.2.1 Git特点
1. 直接记录快照，而不是差异比较: 
    其他版本控制系统(`CVS`、`Subversion`、`Perforce`、`Bazaar` 等等)将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异: 
    ![](/images/工具/Git/git_start_cvs_record.png)
    在`Git`中保存项时，它主要对当时的全部文件制作一个快照并保存这个快照的索引，`Git` 对待数据更像是一个 `快照流`。
    ![](/images/工具/Git/git_start_git_record.png)
2. 近乎所有操作都是本地执行
3. Git保证完整性: Git数据库中保存的信息都是以文件内容的哈希值（40个十六进制字符 `0-9` 和 `a-f`）来索引，而不是文件名。
4. 执行的Git操作，只往Git数据库中增加数据，而且Git操作不可逆。

#### 2.2.2 Git工作区、暂存区和版本库
Git有三种状态，你的文件有可能处于其中之一：
- `已提交`：表示数据已经安全的保存在本地数据库中
- `已修改`：表示修改了文件，但还没保存到数据库中
- `已暂存`：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中

由此可以引入Git的三个工作区：
![](/images/工具/Git/git_start_work_zone.png)
- `工作区`：就是你在电脑里能看到的目录
- `暂存区`：英文叫 `stage` 或 `index`。一般存放在 `.git` 目录下的 `index` 文件 `（.git/index）`中，所以我们把暂存区有时也叫作索引（index）
- `版本库`：工作区有一个隐藏目录 `.git`，这个不算工作区，而是 Git 的版本库

#### 2.2.3 Git工作流程
![](/images/工具/Git/git_start_git_process.png)
一般工作流程如下：
1. 克隆Git资源作为工作目录
2. 在工作目录中添加或者修改文件
3. 如果其他人修改了，你可以更新资源
4. 在提交前查看修改
5. 提交修改，将文件的快照放入暂存区域
6. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录

### 2.3 Git安装
Git 本身支持 `Mac OS X`、`Windows`、`Linux/Unix` 这些主流的平台。

#### 2.3.1 Mac OS
在 Mac 上安装 Git 有多种方式。 最简单的方法是安装 `Xcode Command Line Tools`。 `Mavericks （10.9）` 或更高版本的系统中，在 `Terminal` 里尝试首次运行 git 命令即可:
```
git --version
```
如果没有安装过命令行开发者工具，将会提示你安装。

> 还可以通过 `homebrew` 安装Git，具体方法请参考homebrew的文档：[http://brew.sh/](http://brew.sh/)。

### 2.4 Git配置
Git 的配置主要就是 Git 命令 `git config` 的使用。
#### 2.4.1 设置用户信息
安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：
```
$ git config --global user.name "username"
$ git config --global user.email username@example.com
```
> 如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情，Git 都会使用那些信息。当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

#### 2.4.2 检查配置信息
如果想要检查你的配置，可以使用 git config --list 命令来列出所有 Git 当时能找到的配置：
```
$ git config --list
credential.helper=osxkeychain
core.excludesfile=/Users/username/.gitignore_global
difftool.sourcetree.cmd=opendiff "$LOCAL" "$REMOTE"
difftool.sourcetree.path=
mergetool.sourcetree.cmd=/Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh "$LOCAL" "$REMOTE" -ancestor "$BASE" -merge "$MERGED"
mergetool.sourcetree.trustexitcode=true
user.name=username
user.email=email@email.com
commit.template=/Users/username/.stCommitMsg
http.postbuffer=524288000
http.lowspeedlimit=0
http.lowspeedtime=999999
credential.helper=osxkeychain
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=git@github.com:username/repository.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
```
你可以通过输入 `git config <key>` 来检查 Git 的某一项配置:
```
$ git config user.name
username
```