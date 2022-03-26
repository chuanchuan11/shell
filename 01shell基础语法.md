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

    在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，其中 $0 为执行的文件名（包含文件路径)
    通过$n的形式来接收的参数，在 Shell 中称为位置参数
```
示例：
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

(1)if

```
语法：
    if condition1
    then
        command1
    elif condition2 
    then 
        command2
    else
        commandN
    fi
    
示例：
    a=10
    b=20
    
    if [ $a == $b ]
    then
       echo "a 等于 b"
    elif [ $a -gt $b ]
    then
       echo "a 大于 b"
    elif [ $a -lt $b ]
    then
       echo "a 小于 b"
    else
       echo "没有符合的条件"
    fi

输出结果：
a 小于 b
```

注意：在 sh/bash 里可不能这么写，如果 else 分支没有语句执行，就不要写这个 else

(2) for 循环

```
语法：
    for var in item1 item2 ... itemN  #in 列表可以包含替换、字符串和文件名。in列表是可选的，如果不用它，for循环使用命令行的位置参数
    do
        command1
        command2
        ...
        commandN
    done

示例：
    for loop in 1 2 3 4 5
    do
        echo "The value is: $loop"
    done
    
    输出结果：
        The value is: 1
        The value is: 2
        The value is: 3
        The value is: 4
        The value is: 5
```

(3) while语句

```
语法：
    while condition
    do
        command
    done
    
示例：
    #!/bin/bash
    int=1
    while(( $int<=5 ))
    do
        echo $int
        let "int++"
    done
    运行脚本，输出：
    1
    2
    3
    4
    5
```

(4) 无线循环语句

```
无限循环语法格式：
                while :
                do
                    command
                done
                
                或者

                while true
                    do
                       command
                done
                
               或者 
               
                  for (( ; ; ))
````

(5) until语句

    until 循环执行一系列命令直至条件为 true 时停止
    
```
语法：
    until condition  # condition 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环。
    do
        command
    done
    
实例
    #!/bin/bash

    a=0

    until [ ! $a -lt 5 ]
    do
       echo $a
       a=`expr $a + 1`
    done

输出结果为：
    0
    1
    2
    3
    4
    5
```

(6) case 语句

```
语法：
    case 值 in
    模式1)
        command1
        command2
        ...
        commandN
        ;;
    模式2)           # 如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。
        command1
        command2
        ...
        commandN
        ;;
     esac

示例：

echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac

输出结果：

输入 1 到 4 之间的数字:
你输入的数字为:
3
你选择了 3
```

(7)跳出循环：break和continue

    break命令允许跳出所有循环（终止执行后面的所有循环）
    continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环
    
```
示例1：
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"          #多模式匹配
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done

示例2：
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done

```

> 函数

(1)函数定义

```
语法：
function name() {
    statements
    [return value]
}

1、可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
2、参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)

示例：
name() {
    statements
    [return value]
}

如果写了 function 关键字，也可以省略函数名后面的小括号：
function name {
    statements
    [return value]
}
```

(2) 函数调用

    1) 调用 Shell 函数时可以给它传递参数，也可以不传递。如果不传递参数，直接给出函数名字即可：
       name

    2) 如果传递参数，那么多个参数之间以空格分隔：
       name param1 param2 param3
    
       不管是哪种形式，函数名字后面都不需要带括号

    3) 函数返回值在调用该函数后通过 $? 来获得

       a) $? 获取上一个命令的退出状态
       b) Shell 函数中的 return 关键字用来表示函数的退出状态，而不是函数的返回值；Shell 不像其它编程语言，没有专门处理返回值的关键字

```
示例1：
#!/bin/bash
function getsum(){
    local sum=0
    for n in $@   #遍历所有输入参数
    do
         ((sum+=n))
    done
    return $sum
}
getsum 10 20 55 15  #调用函数并传递参数
echo $?  #获取函数返回值
total=$(getsum 10 20 55 15)  #调用函数并传递参数，最后将结果赋值给一个变量，有些系统不支持
echo $total

输出结果：
 100
 100
```

