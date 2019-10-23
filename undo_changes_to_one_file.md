# 撤销对单个文件的修改

最终命令：

```bash
git checkout HEAD a.txt                             # 撤销对a.txt文件的修改
git restore --source=HEAD --staged --worktree a.txt # 也可以使用这个命令
```

情景模拟：

先使用下面的命令初始化一个测试用的Git仓库：

```bash
# 初始化一个空的Git仓库
mkdir repo && cd repo
git init

# 将a.txt加入到版本控制中
echo A1 > a.txt
git add .
git commit -m init

# 修改a.txt，并把这次修改加入到Git的staging area中
echo A2 >> a.txt
git add .

# 修改a.txt，不把这次修改加入到Git的staging area中
echo A3 >> a.txt
```

执行完上面的命令后，看下该Git仓库的当前状态：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   a.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   a.txt
```

现在我们想撤销对a.txt文件的修改，可执行如下命令：

```bash
$ git checkout HEAD a.txt
Updated 1 path from b5ab058

$ git status
On branch master
nothing to commit, working tree clean

$ cat a.txt
A1
```

由上可见，checkout命令撤销了对a.txt文件的所有修改，包括staging area中的修改。