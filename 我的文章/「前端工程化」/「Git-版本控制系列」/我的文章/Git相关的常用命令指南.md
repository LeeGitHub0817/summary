

### 序言

- git日常命令总结

- 注：在使用这些命令前请安装好Git软件，地址：https://git-scm.com/downloads，同时去注册一个git类的数据仓库账号，国外的如github、gitlab，国内的如码云等。

### 1、在建好的目录下来初始化一个git项目
```
git init
```

### 2、添加文件
#### 2.1、添加所有文件
```
git add .
```

#### 2.2、添加指定文件

```
git add 文件名
eg:  git add readme.md
```

### 3、提交到仓库
```
git commit -m "说明"
eg: git commit -m "Update"
```

### 4、查看仓库状态
#### 4.1、如果你修改了某个文件，我们可以通过以下命令来查看状态
```
git status
```

#### 4.2、如果想知道某个文件具体修改了哪些内容，用以下命令
```
git diff 文件名
eg: git diff readme.md
```

### 5、显示从最近到最远的提交日志
```
git log // 查看commit id, Author, Date, commit info
git log --stat // 查看每个commit下增删查改了哪些文件
```

### 6、版本回退
在Git中，用HEAD表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

```
git reset --hard <commit id> // 回退到某个版本
eg: git reset --hard f8dad // 注: 这个值只需要取前五位即可。

git reset --hard HEAD^ // 回退到上一个版本
```

### 7、查看回退记录
git reflog 可以查看所有分支的所有操作记录（包括（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能查看已经删除了的commit记录

```
git reflog
```

这儿引用网友的一个例子：



具体一个例子，假设有三个commit:

commit3: add test3.c

commit2: add test2.c

commit1: add test1.c

如果执行git reset --hard HEAD~1则 删除了commit3，如果发现删除错误了，需要恢复commit3，这个时候就要使用git reflog



### 8、查看工作区和版本库里面最新版本的区别
```
git diff HEAD -- 文件名
eg: git diff HEAD -- readme.txt
```

>  注：每次修改，如果不用git add到暂存区，那就不会加入到commit中。
### 9、让文件回到最近一次git commit或git add时的状态。
```
git checkout -- 文件名
eg: git checkout -- readme.txt  // 把readme.txt文件在工作区的修改全部撤销

git restore 文件名
eg: git restore readme.md
```

### 10、删除版本库的文件
```
git rm 文件名
eg: git rm test.txt
```

> 之后再使用git commit -m "description"，文件就从版本库里面删除了
### 11、把误删的文件恢复到最新版本
```
git checkout HEAD -- test.txt // 这个恢复的前提是没有执行commit命令才行。
```

> 注意：git checkout -- test.txt 针对的是rm test.txt
> 如果是git rm test.txt, 参看之前的版本回退, git reset -- hard 文件名

### 12、将本地仓库推送到远端仓库
#### 12.1、建立远端地址
```
git remote add origin 你远端的github地址
eg:  git remote add origin git@github.com:michaelliao/learngit.git
```

#### 12.2、推送到远端仓库
```
git push -u origin master // 推送到主分支,以后可以直接：git push origin master
```

### 13、把创建好的远程仓库上克隆到本地
```
git clone 远程地址
eg: git clone git@github.com:michaelliao/gitskills.git
```

git里面重要概念--分支（具体概念参考教程：廖雪峰教程-分支)
---
### 14、创建分支
比如我们要创建dev分支：
```
git checkout -b dev
```

注：git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：

```
git branch dev
git checkout dev
```

### 15、切换分支和查看当前分支
#### 15.1、切换

```
git checkout master
```

#### 15.2、查看当前分支
```
git branch  // git branch命令会列出所有分支，当前分支前面会标一个*号。
```

> 注：你切换到分支上过后，就可以专注自己分支的开发，使用git add和git commit来进行操作就可以了。提交过后你切换到master分支是无法查看到刚才你在分支上提交的内容的。

### 16、合并分支
```
git merge dev   // 合并指定分支到当前分支(比如这个在master下合并dev)
```

### 17、删除分支
```
git branch -d dev // 删除dev分支
git branch -D dev // 强制删除dev分支
```

### 18、分支合并冲突
> 一般来说在合并分支时与master存在冲突的情况下只能手动去把文件修改一致才行。合并过程冲突的话，可以使用cat 文件名来查看冲突的内容，冲突部分会用<<<<<<< HEAD这样的字样标注。
### 19、分支管理策略
前面的分支操作属于Fast Forward模式，这种模式下，删除分支后，会丢掉分支信息，接下来不使用这种模式。
使用这个模式的其它操作几乎都一样，只是在合并的时候加一个参数和一个commit信息。如下：
```
git merge --no-ff -m 'no-ff' dev
```

查看分支历史图
```
git log --graph --pretty=oneline --abbrev-commit
```