![image](https://user-images.githubusercontent.com/42632290/159164933-f0a4537b-0edd-4736-b3f4-acaacc27a7d7.png)

> {}、()、[]与 $()、${}、$[]、$(())、(())、[[]]、的作用与区别

(1) ()和{}

    1) 单小括号()
        
        a) 命令组：括号中的命令将会新开一个子shell顺序执行，所以括号内的变量不能被脚本余下的部分使用。括号中多个命令之间用;隔开, 命令与括号之间不必加空格
        b) 用于初始化数组，如：array=(1 2 3 4)
    
    2) 单花括号{}
    
        a) 括号扩展：如：对括号中以,分割的文件列表进行扩展。对括号内以...分割的文件列表进行扩展
        
        ![image](https://user-images.githubusercontent.com/42632290/160232871-6f6566c4-76c8-45db-8eac-8083ca8864da.png)

        b) 代码块：这个结果实际上创建了一个匿名函数。与小括号的命令不同，花括号内的命令不会新开一个shell运行，即脚本余下部分仍可使用括号内的变量，括号内命令间用;分开，
           最后一个也必须有分号。{}的第一个括号和命令之间必须有一个空格
```
#!/bin/bash

(a=666;echo "internal, a= $a")
echo "external, a= $a"    # 括号内的变量不能被脚本余下的部分使用

{ b=999;echo "internal, b= $b";}
echo "external, b= $b"    # 括号内的变量可以被脚本余下部分使用

```
 
注意：
     对于()和{}，括号内的重定向符号只能影响到该条命令，括号外的重定向将影响到括号内的所有命令
 
(2) 单中括号[]----> test命令

    test 通常和 if 语句一起使用, 判断 expression 是否成立，成立时退出状态为 0，否则为非 0 值。
    
    test 命令有很多选项，可以进行数值、字符串和文件三个方面的检测，Shell test 命令的用法为：test expression，简写为：[ expression ]

