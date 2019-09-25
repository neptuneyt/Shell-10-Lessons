
**哭着也要更完**

Shell编程目前已经写了3篇：

[shell教程(1)：有没有兴趣玩耍一下shell版俄罗斯方块？](https://mp.weixin.qq.com/s/IFySiOtigDutx-q0_7o_XQ)

[shell教程(2)：积木游戏之认识积木--重要的系统命令](https://mp.weixin.qq.com/s/tF05tKGkeQg17c-fqZR9Bg)

[shell教程（3）变量（一）：a=1？没那么简单](https://mp.weixin.qq.com/s/zxtXuMVNankAU66qAXE6Ig)

同期也把这个项目放在github上：[Shell-10-Lessons](https://github.com/neptuneyt/Shell-10-Lessons)
(https://github.com/neptuneyt/Shell-10-Lessons)

但无论是在公众号还是在GitHub阅读量都惨不忍睹，在GitHub甚至是0Watch、0Star、1Fork，小编已经哭晕在厕所。。。
![c89b228ee0b11a0a79c7162bfea5c789.png](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/lesson4-variable(2):Variable%20length%2C%20interception%2C%20replacement/Image.png)
好在唯一的那个Fork居然是小明同学的偶像天津医科大学的小伊老师，截图为证：

![dcee0bb36d811c1c8c4e813668107ba8.png](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/lesson4-variable(2):Variable%20length%2C%20interception%2C%20replacement/Image2.png)

虽然不知道小伊老师是不是手抖误点了Fork键，但小编还是万分激动的，有大佬关注，小编就是哭着也要更完。

前面一讲提到了Shell变量存储数据的主要类型是字符串，例如```net="www.baidu.com"```，其实Shell对字符串变量的操作是十分强大的，想象一下你是程序语言设计开发者，对于一个诸如```"www.baidu.com"```的变量```net```，你允许对它进行什么操作呢？是不是要能知道这个字符串的长度？是不是要能随意截取字符串？是不是要能随意替换、删除字符串？变量为空时能不能赋值一个默认的字符串数据？等等的操作，在Shell字符串的变量操作中都是可以实现的。
## 获取变量字符串长度
想要知道```"www.baidu.com"```的变量```net```的长度十分简单，通过```${#net}```即可获取。
```bash
[Neptuneyt]$ net="www.baidu.com"
[Neptuneyt]$ echo ${#net}
13
```
当然，在Shell中获取字符串变量的长度的方法有许多种，但是```${#variable}```作为一种系统内建的方法是最快的。
```bash
[Neptuneyt]$ echo ${#net}
13
[Neptuneyt]$ echo ${net}|wc -L
13
[Neptuneyt]$ expr length ${net}
13
[Neptuneyt]$ echo ${net}|awk '{print length($0)}'
13
```
获取字符串长度十分有用，文末给出一个统计文章单词词频数和字长频数的小脚本。
## 变量的截取
字符串的截取有多种方式，常见的包括：
### 指定位置截取字符串
```bash
[Neptuneyt]$ net="www.baidu.com"
[Neptuneyt]$ # 从第4个字符截取到baidu
[Neptuneyt]$ echo ${net:4:5} #从第4个字符.开始截取5个字符
baidu
[Neptuneyt]$ # 截取baidu.com
[Neptuneyt]$ echo ${net:4}   #起始位置后不接截取字符长度则默认截取之后所有的
baidu.com
[Neptuneyt]$ # 用倒数截取com
[Neptuneyt]$ echo ${net:0-3} #从倒数第三个字符截取到末尾
com
[Neptuneyt]$ echo ${net: -3} #另外的写法，一定要注意冒号和-3之间有空格
com
[Neptuneyt]$ echo ${net:-3}  #不加空格，截取失败
www.baidu.com
```
### 匹配字符串截取
```bash
[Neptuneyt]$ echo $net
www.baidu.com

# 删除匹配字符串的左边，留下剩余部分
[Neptuneyt]$ echo ${net#*.} #这里用*.表示匹配到www.，用一个#表示删除匹配到的字符串，留下剩余的部分
baidu.com

# 用2个#号表示尽可能多的删除匹配到的字符串
[Neptuneyt]$ echo ${net##*.}
com

# 同理也可以匹配字符串的右边，留下剩余部分
[Neptuneyt]$ echo ${net%.*} #用.*匹配到.com,用%删除
www.baidu

# 用2个%号表示尽可能多的删除匹配到的字符串
[Neptuneyt]$ echo ${net%%.*}    #因为2个%，这里.*表示匹配到最长的.baidu.com
www
```

总的来说:
```#*chr```表示删除从左到右第一个遇到的字符chr及其左侧的字符
```##*chr```表示删除从左到右最后一个遇到的字符chr及其左侧的字符（贪婪模式）
```%chr*```表示删除从右向左第一个遇到的字符chr及其右侧的字符
```%%chr*```表示删除从右到左最后一个遇到的字符chr及其右侧的字符（贪婪模式）
在键盘上，#在$符的左边，%号在$符的右边，为了便于记忆，大家因此可以记住#删除左边字符，%删除右边字符
## 变量的字符串替换
想要将```net```的 ```baidu```替换成```google```怎么写呢？只需```${net/baidu/google}```即可，需要注意的是原变量并未修改
```bash
[Neptuneyt]$ echo ${net/baidu/google} #/匹配字符/替换字符
www.google.com
[Neptuneyt]$ echo $net  #原变量并未修改
www.baidu.com
```
如果是替换所有匹配到的字符，应该通过```${variable//pattern/sub}```
例如将```net```的```.```替换为```-```或```/```：
```bash
[Neptuneyt]$ echo ${net//./-}
www-baidu-com
[Neptuneyt]$ echo ${net//.//}
www/baidu/com
```
除此之外，还有两种专门针对字符串开头和结尾的替换方式
只替换开头匹配的字符串```${variable/#pattern/sub}```
只替换结尾匹配的字符串```${variable/%pattern/sub}```
例如对于```add=www.xiaomi.com.www```的开头或者结尾的```www```替换为```-```：
```bash
[Neptuneyt]$ add=www.xiaomi.com.www
[Neptuneyt]$ echo ${add/#www/-}
-.xiaomi.com.www
[Neptuneyt]$ echo ${add/%www/-}
www.xiaomi.com.-
```

## 删除字符串
其实学会了替换字符串删除字符串就更简单了，只需将替换部分写成空即可，即```${variable/pattern/null}```，例如将```net```的第一个```.```删除，只需
```bash
[Neptuneyt]$ echo ${net/./}
wwwbaidu.com
[Neptuneyt]$ echo ${net/.}  #最后一个/可以不用写
wwwbaidu.com
```
若要删除所有匹配到的只需即```${variable//pattern}```，例如将```net```的```.```都删除，只需
```
[Neptuneyt]$ echo ${net//.}
wwwbaiducom
```
同理，只删除开头或者结尾匹配到的字符也是类似操作，这里就不赘述了。

## 变量为空时赋默认值
当我们在写脚本时往往需要给脚本传递一些参数，在Shell中传递参数十分简单，只需利用特殊的位置参数变量诸如```$1,$2,$3...${10}...```即可，例如，以下脚本传递2个参数：
```bash
# PassArgument.sh
#!/bin/env bash
# pass 2 arguments
arg1=$1
arg2=$2
echo $arg1 $arg2

[Neptuneyt]$ bash PassArgument.sh Hello word #参数以空格隔开
Hello word
```
有时候，我们想省掉最后一个参数，让它使用默认值，这个时候只需通过```${variable:='default value'}```即可，即当变量有值的时候则使用原值，若没有值则使用括号中默认定义好的值。例如，如下脚本表示当第二个参数为空时默认使用定义好的值“word”,否则是用户自己传递的参数：
```bash
# PassArgument.sh
#!/bin/env bash
arg1=$1
arg2=$2
echo $arg1 ${arg2:='word'}  #第二个参数设置默认值

[Neptuneyt]$ bash PassArgument.sh Hello #第二个参数为空时使用默认值
Hello word
[Neptuneyt]$ bash PassArgument.sh Hello Shell   #第二个参数不为空时使用参数传递的值
Hello Shell
```

除了```${variable:='default value'}```外，还有```${variable:-'default value'}```,```${variable:+'default value'}```和```${variable:？'default value'}```，它们有什么区别呢？
对于```${variable:='default value'}```，表示变量为空时把默认值赋值给该变量，例如：
```bash
[Neptuneyt]$ net=
[Neptuneyt]$ echo ${net:='www.baidu.com'}
www.baidu.com
[Neptuneyt]$ echo $net
www.baidu.com
```
对于```${variable:-'default value'}```,表示变量为空时返回默认值**但是并不把默认值赋值给该变量，** 例如：
```bash
[Neptuneyt]$ net=
[Neptuneyt]$ echo ${net:-'www.baidu.com'}
www.baidu.com
[Neptuneyt]$ echo $net  #此时，变量依旧为空
```
对于```${variable:+'default value'}```,则表示变量不为空时，返回默认值，并且也不重新赋值，例如：
```bash
[Neptuneyt]$ net=www.baidu.com
[Neptuneyt]$ echo ${net:+'www.google.com'}
www.google.com
[Neptuneyt]$ echo $net  #不改变变量原值
www.baidu.com
```
最后，对于```${variable:？'default value'}```,则表示当变量为空时，使用bash风格的报错，例如：
```bash
[Neptuneyt]$ net=
[Neptuneyt]$ echo ${net:?'error:null value'}
-bash: net: error:null value
```
## 实战：统计文章单词情况
这里想要统计Martin Luther King在1963年著名的**I have a dream** [(戳这里下载)](
https://github.com/neptuneyt/Shell-10-Lessons/blob/master/lesson4-variable(2):Variable%20length%2C%20interception%2C%20replacement/I_have_a_dream.txt)演讲中都使用了哪些词，哪些是高频词，单词字长如何。

![cabddef6ecf83ad418a8b9640659f930.png](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/lesson4-variable(2):Variable%20length%2C%20interception%2C%20replacement/Image3.png)
思路：
高频词统计：将所有单词单行输出，删除空行，删除```,./?```等非字母符号，用sort排序后使用uniq统计即可。
字长频数统计：将所有单词单行输出，删除空行，删除```,./?```等非字母符号，使用while循环遍历每个单词，使用```${#variable}```统计单词长读频数。
```bash
# 高频词统计
echo "高频词统计："
echo "频数" "单词"
tr " " "\n" <IHaveADream.txt | \
#使用tr将空格转换成换行符，使得每行一个单词，\续行符表示一行命令未写完可换行书写，切记其后什么字符也不能接，包括空格和注释
sed -e "/[^a-Z]/d;/^$/d" | \
#使用sed匹配非字母字符和空行并删除：-e 表示执行多个操作； /[^a-Z]/，双斜线//表示匹配部分，^表示匹配除开后面a-Z的所有字符，d表示对前面匹配部分删除；/^$/表示匹配空行，^、$分别表示行首和行尾
sort |uniq -c |    \
#排序之后使用uniq统计，-c表示统计单词出现的次数
sort -nr | column -t|head #将次数最多的单词排在前面，-n表示按数值排序，-r从大到小倒序排，column -t表格式输出

# 字长频数统计
echo
echo "字长频数统计："
echo "频数" "单词长度"
tr " " "\n" <IHaveADream.txt | \
sed -e "/[^a-Z]/d;/^$/d" | \
while read word
do
  echo ${#word}
done |\
# 用while和read每次读入一个单词，使用${#word}统计单词长度
sort |uniq -c|sort -nr|column -t|head
```

![ab475a2f31430877e0588f325adbd836.png](https://github.com/neptuneyt/Shell-10-Lessons/blob/master/lesson4-variable(2):Variable%20length%2C%20interception%2C%20replacement/Image4.png)

不出所料，文中出现次数最多的是冠词和连词类，使用次数最多单词的长度为2、3、4个字母。

如上，短短几句简单的命令行就能完成较为复杂的任务，Shell是不是很简单、便捷呢。希望枯燥的语法也不能阻碍大家学习Shell的脚步，给小编Github的项目多几个Watch、Star、Fork，小编才有继续更的动力啊。

![Meng](https://b-ssl.duitang.com/uploads/item/201709/16/20170916195350_dHTtL.thumb.224_0.gif)
## 参考
[Shell Scripting Tutorial](https://www.shellscript.sh/)

[朱双印博客](http://www.zsythink.net/)


