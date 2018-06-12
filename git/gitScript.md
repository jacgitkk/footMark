#目录

FingerPrinter

[TOC]

##初始化 

创建空目录

```
$ mkdir learngit（目录名）
$ cd learngit
$ pwd
```

初始化Git仓库 

```
$ git init
```

`[点击跳转](#jump)` 

##更新提交

用命令`git add`告诉Git，把文件添加到仓库 

```
$ git add readme.txt（目标文件名）
```

用命令`git commit`告诉Git，把文件提交到仓库 

```
$ git commit -m "wrote a readme file"（版本描述）
$ git commit -a ???
```

##导航

###状态与日志

运行`git status`命令掌握仓库当前的状态 

```
$ git status
```

`git diff`顾名思义就是查看仓库缓存及工作区对象difference 

```
$ git diff readme.txt （目标文件名）
```

版本控制系统的历史记录，用`git log`命令查看；

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数 

```
$ git log
$ git log --pretty=oneline
```

###回退与前进

在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100` 

```
$ git reset --hard HEAD^
```

Git提供了一个命令`git reflog`用来记录你的每一次命令： 

` reset`让`HEAD`指向哪个版本号，你就把当前版本定位在哪，然后顺便把工作区的文件更新了。

```
$ git reflog
$ git reset --hard 1094a(commit id)
```

###GIt 资源结构

工作区（Working Directory）就是你在电脑里能看到的目录，文件夹就是一个工作区 

版本库（Repository）工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库 

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的history第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。 

![1528443612717](C:\Users\Jack\AppData\Local\Temp\1528443612717.png)

`git add`把文件添加进去，实际上就是把文件修改添加到暂存区 

`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支 

`git status`查看工作区与stage的状态 

`git diff HEAD -- 文件名.类型`命令可以查看工作区和版本库里面最新版本的区别

###撤销修改

*场景1*

`git checkout -- file`可以丢弃工作区的修改 ，

命令中的`--`很重要，用来标识当前分支，没有`--`，就变成了“切换到另一个分支”的命令 ：

```
$ git checkout -- readme.txt
```

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态；

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

*场景2*

用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区： 

```
$ git reset HEAD readme.txt
```

`git reset`之后`git status`可以发现暂存区是干净的，工作区有修改，

进而可以利用`git checkout -- file`进一步丢弃工作区的修改。

###删除与恢复

文件管理器中把没用的文件删了，或者用`rm`命令删了 

```
$ rm test.txt
```

`git status`命令会立刻告诉你哪些文件被删除了，

确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`： 

```
$ git status
$ git rm test.txt
$ git commit -m "remove test.txt"
```

删错了，因为版本库里还有，用`git checkout/reset+checkout`可以用版本库里的临时/正式版本替换工作区的版本，实现还原恢复。 

## 远程仓库与 GitHub 

打开Git Bash，创建SSH Key，在用户主目录下，.ssh目录下`id_rsa`和`id_rsa.pub`这两个文件 

`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥 

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

远程仓库与 GitHub，“Add SSH Key”，任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容 

###远程库添加/推送/连接

本地创建了一个Git仓库后，想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步 

把一个已有的本地仓库与之关联 

```
$ git remote add origin git@github.com:userName/gitrepoName.git
```

把本地库的所有内容推送到远程库上,用`git push`命令，实际上是把当前分支`master`推送到远程 

```
$ git push -u origin master  
(第一次推送master分支时，加上-u参数，不但会把master内容推送到远程，还会把本地的master分支和远程的master分支关联起来)
$ git push origin master
日常性本地提交
```

SSH警告

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

###远程库克隆

用命令`git clone`克隆一个本地库 

```
$ git clone git@github.com:userName/gitrepoName.git
$ cd gitskills （gitrepoName）
$ ls
```

Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

用`https`除了速度慢以外，有个最大的麻烦是每次推送都必须输入口令。

##分支管理

`master`指向提交，`HEAD`指向的就是当前分支。 

###创建分支

创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上： 

```
$ git checkout -b dev
```

命令加上`-b`参数表示创建并切换，相当于以下两条命令： 

```
$ git branch dev
$ git checkout dev
```

用`git branch`命令查看当前分支： 

```
$ git branch
```

###合并分支`Fast-forward` 

![git-br-ff-merge](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0) 

切换回`master`分支 

```
$ git checkout master
```

合并某分支到当前分支，把`dev`分支的工作成果合并到`master`分支上： 

```
$ git merge dev
```

合并完成后，就可以放心地删除`dev`分支了： 

```
$ git branch -d dev
```

### 冲突处理

![git-br-conflict-merged](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0) 

合并就可能会有冲突， `git status`也可以告诉我们冲突的文件 

```
$ git merge feature1
$ git status
```

查看文件，Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改，再提交： 

