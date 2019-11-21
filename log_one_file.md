# 查看指定文件的历史修改记录

相关命令：

```sh
git log --follow -p 想要查看的文件
```

情景模拟：

先用下面的命令创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo
cd repo
git init

# 第一次提交
echo a1 > a.txt
echo b1 > b.txt
git add .
git commit -m 1

# 第二次提交
echo b2 >> b.txt
git add .
git commit -m 2

# 第三次提交
mv a.txt a.md
echo a3 >> a.md
git add .
git commit -m 3

# 第四次提交
echo a4 >> a.md
echo b4 >> b.txt
git add .
git commit -m 4
```

假设我们想要查看a.md文件的历史修改记录，可以执行下面的命令：

```sh
$ git -P log --follow -p a.md
commit ab81ba50598e91fbe7985ab2d76351fcb3a6db90 (HEAD -> master)
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 18:20:37 2019 +0800

    4

diff --git a/a.md b/a.md
index 76642d4..2caff4f 100644
--- a/a.md
+++ b/a.md
@@ -1,2 +1,3 @@
 a1
 a3
+a4

commit 1714c5b6c581ea9b9d43d03702f2d650d30bb681
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 18:20:37 2019 +0800

    3

diff --git a/a.txt b/a.md
similarity index 50%
rename from a.txt
rename to a.md
index da0f8ed..76642d4 100644
--- a/a.txt
+++ b/a.md
@@ -1 +1,2 @@
 a1
+a3

commit f550041c84c33140f8f6da910bf961f002827c99
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 18:20:37 2019 +0800

    1

diff --git a/a.txt b/a.txt
new file mode 100644
index 0000000..da0f8ed
--- /dev/null
+++ b/a.txt
@@ -0,0 +1 @@
+a1
```

由上可见，该命令正确输出了所有修改了a.md文件的提交，包括第三次提交中把a.txt改名为a.md。

有了这个命令，以后再想查看一个bug是什么时候以及谁写的，就非常方便了。