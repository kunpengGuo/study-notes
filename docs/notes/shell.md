# Shell脚本 语法

## 一、简介
linux命令语言

## 二、基本命令

### 变量
1、变量赋值
变量名和等号之间不能有空格
命名只能使用英文字母，数字，下划线，首个字符不能以数字开头
中间不能有空格，可以有下划线
不能使用标点符号
不能使用bash里的关键字(可使用help命令查看保留关键字)
```bash
your_name="runoob.com"
```

语句给变量赋值
```bash
for file in `ls /etc`
OR
for file in $(ls /etc)
```
以上语句将/etc下目录的文件名循环出来

只读变量
```bash
your_name="test"
readonly your_name
your_name="preview" # 再次赋值执行脚本会报错: /bin/sh: NAME: This variable is read only.
```

2、使用变量
```bash
your_name="test"
echo $your_name
echo ${your_name}
```

3、删除变量
```bash
unset your_name
```
不能删除只读变量

4、变量类型
- 1) 局部变量 局部变量在脚本或命令中定义, 仅在当前shell实例中有效, 其他shell启动的程序不能访问局部变量
- 2) 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量
- 3) shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

### 字符串
```bash
str='this is a string' # 单引号
your_name="runoob" # 双引号
str="Hello, I know you are \"${your_name}\"! \n" # 字符串拼接
```
双引号解析变量，单引号不解析变量

获取字符串长度
```bash
str="abcd"
echo ${#str} # 输出 4
```
变量为数组时 ${#str} 等价于 ${#str[0]}

截取字符串
> 从字符串第2个开始截取4个字符
```bash
str="runoob is a great site"
echo ${str:1:4} # 输出 unoo
```

查找字符串
> 查找子字符串i或o的位置
```bash
str="runoob is a great site"
echo `expr index "$str" io`  # 输出 4
```

### 数组
bash仅支持一维数组,不支持多维数组,没有限制数组大小

1、定义数组
```bash
array_name=(value0 value1 value2)
OR
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```
2、关联数组
```bash
declare -A site=(["baidu"]="www.baidu.com",["taobao"]="www.taobao.com")
# 或者先声明 再设置键和值
declare -A site
site["baidu"]="www.baidu.com"
site["taobao"]="www.taobao.com"
```
3、读取数组
${数组名[下标]}
```bash
value1=${array_name[1]}
valuen=${array_name[n]}
echo ${array_name[@]} # 使用@或*符号可以获取所有元素 输出: value0 value1
echo "数组的键为: ${!site[*]}" # 在数组前加 ! 可以获取数组的所有键 输出 0 1 
```

3、获取数组长度
```bash
length=${#array_name[@]}
# 或
length=${#array_name[*]}
```

### 传参
脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数
```bash
# 执行 sh test.sh a b c d
echo "执行的文件名: $0"; # 输出 执行的文件名: test.sh
echo "第一个参数: $1"; # 输出 第一个参数: a
echo "第二个参数: $2"; # 输出 第二个参数: b
echo "第二个参数: $3"; # 输出 第二个参数: c
```
特殊字符处理
| 参数处理 | 说 明 |
|  ---  | ---  |
| $# | 传递到脚本的参数个数 |
| $* | 总行数 |
| $$ | 脚本运行的当前进程ID号 |
| $! | 后台运行的最后一个进程ID号 |
| $@ | 与 $* 相同，但是使用时加引号，并在引号中返回每个参数。 <br> 如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数 |

### 运算符
1、算数运算符
原生bash不支持简单的数学运算，可以通过其他命令来实现，例如 awk和expr, expr最常用
表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2
```bash
val=`expr 2 + 2`
echo "两数之和: ${val}" # 输出: 4
a=10
b=20
val=`expr $a + $b`
echo "a + b : $val" # 输出: a + b : 30
val=`expr $a \* $b`
echo "a * b : $val" # 乘号(*)前边必须加反斜线才能实现乘法运算
```
2、关系运算符
关系运算符只支持数字，不支持字符串，除非字符串的值是数字
| 运算符 | 说 明 | 举例 |
|  ---  | ---  | --- |
| -eq | 检测两个数是否相等(=),相等返回 true | [ $a -eq $b ] |
| -ne | 检测两个数是否不相等(!=)，不相等返回 true | [ $a -ne $b ]  |
| -gt | 检测左边的数是否大于右边的(>)，如果是，则返回 true。 | [ $a -gt $b ] |
| -lt | 检测左边的数是否小于右边的(<)，如果是，则返回 true | [ $a -lt $b ] |
| -ge | 检测左边的数是否大于等于右边的(>=)，如果是，则返回 true | [ $a -ge $b ] |
| -le | 检测左边的数是否小于等于右边的(<=)，如果是，则返回 true | [ $a -le $b ] |

```bash
a=10
b=20

if [ $a -eq $b ]
then
	echo "$a -eq $b : a 等于 b"
else
	echo "$a -eq $b : a 不等于 b"
fi
```

3、布尔运算符
| 运算符 | 说 明 |
|  ---  | ---  |
| ! | 非运算 |
| -o | 或运算 |
| -a | 与运算 |

4、逻辑运算符
| 运算符 | 说 明 |
|  ---  | ---  |
| && | 与运算 |
| || | 或运算 |

5、字符串运算符
| 运算符 | 说 明 |
|  ---  | ---  |
| = | 等于 |
| != | 不等于 |
| -z | 检测字符串长度是否为0,为0返回 true |
| -n | 检测字符串长度是否不为0,不为0返回 true |
| $  | 检测字符串长度是否不为空,不为空返回 true |

6、文件测试运算符
| 操作符 | 说 明 |
|  ---  | ---  |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true |
| -d file | 检测文件是否是目录，如果是，则返回 true |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true |
| -p file | 检测文件是否是有名管道，如果是，则返回 true |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true |
| -r file | 检测文件是否可读，如果是，则返回 true |
| -w file | 检测文件是否可写，如果是，则返回 true |
| -x file | 检测文件是否可执行，如果是，则返回 true |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true |

### 流程控制
sh 的流程控制不可为空
1、 if-else
if else-if else 格式:
```bash
if condition1
then
	command1
elif condition2
then
	condition2
else
	condition3
fi
```
if eles 的[...] 判断语言中大于使用 -gt,小于使用 -lt
```bash
if [ "$a" -gt "$b"]; then
	...
fi
```
使用 ((...)) 作判断语句，大于小于之间用 >和<
```bash
if (( a > b));then
	...
fi
```

2、for循环
```bash
for var in item1 item2 ... itemN
do
	command1
	command2
	...
	commandn
done
```

### 函数
- 可以带 function fun() 定义，也可以直接 func() 定义
- 参数返回,可以显示加: return返回，如果不加,将以最后一条命令运行结果作为返回值

```bash
demoFun(){
	echo "这是一个 shell 函数"
}
echo "----函数开始执行----"
demoFun
echo "----函数执行完毕----"
```

带参数
```bash
test(){
	echo "第一个参数: $1"
	echo "第二个参数: $2"
}
test 1 2
```