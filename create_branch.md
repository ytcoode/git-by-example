# 创建本地分支及远程分支

相关命令：

```sh
git branch 新分支名 # 基于当前分支创建一个新分支
git push --set-upstream origin 新分支名 # 将新分支推送到远端
```

情景模拟：

为了方便测试，我们先在GitHub上创建一个名为git-test-repo的仓库，然后将其克隆到本地，并看下其当前的分支情况：

```sh
$ git clone https://github.com/wangyuntao/git-test-repo.git
Cloning into 'git-test-repo'...
# 省略部分输出 #
$ cd git-test-repo
$ git -P branch -avv
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master ab5a63d Initial commit
```

由上可见，该仓库目前有个本地分支master，其对应的远程分支为origin/master（就是该仓库在GitHub上的master分支）。

现在我们基于master分支，再创建一个分支b1：

```sh
$ git branch b1      # 创建分支b1
$ git -P branch -avv # 查看当前分支情况
  b1                    ab5a63d Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master ab5a63d Initial commit
```

由上可见，该仓库现在多了一个本地分支b1，但其目前并没有对应的远程分支。

下面我们用git push命令，为b1创建一个远程分支。

```sh
$ git push --set-upstream origin b1 # 将本地b1分支推送到远端
# 省略输出 #
$ git -P branch -avv # 查看当前分支情况
  b1                    ab5a63d [origin/b1] Initial commit
* master                ab5a63d [origin/master] Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/b1     ab5a63d Initial commit
  remotes/origin/master ab5a63d Initial commit
```

由上可见，在执行完上面的git push命令后，本地b1分支就有了对应的远程分支origin/b1。

此时，如果我们到GitHub上的仓库去看下的话，也是能找到这个分支的。

这样，一个本地分支对应的远程分支就创建成功了。