```
注意：
      a) 必须在左括号的右侧和右括号的左侧各加一个空格，否则会报错。
      b) test命令使用标准的 **数学比较符号** 来表示字符串的比较，而用 **文本符号** 来表示数值的比较。
      c) 大于符号或小于符号必须要转义，否则会被理解成重定向  
      
示例：
read age
if test $age -le 2; then
    echo "婴儿"
elif test $age -ge 3 && test $age -le 8; then
    echo "幼儿"
elif [ $age -ge 9 ] && [ $age -le 17 ]; then
    echo "少年"
elif [ $age -ge 18 ] && [ $age -le 25 ]; then
    echo "成年"
elif test $age -ge 26 && test $age -le 40; then
    echo "青年"
elif test $age -ge 41 && [ $age -le 60 ]; then
    echo "中年"
else
    echo "老年"
fi
```
![image](https://user-images.githubusercontent.com/42632290/160230887-66a5bf1e-dbc7-4f35-8cde-6530d28df007.png)

![image](https://user-images.githubusercontent.com/42632290/160230891-23d7cf15-4aef-436e-8453-4ea068d3b5a0.png)

![image](https://user-images.githubusercontent.com/42632290/160230905-89dc8217-4924-401d-8e8c-b73fa867c363.png)

![image](https://user-images.githubusercontent.com/42632290/160230926-fb9938e4-be61-4bb5-ba03-5b050bb11ca0.png)

![image](https://user-images.githubusercontent.com/42632290/160230935-b9f022f7-f77c-40e1-8654-699f551752eb.png)

注意：
      a) ==、>、< 在大部分编程语言中都用来比较数字，而在 Shell 中，它们只能用来比较字符串，不能比较数字。
      b) 其次，不管是比较数字还是字符串，Shell 都不支持 >= 和 <= 运算符，切记

![image](https://user-images.githubusercontent.com/42632290/160231000-3e3adac9-c9dd-407b-a9d1-0df6ec5bad39.png)

```
示例1：
#!/bin/bash
read str1
read str2
#检测字符串是否为空
if [ -z "$str1" ] || [ -z "$str2" ]
then
    echo "字符串不能为空"
    exit 0
fi

示例2：
#!/bin/bash
read str1
read str2
#检测字符串是否为空
if [ -z "$str1" -o -z "$str2" ]  #使用 -o 选项取代之前的 ||
then
    echo "字符串不能为空"
    exit 0
fi

注意：
    1) 前面的代码我们使用两个[]命令，并使用||运算符将它们连接起来，这里我们改成-o选项，只使用一个[]命令就可以了
    2) 当你在 test 命令中使用变量时，强烈建议将变量用双引号""包围起来，这样能避免变量为空值时导致的很多奇葩问题
```

   test 命令比较奇葩，>、<、== 只能用来比较字符串，不能用来比较数字，比较数字需要使用 -eq、-gt 等选项；不管是比较字符串还是数字，test 都不支持 >= 和 <=。
   对于整型数字的比较，建议使用 (()), 支持各种运算符，写法也符合数学规则，用起来更加方便

参考：http://c.biancheng.net/view/2742.html


(3) (( ))及[[ ]] 

    分别是[]的针对 **数学比较表达式** 和 **字符串表达式** 的加强版

    1) (( )) 是专门用来进行整数运算的命令, 是常用的运算命令。

       注意：(( )) 只能进行整数运算，不能对小数（浮点数）或者字符串进行运算
```
语法格式为：
          ((表达式))
注意：
     a) 表达式可以只有一个，也可以有多个，多个表达式之间以逗号,分隔。对于多个表达式的情况，以最后一个表达式的值作为整个 (( )) 命令的执行结果。
     b) 可以使用$获取 (( )) 命令的结果，这和使用$获得变量值是类似的

```

![image](https://user-images.githubusercontent.com/42632290/160231466-0ade3d0f-5f00-4293-b702-9fbe36c6b97b.png)

注意：
    不需要再将表达式里面的大小于符号转义，除了可以使用标准的数学运算符外，还增加了以下符号：

![image](https://user-images.githubusercontent.com/42632290/160231814-4efa5a40-3360-4c28-8739-b1e2583b046a.png)

    2) [[ ]]是 Shell 内置关键字，它和 test 命令类似，也用来检测某个条件是否成立
    
```
语法为：
        [[ expression ]]

     当 [[ ]] 判断 expression 成立时，退出状态为 0，否则为非 0 值。注意[[ ]]和expression之间的空格，这两个空格是必须的，否则会导致语法错误
注意：
     [ ]] 是 Shell 内置关键字，不是命令，在使用时没有给函数传递参数的过程，所以 test 命令的某些注意事项在 [[ ]] 中就不存在了，具体包括：
         a) 不需要把变量名用双引号""包围起来，即使变量是空值，也不会出错。
         b) 不需要、也不能对 >、< 进行转义，转义后会出错
```

![image](https://user-images.githubusercontent.com/42632290/160231695-84392d3a-7c07-4458-94b1-d9670c18ffe9.png)

注意：
    [[ ]] 对数字的比较仍然不友好，所以建议，使用 if 判断条件时，用 (()) 来处理整型数字，用 [[ ]] 来处理字符串或者文件

(4) $()和 ``命令替换
     
     Shell 命令替换是指将**命令的输出结果**赋值给某个变量。比如，在某个目录中输入 ls 命令可查看当前目录中所有的文件，但如何将输出内容存入某个变量中呢？这就需要使用命令替换了，这也是 Shell 编程中使用非常频繁的功能。
     
     Shell 中有两种方式可以完成命令替换，一种是反引号``，一种是$()

```
例如:  
    variable=`commands`
    variable=$(commands)

其中，variable 是变量名，commands 是要执行的命令。commands 可以只有一个命令，也可以有多个命令，多个命令之间以分号;分隔

各自的优缺点：
    1) ` ` 基本上可用在全部的 unix shell 中使用，若写成 shell script ，其移植性比较高，但反单引号容易打错或看错。
    2) $()并不是所有shell都支持

```

(5) ${}

    1) 变量替换
    
        $var 与${var} 并没有什么不一样，但是用 ${ } 会比较精确的界定变量名称的范围
```
例子：
    $A=B
    echo $AB
    原本是打算先将 $A 的结果替换出来，然后再补一个 B 字母于其后，但在命令行上，真正的结果却是只会提换变量名称为 AB 的值出来…
    若使用 ${ } 就没问题了：
    echo ${A}B
    BB
```

    2) 模式匹配--字符串操作
    
![image](https://user-images.githubusercontent.com/42632290/160229151-29ade91e-de2a-4564-80ca-6cb8214e4c86.png)  

```
示例1：从字符串左边开始计数
    url="c.biancheng.net"
    echo ${url: 2: 9}      # 结果为：biancheng
    echo ${url: 2}         # 省略length，截取到字符串末尾，结果为：biancheng.net
    
示例2：从右边开始计数
    url="c.biancheng.net"
    echo ${url: 0-13: 9}   # 结果为biancheng。从右边数，b是第 13 个字符。
    echo ${url: 0-13}      # 省略length，直接截取到字符串末尾，结果为：biancheng.net

    同第 1) 种格式相比，第 2) 种格式仅仅多了0-，这是固定的写法，专门用来表示从字符串右边开始计数。
这里需要强调两点：
    1) 从左边开始计数时，起始数字是 0（这符合程序员思维）；从右边开始计数时，起始数字是 1（这符合常人思维）。计数方向不同，起始数字也不同。
    2) 不管从哪边开始计数，截取方向都是从左到右。    
 
示例3：使用 # 号删除左边字符
    url="http://c.biancheng.net/index.html"
    echo ${url#*/}    #匹配到第一个字符：结果为 /c.biancheng.net/index.html
    echo ${url##*/}   #匹配到最后一个字符：结果为 index.html
 
示例4：使用 % 删除右边字符  
    url="http://c.biancheng.net/index.html"
    echo ${url%/*}    #匹配到第一个字符串：结果为 http://c.biancheng.net
    echo ${url%%/*}   #匹配到最后一个字符串：结果为 http:
```

(6) $[] $(()) 数学运算

    $[]和$(())是一样的，都是进行数学运算的。支持+ - * / %（“加、减、乘、除、取模”）。但是注意，bash只能作整数运算，对于浮点数是当作字符串处理的

```
示例：
   a=5; b=7; c=2
   echo $(( a+b*c ))    # 输出结果为：19
   echo $(( (a+b)/c ))  # 输出结果为：6
   echo $(( (a*b)%c))   # 输出结果为：1

  1) 在 $(( )) 中的变量名称，可于其前面加 $ 符号来替换，也可以不用，如：$(( $a + $b * $c)) 也可得到 19 的结果
  2) $(( )) 还可作不同进位(如二进制、八进位、十六进制)运算，只是输出结果皆为十进制而已,如：echo $((16#2a)) 结果为 42 (16进位转十进制)

```

参考：http://c.biancheng.net/view/2751.html

> 输入输出重定向

    输入输出重定向就是「改变输入与输出的方向」

    为了表示和区分已经打开的文件，Linux 会给每个文件分配一个 ID，这个 ID 就是一个整数，被称为文件描述符（File Descriptor），
    
    stdin、stdout、stderr 默认都是打开的，在重定向的过程中，0、1、2 这三个文件描述符可以直接使用
    
![image](https://user-images.githubusercontent.com/42632290/160234453-5107298f-e0cc-47f3-939e-2e04a9333e9b.png)

![image](https://user-images.githubusercontent.com/42632290/160234564-6d7cca69-782b-45fa-9bee-1360c7e1794c.png)

    (1) 输出重定向:
    
      是指命令的结果不再输出到显示器上，而是输出到其它地方，一般是文件中。这样做的最大好处就是把命令的结果保存起来，当我们需要的时候可以随时查询

![image](https://user-images.githubusercontent.com/42632290/160234490-b74e725d-7ad5-4d0b-ac40-114b6d0c4745.png)

注意：
    1) 输出重定向的完整写法其实是: fd>file或者fd>>file，其中 fd 表示文件描述符，如果不写，默认为 1，也就是标准输出文件[文件描述符为大于 1 的值时，比如 2，就必须写上]
    2) 需要重点说明的是，fd和>之间不能有空格，否则 Shell 会解析失败；>和file之间的空格可有可无

```
示例1：把正确结果和错误信息都保存到一个文件中
ls -l >out.log 2>&1
ls java >>out.log 2>&1
cat out.log
总用量 12
drwxr-xr-x. 2 root     root      21 7月   1 2016 abc
-rw-r--r--. 1 mozhiyan mozhiyan 399 3月  11 17:12 demo.sh
-rw-rw-r--. 1 mozhiyan mozhiyan 278 3月  16 17:17 main.c
-rw-rw-r--. 1 mozhiyan mozhiyan   0 3月  22 17:39 out.log
-rwxr-xr-x. 1 mozhiyan mozhiyan 187 3月  22 17:16 test.sh
ls: 无法访问java: 没有那个文件或目录

示例2：把正确结果和错误信息分开保存到不同的文件中
ls -l >>out.log 2>>err.log


```

    (2) 输入重定向
       
       输入重定向就是改变输入的方向，不再使用键盘作为命令输入的来源，而是使用文件作为命令的输入

![image](https://user-images.githubusercontent.com/42632290/160234855-f0631722-ccc9-4f8c-bf8a-a8b71a449164.png)

       和输出重定向类似，输入重定向的完整写法是fd<file，其中 fd 表示文件描述符，如果不写，默认为 0，也就是标准输入文件

```
示例1： 
     Linux wc 命令可以用来对文本进行统计，包括单词个数、行数、字节数，它的用法如下：
     wc  [选项]  [文件名]
     其中，-c选项统计字节数，-w选项统计单词数，-l选项统计行数

#统计文件行数
cat readme.txt  #预览一下文件内容
    hello
    world
wc -l <readme.txt  #输入重定向
2

#逐行读取文件内容
#!/bin/bash
while read str; do
    echo $str
done <readme.txt
hello
world
```
    (3)连续交互输入

```
command <<EOF
    document
EOF
``
    注意：
         1) <<之后的分界符可以自由定义，只要再碰到相同的分界符，两个分界符之间的内容将作为命令的输入, 使用特定的分界符作为命令输入的结束标志，而不使用 Ctrl+D 键
         2) 输出也可以使用分界符方式
 ```
 示例1：
$ wc -l <<END
    欢迎来到
    菜鸟教程
    www.runoob.com
$END
3 # 输出结果为 3 行
 ```
 
   (4) /dev/null 文件
              
      如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：
                                   
                     command > /dev/null

      /dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；
    
      如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

      如果希望屏蔽 stdout 和 stderr，可以这样写：command > /dev/null 2>&1          
              
> 文件包含

     和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

```
Shell 文件包含的语法格式如下：

. filename   # 注意点号(.)和文件名中间有一空格

或
source filename
    
示例1：
test1.sh
    #!/bin/bash
    url="http://www.baidu.com"

test2.sh
    #使用 . 号来引用test1.sh 文件
    . ./test1.sh

    # 或者使用以下包含文件代码
    # source ./test1.sh

    echo "网址为：$url"

注意：
     被包含的文件 test1.sh 不需要可执行权限。
```

> echo/printf 命令

  (1)echo
   
    Shell 的 echo 指令是用于字符串的输出。命令格式:
    
        echo string

```
例子1：
    echo -e "OK! \n"      # -e 开启转义 \n 换行
    echo "It is a test"
输出结果：
    OK!
    
    It is a test  
    
例子2：
    echo -e "OK! \c"     # -e 开启转义 \c 不换行
    echo "It is a test"
输出结果：
    OK! It is a test
```
    
  (2)printf

    printf 命令模仿 C 程序库（library）里的 printf() 程序
    
    printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认的 printf 不会像 echo 自动添加换行符，我们可以手动添加 \n。

```
 printf 命令的语法：

    printf  format-string  [arguments...]
    
参数说明：
    format-string: 为格式控制字符串
    arguments: 为参数列表。

示例1：
    $ echo "Hello, Shell"
      Hello, Shell
    $ printf "Hello, Shell\n"
      Hello, Shell

示例2：
    printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
    printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
    printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
    printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
输出：
    姓名     性别   体重kg
    郭靖     男      66.12
    杨过     男      48.65
    郭芙     女      47.99

注意：
    
％s 输出一个字符串，％d 整型输出，％c 输出一个字符，％f 输出实数，以小数形式输出。

%-10s 指一个宽度为 10 个字符（- 表示左对齐，没有则表示右对齐），任何字符都会被显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

%-4.2f 指格式化为小数，其中 .2 指保留2位小数   

```

    