```
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

用带参数的`git log`也可以看到分支的合并情况 ，`git log --graph`命令可以看到分支合并图，最后，删除`feature1`分支： 

```
$ git log --graph --pretty=oneline --abbrev-commit
$ git branch -d feature1
```

###分支策略`--no-ff`

`--no-ff`参数，表示禁用`Fast forward` ，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。 

因为合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去 

```
$ git merge --no-ff -m "merge with no-ff" dev
```

合并后，我们用`git log`看看分支历史： 

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
```

![git-br-policy](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0) 

- `master`分支应该是非常稳定的，也就是仅用来发布新版本 
- 干活都在`dev`分支上，到版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布版本 
- 每个人都有自己的分支，时不时地往`dev`分支上合并 

### Bug分支

处理Bug需要暂停当前工作，`stash`功能，可以把当前工作现场“储藏”起来，等待后续恢复现场，继续工作 。

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支： 

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

回到`dev`分支,用`git stash list`命令看看储藏的工作区

```
$ git checkout dev
$ git status
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

恢复工作区：

一种是用`git stash pop`，恢复的同时把stash内容也删；

> 另一种是用`git stash apply`恢复，你需要用`git stash drop`来手动删除stash内容；但可以多次stash，不删除，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令参数 `stash@{0}`

```
$ git stash pop

$ git stash list
$ git stash apply stash@{0}
```

###Feature分支

开发一个新feature，最好新建一个分支，以方便管理开发更新与合并控制；

- Master合并管理、发布控制；
- Common公共共有的功能；
- Feature特定指向性的功能；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

```
$ git checkout -b feature-vulcan
$ git add vulcan.py
$ git status
$ git commit -m "add feature vulcan"
$ git checkout dev
$ git merge feature-vulcan

$ git branch -D feature-vulcan
```

##多人协作

###协作方从远程仓库克隆

Git自动把本地的`master`分支和远程的`master`分支对应，要查看远程库的信息，用`git remote` 远程仓库的默认名`origin` 

```
$ git remote
origin
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

###推送分支

Git把该分支上的所有本地提交推送到远程库 

```

$ git push origin master

$ git push origin dev
```

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

###抓取分支

从远程库clone时，默认情况下，只能看到本地的`master`分支。

可以用`git branch`命令查看： 

```
$ git branch
* master
```

要在`dev`分支上开发，必须创建远程`origin`的`dev`分支到本地： 

```
$ git checkout -b dev origin/dev

$ git add env.txt
$ git commit -m "add env"

$ git push origin dev
```

当协作方的最新提交和你试图推送的提交有冲突时，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送： 

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

```
$ git branch --set-upstream-to=origin/dev dev
$ git pull

（人工手动解决冲突）
$ git commit -m "fix env conflict"  

$ git push origin dev
```

##基线管控

Git用`(HEAD -> master)`和`(origin/master)`标识出当前分支的HEAD和远程origin的位置 

```
$ git status
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
```

命令`git rebase` 操作：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交被修改。 

```
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: add comment
Using index info to reconstruct a base tree...
M    hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
Applying: add author
Using index info to reconstruct a base tree...
M    hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
```

通过`push`操作把本地分支推送到远程，用`git log`可看到效果

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master) add author
* 3611cfe add comment
* f005ed4 (origin/master) set exit=1
* d1be385 init hello

$ git push

$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master, origin/master) add author
* 3611cfe add comment
* f005ed4 set exit=1
* d1be385 init hello

```

## 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。 

### 创建标签

切换到需要打标签的分支上 , 命令`git tag <name>`就可以打一个新标签 ,默认标签是打在最新提交的commit上的。 

```
$ git branch
* dev
  master
$ git checkout master

$ git tag v1.0
```

命令`git tag`查看所有标签 

```
$ git tag
v1.0
```

标签到特定commit时：

```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file

$ git tag v0.9 f52c633

$ git tag
v0.9
v1.0
```

命令`git tag`，用`-a`指定标签名，`-m`指定说明文字

标签查询不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息 ，带有说明的标签： 

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb

$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
```

### 操作标签

推送标签到远程 

```
$ git push origin v1.0        （单个）
$ git push origin --tags      （多个）
```

标签打错了，也可以删除 

```
$ git tag -d v0.1                             （本地）
Deleted tag 'v0.1' (was f15b0dd)

$ git push origin :refs/tags/v0.9             （远程）
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```

##GitHub

通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。 

参与一个开源项目，可以访问它的项目主页，点“Fork”就在自己的账号下克隆了一个仓库，然后，从自己的账号下clone。

一定要从自己的账号下clone仓库，这样你才能推送修改。 

![github-repos](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384926554932eb5e65df912341c1a48045bc274ba4bf000/0) 

如果修复了bug，或者新增了功能， 希望官方库能接受修改，你就可以在GitHub上发起一个pull request。 













