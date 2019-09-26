说到变量，大伙会想那还不简单吗？a=1，这不就是变量吗？还有什么好说的呢。但，程序中为什么需要变量？变量在计算机底层上究竟代表了什么？在bash中变量有哪些需要注意的呢？所谓基础不牢，地动山摇，所以各位看官大爷还是耐着性子往下看吧。

![](http://dingyue.nosdn.127.net/2019/01/04/d632726cecd04d51bb662dd7c26471b4.jpeg)
## 什么是变量，为什么需要变量？
变量，顾名思义，就是可变的量。几乎所有的编程语言都有变量的概念，所谓变量其实是指计算机中一个内存块的符号名称，我们可以为其赋值、读取和操作。为什么需要变量呢？**程序在代码执行的时候遵循着自上而下，自前而后的原则，可以想到的是，前面运行的结果往往在后面需要再次利用，若不保存之前的结果，我们便无法再次复用。而变量便应运而生，它可以保存程序运行中产生的任何类型的数据，并且当我们需要再次用到之前产生的数据结果时，直接调用它即可。** 

## 从底层理解变量
既然变量是用来保存程序运行中的数据的，那我们从计算机执行程序的底层来理解变量。我们知道存储在磁盘中的程序需要读入到内存后才能让CPU运行程序。

![CPU, cache and disk](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565263474504&di=f27a9245e82fe45dcd28a0e34c4d8496&imgtype=0&src=http%3A%2F%2Fsee.xidian.edu.cn%2Fcpp%2Fuploads%2Fallimg%2F141105%2F1-141105150121320.png)

一旦在程序运行中产生数据，那么势必要为这些数据在内存中开辟特定内存空间，从而存储产生的数据。那么这个时候带来一个问题，开辟内存空间容易，以后如何找到这些数据呢？
这个时候就有了内存地址的概念，有了地址，程序才能找到这些数据。这很好理解，想想你寄快递的时候要填收件地址。我们程序中的每一行代码，代码中用到的每个数据，都需要在内存上有其映射地址。内存地址一般由4位16进制或8位16进制表示，一个内存地址代表一个字节（8bit）的存储空间。为什么说32位的操作系统最多只支持4GB的内存空间？拿学校宿舍号来说，如果告诉你中科院某个校区的宿舍编号是从000到999的三位数代表的编号（例如910），你肯定很轻易的知道这个校区的总的宿舍是1000个（10\*10\*10），对于计算机的内存也是如此，32位的计算机CPU也只能寻址2的32次方（4GB）个字节位。（注意这里的4GB是以Byte为单位的，1Byte=8bit，顺便提一句，如果你是8GB内存的电脑，但是却安装着32位的操作系统，那么意味着你浪费的一半的内存空间）。

很显然，无论是4位16进制还是8位16进制的内存地址，我们都没法在程序中使用，你能想象在你的程序中名为0x00000001表示的是编号为1的内存地址吗？**幸运的是，所有高级语言只需要你给存储的数据起一个有意义的名字，它就能自动为其分配一个内存地址，从而通过变量名称找到对应的内存地址，找到内存地址就能找到存储的数据。** 内存地址和变量的关系其实跟我们的身份证号和名字的关系很像，每一个身份证号（内存地址）都是唯一的，通过身份证号就能查到你的住址，缺点是太长不好记和使用，但是没关系我们还有名字，虽然可能有重名的风险，但是它好用。

![variable in memory](https://gss2.bdstatic.com/9fo3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike150%2C5%2C5%2C150%2C50/sign=a2c7abe0bc3eb13550cabfe9c777c3b6/a5c27d1ed21b0ef439181f56ddc451da80cb3ec1.jpg)

**总结来说，变量（名称）其实只是一个标签，真正发挥作用的是它联系的内存地址，只有通过内存地址计算机才能找到特定的存储的数据。**

## 创建和引用变量
**如何创建（赋值）一个变量呢？使用=即可。如何引用变量呢？使用```${变量名}```即可，多数时候{}可以不写，但是规范起见尽量用${变量名}的形式，除了规范也可以避免一些意外的bug。**

* 语法
```bash
variable_name=content(character,number,array...)  #assignment
echo ${variable_name}  #view variable content
```
shell中用单个=给变量赋值，等号左边是自定义的变量名称，右边是想保存的数据，例如字符、数值、数组等等内容。
* 示例
```bash
#!/bin/bash
#   variable assignment and view variable content
greeting="Hello Shell"  #assignment
pi=3.14
echo ${greeting} #view variable content
echo ${pi}
```
>Hello Shell
3.14

上例中greeting="Hello Shell"即是将"Hello Shell" 这个字符串赋值给名为greeting的变量；Shell是弱类型的语言，它能自动识别保存的数据对象，而不用根据不同的数据对象声明不同的变量类型；```echo ${greeting}```即是查看greeting这个变量中保存数据的内容。

需要注意的是在变量赋值的时候=两边不能有空格：VAR=value有效; VAR = value无效。第二种赋值方式无效的原因是什么呢？大家是否记得在前面第二讲[
shell教程(2)：积木游戏之认识积木--重要的系统命令](https://mp.weixin.qq.com/s/tF05tKGkeQg17c-fqZR9Bg)，我们分析的shell命令执行的一般规律，即命令加空格加参数的形式，所以这里的空格会让bash误认为哟哥们（VAR）你是一条命令啊，你后面俩小弟一定是你的参数吧（=和value）,好，那让我执行你吧！

另外需要注意的是第一句```#!/bin/bash```，以#!紧接程序解释器路径```/bin/bash```的方式告诉计算机要用```/bin/bash```程序来解释执行我们的脚本。类似的，perl或python程序可以换成相应的路径```/bin/perl```或者```/bin/python```。
第二句以#开头表示该行是注释行，程序在执行的时候不会把#后的内容作为bash语法进行解析。注释的目的在于，对于一些复杂的代码，良好的注释会让自己或者小伙伴更易于理解代码的含义，便于更加高效的协同开发、维护程序。

除了通过=赋值，还可以通过read命令实现交互式创建变量，这样我们的脚本可以让用户实现自主输入内容，示例：
```bash
#!/bin/bash
read -t 30 -p "enter your input:"  user_input
echo ${user_input}
```
>enter your input:Hello Shell
Hello Shell

上例中使用read创建一个user_input的变量，-t是超时设置，秒为单位，即超过30s用户没有响应则退出；-p后的"enter your input:"是提示语，提示用户在此输入数据。

严格来说，除了上述两种创建变量的方法，还有一种是保存由命令行产生的数据，即通过```$(command)```或````command````(反引号，tab上面的键)执行其中的命令，推荐使用$()的形式。示例：
```bash
#!/bin/bash
my_date=$(date)  #$()中的date是显示日期的系统命令，通过$()执行该命令，并通过=赋值给my_date变量
echo ${my_date}
```

## 变量命名规则
给变量起一个有意义的名字对于程序开发来说十分重要，如同你也不想爸妈叫你狗蛋。总的来说变量命令遵循以下规则：
* 变量名必须是以字母或下划线字符“_”开头，后面跟字母、数字或下划线字符
* 不要使用?、*或其他特殊字符命名你的变量
* 变量名和等号之间不能有空格
* 变量名称中间不能有空格
* 避免使用bash里的关键字（ls, top, uniq...）
* 有意义，便于理解
* 可以参考[Python语言规范](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/#id16)，例如局部变量用类似local_var,全局变量用类似GLOBAL_VAR的形式。
* 示例
```bash
#!/bin/bash
# good variable name
age
_name
local_path
LIBRARY_PATH
# bad variable name
2019
??
age*sex
local path
```
## 变量的作用域和变量类型
上面提到局部变量和全局变量究竟指什么呢？其实这就涉及到变量的作用域的问题。
**Shell变量的作用域（Scope），就是 Shell变量的有效范围（可以使用的范围）。** 

在不同的作用域中，同名的变量不会相互干涉，就好像 A班有个叫小明的同学，B班也有个叫小明的同学，虽然他们都叫小明（对应于变量名），但是由于所在的班级（对应于作用域）不同，所以不会造成混乱。但是如果同一个班级中有两个叫小明的同学，就必须用类似于“大小明”、“小小明”这样的命名来区分他们。总的来说根据Shell变量的作用域可以分为三种类型的变量:
* 全局变量（global variable），可以在当前Shell进程中使用
* 局部变量（local variable），只能在函数内部使用
* 环境变量（environment variable），可以在子进程中使用
所谓Shell进程，你可以简单的理解为打开一个新的Shell终端或者执行bash xx.sh脚本就是产生一个新的进程。
### 全局变量
可以在当前Shell的整个进程中使用，在Shell中定义的变量默认就是全局变量。全局变量只对当前的Shell进程起作用，如果是其他的进程则不生效。假设打开两个Shell终端窗口（一个窗口就是一个Shell进程），在第一个Shell终端定义一个变量var=1,执行echo $var是可以正常显示其值1的，然而在第二个窗口执行echo $var,会显示空值，这就说明全局变量只对当前Shell进程生效。
### 局部变量
Shell中支持自定义的函数，然而与python等语言不同的是，**Shell函数中的变量默认是全局变量**，也就是说在函数内部定义的变量不止在函数内部发挥作用，同样还会在函数外部生效。例如：
```bash
#!/bin/bash
#       create a function
function Func(){
        age=18  #default global variable
echo "within the function: ${age}"
}
Func    #call function
echo "outside of the function: ${age}"
```
>within the function: 18
outside of the function: 18

从上面的示例中我们可以看到，在函数Func中定义的的age，在函数内部和外部都能引用，都打印出正确的值。那么，如果我们只希望函数内部定义的变量只在函数内部发挥作用该怎么办呢，这时我们可以**通过在变量名称之前加上```local```关键字定义所谓的局部变量。** 稍微改一下上面的代码，我们看看二者的区别。
```bash
function Func(){
        local age=18    #create local variable
echo "within the function: ${age}"
}
Func    #call function
echo "outside of the function: ${age}"
```
>within the function: 18
outside of the function:

可见，在函数内部使用```local```关键字定义的局部变量age只在函数中起作用，在函数外部无法引用。
### 环境变量
全局变量只在当前Shell进程中有效，对其它Shell进程和子进程都无效。如果**使用```export```命令将全局变量导出，那么它就在所有的子进程中也有效了，这称为“环境变量”。** 
环境变量被创建时所处的 Shell 进程称为父进程，如果在父进程中再创建一个新的进程来执行 Shell 命令，那么这个新的进程被称作 Shell 子进程。当 Shell 子进程产生时，它会继承父进程的环境变量为自己所用，所以说环境变量可从父进程传给子进程。不难理解，环境变量还可以传递给孙进程。注意，两个没有父子关系的 Shell 进程是不能传递环境变量的，并且环境变量只能向下传递而不能向上传递，即“传子不传父”。
通过下面的示例更好的理解环境变量和全局变量的区别：
```bash
[Neptuneyut$] var=1 #当前Shell进程（假设叫P）中，定义一个全局变量var,赋值为1
[Neptuneyut$] echo $var #在当前进程中引用变量
1
[Neptuneyut$]bash   #用bash打开一个子进程（假设叫C1）
[Neptuneyut$]echo $var  #输出为空，验证全局变量只在定义的进程中有效
[Neptuneyut$]exit   #通过exit命令可以一层一层地退出Shell，退出子进程,这里执行一次，回到之前的P进程
[Neptuneyut$]export var=1   #在P进程中用export将全局变量var导出为环境变量
[Neptuneyut$]bash   #用bash打开一个新子进程（不是之前的那个子进程，假设叫C2）
[Neptuneyut$]echo $var  #在C2子进程中引用环境变量
1   #在子进程中引用成功
```
需要强调的是环境变量只在Shell子进程中有效，并没有说它在所有的Shell进程中都有效；如果你通过终端创建了一个新的Shell窗口，那它就不是当前Shell的子进程，环境变量对这个新的Shell进程仍然是无效的。
另外，通过export导出的环境变量只对当前Shell进程以及所有的子进程有效，如果最顶层的父进程被关闭了，那么环境变量也就随之消失了，其它的进程也就无法使用了，所以说环境变量也是临时的。那么如果想让一个变量在所有Shell进程中都有效，不管它们之间是否存在父子关系，该怎么办呢？这就必须将变量写入Shell配置文件中，Shell进程每次启动时都会执行配置文件中的代码做一些初始化工作，如果将变量放在配置文件中，那么每次启动进程都会定义这个变量，使得对所有的进程都生效。

## 变量的存活期
变量存在的时间即存活期，与变量的作用域密切相关。环境变量在任何Shell进程中都是存在的，而全局变量只在定义的进程中有效，局部变量影响则更有限，只在某个进程的函数内部有效。一般来说，我们的Shell脚本中定义的变量在脚本运行结束之后（即退出运行脚本的那个子进程），所有的变量都失去作用。
**为什么需要不同类型的变量？一个原因在于我们使用变量时更为灵活，另外一个原因在于全局变量和局部变量只在有限的范围内有效，一旦退出相应的进程或者变量作用域，就失去作用，这样的好处在于及时清除变量对应的数据、释放无用的内存。**

![](http://pic.downcc.com/upload/2019-7/15625715066320646.jpg)

## 参考
[Shell Scripting Tutorial](https://www.shellscript.sh/)

https://www.cnblogs.com/VIPler/p/4282584.html

http://c.biancheng.net/view/773.html
