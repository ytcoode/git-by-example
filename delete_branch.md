# 删除本地分支及远程分支

相关命令：

```sh
git branch -d 要删除的分支      # 删除本地分支
git branch -D 要删除的分支      # 强制删除本地分支
git push -d origin 要删除的分支 # 删除远程分支
```

情景模拟：

为了方便测试，我们先在GitHub上创建一个名为git-test-repo的仓库，然后将其克隆到本地，并看下其当前的分支情况：

```
$ git clone https://github.com/wangyuntao/git-test-repo.git
Cloning into 'git-test-repo'...
# 省略部分输出 #
$ cd git-test-repo
$ git -P branch -avv
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master ab5a63d Initial commit
```

由上可见，该仓库目前只有本地分支master，其对应的远程分支为origin/master（就是该仓库在GitHub上的master分支）。

下面我们用[上一篇文章](create_branch.md)中介绍过的命令，创建一个测试分支，并同步到远端：

```sh
$ git branch b1
$ git push --set-upstream origin b1
# 省略输出 #
$ git -P branch -avv
  b1                    ab5a63d [origin/b1] Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b1     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

由上可见，我们创建了一个本地分支b1，然后将其同步到了GitHub上（orgin/b1）。

下面我们来测试下对应的删除命令。

先删除本地分支：

```sh
$ git branch -d b1   # 删除本地分支b1
Deleted branch b1 (was ab5a63d).
$ git -P branch -avv # 查看当前分支情况
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b1     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

由上可见，本地的b1分支已经没有了，但其对应的远程分支origin/b1还在。

我们再用下面的命令删除其对应的远程分支：

```sh
$ git push -d origin b1 # 删除远端的b1分支
To https://github.com/wangyuntao/git-test-repo.git
 - [deleted]         b1
$ git -P branch -avv    # 查看当前的分支情况
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master ab5a63d Initial commit
```

由上可见，b1对应的远程分支origin/b1也被删除了，此时如果我们到GitHub上看一下的话，也会发现，b1分支已经没有了。

好了，到这里有关本地分支及远程分支的删除操作就已经讲完了，希望对你有所帮助。