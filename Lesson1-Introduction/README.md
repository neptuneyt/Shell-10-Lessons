[toc]
# 来，我要诱惑你
想要入门生信吗？

想要学习Linux系统吗？

想要花最少的时间学会一门编程语言？

想要写一个shell版俄罗斯方块？

想要把生信分析流程（WES、RNA-Seq、Chip-Seq、Metagenomics-Seq...）捆成一个软件，批量运行？

...

来吧，学点shell！

![](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/Lesson1-Introduction/sailer_grandpa.gif)


当然，万丈高楼平地起，我会从Linux系统、常用命令和shell语法等基础讲起。废话不多说，首先看看什么是shell？shell能干嘛？学会shell你要付出多少时间？


**温馨提醒：由于阅读人群基础不一，这里默认面向零基础的群体，因此有很多啰嗦的话，老司机们请自行跳过某些部分。(毕竟，在这个知识爆炸的时代，浪费别人的时间实在是不道德的事）**
# Linux系统与shell
在互联网+的时代，无论是开发端游、手游、操作系统的程序猿，还是做大数据分析、人工智能的工程师，更或者是像我们这种苦逼的生信猿，无一例外都要用到Linux操作系统。**这个由被称为“大鼻子男孩”Linus Torvalds在1991年开发的类Unix操作系统，现在已经成了除微软以外应用最广的的操作系统，尤其是一些大公司，甚至连微软自己都用。** 另外一个你想不到的是，我们所有Android系统手机也是基于Linux系统的。Linux系统自创建以来就以开源（open source）为宗旨，现在已经有很多发行版本，例如像CentOS、Ubuntu、Debian等，每个版本一般都有桌面版本和服务器版本。Linux系统与Windows系统差别最大的一点是它以命令行模式为主，美国大片里各种狂拽酷炫屌炸天的黑客攻击的镜头，就是在Linux系统上干的。
![rolling_screen](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1559399458493&di=a395ade44dbd4042c74c533ce051661b&imgtype=0&src=http%3A%2F%2Fpic4.zhimg.com%2F50%2Fv2-815b2f66016e4699e59a0a6e7df58adc_hd.gif)
## 安装属于你的Linux系统
每个人都可以轻松拥有一个Linux操作系统（当然前提是你得有电脑），如果你是苹果电脑，直接打开shell程序即可进入命令行模式，因为苹果电脑本身就是Unix系统；如果不是，推荐在Windows 系统上安装虚拟机，好用的虚拟机软件例如VirtualBox、VMware,安装方法如同安装QQ、微信一样，具体的过程可以参考[兄弟连Linux视频教程](https://www.bilibili.com/video/av7797044/?p=5)
## shell的功能
无论哪个发行版本，**Linux操作系统的核心原理都是基于shell实现外层应用程序和内核以及硬件的通信，从而实现人对机器的控制。**  可见shell的初衷是帮助用户和管理员更好的维护和管理操作系统，但其实shell对于通常的文本处理（例如某些表格、系统运维日志），甚至是某些数值运算都是友好的。因此，学好shell既能帮助你把Linux系统玩得很溜，又能帮助你快速、便捷地处理一些简单或者复杂的编程项目，一举两得，何乐而不为呢？总结shell的主要功能：
* Linux系统管理和运维：例如批量增加/删除多个用户
* 文本处理
* 数值运算
* 整合其他软件/语言，搭建流程

![](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=4103929619,1992524977&fm=26&gp=0.jpg)

## shell特点
* **简单**：shell可以分为交互，和非交互的使用方式。在交互式模式，shell从键盘接收输入，所键即所得；在非交互式模式，shell从文件中获取输入，即运行shell脚本。
* **还是简单**：在Linux系统使用中，你所用到的每个命令都可以为shell 脚本所用，**可以说只要你了解一些系统常用命令和shell基本语法，然后就可以像搭积木一样组合成一个有用的脚本。** 最低的学习成本，获得高效的编程体验。对于毫无编程经验的小白来说，实在是跟python一样入门编程语言的首选。
* **方便**：shell语法精炼、高效，单行代码让你搞定问题。
* **强大**：虽然简单，但shell支持数组、函数、变量截取以及其他软件/语言的调用，灵活而强大。
## shell版本
shell作为一门语言，也如java、perl、python等语言拥有不同的版本。**不同的shell版本，基本功能、语法大致相同，但有些语法细节、命令参数使用会因版本有小的差异，在学习的时候需要注意。否则，在多个版本的切换中，你的shell脚本可能暗藏一些bug。**
>常见的shell版本

* **sh:即Bourne shell，是最基础的shell。**
* **bash :Bourne-Again SHell，在sh的基础上增加了一些特性，最常用的shell，后面都以它为例讲解。bash可以完全兼容sh,也就是说用sh语法的脚本不加修改可以在bash中执行，反过来却不行，bash的脚本在sh上运行容易报语法错误。这一点一定要注意。**
* csh(C shell):类C的语法
* tcsh:csh的增强版
* zsh:Mac系统上超级增强版shell，被称为“终极shell”功能更强大，但配置什么也更复杂。zsh在github上star已经快90k了，fork也快17k了，如果有大神想用，请移步[zsh github](https://github.com/robbyrussell/oh-my-zsh)。

>查看你系统默认用shell版本以及安装的所有shell版本

```bash
echo $SHELL #echo命令用于输出、打印，在脚本调试时很有用
/bin/bash   #可见我系统默认shell是bash

cat /etc/shells #cat用于查看文本
/bin/sh #可见我系统自带4种shell
/bin/bash
/bin/tcsh
/bin/csh
```

# shell初体验
## Hello Shell
按照惯例，我们在非交互模式下写一个学习每一门语言都会写的第一个脚本--打印Hello Shell(交互模式下直接在命令行提示符键入echo "Hello Shell!"）。
```bash
[Neptuneyt]$ vi helloShell.sh #用vi/vim新建一个名为helloShell.sh的脚本,vi （vim）是linux系统一个强大的文本编辑器
#！/bin/bash
echo "Hello Shell!" #按i输入左边内容，esc+shift+:+x保存退出
[Neptuneyt]$ bash helloShell.sh #执行脚本
Hello Shell!
```
## shell版俄罗斯方块
为了增加大家的学习兴趣，特地找了一个[俄罗斯方块游戏的shell脚本](https://github.com/neptuneyt/Tetris/blob/master/Tetris.sh)，希望大家在看了本系列教程后，也能看懂这个脚本如何work的，或者至少能写出一个生信流程。
（将Tetris.sh里的shell源码复制到你的Linux系统，$提示符后键入bash Tetris.sh
现在开启童年模式，愉快的去玩耍俄罗斯方块吧！）

![Tetris game](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/Lesson1-Introduction/tetris_game.gif)

# 怎么学shell
**如果说编程就是搭积木游戏，你可以参考下面的套路：**
* **知道每一块积木的用途**
对于shell来说，你需要尽可能熟悉Linux系统命令，常用的可能20多个，例如grep、sed、awk等。
* **遵守积木搭建的游戏规则**
其实就是语法，包括变量和流程控制。不同变量存储不同类型的数据（字符串、数值、数组等），流程控制决定整体运行方向（循环或者判断分支）。变量怎么命名？循环是什么结构？
* **优化搭好积木的结构**
结构上是不是能精简？代码是不是能复用（函数）？循环时间是不是能压缩。。。
* **搭一万次积木**
神枪手都是子弹喂出来的，程序猿也是，结合实际需求，多练多写才是王道。（当然，某些天才是例外，人生来是为了创造语言的）

# 参考
[Shell script wikipedia](https://en.wikipedia.org/wiki/Shell_script)
[Shell Scripting Tutorial](https://www.shellscript.sh/index.html)
