# git 学习笔记
## git 一些基础概念

- 仓库的本质<br> 1、仓库是一个可以追溯历史的文件夹。<br>2、仓库最小单位是仓库在某个时刻的状态（快照）。<br>
3、标识仓库在某个时刻的状态（快照）。如下四种：commit 对象的 sha1（不需要写全，唯一就行）、HEAD(当前指针)、branch 名字、tag、十六进制中的前6到7位
- commit 的本质<br> 从某个时刻的状态转移另一个时刻的状态的变化。commit 是不可变的
- branch 的本质 <br>
1、一个可以移动的指针，它指向某个提交对象，一个提交对象可以有多个指针指向它。每次提交，对应分支的指针向前移动一位<br>
2、新建分支，就是新建一个指针，它开始是指向原分支的那个对象，随着这个分支的提交，新建分支也是随着移动<br>
3、HEAD 特殊的指针，指的是当前最新提交
- merge 的本质<br>
1、将两个或者多个状态（代码快照\副本）合并，并产生一个新的状态提交<br>
2、逐行检查，发现冲突，尝试解决，解决不了，报错，需要人工干预<br>
3、解决冲突，就是修改冲突的文件，选择那些冲突应该正确的东西<br>
4、严格按照提示进行操作， -- continue(继续)、skip(跳过)、abort(忽略)
- rebase 的本质<br>
将某个状态上的变更（commit）进行重演，在另一个分支上的基础上重演提交。
- fetch 的本质 
就是获取远程分支的最新代码，它不会合并本地的分支
- pull 的本质
pull = fetch + merge<br>
pull --rebase=fetch +rebase
- push 的本质
你 push 的前提是你本地分支跟你要 push 的远程分支是同一个祖先
### rebase 、merge、merge --squash区别和注意事项
1、在当前分支 rebase 另一个分支，那历史就是在另一个分支的历史状态基础上，将当期分支的提交进行重演，本分支的历史会是一条直线<br>
2、在当前分支 merge 另一个分支，在本分支会记录令一个分支的提交历史，然后解决冲突产生一个新的提交，本分支除了自己，还会有另一个分支的历史
3、在当前分支 merge --squash 将另一个分支的所有提交合并成一个提交，本分支除了自己，还会有另一个分支的历史<br>
#### 我们有如下状态的三对分支：

```
	  A---B---C test-my-feature-1
	 /
    D---E---F test-master-1
    
	  A---B---C test-my-feature-2
	 /
    D---E---F test-master-2

	  A---B---C test-my-feature-3
	 /
    D---E---F test-master-3 
```

### merge

请在`test-master-1`分支上合并`test-my-feature-1`分支，并产生一个合并提交，如图所示：

```
	  A---B---C test-my-feature-1
	 /         \
     D---E---F------G test-master-1
```

### rebase

请在`test-my-feature-2`分支上rebase `test-master-2`分支，如图所示：

```
D---E---F---A'---B'---C' test-my-feature-2
        |
        test-master-2
```

### merge --squash练习

请将`test-my-feature-3`分支上的3个提交使用`merge --squash`方式，使得这三个提交被合并成一个提交进入`test-master-3`分支

```
	  A---B---C test-my-feature-3
	 /
    D---E---F---G test-master-3 
                |
               压扁后的提交
```
<br>
上面的这些图就是可以区分这三种的关系。
#### 注意事项
1、rebase 命令 最好是在你独占的分支上面使用这个命令。<br>
2、merge 历史会比较混乱
3、merge --squash 会丢失你要合并的那个分支的历史

- 仓库、暂存区、工作区、【没有被 git 跟踪的区，叫 untrack 区】<br>
1、 我们新建一个文件后，这个文件就是在 untrack 区。这个时候需要我们通过命令行 ` git add 新建的文件 ` 就会把新建的文件从 unstrack 区移动到暂存区，<br> 
2、commit 只从暂存区提交数据。当你修改一个数据后，它就从暂存区回到工作区，你需要 git add 下这个文件，让它从工作区回到暂存区。commit 则是让暂存区的文化移动到看仓库。

 
## git 基础命令 本地仓库操作
### 初始化仓库
- git init 在一个空的文件夹运行该命令【建议】 会创建一个 .git文件 这个文件里面放的就是所有 git 相关的东西【仓库】
### 一些常用操作
- git add 需要提交的内容
- git commit -m/v 要提交的东西 我们的每一次提交后都是一次快照，代表了当时所有文件的状态。而这些所有的快照集中在 .git 中，我们的每次 reset 都是基于记录了快照才能进行切换的
- git stash 把文件放入通灵区【开玩笑的，主要是不在工作区】，这样你提交或者更新就不要担心冲突，后面可以用 git stash pop 拿出来
- .gitignore 文件描述不提交的内容、比如一些以 .lm 等等文件或者文件夹不想被提交，这个如果不会就直接去Google
- git log 看提交历史 --oneline 可以简化显示内容
- git reflog 查看所有操作历史，包括你 reset、revert 等操作的记录
- git status [-sb] 查询文件的状态，那些文件是被跟踪的，那些是变动的跟上次 commit 对比
- git reset --hard xxxx xxxx可以用来代表分支的东西，比如 分支名、tag、十六进制中的前6到7位 回到指定的特殊状态。<br>
-- hard --mixed --soft 它们之间的区别 首先 mixed 是默认的选项，它和 soft 一样只是把指针移动到指定的状态，但是仓库的之前的那些提交不会变动。<br> mixed 与 soft 区别是 当你 git status 时，会发现 soft 是跟 git stauts 显示一样，暂存区和工作区的变动东西都会显示，但是 mixed 只会显示工作区的变动，暂存区不会显示。<br>
hard 则是移动指针到指定的状态同时，之前的那些提交会消失掉。仓库此时的状态全部回到你指定的状态

