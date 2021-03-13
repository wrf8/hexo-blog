---
title: Git --- 基础
categories:
  - 工具
  - Git
abbrlink: 28b33e30
date: 2021-03-10 23:16:21
tags:
---

## 1.获取Git仓库
通常有两种获取 `Git` 项目仓库的方式：
1. 本地创建 Git 仓库；
2. 从其它服务器 `克隆` 一个已存在的 Git 仓库

### 1.1 本地创建 Git 仓库
#### 1.1.1 创建版本库
创建版本库很简单，首先，选择一个合适的地方，创建一个空目录：
```
mkdir Git仓库目录
cd Git仓库目录
pwd //显示当前目录
```
然后通过 `git init` 命令把这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/username/Git_Repository/.git/
```
这样Git仓库就创建好了，目前还是一个空的仓库，但是该目录下多了一个 `.git` 目录，这个目录是Git来跟踪管理版本库的，不可随意改动。
> 如果你没有看到 `.git` 目录，那是因为这个目录默认是隐藏的，用 `ls -ah` 命令就可以看见。

#### 1.1.2 本地Git关联远程仓库
以 `GitHub` 为例，可以从 GitHub 仓库克隆出新的仓库，也可以关联一个已有的本地仓库，然后将本地仓库的内容推送到 GitHub 仓库。打开本地仓库，执行以下命令来进行关联：
```
git remote add origin git@github.com:username/Git_Repository.git
```
下一步，就可以把本地库的所有内容推送到远程库上：
```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样。以后只要本地有修改，就可以通过以下命令把本地master分支的最新修改推送至GitHub：
```
$ git push origin master
```
> 由于远程库是空的，我们第一次推送master分支时，加上了 `-u` 参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

#### 1.1.3 删除远程库
如果添加的时候地址写错了，或者就是想删除远程库，可以用 `git remote rm <name>` 命令。使用前，建议先用 `git remote -v` 查看远程库信息：
```
$ git remote -v
origin	git@github.com:username/Git_Repository.git (fetch)
origin	git@github.com:username/Git_Repository.git (push)
```
然后根据名字删除：
```
$ git remote rm origin
```
### 1.2 克隆现有的仓库
克隆仓库的命令是 `git clone <url>`，url为仓库地址，比如：
```
git clone https://github.com/libgit2/libgit2
```
> 这会在当前目录下创建一个名为 `libgit2` 的目录，并在这个目录下初始化一个 `.git` 文件夹， 从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝。 如果你进入到这个新建的 libgit2 文件夹，你会发现所有的项目文件已经在里面了，准备就绪等待后续的开发和使用。

如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以通过额外的参数指定新的目录名：
```
git clone https://github.com/libgit2/libgit2 mylibgit
```
这会执行与上一条命令相同的操作，但目标目录名变为了 `mylibgit`。
> `Git` 支持多种数据传输协议。上面的例子使用的是 `https://` 协议，不过你也可以使用 `git://` 协议或者使用 `SSH` 传输协议，比如 `user@server:path/to/repo.git`。

## 2.Git基本操作
### 2.1 创建仓库
| 命令 | 说明 |
| :---: | :---: |
| git init | 初始化仓库 |
| git clone | 克隆远程仓库 |

### 2.2 提交与修改
#### 2.2.1 检查当前文件状态
可以用 `git status` 命令查看哪些文件处于什么状态，对于没有任何文件修改的仓库执行完命令之后显示
![](/images/工具/Git/git_start_action_status_0.png)

#### 2.2.2 跟踪新文件
新建 `_config.yml` 文件，执行 `git status` 之后显示：
![](/images/工具/Git/git_start_action_status_1.png)
此时代表 `_config.yml` 文件还未被跟踪，我们使用命令 `git add` 来跟踪文件，运行：
```
git add _config.yml
```
然后再执行 `git status`，就会看到 `_config.yml` 文件已被跟踪，并处于暂存状态：
![](/images/工具/Git/git_start_action_status_2.png)
> `git add` 命令使用文件或目录的路径作为参数

#### 2.2.3 暂存已修改的文件
修改一个已被跟踪的文件，以 `_config.yml` 为例，修改之后执行 `git status` 命令之后，会看到下面内容：
![](/images/工具/Git/git_start_action_status_3.png)
暂存这次修改，再次执行 `git add` 命令。
> `_config.yml` 文件同时出现在暂存区 和 非暂存区，但是 `git commit`时，只会提交最后一次运行 `git add` 时的版本。

