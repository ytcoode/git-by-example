# 命令别名

相关命令：

```sh
git config --global alias.别名 别名代表的真正命令
```

对于那些经常使用的，或者是特别复杂的Git命令，我们可以为其设置别名，这样在我们想要执行对应的Git命令时，只要执行这个别名命令就好了，简单方便。

下面来演示下。

当我们在命令行中，想要以图形化的方式查看当前分支的提交日志时，可以使用下面的命令：

```sh
$ git log --graph --oneline
*   8005803a2ca0 (HEAD -> master, origin/master, origin/HEAD) Merge tag 'arc-5.4-rc6' of git://git.kernel.org/pub/scm/linux/kernel/git/vgupta/arc
|\
| * 5effc09c4907 ARC: perf: Accommodate big-endian CPU
| * ab563bf54a4d ARC: [plat-hsdk]: Enable on-boardi SPI ADC IC
| * 8ca8fa7f22dc ARC: [plat-hsdk]: Enable on-board SPI NOR flash IC
* |   0365fb6baeb1 Merge branch 'for-linus' of git://git.kernel.org/pub/scm/linux/kernel/git/hid/hid
|\ \
| * | 09f3dbe47473 HID: i2c-hid: add Trekstor Primebook C11B to descriptor override
| * | 08c453f6d073 HID: logitech-hidpp: do all FF cleanup in hidpp_ff_destroy()
| * | 905d754c53a5 HID: logitech-hidpp: rework device validation
# 省略输出 #
```

上面命令显示的是当前linux内核master分支的提交日志。

该命令挺有用的，但就是参数太多了，此时我们就可以用别名的方式来简化该命令的使用。

比如，我们可以为上面命令中的 `log --graph --oneline` 部分设置别名为 `l`，具体命令如下：

```sh
$ git config --global alias.l 'log --graph --oneline'
```

在执行完上面的命令后，别名就设置好了，这样当我们执行 `git l` 的时候，Git帮我们执行的真正命令其实是 `git log --graph --oneline`。

我们来试下：

```sh
$ git l
*   8005803a2ca0 (HEAD -> master, origin/master, origin/HEAD) Merge tag 'arc-5.4-rc6' of git://git.kernel.org/pub/scm/linux/kernel/git/vgupta/arc
|\
| * 5effc09c4907 ARC: perf: Accommodate big-endian CPU
| * ab563bf54a4d ARC: [plat-hsdk]: Enable on-boardi SPI ADC IC
| * 8ca8fa7f22dc ARC: [plat-hsdk]: Enable on-board SPI NOR flash IC
* |   0365fb6baeb1 Merge branch 'for-linus' of git://git.kernel.org/pub/scm/linux/kernel/git/hid/hid
|\ \
| * | 09f3dbe47473 HID: i2c-hid: add Trekstor Primebook C11B to descriptor override
| * | 08c453f6d073 HID: logitech-hidpp: do all FF cleanup in hidpp_ff_destroy()
| * | 905d754c53a5 HID: logitech-hidpp: rework device validation
# 省略输出 #
```

成功了，和原命令的输出完全一样。

通过使用Git的命令别名，我们可以极大简化日常的Git操作，非常方便。

希望你喜欢。