> 1 第一个shell脚本 hello word

(1) shell 脚本第一行必须为：
    #!/bin/bash

    例子：
```
    #!/bin/bash
    
    echo "hello world"
note:
    #!  -->告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序
```

(2)shell 脚本的执行方式

```
   1) 作为可执行程序
      将上面的代码保存为 test.sh，并 cd 到相应目录：
      
         chmod +x ./test.sh  #使脚本具有执行权限
         ./test.sh  #执行脚本  
   
      注意：一定要写成 ./test.sh，而不是 test.sh，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。
      
   2) 作为解释器参数
      直接运行解释器，其参数就是 shell 脚本的文件名，如：
      
      /bin/sh test.sh
      
```

> shell变量

(1) 变量定义

    注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

    1) 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
    2) 中间不能有空格，可以使用下划线 _。
    3) 不能使用标点符号。
    4) 不能使用bash里的关键字（可用help命令查看保留关键字）。
    
    如：
       your_name="runoob.com"

(2) 变量使用

```
     使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

     实例
        your_name="qinjx"
        echo $your_name
        echo ${your_name}
     变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

     实例
        for skill in Ada Coffe Action Java; do
        echo "I am good at ${skill}Script"
        done
      如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。
      推荐给所有变量加上花括号，这是个好的编程习惯。
```

(3) 只读变量与变量删除

    *只读变量*
```
    使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

    下面的例子尝试更改只读变量，结果报错：

     实例
         #!/bin/bash

         myUrl="https://www.google.com"
         readonly myUrl
         myUrl="https://www.runoob.com"
         运行脚本，结果如下：

         /bin/sh: NAME: This variable is read only.
```
    **删除变量**
```
       使用 unset 命令可以删除变量。语法：

        unset variable_name
        变量被删除后不能再次使用。unset 命令不能删除只读变量。

        实例
            #!/bin/sh

            myUrl="https://www.runoob.com"
            unset myUrl
            echo $myUrl
            以上实例执行将没有任何输出
```

> shell字符串

(1)单引号
   
   单引号字符串的限制：
      单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
      单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用
```
    示例：
          # 使用单引号拼接
            your_name="runoob"
            greeting_2='hello, '$your_name' !'
            greeting_3='hello, ${your_name} !'
            echo $greeting_2  $greeting_3
          # 输出
            hello, runoob ! hello, ${your_name} !
```

(2)双引号
   双引号的优点：
       双引号里可以有变量
       双引号里可以出现转义字符

```
    示例：
        your_name="runoob"
        # 使用双引号拼接
          greeting="hello, "$your_name" !"
          greeting_1="hello, ${your_name} !"
          greeting_2="hello, \"${your_name}\" !"
          echo $greeting  $greeting_1 $greeting_2
        # 输出
          hello, runoob ! hello, runoob ! hello, "runoob" !  
```
(3) 获取字符串长度
```
    string="abcd"
    echo ${#string}   # 输出 4

    变量为数组时，${#string} 等价于 ${#string[0]}:
```

> shell注释

(1)单行注释

    以 # 开头的行就是注释

(2)多行注释
```
   :<<EOF
    注释内容...
    注释内容...
    注释内容...
   EOF
   
   EOF 可以使用任意字符代替，如：
   
   :<<!
    注释内容...
    注释内容...
    注释内容...
   !
```

> shell 传递参数

    在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，其中 $0 为执行的文件名（包含文件路径）：
```
    #!/bin/bash

    echo "Shell 传递参数实例！";
    echo "执行的文件名：$0";
    echo "第一个参数为：$1";
    echo "第二个参数为：$2";
    echo "第三个参数为：$3";

  输出结果如下所示：
    $ sh ./test.sh 1 2 3
    Shell 传递参数实例！
    执行的文件名：./test.sh
    第一个参数为：1
    第二个参数为：2
    第三个参数为：3
    
一些特殊的参数：
    $#	传递到脚本的参数个数
    $*	以一个单字符串显示所有向脚本传递的参数。以"$1 $2 … $n"的形式输出所有参数。
    $$	脚本运行的当前进程ID号
    $!	后台运行的最后一个进程的ID号
    $@	与$*相同，但是使用时加引号，并在引号中返回每个参数。以"$1" "$2" … "$n" 的形式输出所有参数。
    $-	显示Shell使用的当前选项，与set命令功能相同。
    $?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

$* 与 $@ 区别：
    相同点：都是引用所有参数。
    不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。    

示例：
    #!/bin/bash

    echo "-- \$* 演示 ---"
    for i in "$*"; do
        echo $i
    done

    echo "-- \$@ 演示 ---"
    for i in "$@"; do
        echo $i
    done
    
执行脚本，输出结果如下所示：

    $ ./test.sh 1 2 3
    -- $* 演示 ---
    1 2 3
    -- $@ 演示 ---
    1
    2
    3
```
 
> shell数组

    bash支持一维数组（不支持多维数组），初始化时不需要定义数组大小。数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

(1)数组定义

   用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：
                   数组名=(值1 值2 ... 值n)
   例如：
                    array_name=(value0 value1 value2 value3)
```
示例：
    #!/bin/bash

    my_array=(A B "C" D)
```

(2)数组使用

    读取数组元素值的一般格式是：
                        ${数组名[下标]}
    例如：
                    valuen=${array_name[n]}
                    
