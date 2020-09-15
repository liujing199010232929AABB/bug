# git配置Beyond Compare

## 我的电脑是win10 64位系统 所以本文章只适用于windows电脑

**本文章具有局限性，仅供参考，不喜勿喷**

1.第一步，不多BB，[下载Beyond Compare](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.scootersoftware.com%2Fdownload.php)。我没有那么厉害，搞不到破解版的，就直接下的正版试用的那种。反正三十天试用期到了，卸载再重新下一个就是了（亲测可行，而且只要两次路保存的路径一样，还不用多次配置。嘿爽歪歪）

2.第二步，查看电脑当前系统支持的git diff/git merge插件

```
git difftool --tool-help
git mergetool --tool-help
```

运行结果如下所示：

![](D:\web\Typora\bug\static\pic\gitconfig01.webp)

如果你的运行结果中，没有出现bc或者bc3的话，那基本上可以放弃了，电脑可能会不支持。

但是因为我周围的人，都有显示bc或者bc3，所以我也不知道到底会不会不支持，如果有谁运行完了之后没有显示，可以贴上图我们一起研究一下~

3.第三步，difftool/mergetool配置

difftool

```
git config --global diff.tool bc4
git config --global difftool.bc4.path "bcomp.exe的路径"
```

mergetool

```
git config --global merge.tool bc4
git config --global mergetool.bc4.path "bcomp.exe的路径"
```

这里要注意的是：**"bcomp.exe的路径"**这个东西

我一开始的时候以为是有人给简写了，所以我找到了"BCompare.exe"这个东西，错错错！！！不是他，是**"bcomp.exe"**

上图：

![](D:\web\Typora\bug\static\pic\gitconfig02.webp)

千万记得这个文件，不要错了。不用区分大小写。但是路径要写全，例如我的路径是：

```
D:\Beyond Compare 4\bcomp.exe
```

4.第四步：如果出现虽然安装了bc但mergetool不可用的情况，可以通过修改用户目录下的 gitconfig追加difftool和mergetool的配置

其实我觉得这一步是必须的。。。。。

内容如下，mergetool 的名字可以自定，路径修改为本地 bcomp.exe 的路径即可

首先要找到你需要改的文件**".gitconfig"**，下图是我的文件位置。

![](D:\web\Typora\bug\static\pic\gitconfig03.png)

然后就是把你的difftool和mergetool追加进去了~

你可以用一万种方式打开那个 ".gitconfig" 文件

只需要改动以下部分就好了：
 ***注意：\***
 1.不要忘记改成像我一样的**"cmd = "**;
 2.看清楚路径的链接的斜线是往哪个方向斜的。不要咔咔一顿怼，全给怼上向右斜的了；
 3.将示例中的路径换成自己的。。。对没错，你用的不是我的电脑，所以写我的路径不一定好使。
 4.不要忽略每一段路径后面的那个空格，不管是直接写的路径，还是环境变量，后面都有个空格，不要忽略掉。要不会报错。。（不要问我怎么知道的）

```
[diff]
    tool = bc4
[difftool "bc4"]
    cmd = \"D:/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" 
[merge]
    tool = bc4
[mergetool "bc4"]
    cmd = \"D:/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
```

```
D:/Beyond Compare 4/bcomp.exe     //就是这个，改成自己的路径
```

千万记得

这一步配置结束之后，就可以使用Beyond Compare来merge或者diff你的代码了~
 个人认为这个工具还是比较Diao的，你修改了什么，一目了然。哪句想留下，哪句想扔掉，随意~

ok~结束，这就是我配置的时候遇到的一些坑。。。打完收工...
 还是那句话，我只是一个前端小小小小白。。。以上所有，仅仅是我个人的一些小见解和小看法，如有不妥之处。还请各位大佬批评指正。大家一起学习一起进步！



## 我的电脑是mac电脑

### 使用Beyond Compare作为git mergetool的默认对比工具 For Mac

### 第一步

点击下载下载[Beyond Compare](https://link.jianshu.com/?t=http%3A%2F%2F219.238.7.71%2Ffiles%2F424300000878E137%2Fxiazai.beyondcompare.cc%2Fwm%2FBeyond_Compare-Mac-Trial.zip)

### 第二步

![](D:\web\Typora\bug\static\pic\gitconfig04.webp)

### 第三步

Mac上需要在user目录下的.gitconfig文件中加入下面的配置：

```
或用终端打开.gitconfig文件在里面添加下面的配置，
终端指令
$git config --edit --global
```

```
[diff]
        tool = bcomp
[difftool "bcomp"]
        cmd = \"/usr/local/bin/bcomp\" \"$LOCAL\" \"$REMOTE\"
[difftool]
        prompt = false
[merge]
        tool = bcomp
[mergetool]
        prompt = false
[mergetool "bcomp"]
        cmd = \"/usr/local/bin/bcomp\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
```

### 第四步

如果项目有冲突执行指令：

```
$ git mergetool
```