- git reset【Reset current HEAD to the specified state】 revert【Revert some existing commits】 restore【Restore working tree files】 之前的区别 。restore 是在 commit 之前你修改的文件进行恢复。 当你 add 以后 你只需要 --staged 就可以撤销之前的 add，然后 restore 就可以恢复你改动的地方。<br>revert 是针对已经 commit 的文件，可以恢复到你之前 commit 的任何一次。 而 reset 则是针对整体而言的，它是指可以回到每一次快照，这个不仅针对 commit，比如你做的 reset、revert、restore 当时的快照也可以回到。
- git branch name-branch 新建分支，是在于你所在的分支而言【当只有一个时，那就是主分支】
- git checkout name-branch 从当前分支切换到 name-branch 这个分支。注意一点的就是当你切换的时候，一定要注意你的代码跟你切换后的分支代码是否有冲突，<br>如果有，那就会报错，那就需要 merge 或者 stash 。如果没有暂时不管<br>
git checkout -b new-branch-name 切换的分支 在切换分支的同时也新建看一个切换的分支
- git stash pop 把这些冲突暂时存起来，类似封装起来，让这些即不在三个分区内，方便你切换
- git merge x 选中需要保留的分区， x 是要合并到保留的分区中的分支。在这个过程中如果有冲突解决冲突，<br>
冲突就是你们两个文件有相同的地方都做了修改，你只需要把你的和别人的对比后，确定正确的逻辑，可能会留下一部分，删除一部分。
把这些冲突解决后的文件 commit 下就可以啦
- git branch -d x 合并后删除被合并的那个分支
#### 高级命令
- cherry pick xxx 将某个提交在另外一个分支重放。意思就是比如你现在修改的了一个 bug 你提交以后，发现你之前的版本也有这个 bug 。这个时候，<br>
你就可以在你之前的版本选择一个，进入 运行 cherry pick xxx(你之前修改 bug 后的提交 sha1)
- bisect 在历史中查找某处引入的 bug。当你发现你现在的版本有 bug ，而你通过日志一些也不好发现是谁提交的，可以通过这个来找到第一次引入 bug 的提交<br>
你需要首先进入最新的分支，git bisect bad，然后根据提示确定 Y,然后进行最开始的分支,git bisect good。这个时候一般有会提示教你怎么做的。但是当版本多的时候，就需要通过脚本来帮我们手工操作，这个时候key git bisect run ./xxx.sh
## 远程仓库 github
git 是分布式系统管理，远程仓库和本地仓库、fork本质都是复制了整个文件夹。本地仓库和远程仓库是一致的，当远程挂了，你本地也可以当远程使用。
- 首先你需要让你的电脑和你的 github 账户关联起来，一种是 ssh 一种是 https 前者需要生成一对公私钥，把公钥配置到 GitHub 上面，后者则是每次你上次东西都需要填写账号，密码等（麻烦）
- 其次需要你创建一个仓库
- 然后在你电脑选择一个文件夹 运行　git init ，然后 git remote add origin xxxxx(地址) git push -u origin master 这些命令会在你创建命令时有提示的。<br> 上面的意思就是 添加远程仓库，一个叫 origin 的，地址是 xxxx，后面是 推送到上游远程仓库的 master 分支
- git clone 地址 把地址的代码拷贝到你本地
### github 如果你不用它作为版本服务器的话，那就自己搭建私有的服务器 gitlab、gogs 具体怎么搭建，可以在网上搜索
## git 工作流
- git 一般维护两个稳定的分支，master、develop 一个主分支，一个是开发分支。
几个临时分支 bugfix、release、feature 等<br>
我们工作，首先是创建一个 develop 分支，这个时候可能会根据不同的功能，新建多个 feature 分支，等 feature 等分支开发人员自测没有问题，那就合并到 develop 分支，同时删除原有的 feature 等分支<br>
这个时候就可以在 develop 分支上新建一个 release 分支，用来测试，测试的 bug 也是在这个分支上面改动。等测试通过了，就把这个 release 分支合并到 master 分支，develop 分支。同时删除 release 分支，然后在 master 分支上 tag 一个版本拿到线上运行。<br>
bugfix 分支是发现了有 bug,那就从 master 上新建一个分支出来修复后，再是 release 一套流程<br>
总之，master 对应正式上线版本的分支，确定稳定可靠。develop 开发分支，release 分支是测试分支，测试通过就删除掉，bugfix 则是运行中发现 bug 就临时新建的分支<br> feature 则是功能分支。它们都是临时的，完成对应的任务后就会合并到 master、develop 中，就会被删除

# 重点，当你是在敲击命令行时，关注命令行的提示，一般按照提示操作就可以解决问题。第二当你真正的做项目时，项目大了，合并这些还是要学会使用工具，而不是敲命令。最后一个就是出现解决不了问题，google 一下。
