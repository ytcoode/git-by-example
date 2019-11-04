# 合并多次提交为一次

相关命令：

```sh
# 把当前分支commit提交之后的所有提交合并为一次
# 其实该命令可以做很多事，我们这里只讲合并提交
git rebase -i <commit>

# 也可以用下面的命令，先把当前分支commit之后的
# 所有提交reset到Git的staging area，然后再把它
# 们作为一个整体重新提交
git reset --soft <commit>
git commit -m 新的提交日志
```

情景模拟：

先执行下面的命令，创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo
cd repo
git init

# 初始提交
touch a.txt
git add .
git commit -m "initial"

# 测试提交
for i in {1..5}
do
    echo $i >> a.txt
    git commit -am $i
done
```

查看当前的提交日志：

```sh
$ git -P log --oneline
b340ba5 (HEAD -> master) 5
8d2b211 4
db287a3 3
e6ee31a 2
6e209bc 1
ca16b3c initial
```

假设我们想把提交1到5合并成一次，可以执行以下命令：

```sh
$ git rebase -i ca16b3c # ca16b3c指的是initial提交
```

执行完上述命令后，Git会弹出一个编辑器，让我们指定要对提交1到5做什么操作。

此时编辑器里的内容类似下面这样：

```sh
pick 6e209bc 1
pick e6ee31a 2
pick db287a3 3
pick 8d2b211 4
pick b340ba5 5

# 后面还有一些教程性质的注释，我们这里就省略掉了
```

因为我们的目的是要把提交1到5合并成一次，所以在编辑器中，我们把第一列的内容改成下面这个样子：

```sh
pick 6e209bc 1
fixup e6ee31a 2
fixup db287a3 3
fixup 8d2b211 4
fixup b340ba5 5
```

之后，保存该文件并退出，Git就会帮我们把多次提交合并成一次了。

查看下当前的提交日志及每次提交的修改内容来验证下：

```sh
$ git -P log -p
commit 675f74d7f006b9699c33c32855077f6d941feed1 (HEAD -> master)
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Sun Nov 3 21:00:04 2019 +0800

    1

diff --git a/a.txt b/a.txt
index e69de29..8a1218a 100644
--- a/a.txt
+++ b/a.txt
@@ -0,0 +1,5 @@
+1
+2
+3
+4
+5

commit ca16b3c326c65d05bc3277580e9dabb4be9ab467
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Sun Nov 3 21:00:04 2019 +0800

    initial

diff --git a/a.txt b/a.txt
new file mode 100644
index 0000000..e69de29
```

由上可见，这五次提交确实是被合并成一次了。

当在日常开发一个系统的过程中，我们可能经常会阶段性的提交一些内容，但当我们开发完毕这个系统之后，我们应该把这些阶段性的多次提交合并成一次，这样不管是对提交日志的整洁度还是对其他人员做code review都有很大的帮助。

所以，该命令还是挺重要的。