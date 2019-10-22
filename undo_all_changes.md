# 撤销对所有文件的修改

最终命令：

```sh
$ git reset --hard # 撤销所有文件的修改（不算未进入版本控制的文件）
$ git clean -fd    # 删除所有未进入版本控制的文件
```

下面用一个例子展示下这两个命令的使用。

先用下面的命令初始化一个测试用的Git仓库：

```sh
$ mkdir repo
$ cd repo
$ git init         # 初始化一个空Git仓库

$ echo a > f1.txt
$ git add .
$ git commit -m f1 # 将f1.txt加入到版本控制中

$ echo b > f1.txt  # 修改f1.txt的内容
$ touch f2.txt     # 创建新文件f2.txt，其并未进入到版本控制中
```

执行完上面的命令后，我们已经有了一个可供测试的Git仓库。

再用下面的命令看下文件的变化情况：

```sh
$ git status -s
 M f1.txt
?? f2.txt

$ git -P diff
diff --git a/f1.txt b/f1.txt
index 7898192..6178079 100644
--- a/f1.txt
+++ b/f1.txt
@@ -1 +1 @@
-a
+b
```

由上可见，f1.txt的内容由a变为了b，f2.txt是新创建的，还未进入到版本控制中。

现在执行上面的撤销命令，看下是如何撤销修改的：

```sh
$ git reset --hard
HEAD is now at 5b3c640 f1

$ git status -s
?? f2.txt

$ git clean -fd
Removing f2.txt

$ git status
On branch master
nothing to commit, working tree clean
```

由上可见，执行完reset命令后，f1.txt文件的修改被撤销，但f2.txt文件还在。

执行完clean命令后，f2.txt文件也不在了。

至此，两条命令撤销了对所有文件的修改，Git仓库回到了原始状态。