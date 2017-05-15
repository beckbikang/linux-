## shell编程的整理

目录

	环境
	变量
	字符串
	传递参数
	数组
	运算符
	echo/printf
	test命令
	流程控制
	函数
	输入输出重定向
	文件包含

1 环境

	1.1 头部定义解释器：#/bin/sh
	1.2 保证文件是可以执行的
2 变量

定义变量

	符合一般的语言规范 ab=”123″
	使用变量 echo $ab,${ab};
	变量的使用
	for lang in dlang,java,c++; do
	echo “language=${lang}”
	done
	只读变量
	readonly ab
	ab=56#[报错]
	删除变量
	unset ab
运行shell时，会同时存在三种变量：

	1) 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
	2) 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
	3) shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行
	
3 字符串

	单引号原样输出，双引号替换 echo ‘$ab’ echo “$ab”结果不一样
	字符串的拼接 将变量放到字符串里面即可
	字符串的长度 echo ${#ab}
	字符串的查找 echo expr index "$ab" 2
	
4 数组

	数组的定义 arr=(1 2 3 4 5)
	使用数组
	
	echo ${arr[2]}
	echo ${arr[@]}
	元素个数或者单个元素长度
	echo ${#arr[@]}
	echo ${#arr[*]}
	echo ${#arr[2]}
	
	
5 传递参数

	$# 参数个数
	$*所有参数
	$$进程id
	$@ 与$*相同，但是使用时加引号，并在引号中返回每个参数。
	$！后台运行的最后一个进程的ID号
	$? 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
	echo $?,$*,$@,$!,$$,$#
	
	“$@”表示N个参数 “$*”只有一个
	
6 shell的运算符

	算数运算符[算术运算符和其他语言类似]
	ab = expr ab + 3
	
	关系运算符
	不是使用 == > < 而是-+英语字母
	-eq 检测两个数是否相等，相等返回 true。 [ $a -eq $b ] 返回 false。
	-ne 检测两个数是否相等，不相等返回 true。 [ $a -ne $b ] 返回 true。
	-gt 检测左边的数是否大于右边的，如果是，则返回 true。 [ $a -gt $b ] 返回 false。
	-lt 检测左边的数是否小于右边的，如果是，则返回 true。 [ $a -lt $b ] 返回 true。
	-ge 检测左边的数是否大等于右边的，如果是，则返回 true。 [ $a -ge $b ] 返回 false。
	-le 检测左边的数是否小于等于右边的，如果是，则返回 true。 [ $a -le $b ] 返回 true。
	
	布尔运算符
	! 非运算，表达式为 true 则返回 false，否则返回 true。 [ ! false ] 返回 true。
	-o 或运算，有一个表达式为 true 则返回 true。
	-a 与运算，两个表达式都为 true 才返回 true。
	逻辑运算符
	&& 逻辑的 AND [[ $a -lt 100 && $b -gt 100 ]] 返回 false
	|| 逻辑的 OR [[ $a -lt 100 || $b -gt 100 ]] 返回 true
	字符串
	= 检测两个字符串是否相等，相等返回 true。
	!= 检测两个字符串是否相等，不相等返回 true。
	-z 检测字符串长度是否为0，为0返回 true。
	-n 检测字符串长度是否为0，不为0返回 true。
	str 检测字符串是否为空，不为空返回 true。 [ $a ] 返回 true。
	文件测试运算符
	-b file 检测文件是否是块设备文件，如果是，则返回 true。
	-c file 检测文件是否是字符设备文件，如果是，则返回 true。
	-d file 检测文件是否是目录，如果是，则返回 true。
	-f file 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。
	-g file 检测文件是否设置了 SGID 位，如果是，则返回 true。
	-k file 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。
	-p file 检测文件是否是具名管道，如果是，则返回 true。
	-u file 检测文件是否设置了 SUID 位，如果是，则返回 true。
	-r file 检测文件是否可读，如果是，则返回 true。
	-w file 检测文件是否可写，如果是，则返回 true。
	-x file 检测文件是否可执行，如果是，则返回 true。
	-s file 检测文件是否为空（文件大小是否大于0），不为空返回 true。
	-e file 检测文件（包括目录）是否存在，如果是，则返回 true。

7 echo/printf

	echo 输出
	printf 格式化输出类似c语言 printf “cd1=%d”,$cd1 

8 test命令
	
	test+上面的运算符即可 if [ test -e 1.log ] …

9函数

函数的定义

	[ function ] funname [()]
	{
	action;
	[return int;]
	}
	定义函数和使用函数的例子
	test1(){
	echo “test1”;
	}
	test1
	文件参数就是函数参数，见咱们上面的$

10 输入输出重定向

使用系统的重定向即可

	command > file 将输出重定向到 file。
	command < file 将输入重定向到 file。
	command >> file 将输出以追加的方式重定向到 file。
	n > file 将文件描述符为 n 的文件重定向到 file。
	n >> file 将文件描述符为 n 的文件以追加的方式重定向到 file。
	n >& m 将输出文件 m 和 n 合并。
	n <& m 将输入文件 m 和 n 合并。
	<< tag 将开始标记 tag 和结束标记 tag 之间的内容作为输入。
	
11 包含文件

	使用.+文件路径包含文件
	例如 . ./t_include.sh 号包含
	
12 流程控制

if语句

	if condition1
	then
		command1
	elif condition2
	then
		command2
	else
		commandN
	fi
for语句

	for var in item1 item2 … itemN
	do
		command1
		command2
		…
		commandN
	done
	
while语句

	while condition
	do
		command
	done
	例子
	int=1
	while(( $int<=5 ))
	do
		echo $int
	let “int++”
	done
	
until格式

	until condition
	do
		command
	done
	
case 类似switch 只是格式有点不一样
	
	echo ‘输入 1 到 4 之间的数字:’
	echo ‘你输入的数字为:’
	read aNum
	case $aNum in
		1) echo ‘你选择了 1’
		;;
		2) echo ‘你选择了 2’
		;;
		3) echo ‘你选择了 3’
		;;
		4) echo ‘你选择了 4’
		;;
		*) echo ‘你没有输入 1 到 4 之间的数字’
		;;
	esac
	可以使用break跳出循环或者continue跳出本次循环