#### 2.2.4 状态预览
`git status` 命令的输出非常详细，但其用语有些繁琐。可以使用 `git status -s` 或 `git status -short` 来进行简洁输出：
![](/images/工具/Git/git_start_action_status_preview.png)
> 输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态:
> `??` : 新添加的未跟踪文件
> `A`   : 新添加到暂存区中的文件
> `M`   : 被修改的文件
> `AM` : 添加到暂存区之后又有改动

#### 2.2.5 忽略文件
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表，在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件的模式。

文件 `.gitignore` 的格式规范如下：
- 所有空行或者以 `#` 开头的行都会被 Git 忽略
- 可以使用标准的 `glob` 模式匹配，它会递归地应用在整个工作区中
- 匹配模式可以以（`/`）开头防止递归
- 匹配模式可以以（`/`）结尾指定目录
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（`!`）取反

> 所谓的 `glob` 模式是指 `shell` 所使用的简化了的正则表达式:
> `*` : 匹配零个或多个任意字符
> `[abc]` : 匹配任何一个列在方括号中的字符(要么匹配一个 `a`，要么匹配一个 `b`，要么匹配一个 `c`)
> `?` : 只匹配一个任意字符
> `[0-9]` : 匹配所有 `0` 到 `9` 的数字
> `**` : 匹配任意中间目录(比如：`a/**/z` 可以匹配 `a/z` 、 `a/b/z` 或 `a/b/c/z` 等)

`.gitignore` 文件示例：
```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

#### 2.2.6 比较暂存区和工作区的差异
比较文件在暂存区和工作区的差异，使用命令：
```
$ git diff
```
- 查看文件修改之后还没有暂存的变化部分，直接使用命令：
```
$ git diff [file]
```
- 显示已暂存文件和上次提交的差异，使用命令：
```
$ git diff --staged
或
$ git diff --cached
```
- 显示两次提交之间的差异，使用命令：
```
$ git diff [first-branch]...[second-branch]
```

#### 2.2.7 提交更新
提交暂存区到本地仓库中，命令为：
```
$ git commit -m [message] //message为提交log信息
```
提交暂存区的指定文件到本地仓库，使用命令：
```
git commit [file1] [file2] ... -m [message]
```
修改文件后，不执行 `git add` 命令，直接跳过使用暂存区来提交，使用命令：
```
git commit -a -m [message]
```
> `-a` 选项使提交包含了所有修改过的文件，执行时 Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，跳过 `git add` 步骤。
提交之后，发现漏提文件或者提交信息写错了，可以重新提交，使用命令：
```
git commit -m "message"
git add forgotten_file
git commit --amend
```
> 最终只会有一个提交——第二次提交将代替第一次提交的结果

#### 2.2.8 移除文件
1. 从工作区删除文件
从工作区中移除某个文件，可以直接删除，也可以用 `rm` 命令删：
```
rm [file]
```
这只是简单的从工作目录中删除了文件，运行 `git status` 时就会有 `Changes not staged for commit` 部分：
```
$ rm test.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        deleted:    test.md
no changes added to commit (use "git add" and/or "git commit -a")
```
然后再运行 `git rm ` 记录此次移除文件的操作，然后提交，文件从版本库中被删除：
```
$ git rm test.md
rm 'test.md'
$ git commit -m "remove test.md"
[master d46f35e] remove test.md
 1 file changed, 1 deletion(-)
 delete mode 100644 test.md
```
> 如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `git rm -f [file]`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。

2. 如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 `--cached` 选项即可：
```
git rm --cached test.md
```

#### 2.2.9 移动文件
`git mv` 命令用于移动或重命名一个文件、目录或软连接:
```
$ git mv file_from file_to
```
`git mv` 相当于运行了下面三条命令：
```
$ mv file_from file_to
$ git rm file_from
$ git add file_to
```

#### 2.2.10 撤销修改
文件改错或者删除错了，可以把文件从版本库中恢复：
```
$ git checkout -- test.md //无论时修改，还是删除，都可以从版本库中还原
```
> 从来没有被添加到版本库就被删除的文件，是无法恢复的！
