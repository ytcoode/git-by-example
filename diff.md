# 灵活使用git diff命令

相关命令：

```sh
# 比较当前工作区和Git的staging area里内容的区别
git diff

# 比较Git的staging area和当前分支指向内容的区别
git diff --staged

# 比较任意两次提交指向内容的区别
git diff <commit> <commit>
```

情景模拟：

先执行下面的命令，创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo
cd repo
git init

# 提交一次
echo a1 > a.txt
git add .
git commit -m "Initial commit"
```

然后再执行下面的命令，对a.txt文件做一些修改：

```sh
echo a2 > a.txt
```

最后我们执行两次diff命令（参数不一样），看下输出有什么不同：

```sh
$ git -P diff # 参数-P可以不管，下同
diff --git a/a.txt b/a.txt
index da0f8ed..c1827f0 100644
--- a/a.txt
+++ b/a.txt
@@ -1 +1 @@
-a1
+a2

$ git -P diff --staged # 没有任何输出
```

由上可见，没有--staged参数的diff命令输出了文件变化，而有--staged参数的diff命令没有任何输出，即表示没有任何变化。

这是因为，没有--staged参数的diff命令比较的是工作区和Git的staging area里的内容的区别，因为我们上面修改了a.txt文件，即工作区里的内容变化了，但此时Git的staging area里的内容并没有任何变化，即还是原内容，所以该次diff命令就正确输出了我们对a.txt文件的修改。

而有--staged参数的diff命令比较的是Git的staging area和当前分支指向的内容的区别，因为此时这两个地方的内容都没有变化，所以该次diff命令没有任何输出。

我们再执行下面的命令，看下这次不同的diff命令有怎样的输出：

```sh
$ git add a.txt # 将a.txt的修改提交到Git的staging area
$ git -P diff   # 没有任何输出
$ git -P diff --staged
diff --git a/a.txt b/a.txt
index da0f8ed..c1827f0 100644
--- a/a.txt
+++ b/a.txt
@@ -1 +1 @@
-a1
+a2
```

这次的结果正好反过来了，有--staged参数的diff命令有输出，而没有--staged参数的diff命令没有输出。

这是因为通过上面的git add命令，工作区里的文件内容已经同步到了Git的staging area里，所以此时这两个地方的文件内容是一样的，这样就导致了第一次diff命令没有任何输出。

但正是因为这次同步导致的Git的staging area里的内容变化，使其和当前分支指向的内容不再相同（当前分支指向的还是原内容），所以有第二次diff命令就有了输出。

继续上面的例子，我们再执行下面的命令：

```sh
$ git commit -m 1
[master 867736c] 1
 1 file changed, 1 insertion(+), 1 deletion(-)
 
$ git -P diff          # 无任何输出
$ git -P diff --staged # 无任何输出
```

因为这次先执行了git commit命令，导致staging area里的内容被同步到了Git仓库里，所以这两次diff命令都没有任何输出。

注意，我们在描述git add和git commit命令时，用的都是同步文件内容到下一个区域，而不是添加这次的修改到下一个区域，这个概念是很重要的。

Git在进行版本管理时，保存文件的地方分为三个区域，分别是工作区、staging area 和 Git仓库，我们要把这三个区域都想像成各自保存了所有文件的一份拷贝，而不是存放的一次次提交的零散的变化。

当我们修改文件时，我们改动的是工作区里的内容。

当我们执行git add命令时，我们是把对工作区的修改同步到了staging area里，使其当前内容和工作区内容相同。

当我们执行git commit命令时，我们是把staging area里变动的部分同步到了Git仓库里，使Git仓库里的内容和工作区以及staging area里的内容都相同。

所有命令的执行，目的都是将上一区域里变化的内容同步到下一区域，使这两个区域之间的内容完全相同。

用这种方式思考Git的版本管理机制，对于我们日后理解Git的各种命令有非常大的帮助。

话题太大，这里就不再展开了。

最后我们再来看下不同提交之间的比较是什么样子的。

我们先在当前Git仓库的基础上继续执行下面的命令：

```sh
# 创建并切换到分支b
git checkout -b b

# 在分支b上连续提交两次
echo b1 > a.txt && git commit -am b1
echo b2 >> a.txt && git commit -am b2

# 切换回master分支
git checkout -

# 在master分支上再连续提交两次
echo m1 > a.txt &&  git commit -am m1
echo m2 >> a.txt && git commit -am m2
```

执行完上述命令后，我们用下面的命令比较下当前b分支和master分支的区别：

```sh
$ git -P diff master b
diff --git a/a.txt b/a.txt
index b5692e5..9b89cd5 100644
--- a/a.txt
+++ b/a.txt
@@ -1,2 +1,2 @@
-m1
-m2
+b1
+b2
```

看到了吧，这次的diff命令清晰的显示了两个分支对a.txt做的修改，以及他们导致的冲突。

该命令在分支合并时是非常有用的，我们可以使用该命令在合并前看下被合并分支对当前分支的文件内容做了哪些修改。

git diff命令还有很多更好玩和更加强大的执行方式，限于篇幅原因，这里就不一一指出了，有兴趣的同学可以看下Git自带的文档，执行 `git help diff` 就可以看到了。

完。