# git 学习笔记
## git 三个分区 
- 仓库、暂存区、工作区、【没有被 git 跟踪的区，叫 untrack 区】<br>
1、 我们新建一个文件后，这个文件就是在 untrack 区。这个时候需要我们通过命令行 ` git add 新建的文件 ` 就会把新建的文件从 unstrack 区移动到工作区<br>
如果这个时候我们修改这个新建的文件，也是需要进行 ` git add 修改后的文件 ` 
2、 暂存区，就是通过 ` git commit file -m/v ` 
### git 基础命令 本地仓库操作
#### 初始化仓库
- git init 在一个空的文件夹运行该命令【建议】 会创建一个 .git文件 这个文件里面放的就是所有 git 相关的东西【仓库】
#### 主分支（master）操作
- git add 需要提交的内容
- git commit -m/v 要提交的东西 我们的每一次提交都是一次快照，代表了当时所有文件的状态。而这些所有的快照集中在 .git 中，我们的每次 reset 都是基于记录了快照才能进行切换的
- git stash 把文件放入通灵区【开玩笑的，主要是不在工作区】，这样你提交或者更新就不要担心冲突，后面可以用 git stash pop 拿出来
- .gitignore 文件描述不提交的内容、比如一些以 .lm 等等文件或者文件夹不想被提交，这个如果不会就直接去Google
- git log 看提交历史
- git status [-sb] 查询文件的状态，那些文件是被跟踪的，那些是变动的跟上次 commit 对比
- git reset --hard xxxx xxxx可以用来代表分支的东西，比如 分支名、tag、十六进制中的前6到7位 回到指定的特殊状态。
- git reflog 用来查看所有记录 包括你 reset 的记录
- git reset【Reset current HEAD to the specified state】 revert【Revert some existing commits】 restore【Restore working tree files】 之前的区别 。restore 是在 commit 之前你修改的文件进行恢复。 当你 add 以后 你只需要 --staged 就可以撤销之前的 add，然后 restore 就可以恢复你改动的地方。<br>revert 是针对已经 commit 的文件，可以恢复到你之前 commit 的任何一次。 而 reset 则是针对整体而言的，它是指可以回到每一次快照，这个不仅针对 commit，比如你做的 reset、revert、restore 当时的快照也可以回到。
##### master 不够用，分支、合并
- git branch name-branch 新建分支，是在于你所在的分支而言【当只有一个时，那就是主分支】
- git checkout name-branch 从当前分支切换到 name-branch 这个分支。注意一点的就是当你切换的时候，一定要注意你的代码跟你切换后的分支代码是否有冲突，<br>
如果有，那就会报错，那就需要 merge 或者 stash 。如果没有暂时不管
- git stash pop 把这些冲突暂时存起来，类似封装起来，让这些即不在三个分区内，方便你切换
- git merge x 选中需要保留的分区， x 是要合并到保留的分区中的分支。在这个过程中如果有冲突解决冲突，<br>
冲突就是你们两个文件有相同的地方都做了修改，你只需要把你的和别人的对比后，确定正确的逻辑，可能会留下一部分，删除一部分。
把这些冲突解决后的文件 commit 下就可以啦
- git branch -d x 合并后删除被合并的那个分支
#### 远程仓库 github
- 首先你需要让你的电脑和你的 github 账户关联起来，一种是 ssh 一种是 https 前者需要生成一对公私钥，把公钥配置到 GitHub 上面，后者则是每次你上次东西都需要填写账号，密码等（麻烦）
- 其次需要你创建一个仓库
- 然后在你电脑选择一个文件夹 运行　git init ，然后 git remote add origin xxxxx(地址) git push -u origin master 这些命令会在你创建命令时有提示的。<br> 上面的意思就是 添加远程仓库，一个叫 origin 的，地址是 xxxx，后面是 推送到上游远程仓库的 master 分支
- git clone 地址 把地址的代码拷贝到你本地
