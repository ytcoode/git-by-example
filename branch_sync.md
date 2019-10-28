# 同步本地分支的添加删除状态到远程（或反之）

相关命令：

```sh
# 遍历本地仓库中的所有分支，如果该分支在远程仓库中不存在，则在远程仓库中创建该分支
# 遍历远程仓库中的所有分支，如果该分支在本地仓库中不存在，则在远程仓库中删除该分支
git push --all --prune

# 遍历远程仓库中的所有分支，如果该分支在本地仓库中没有对应的远程追踪分支，则在本地仓库中创建该分支
# 遍历本地仓库中的所有远程追踪分支，如果该分支在远程仓库中没有对应的分支，则将其删除
git fetch --prune
```

情景模拟：

为了方便测试，我们先在GitHub上创建一个名为git-test-repo的仓库，然后将其克隆到本地，之后，我们再用相应的命令创建一个测试分支，并将其同步到远端，具体命令如下：

```sh
$ git clone https://github.com/wangyuntao/git-test-repo.git repo1
# 省略输出 #
$ cd repo1
$ git push origin master:b3 # 创建一个远程分支b3
# 省略输出 #
$ git -P branch -avv        # 查看当前分支状态
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b3     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

我们再打开一个终端，将该仓库再克隆一份到本地备用：

```sh
$ git clone https://github.com/wangyuntao/git-test-repo.git repo2
# 省略输出 #
$ cd repo2
$ git -P branch -avv
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b3     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

现在我们回到repo1中，执行下面的命令：

```sh
$ git branch b1          # 创建本地分支b1
$ git branch b2          # 创建本地分支b2

$ git push --all --prune # 将本地分支的添加删除状态同步到远程
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/wangyuntao/git-test-repo.git
 - [deleted]         b3
 * [new branch]      b1 -> b1
 * [new branch]      b2 -> b2
 
$ git -P branch -avv     # 查看当前的分支状态
  b1                    ab5a63d Initial commit
  b2                    ab5a63d Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b1     ab5a63d Initial commit
  remotes/origin/b2     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

由上可见，因为本地仓库中没有b3分支，所以 git push --all --prune 命令删除了远程仓库中的b3分支，又因为本地仓库中新建了b1和b2分支，所以该命令在远程仓库中也创建了这两个分支。

现在我们再切换到repo2，执行下面的命令：

```sh
$ git branch b3 origin/b3 # 创建远程追踪分支origin/b3的本地分支b3
Branch 'b3' set up to track remote branch 'b3' from 'origin'.

$ git -P branch -avv
  b3                    ab5a63d [origin/b3] Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b3     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
  
$ git fetch --prune       # 将远程分支的添加删除状态同步到本地
From https://github.com/wangyuntao/git-test-repo
 - [deleted]         (none)     -> origin/b3
 * [new branch]      b1         -> origin/b1
 * [new branch]      b2         -> origin/b2
 
$ git -P branch -avv
  b3                    ab5a63d [origin/b3: gone] Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b1     ab5a63d Initial commit
  remotes/origin/b2     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

由上可见，因为远程仓库中的b3分支被删除，并且又创建了b1和b2分支，所以 git fetch --prune 命令删除了本地仓库中的远程追踪分支 origin/b3（但没有删除本地分支b3），并创建了远程追踪分支 origin/b1 和 origin/b2。

到这里，有关本地仓库和远程仓库分支添加删除状态的同步就讲完了，希望对你有所帮助。