### 20、实际开发使用分支管理的原则
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
### 21、bug分支
有时候我们在开发的过程中遇到bug，需要及时去修复，但是目前分支上的开发任务又没法提交，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等修复bug以后恢复现场后继续工作。
#### 21.1、保存当前分支工作现场
```
git stash //执行了这个命令后用git status就不会看到未提交的信息。
```

> 注：使用这个命令的前提是你的文件已经git的暂存区里面了。在保存好上面的工作现场后，你就需要去创建自己专门的bug修复分支，来进行修复，之后再合并和删除bug分支即可，做完这些过后你需要恢复我们开始保存好的工作现场。

#### 21.2、恢复工作现场
```
git stash list // 查看工作现场的位置
	
git stash apply // 恢复后，stash内容并不删除，你需要用git stash drop来删除

git stash pop  // 恢复的同时把stash内容也删了

git stash apply stash@{0} // 恢复指定的stash,这个stash@{0}参数可以从git stash list查看
```
### 22、features分支
当我们在开发新功能时需要新建一个分支，但是在开发完过后不要这个功能了，需要删除这个未合并的分支。
```
git branch -D feature // 删除未合并的分支需要使用大D。
```

### 23、可以抓取和推送的origin的地址
```
git remote -v // 如果没有推送权限，就看不到push的地址。
```

### 24、推送到分支
####  24.1、推送到主分支
```
git push origin master
```

#### 24.2、推送到其它分支
```
git push origin dev
```

#### 25、多人协作开发
总结为以下几步：
```
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
```
---
### 补充
#### 1、将本地分支推送到远程仓库
> `git push --set-upstream origin 分支名`
> `eg: git push --set-upstream origin dev`

#### 2、将本地代码推送到远端仓库（首次推送）

```
// 添加远程地址
git remote add origin [远端地址]

egg: git remote add origin https://github.com/cookiepool/medical-web-proj.git

// 提交到远端
git push -u origin master (master分支)
```

#### 3、取消合并操作
如果只是执行了merge命令，没有执行add命令，那输入以下命令即可取消。
```
git merge --abort
```

如果add过，我们需要这样操作，首先执行以下命令
```
git reflog
```
会显示一些你提交过的操作，选择你要回滚的提交记录上
```
git reset --hard ID号

egg: git reset --hard 7335548
```
这样就可以了。

#### 4、在建立分支的时候指定分支
```
git checkout -b hotfix/1.0.1 master
```
比如上面的命令，我如果当前在dev分支下，但是我想建立基于master的分支，想上面那样操作就可以了。

#### 5、远程仓库已经没有相关的分支了，但是本地还有
首先你可以执行以下命令来查看相关的分支信息
```
git remote show origin
```
对比过后，使用以下命令来同步一下就可以了
```
git remote prune origin
```

#### 6、本地仓库和远端仓库建立连接

```
git branch --set-upstream-to=origin/<branch>
```

比如我本地的dev-workbench分支跟远端dev-workbench建立链接。

```
git branch --set-upstream-to=origin/feature/dev-workbench
```

#### 7、比较远端分支和本地分支的区别

```
git log -p feature/dev-workbench-gzt..origin/feature/dev-workbench-gzt
```

这个命令会告诉你两个分支修改的文件，以及两个文件的代码差异

### 8、比较两个分支的差异

```
git diff branch1 branch2 --stat // 显示出所有有差异的文件列表

git diff branch1 branch2 文件命（路径） // 显示指定文件的详细差异

git diff branch1 branch2 // 显示出所有有差异的文件的详细差异
```

### 9、为git添加代理

> 设置局部代理还有一个最简单的方法就是在隐藏的`.git`文件夹下有一个`config`文件，里面就可以设置针对本项目的局部代理。

#### 全局代理
最简单的方法就是在用户文件夹找到`.gitconfig`文件，添加代理如下：
```
[http]
	proxy = socks5://127.0.0.1:10808
[https]
	proxy = socks5://127.0.0.1:10808
```
上面是添加socks5代理，如果要添加http的话，直接去掉socks5://

如果你需要命令添加全局代理，这样操作
```
http代理：
git config --global https.proxy http://127.0.0.1:10800
git config --global https.proxy https://127.0.0.1:10800

socks5代理：
git config --global http.proxy 'socks5://127.0.0.1:10800'
git config --global https.proxy 'socks5://127.0.0.1:10800'
```

查询全局代理
```
git config --global http.proxy
```

取消全局代理
```
git config --global --unset http.proxy

git config --global --unset https.proxy
```

#### 局部代理
如果你只是想给某个单独的项目添加代理的话
```
http代理：
git config --local https.proxy http://127.0.0.1:10800
git config --local https.proxy https://127.0.0.1:10800

socks5代理：
git config --local http.proxy 'socks5://127.0.0.1:10800'
git config --local https.proxy 'socks5://127.0.0.1:10800'
```

