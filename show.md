# 查看指定提交的修改内容

相关命令：

```sh
# 查看<commit>提交修改的文件
git show --stat <commit>

# 查看<commit>提交修改的文件及内容
git show <commit>
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
echo a2 >> a.txt
echo b2 >> b.txt
git add .
git commit -m 2
```

然后执行下面的命令，查看该Git仓库的历史提交记录：

```sh
$ git -P log
commit f0ddc4a3e1e8d4adc50cbfe6d02c66ff4e53416c (HEAD -> master)
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 17:35:32 2019 +0800

    2

commit 7dd760e27ef7716ac603b104d4841170afd501a6
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 17:35:32 2019 +0800

    1
```

假设我们想查看第一次提交修改了哪些文件，可以执行下面的命令：

```sh
$ git -P show --stat 7dd760e27ef7716ac603b104d4841170afd501a6
commit 7dd760e27ef7716ac603b104d4841170afd501a6
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 17:35:32 2019 +0800

    1

 a.txt | 1 +
 b.txt | 1 +
 2 files changed, 2 insertions(+)
```

假设我们想查看第一次提交修改了哪些内容，可以执行下面的命令：

```sh
$ git -P show 7dd760e27ef7716ac603b104d4841170afd501a6
commit 7dd760e27ef7716ac603b104d4841170afd501a6
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Thu Nov 21 17:35:32 2019 +0800

    1

diff --git a/a.txt b/a.txt
new file mode 100644
index 0000000..da0f8ed
--- /dev/null
+++ b/a.txt
@@ -0,0 +1 @@
+a1
diff --git a/b.txt b/b.txt
new file mode 100644
index 0000000..c9c6af7
--- /dev/null
+++ b/b.txt
@@ -0,0 +1 @@
+b1
```

简单又实用。