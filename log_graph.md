# 命令行中图形化显示提交日志

相关命令：

```sh
# 图形化显示当前分支的提交日志
git log --graph --oneline

# 图形化显示当前分支的提交日志及每次提交的变更内容
git log --graph --patch

# 图形化显示所有分支的提交日志
git log --graph --oneline --all

# 图形化显示所有分支的提交日志及每次提交的变更内容
git log --graph --patch --all

```

效果演示：

我们先用下面的命令创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo && cd repo && git init

# master分支提交m1
echo m1 > m1.txt && git add . && git commit -m m1

# b分支提交b1、b2
git checkout -b b
echo b1 > b1.txt && git add . && git commit -m b1
echo b2 > b2.txt && git add . && git commit -m b2

# 切换到master分支
git checkout master

# master分支提交m2
echo m2 > m2.txt && git add . && git commit -m m2

# 合并b分支
git merge b --no-edit

# master分支提交m3、m4
echo m3 > m3.txt && git add . && git commit -m m3
echo m4 > m4.txt && git add . && git commit -m m4

# b分支提交b3、b4
git checkout b
echo b3 > b3.txt && git add . && git commit -m b3
echo b4 > b4.txt && git add . && git commit -m b4

# 切换到master分支
git checkout master

```

先看下当前分支提交日志的图形化效果：

```sh
$ git -P log --graph --oneline
* 1b95716 (HEAD -> master) m4
* 9629016 m3
*   960ae54 Merge branch 'b'
|\
| * 5c4e7a0 b2
| * 82e6569 b1
* | cec7a59 m2
|/
* 3706b17 m1
```

再看下所有分支提交日志的图形化效果：

```sh
$ git -P log --graph --oneline --all
* 85cf84a (b) b4
* 6c72eb7 b3
| * 1b95716 (HEAD -> master) m4
| * 9629016 m3
| *   960ae54 Merge branch 'b'
| |\
| |/
|/|
* | 5c4e7a0 b2
* | 82e6569 b1
| * cec7a59 m2
|/
* 3706b17 m1
```

最后看下当前分支每次提交的变更内容：

```sh
$ git -P log --graph --patch
* commit 1b9571692938cae69e4cb6f1c90195629b99ee14 (HEAD -> master)
| Author: wangyuntao <wyt.daily@gmail.com>
| Date:   Tue Oct 29 17:58:06 2019 +0800
|
|     m4
|
| diff --git a/m4.txt b/m4.txt
| new file mode 100644
| index 0000000..995fb87
| --- /dev/null
| +++ b/m4.txt
| @@ -0,0 +1 @@
| +m4
|
* commit 96290165e6cf0869a7cb5f74dd84596e5b156017
| Author: wangyuntao <wyt.daily@gmail.com>
| Date:   Tue Oct 29 17:58:06 2019 +0800
|
|     m3
|
| diff --git a/m3.txt b/m3.txt
| new file mode 100644
| index 0000000..e5c9ce9
| --- /dev/null
| +++ b/m3.txt
| @@ -0,0 +1 @@
| +m3
|
*   commit 960ae547f7751191c9d30e17eac2fbb85589d778
|\  Merge: cec7a59 5c4e7a0
| | Author: wangyuntao <wyt.daily@gmail.com>
| | Date:   Tue Oct 29 17:58:06 2019 +0800
| |
| |     Merge branch 'b'
| |
| * commit 5c4e7a015a519e60134fe2d7895590a756887d4b
| | Author: wangyuntao <wyt.daily@gmail.com>
| | Date:   Tue Oct 29 17:58:06 2019 +0800
| |
| |     b2
| |
| | diff --git a/b2.txt b/b2.txt
| | new file mode 100644
| | index 0000000..e6bfff5
| | --- /dev/null
| | +++ b/b2.txt
| | @@ -0,0 +1 @@
| | +b2
| |
| * commit 82e6569460ce15d46b5f7d44fa4b68080bf952a6
| | Author: wangyuntao <wyt.daily@gmail.com>
| | Date:   Tue Oct 29 17:58:06 2019 +0800
| |
| |     b1
| |
| | diff --git a/b1.txt b/b1.txt
| | new file mode 100644
| | index 0000000..c9c6af7
| | --- /dev/null
| | +++ b/b1.txt
| | @@ -0,0 +1 @@
| | +b1
| |
* | commit cec7a592810edb921e602c214881e3e8d37bb449
|/  Author: wangyuntao <wyt.daily@gmail.com>
|   Date:   Tue Oct 29 17:58:06 2019 +0800
|
|       m2
|
|   diff --git a/m2.txt b/m2.txt
|   new file mode 100644
|   index 0000000..08bb233
|   --- /dev/null
|   +++ b/m2.txt
|   @@ -0,0 +1 @@
|   +m2
|
* commit 3706b17072a7eb78f8093b36774c63fbcd9dc106
  Author: wangyuntao <wyt.daily@gmail.com>
  Date:   Tue Oct 29 17:58:06 2019 +0800

      m1

  diff --git a/m1.txt b/m1.txt
  new file mode 100644
  index 0000000..63a911f
  --- /dev/null
  +++ b/m1.txt
  @@ -0,0 +1 @@
  +m1
```

以上命令是不是比直接用git log命令直观多了呢？

好了，命令行中图形化显示提交日志的内容到这里就结束了，希望对你有所帮助。