查询局部代理
```
git config --local http.proxy

git config --local https.proxy
```

取消局部代理
```
git config --local --unset http.proxy

git config --local --unset https.proxy
```

### 10、使用反悔操作
#### 修改的内容还在工作区（未执行add命令）

查看所有修改的文件的具体修改内容：

```
git diff

// 指定文件
git diff 文件命 或者 git diff -- 文件名
eg: git diff lala.txt
```

> 下面的命令都是建立在文件不是第一次建立的情况，比如你才在工作区新建的一个文件，还没做过任何add或commit操作，那么下面的命令不起作用

如果你修改了一些文件代码，但是现在想全部撤销掉修改，可以使用这个命令：
```
git checkout -- .
或者
git restore -- .
```

这样会把所有你修改过的文件全部撤销，这个撤销没有反悔操作。如果你只想撤销指定的文件，指定一下文件即可：

```
git checkout -- 文件名
eg: git checkout -- lala.txt
或者
git restore 文件命
eg: git restore lala.txt
```

#### 执行了add命令

我们执行了add命令后想取消掉add操作怎么办呢

首先查看修改了什么需要使用这个命令，上面的diff命令不显示修改，加一个参数：

```
git diff --staged

// 具体某个文件
git diff --staged 文件命
```

然后使用以下命令来取消所有暂存文件：

```
git reset .

// 具体某个文件
git reset 文件名 或者 git reset -- 文件名
```

然后你还想撤销文件的修改的话，再执行上面示例的checkout命令即可，不想分开操作的话可以直接使用以下命令，然后就能一劳永逸，全部撤销：

```
git reset --hard

// 等效于
git reset .
git checkout -- .
```

#### 修改的代码已经提到了本地仓库

如果我们的代码提交到了本地仓库（执行了commit命令）的话，可以先使用这个命名查看最近的commit ID：

```
git log
```

在打印输出的一列列的log信息里面选择你想要回滚到的地方，执行以下命令：

```
// 注意我在git版本为2.23.0的windows版本上发现用checkout不好使，它会把它切换到commit id下，类似于分支切换那种
// 这里建议使用git reset --hard Commit ID
eg: git reset --hard 537d5822b8eaf334c1fe18397fd5d03580fe64d3

git checkout '提交ID'
eg: git checkout 537d5822b8eaf334c1fe18397fd5d03580fe64d3
```

还有一种快捷的方式，不需要去查Commit ID，但是需要你自己知道想回到第几的位置，比如

```
git reset --hard HEAD^ // 回到上一个版本
git reset --hard HEAD^^ // 回到上上一个版本
git reset --hard HEAD~2 // 回到上上一个版本
```

- 注：这个方式可以进行二次反悔操作，使用`git reflog`，查看返回对应的的你反悔的Commit ID

```
2e76643 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
4d27340 HEAD@{1}: commit: second
2e76643 (HEAD -> master) HEAD@{2}: commit (initial): first
```

比如上面这串使用git reflog打印的commit信息，可以看出来我们执行的反悔记录时`2e76643 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^`，如果我们对我们的反悔操作反悔了，那么要回去，执行以下代码即可：

```
git reset --hard 4d27340
```

#### 重命名分支名

```
git branch 旧分支名 新分支名
eg: git branch dev-1 dev
```

#### git rebase

............待续


### 11、打tag
一般来说，打tag都是在master分支下

```
// 显示所有tag
git tag

// 加上-l命令可以使用通配符来过滤tag
git tag -l "v3.3.*"  // 可以把v3.3开头的tag筛选出来

// 新建tag
git tag tag名
eg: git tag v1.0.0

// 加上-a参数来创建一个带备注的tag，备注信息由-m指定
git tag -a tag名 -m 备注
eg: git tag -a v1.0.0 -m 'v1.0.0版本'

// git show命令可以查看tag的详细信息，包括commit号等
git show tag名
eg: git show v1.0.0

// 切换到当前tag
git checkout tag名
eg: git checkout v1.0.0

// 推送tag到远端
git push origin tag名
eg: git push origin v1.0.0

// 删除本地tag
git tag -d tag名
eg: git tag -d v1.0.0

// 删除远端tag
git push origin :refs/tags/<tagName>
eg: git push origin :refs/tags/v1.0.0

git push origin --delete <tagname>
eg: git push origin --delete v1.0.0
```

## 12、当两个分支信息区别大是无法合并
如果你的两个分支信息差别特别大，git是不允许你合并的，解决办法是加一个参数`--allow-unrelated-histories`
```
git merge dev --allow-unrelated-histories
```
这样就可以合并两个差别比较大的分支信息了。

## 13、合并前看两个分支是否有没合并的差异
查看 dev 有，而 master 中没有的
```
git log dev ^master 
```

## 参考资料

地址：https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6