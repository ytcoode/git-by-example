# 有关Git命令无法正确显示中文路径的问题

相关命令：

```sh
# 设置Git让其在输出路径时正确显示中文
git config --global core.quotePath false

# 如果是Mac用户，在执行了上述命令后还是不行
# 可以再看下Git的 core.precomposeUnicode 这个参数
```

情景模拟：

先用下面的命令创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo
cd repo
git init

# 添加一个文件
touch 中文文件名.txt
```
然后执行git status命令：

```sh
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  "\344\270\255\346\226\207\346\226\207\344\273\266\345\220\215.txt"
```

由上可见，我们新添加的文件并没有以中文正确显示。

下面我们再执行下文章开始时介绍的命令设置一下Git：

```sh
$ git config --global core.quotePath false  # 设置Git让其正确显示中文路径
$ git status # 看下设置后的结果
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  中文文件名.txt
```

看到了吧，在我们设置了Git的 _core.quotePath_ 参数后，中文路径就可以正确显示了。

简单又实用。