```
示例：
    #!/bin/bash

    my_array=(A B "C" D)

    echo "第一个元素为: ${my_array[0]}"
    echo "第二个元素为: ${my_array[1]}"
    echo "第三个元素为: ${my_array[2]}"
    echo "第四个元素为: ${my_array[3]}"
    
执行脚本，输出结果如下所示：

    $ ./test.sh
    第一个元素为: A
    第二个元素为: B
    第三个元素为: C
    第四个元素为: D

特殊用法：
    # 使用 @ 或 * 符号可以获取数组中的所有元素，例如：
          echo ${array_name[@]}
          echo ${array_name[*]}
    # 取得数组元素的个数
          length=${#array_name[@]}
    # 或者
          length=${#array_name[*]}
    # 取得数组单个元素的长度
          lengthn=${#array_name[n]}

```

> 运算符

    Shell 和其他编程语言一样，支持多种运算符，包括：

        1) 算数运算符
        2) 关系运算符
        3) 布尔运算符
        4) 字符串运算符
        5) 文件测试运算符

(1) 算数运算符

    原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
    expr 是一款表达式计算工具，使用它能完成表达式的求值操作.
![image](https://user-images.githubusercontent.com/42632290/159151369-53cd978c-d1fc-4ab7-aea3-995bdffddbc5.png)

    注意：
         (1) 表达式和运算符之间要有空格，例如: 2+2 是不对的，必须写成 2 + 2
         (2) 条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。
```
实例:
    #!/bin/bash
    a=10
    b=20

    val=`expr $a + $b`
    echo "a + b : $val"  # a + b : 30

    val=`expr $a - $b`
    echo "a - b : $val"  # a - b : -10

    val=`expr $a \* $b`
    echo "a * b : $val"  # a * b : 200, 表达式中 * 需要转义

    val=`expr $b / $a`
    echo "b / a : $val"  # b / a : 2

    val=`expr $b % $a`
    echo "b % a : $val"  # b % a : 0

    if [ $a == $b ]
    then
       echo "a 等于 b"
    fi
    if [ $a != $b ]
    then
       echo "a 不等于 b" # a 不等于 b
    fi

注意：
    (1) 乘号(*)前边必须加反斜杠(\)才能实现乘法运算；
    (2) 完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边
    (3) 在 MAC 中 shell 的 expr 语法是：$((表达式))，此处表达式中的 "*" 不需要转义符号 "\" 
```

(2) 算数运算符

    关系运算符只支持数字，不支持字符串，除非字符串的值是数字
    
![image](https://user-images.githubusercontent.com/42632290/159152048-88e0e956-37b8-40dd-846f-ec5f6916ec21.png)

```
#!/bin/bash
a=10
b=20

if [ $a -eq $b ]
then
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi

执行脚本，输出结果如下所示：

10 -ne 20: a 不等于 b
10 -gt 20: a 不大于 b
10 -lt 20: a 小于 b
10 -ge 20: a 小于 b
10 -le 20: a 小于或等于 b
```

(3) 布尔运算符

![image](https://user-images.githubusercontent.com/42632290/159152470-9fe8eb1f-5010-4c5f-8af1-56b0d8c17b6e.png)

```
#!/bin/bash

a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a == $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
执行脚本，输出结果如下所示：

10 != 20 : a 不等于 b
10 小于 100 且 20 大于 15 : 返回 true
10 小于 100 或 20 大于 100 : 返回 true
10 小于 5 或 20 大于 100 : 返回 false
```

(4) 逻辑运算符

![image](https://user-images.githubusercontent.com/42632290/159153760-1e054fdb-f06c-4b0c-8349-96a4112433a0.png)

```
#!/bin/bash

a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

执行脚本，输出结果如下所示：

返回 false
返回 true
```

注意：(1) 在shell中，”与”的运算符为”-a”，同时也可以用”&&”表示。”或”的运算符为”-o”，同时也可以用”||”表示。
      (2) ”&&”和”||“有短路功能，也可以称为”短路与”，”短路或“，布尔的与或和逻辑的与或不仅功能上略有区别，在使用方法上也有不同之处
```
    1) 作为条件判断的运算符时，”-a”和”-o“只能用”单大括号”括起，”&&”和”||“只能用”双大括号”括起，如果使用[[]]括起”-a -o”或者使用”单大括号”括起”&& ||”，则都会出现语法错误
    如：if [ $a -lt 100 && $b -gt 100 ]   语法错误
        if [[ $a -lt 100 -a $b -gt 100 ]] 语法错误
    
    2) 短路功能
       cmd1 && cmd2  # 当cmd1执行成功时，则执行cmd2命令，当cmd1执行失败时，cmd2则不会被执行
       cmd1 || cmd2  # 当cmd1执行结果为false，则必须要执行cmd2，如果cmd1的执行结果为true，cmd2则不会被执行
       cmd1 && cmd2 || cmd3  # 当cmd1执行成功，则执行cmd2，如果cmd1执行失败，则不执行cmd2，而是执行cmd3
```

  (5) 字符串运算符
  
![image](https://user-images.githubusercontent.com/42632290/159154283-b8e8c1cc-dba7-4818-a6bf-89285605c64f.png)

```
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi

执行脚本，输出结果如下所示：

abc = efg: a 不等于 b
abc != efg : a 不等于 b
-z abc : 字符串长度不为 0
-n abc : 字符串长度不为 0
abc : 字符串不为空
```

  (6) 文件测试运算符

![image](https://user-images.githubusercontent.com/42632290/159154565-c117d22e-35ff-4afd-98f0-c57319b9d960.png)

其他检查符：
    -S: 判断某文件是否 socket。
    -L: 检测文件是否存在并且是一个符号链接。

> 流程控制


> 函数


> 输入输出重定向


> 文件包含


> echo/printf/test 命令























