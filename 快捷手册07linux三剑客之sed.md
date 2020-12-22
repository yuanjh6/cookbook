# 快捷手册07linux三剑客之sed
快捷手册sed

## 命令模版
 sed [选项] [脚本命令] 文件名


## 常用选项及含义




|           选项           |                                                                                                                      含义                                                                                                                      |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| -e 脚本命令        | 该选项会将其后跟的脚本命令添加到已有的命令中。                                                                                                                                                       |
| -f 脚本命令文件 | 该选项会将其后文件中的脚本命令添加到已有的命令中。                                                                                                                                               |
| -n                         | 默认情况下，sed 会在所有的脚本指定执行完毕后，会自动输出处理后的内容，而该选项会屏蔽启动输出，需使用 print 命令来完成输出。 |
| -i                          | 此选项会直接修改源文件，要慎用。                                                                                                                                                                                   |

## 常用命令
**sed s 替换脚本命令**

基本格式：[address]s/pattern/replacement/flags

flags


|  标记  |                                                                                                                              功能                                                                                                                               |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| n         | 1~512 之间的数字，表示指定要替换的字符串出现第几次时才进行替换，例如，一行中有 3 个 A，但用户只想替换第二个 A，这是就用到这个标记； |
| g         | 对数据中所有匹配到的内容进行替换，如果没有 g，则只会在第一次匹配成功时做替换操作。例如，一行数据中有 3 个 A，则只会替换第一个 A；      |
| p         | 会打印与替换命令中指定的模式匹配的行。此标记通常与 -n 选项一起使用。                                                                                                                              |
| w file | 将缓冲区中的内容写到指定的 file 文件中；                                                                                                                                                                                        |
| &        | 用正则表达式匹配的内容进行替换；                                                                                                                                                                                                    |
| \\n     | 匹配第 n 个子串，该子串之前在 pattern 中用 \\(\\) 指定。                                                                                                                                                             |
| \         | 转义（转义替换部分包含：&、\ 等）。                                                                                                                                                                                               |

举例

```
sed 's/test/trial/2' data4.txt # sed 编辑器只替换每行中第 2 次出现的匹配模式。
sed -n 's/test/trial/p' data5.txt # -n 选项会禁止 sed 输出，但 p 标记会输出修改过的行，将二者匹配使用的效果就是只输出被替换命令修改过的行
sed 's/test/trial/w test.txt' data5.txt # w 标记会将匹配后的结果保存到指定文件中
sed 's/\/bin\/bash/\/bin\/csh/' /etc/passwd # 在使用 s 脚本命令时，替换类似文件路径的字符串会比较麻烦，需要将路径中的正斜线进行转义
```

**sed d 替换脚本命令**

基本格式：[address]d


举例:

```
sed '3d' data6.txt # 删除 data6.txt 文件内容中的第 3 行
sed '2,3d' data6.txt # 删除 data6.txt 文件内容中的第 2、3行
sed '/1/,/3/d' data6.txt  # 两个文本模式来删除某个区间内的行，sed 会删除两个指定行之间的所有行（包括指定的行），从匹配“1”的行开删，到匹配“3”的行停止
sed '3,$d' data6.txt # 删除 data6.txt 文件内容中第 3 行开始的所有的内容
```

**sed a 和 i 脚本命令**

基本格式：[address]a（或 i）\新文本内容


举例:

```
[root@localhost ~]# sed '1i\
> This is one line of new text.\
> This is another line of new text.' data6.txt # 将一个多行数据添加到数据流中，只需对每一行末尾（除最后一行）添加反斜线即可
```


**sed c 替换脚本命令**

基本格式：[address]c\用于替换的新文本

```
[root@localhost ~]# sed '3c\
> This is a changed line of text.' data6.txt #sed 编辑器会修改第三行中的文本
```

**sed y 转换脚本命令**

基本格式：[address]y/inchars/outchars/

举例：

```
 sed 'y/123/789/' data8.txt # inchars 模式中指定字符的每个实例都会被替换成 outchars 模式中相同位置的那个字符
```

**sed p 打印脚本命令**

基本格式：[address]p

举例 :

```
sed -n '/number 3/p' data6.txt  #-n 选项和 p 命令配合使用，我们可以禁止输出其他行，只打印包含匹配文本模式的行。
```

**sed w 脚本命令**

基本格式：[address]w filename

```
sed '1,2w test.txt' data6.txt #将数据流中的前两行打印到一个文本文件中
```

**sed r 脚本命令**

基本格式为：[address]r filename

```
 sed '3r data12.txt' data6.txt #将 filename 文件中的内容插入到 address 指定行的后面
```

**sed q 退出脚本命令**

```
sed '2q' test.txt # 输出第 2 行之后，就停止 
sed '/number 1/{ s/number 1/number 0/;q; }' test.txt # sed 命令会在匹配到 number 1 时，将其替换成 number 0，然后直接退出。
```

## 寻址方式
**以数字形式表示行区间**

```
sed  '2s/line/row/g' file03.txt
sed  '1,3s/line/row/g' file03.txt
sed  '2,$s/line/row/g' file03.txt
```
**用文本模式来过滤出行**

```
sed '/third/s/line/row/g' file03.txt # => /pattern/command(pattern=third,command=s/line/row/g)
```

## 参考
Linux sed命令完全攻略（超级详细）：c.biancheng.net/view/4028.html

Linux之sed入门:https://blog.csdn.net/onlyshenmin/article/details/102532165

Linux命令之sed使用入门概述：https://www.cnblogs.com/jztong/p/6746577.html  