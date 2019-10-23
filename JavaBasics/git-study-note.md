# git 学习笔记
## git 三个分区 
- 仓库、暂存区、工作区、【没有被 git 跟踪的区，叫 untrack 区】<br>
1、 我们新建一个文件后，这个文件就是在 untrack 区。这个时候需要我们通过命令行 ` git add 新建的文件 ` 就会把新建的文件从 unstrack 区移动到工作区<br>
如果这个时候我们修改这个新建的文件，也是需要进行 ` git add 修改后的文件 ` 
2、 暂存区，就是通过 ` git commit file -m/v ` 
### git 基础命令 本地仓库操作
#### 主分支（master）操作
- git add 需要提交的内容
- git commit -m/v 要提交的东西
- .gitignore 文件描述不提交的内容、比如一些以 .lm 等等文件不想被提交
- git log 看提交历史
- git status [-sb] 查询文件的状态，那些文件是被跟踪的，那些是变动的跟上次 commit 对比
- git reset --hard xxxx xxxx可以用来代表分支的东西，比如 分支名、tag、十六进制中的前6到7位 回到指定的特殊状态。
- git reflog 用来查看所有记录 包括你 reset 的记录
##### master 不够用，分支、合并
- git branch name-branch 新建分支，是在于你所在的分支而言【当只有一个时，那就是主分支】
- git checkout name-branch 从当前分支切换到 name-branch 这个分支。注意一点的就是当你切换的时候，一定要注意你的代码跟你切换后的分支代码是否有冲突，<br>
如果有，那就会报错，那就需要 merge 或者 stash 。如果没有暂时不管
- git stash pop 把这些冲突暂时存起来，类似封装起来，让这些即不在三个分区内，方便你切换
- git merge x 选中需要保留的分区， x 是要合并到保留的分区中的分支。在这个过程中如果有冲突解决冲突，<br>
冲突就是你们两个文件有相同的地方都做了修改，你只需要把你的和别人的对比后，确定正确的逻辑，可能会留下一部分，删除一部分。
把这些冲突解决后的文件 commit 下就可以啦
- git branch -d x 合并后删除被合并的那个分支
