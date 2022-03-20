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


> shell数组

    bash支持一维数组（不支持多维数组），并且没有限定数组的大小。数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

(1)数组定义

   用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：
                   数组名=(值1 值2 ... 值n)
   例如：
                    array_name=(value0 value1 value2 value3)

(2)数组使用

    读取数组元素值的一般格式是：
                              ${数组名[下标]}
    例如：
                    valuen=${array_name[n]}
                    
```
    # 使用 @ 符号可以获取数组中的所有元素，例如：
          echo ${array_name[@]}
    # 取得数组元素的个数
          length=${#array_name[@]}
    # 或者
          length=${#array_name[*]}
    # 取得数组单个元素的长度
          lengthn=${#array_name[n]}
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


> shell 数组


> 运算符


> 流程控制


> 函数


> 输入输出重定向


> 文件包含


> echo/printf/test 命令























