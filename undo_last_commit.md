# 撤销上次提交

最终命令：

```sh
git reset HEAD^        # 上次提交内容会被保存到工作目录
git reset --hard HEAD^ # 上次提交内容会被直接丢弃
```

情景模拟：

先使用下面的命令初始化一个测试用的Git仓库：

```sh
# 初始化一个空的Git仓库
mkdir repo && cd repo
git init

# 将a.txt加入到版本控制中
echo A1 > a.txt
git add .
git commit -m 1

# 将a.txt的内容修改为A2并提交
echo A2 > a.txt
git commit -am 2
```

执行完上面的命令后，看下当前的Git日志：

```sh
$ git -P log --pretty=oneline --abbrev-commit
4490479 (HEAD -> master) 2
bf92587 1
```

假设我们想撤销上次提交，但上次提交的内容不丢弃，可以使用下面的命令：

```sh
$ git reset HEAD^
Unstaged changes after reset:
M  a.txt

$ git -P log --pretty=oneline --abbrev-commit
bf92587 (HEAD -> master) 1

$ cat a.txt
A2
```

由上可见，reset命令撤销了上次提交，并把这次提交的内容保存到了工作目录。

如果我们想撤销上次提交，并且丢弃上次提交修改的内容，可以用另外一条reset命令，这个就不在这里演示了，有兴趣的同学可以自己试下。