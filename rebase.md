# 如何使用git rebase命令

相关命令：

```sh
# 假设你的分支情况是这样
#       A---B---C topic
#      /
# D---E---F---G master
# 想改成这样
#               A---B---C topic
#              /
# D---E---F---G master
# 可以使用下面的命令，该命令的意思是：
# 把从topic可达但从master不可达的提交（A,B,C）提取出来
# 然后以当前master为新的起始点将这些提交依次链接起来
git rebase master topic 

# 假设你的分支情况是这样
# o---o---o---o---o  master
#      \
#       o---o---o---o---o  next
#                        \
#                         o1---o2---o3  topic
# 想改成这样
# o---o---o---o---o  master
#     |            \
#     |             o1---o2---o3  topic
#      \
#       o---o---o---o---o  next
# 可以使用下面的命令，该命令的意思是：
# 把从topic可达但从next不可达的提交（o1,o2,o3）提取出来
# 然后以当前master为新的起始点将这些提交依次链接起来
git rebase --onto master next topic
```

情景模拟：

先执行下面的命令，创建一个测试用的Git仓库：

```sh
# 创建一个空的Git仓库
mkdir repo
cd repo
git init

# master: D,E,F,G
for i in D E F G
do
    touch $i
    git add .
    git commit -m $i
done

# topic: A,B,C
git checkout -b topic HEAD~2
for i in A B C
do
    touch $i
    git add .
    git commit -m $i
done
```

查看下当前的分支情况：

```sh
$ git -P log --graph --oneline --all
* ebfa9da (HEAD -> topic) C
* 86a2290 B
* bbd227b A
| * 501d278 (master) G
| * fac2bda F
|/
* 2eebb0e E
* 0669225 D
```

由上可见，该分支情况和我们假设中的第一种情况完全一致。

现在我们想要将topic分支上的ABC提交重新rebase到最新的master分支上，可以执行如下命令：

```sh
$ git rebase master topic
First, rewinding head to replay your work on top of it...
Applying: A
Applying: B
Applying: C

$ git -P log --graph --oneline --all # 查看当前的分支情况
* 27c0804 (HEAD -> topic) C
* b2d89a9 B
* ea4148d A
* 501d278 (master) G
* fac2bda F
* 2eebb0e E
* 0669225 D
```

由上可见，topic分支上的ABC提交已经rebase到最新的master分支上了。

假设中的第二种情况这里就不演示了，比较类似，有兴趣的同学可以自己动手试下。

其实git rebase命令还可以干很多事，比如合并提交、删除指定提交等等，非常推荐大家好好看看git自带的rebase文档 `git help rebase`。