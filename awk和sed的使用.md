##awk && sed && uniq && sort


###awk

awk 的作用。对每行进行切割和处理

	awk的常量
	ARGC               命令行参数个数
	ARGV               命令行参数排列
	ENVIRON            支持队列中系统环境变量的使用
	FILENAME           awk浏览的文件名
	FNR                浏览文件的记录数
	FS                 设置输入域分隔符，等价于命令行 -F选项
	NF                 浏览记录的域的个数
	NR                 已读的记录数
	OFS                输出域分隔符
	ORS                输出记录分隔符
	RS                 控制记录分隔符


awk ‘parttern + action’  {filenames}

命令行：

	1 awk -F command file
	2 #!/bin/awk
	3 awk - F awk-file inputfile

常用
	
	1 打印(可用print printf)
	awk -F’`’ ‘{print $1”\t”$7}’ 或 awk '/zhijun/{printf("ip is %s\n",$3);}' last.txt 

	2头部尾部有操作
	awk -F '`' 'BEGIN{print "begin";}{print $1"\t"$7;}END{print "end";}' isdn_read-2016-02-03_00203  

	3 搜索字符串
		last -n 10 > last.txt
		 awk '/zhijun3/' last.txt  
		 不匹配;
		 	awk '/~zhihu3/' last.txt
		搜索并打印 awk '/zhijun/{print $2}' last.txt 
		tail -10 /data1/sinawap/logs/nginx/error/20170508_error.log  | awk -F " " '/wm/{print $0}'

	4 常量
		awk '/zhijun/{print $2,FILENAME,NR,NF,OFS}' last.txt 
		tail -10 /data1/sinawap/logs/nginx/error/20170508_error.log  | awk -F " " '/wm/{print $1,FILENAME,NR,NF,OFS}'
 
	5 简单统计
		
		awk 'BEGIN{count=0;}/zhijun/{count++;}END{print count}’ last.txt
		awk -F" " 'BEGIN{count=0;}/wm/{count++;}END{print count;}' test.error 

	6 支持if/else   while、do/while、for、break、continue
	
	awk 'BEGIN{count=0;}/zhijun/{sum=0;for(i=0;i<3;i++){sum+=i;} count+=sum;}END{print count}' last.txt
	
	awk 'BEGIN{count=0;}/zhijun/{sum=0;i=0;while(i<3){sum+=i;i++;} count+=sum;}END{print count}’ last.txt
	
	awk 'BEGIN{count=0;}/zhijun/{sum=0;i=0;while(i<3){if(i == 2){i++; continue;}sum+=i;i++;} count+=sum;}END{print count}' last.txt
	
	awk 'BEGIN{count=0;}/zhijun/{sum=0;i=0;while(i<3){if(i == 2){i++; break;}sum+=i;i++;} count+=sum;}END{print count}’ last.txt
	
	6.1
		
	awk -F" " 'BEGIN{count=0;}/wm/{sum=0;for(i=0;i<3;i++){sum=sum+i;};count=count+sum;}END{print count;}' test.error 

	7数组
		
		awk 'BEGIN{count =0;}{name[count] = $1;count = count+1;}END{print count;for(i=0;i<count;i++)print name[i];}' last.txt

	8嵌套
		
		awk '/zhijun/' last.txt | awk '{print $7}’  
	
	~ !~
		awk '/匹配串/ {for(i=1;i<=NF;i++) if($i ~ /字符串/)  print $i i}'  $filename
		awk '/匹配串/ {for(i=1;i<=NF;i++) if($i ~ /(字符串1|…|字符串n)/)  print $i i}'    $filename



###sed的使用

sed的作用-以行为单位进行行的替换，删除，新增，选取 

	sed的格式
	 sed[-nefri] command 文本
	
	-nefri
	常用选项：
	        -n∶使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN的资料一般都会被列出到萤幕上。但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行(或者动作)才会被列出来。
	        -e∶直接在指令列模式上进行 sed 的动作编辑；
	        -f∶直接将 sed 的动作写在一个档案内， -f filename 则可以执行 filename 内的sed 动作；
	        -r∶sed 的动作支援的是延伸型正规表示法的语法。(预设是基础正规表示法语法)
	        -i∶直接修改读取的档案内容，而不是由萤幕输出。       
	
	常用命令：
	        a   ∶新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
	        c   ∶取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
	        d   ∶删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
	         i   ∶插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
	         p  ∶列印，亦即将某个选择的资料印出。通常 p 会与参数 sed -n 一起运作～
	         s  ∶取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！
	
	-n -i -r

	1打印某几行
	 sed -ne '8,$p’ last.txt  获取者 sed  '8,$p' last.txt   
	2 查询几行,使用正则匹配
	 sed -n '/kang/p' last.txt
	3新增几行
	 sed -i ‘1,3a hello’ last.txt
	4 替换一行或多行
	 sed -i '1,3c hi’ last.txt
	5 替换某一部分
	 sed 's/hi/abcdefg/g' last.txt
	6 删除一行或者多行
	 sed '1,2d' last.txt
 


###uniq命令
	
	作用： 只处理连续的重复出现的行，报告或删除文件中重复的行。
	– c 显示输出中，在每行行首加上本行在文件中出现的次数。
	– d 只显示重复行。
	– u 只显示文件中不重复的各行。

###sort命令

	作用sort将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将他们按升序输出。

	sort -u 输出中去除重复的行

	sort -r 降序排列

	sort -o 将结果输出到原文件

	sort -n 按照数值来排序

	sort提供了-t选项，后面可以设定间隔符。

	sort -k来指定列数了。

	-f会将小写字母都转换为大写字母来进行比较，亦即忽略大小写
	
	-c会检查文件是否已排好序，如果乱序，则输出第一个乱序的行的相关信息，最后返回1
	
	-C会检查文件是否已排好序，如果乱序，不输出内容，仅返回1
	
	-M会以月份来排序，比如JAN小于FEB等等
	
	-b会忽略每一行前面的所有空白部分，从第一个可见字符开始比较。







