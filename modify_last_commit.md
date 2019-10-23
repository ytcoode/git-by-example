# 修改上次提交

如果只是修改上次提交的日志，可以直接使用下面的命令：

```sh
git commit --amend -m 新的提交日志
```

如果上次提交的内容有误或者不全，想要修改上次提交中文件的内容，或是添加新的文件，可以执行下面的命令：

```sh
# 先修改对应的文件
# git add 修改的文件或新文件
# 执行下面的命令，将这次修改的内容合并到上次提交
git commit --amend --no-edit
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
```

执行完上面的命令后，你发现提交的日志不太友好，想要修改下，可以使用下面的命令：

```sh
$ git commit --amend -m 正确的日志
[master e80dc2f] 正确的日志
 Date: Wed Oct 23 17:17:41 2019 +0800
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt

$ git -P log --pretty=oneline --abbrev-commit # 确认日志是修改了
e80dc2f (HEAD -> master) 正确的日志
```

由上可见，通过上面的命令，上次提交的日志信息得到了修复。

假设我们又发现上次提交的a.txt文件里的内容是错的，且忘了提交b.txt文件，我们可以使用下面的命令修复上次提交：

```sh
$ echo A2 > a.txt # 修复a.txt文件的内容
$ echo B1 > b.txt # 新建b.txt文件
$ git add .       # 标记a.txt和b.txt都将在下次commit时提交

$ git commit --amend --no-edit # 将这次提交的内容合并到上次提交中
# 省略输出内容 #

$ git -P log --pretty=oneline --abbrev-commit # 查看Git日志，确认只有一条
4f2b621 (HEAD -> master) 正确的日志

$ git -P log -p # 确认修复后的提交包含了我们刚刚做的修改
commit 4f2b621800332c0731f5340dc3a7945a09baf8b9 (HEAD -> master)
Author: wangyuntao <wyt.daily@gmail.com>
Date:   Wed Oct 23 17:17:41 2019 +0800

    正确的日志

diff --git a/a.txt b/a.txt
new file mode 100644
index 0000000..3ce238a
--- /dev/null
+++ b/a.txt
@@ -0,0 +1 @@
+A2
diff --git a/b.txt b/b.txt
new file mode 100644
index 0000000..a19a027
--- /dev/null
+++ b/b.txt
@@ -0,0 +1 @@
+B1
```

由上可见，git comit --amend -no-edit 命令将我们新的修改合并到了上一次提交中。

