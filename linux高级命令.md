##linux常用的命令

参考
	http://man.linuxde.net/strace


watch/tcpdump/starce/ldd/ar/sar  

###watch命令
	
	watch是一个非常实用的命令，基本所有的Linux发行版都带有这个小工具，如同名字一样，watch可以帮你监测一个命令的运行结果，省得你一遍遍的手动运行。在Linux下，watch是周期性的执行下个程序，并全屏显示执行结果。你可以拿他来监测你想要的一切命令的结果变化，比如 tail 一个 log 文件，ls 监测某个文件的大小变化，看你的想象力了！
	1．命令格式：
		watch[参数][命令]
	2．命令功能：
		可以将命令的输出结果输出到标准输出设备，多用于周期性执行命令/定时执行命令
	3．命令参数：
		-n或--interval  watch缺省每2秒运行一下程序，可以用-n或-interval来指定间隔的时间。
		-d或--differences  用-d或--differences 选项watch 会高亮显示变化的区域。
			而-d=cumulative选项会把变动过的地方(不管最近的那次有没有变动)都高亮显示出来。
			-t 或-no-title  会关闭watch命令在顶部的时间间隔,命令，当前时间的输出。
	 	-h, --help 查看帮助文档
	 	
	4．使用实例：
		实例1：
			命令：每隔一秒高亮显示网络链接数的变化情况
				watch -n 1 -d netstat -ant
			说明：
			其它操作：
			切换终端： Ctrl+x
			退出watch：Ctrl+g
			
		实例2：每隔一秒高亮显示http链接数的变化情况
		命令：
		watch -n 1 -d 'pstree|grep http'
		说明：
			每隔一秒高亮显示http链接数的变化情况。 
			后面接的命令若带有管道符，需要加''将命令区域归整。
			
		实例3：实时查看模拟攻击客户机建立起来的连接数
		命令：
			watch 'netstat -an | grep:21 | \ grep<模拟攻击客户机的IP>| wc -l' 
		说明：
		
		实例4：监测当前目录中 scf' 的文件的变化
		命令：
			watch -d 'ls -l|grep scf' 
		
		实例5：10秒一次输出系统的平均负载
		命令：
			watch -n 10 'cat /proc/loadavg'
			
###tcpdump详解

	tcpdump抓包并保存成cap文件
	tcpdump采用命令行方式，它的命令格式为：
	　　tcpdump [ -adeflnNOpqStvx ] [ -c 数量 ] [ -F 文件名 ]
	　　　　　　　　　　[ -i 网络接口 ] [ -r 文件名] [ -s snaplen ]
	　　　　　　　　　　[ -T 类型 ] [ -w 文件名 ] [表达式 ]
		1. tcpdump的选项介绍
		　　　-a 　　　将网络地址和广播地址转变成名字；
		　　　-d 　　　将匹配信息包的代码以人们能够理解的汇编格式给出；
		　　　-dd 　　　将匹配信息包的代码以c语言程序段的格式给出；
		　　　-ddd 　　　将匹配信息包的代码以十进制的形式给出；
		　　　-e 　　　在输出行打印出数据链路层的头部信息；
		　　　-f 　　　将外部的Internet地址以数字的形式打印出来；
		　　　-l 　　　使标准输出变为缓冲行形式；
		　　　-n 　　　不把网络地址转换成名字；
		　　　-t 　　　在输出的每一行不打印时间戳；
		　　　-v 　　　输出一个稍微详细的信息，例如在ip包中可以包括ttl和服务类型的信息；
		　　　-vv 　　　输出详细的报文信息；
		　　　-c 　　　在收到指定的包的数目后，tcpdump就会停止；
		　　　-F 　　　从指定的文件中读取表达式,忽略其它的表达式；
		　　　-i 　　　指定监听的网络接口；
		　　　-r 　　　从指定的文件中读取包(这些包一般通过-w选项产生)；
		　　　-w 　　　直接将包写入文件中，并不分析和打印出来；
		　　　-T 　　　将监听到的包直接解释为指定的类型的报文，常见的类型有rpc（远程过程
		          调用）和snmp（简单网络管理协议；）		2 常用的
		          
		      tcpdump -i eth1 -c 2000 
		      tcpdump -i eth0 -t tcp -s 600
		      tcpdump host 58.240.72.195 -s 60
		      tcpdump 的抓包保存到文件的命令参数是-w xxx.cap
				抓eth1的包 
					tcpdump -i eth1 -w /tmp/xxx.cap 
				抓 192.168.1.123的包 
					tcpdump -i eth1 host 192.168.1.123 -w /tmp/xxx.cap 
				抓192.168.1.123的80端口的包 
					tcpdump -i eth1 host 192.168.1.123 and port 80 -w /tmp/xxx.cap 
				抓192.168.1.123的icmp的包 
					tcpdump -i eth1 host 192.168.1.123 and icmp -w /tmp/xxx.cap 
				抓192.168.1.123的80端口和110和25以外的其他端口的包 
					tcpdump -i eth1 host 192.168.1.123 and ! port 80 and ! port 25 					and ! port 110 -w /tmp/xxx.cap 
				抓vlan 1的包 
					tcpdump -i eth1 port 80 and vlan 1 -w /tmp/xxx.cap 
				抓pppoe的密码 
					tcpdump -i eth1 pppoes -w /tmp/xxx.cap 
				以100m大小分割保存文件， 超过100m另开一个文件 -C 100m 
				抓10000个包后退出 -c 10000 
				后台抓包， 控制台退出也不会影响： 
					nohup tcpdump -i eth1 port 110 -w /tmp/xxx.cap & 
		          
		          
		3 实际的例子
		
			1、抓取回环网口的包：tcpdump -i lo

			2、防止包截断：tcpdump -s0
			
			3、以数字显示主机及端口：tcpdump -n
	
			 第一种是关于类型的关键字，主要包括host，net，port, 例如 host 210.27.48.2，指明 210.27.48.2是一台主机，net 202.0.0.0 指明 202.0.0.0是一个网络地址，port 23 指明端口号是23。如果没有指定类型，缺省的类型是host.
			
			
			第二种是确定传输方向的关键字，主要包括src , dst ,dst or src, dst and src ,这些关键字指明了传输的方向。举例说明，src 210.27.48.2 ,指明ip包中源地址是210.27.48.2 , dst net 202.0.0.0 指明目的网络地址是202.0.0.0 。如果没有指明方向关键字，则缺省是src or dst关键字。
			
			
			第三种是协议的关键字，主要包括fddi,ip,arp,rarp,tcp,udp等类型。Fddi指明是在FDDI(分布式光纤数据接口网络)上的特定 的网络协议，实际上它是"ether"的别名，fddi和ether具有类似的源地址和目的地址，所以可以将fddi协议包当作ether的包进行处理和 分析。其他的几个关键字就是指明了监听的包的协议内容。如果没有指定任何协议，则tcpdump将会监听所有协议的信息包。
			
			 
			
			  除了这三种类型的关键字之外，其他重要的关键字如下：gateway, broadcast,less,greater,还有三种逻辑运算，取非运算是 'not ' '! ', 与运算是'and','&&;或运算 是'or' ,'||'；这些关键字可以组合起来构成强大的组合条件来满足人们的需要，下面举几个例子来说明。
			
			
			  普通情况下，直接启动tcpdump将监视第一个网络界面上所有流过的数据包。
			# tcpdump 
			tcpdump: listening on fxp0
			11:58:47.873028 202.102.245.40.netbios-ns > 202.102.245.127.netbios-ns: udp 50
			11:58:47.974331 0:10:7b:8:3a:56 > 1:80:c2:0:0:0 802.1d ui/C len=43
			                       0000 0000 0080 0000 1007 cf08 0900 0000
			                       0e80 0000 902b 4695 0980 8701 0014 0002
			                       000f 0000 902b 4695 0008 00
			11:58:48.373134 0:0:e8:5b:6d:85 > Broadcast sap e0 ui/C len=97
			                       ffff 0060 0004 ffff ffff ffff ffff ffff
			                       0452 ffff ffff 0000 e85b 6d85 4008 0002
			                       0640 4d41 5354 4552 5f57 4542 0000 0000
			                       0000 00
			
			
			使用-i参数指定tcpdump监听的网络界面，这在计算机具有多个网络界面时非常有用，
			使用-c参数指定要监听的数据包数量，
			使用-w参数指定将监听到的数据包写入文件中保存
			A想要截获所有210.27.48.1 的主机收到的和发出的所有的数据包：
			#tcpdump host 210.27.48.1
			
			
			B想要截获主机210.27.48.1 和主机210.27.48.2 或210.27.48.3的通信，使用命令：（在命令行中适用　括号时，一定要
			#tcpdump host 210.27.48.1 and / (210.27.48.2 or 210.27.48.3 /)
			
			
			C如果想要获取主机210.27.48.1除了和主机210.27.48.2之外所有主机通信的ip包，使用命令：
			#tcpdump ip host 210.27.48.1 and ! 210.27.48.2
			
			
			D如果想要获取主机210.27.48.1接收或发出的telnet包，使用如下命令：
			#tcpdump tcp port 23 host 210.27.48.1
			
			
			E 对本机的udp 123 端口进行监视 123 为ntp的服务端口
			# tcpdump udp port 123
			
			
			 
			
			F 系统将只对名为hostname的主机的通信数据包进行监视。主机名可以是本地主机，也可以是网络上的任何一台计算机。下面的命令可以读取主机hostname发送的所有数据： 
			#tcpdump -i eth0 src host hostname
			
			 
			
			G 下面的命令可以监视所有送到主机hostname的数据包： 
			#tcpdump -i eth0 dst host hostname
			
			
			H  我们还可以监视通过指定网关的数据包： 
			#tcpdump -i eth0 gateway Gatewayname
			
			
			I 如果你还想监视编址到指定端口的TCP或UDP数据包，那么执行以下命令： 
			#tcpdump -i eth0 host hostname and port 80
			
			
			J 如果想要获取主机210.27.48.1除了和主机210.27.48.2之外所有主机通信的ip包
			，使用命令：
			#tcpdump ip host 210.27.48.1 and ! 210.27.48.2
			
			
			K 想要截获主机210.27.48.1 和主机210.27.48.2 或210.27.48.3的通信，使用命令
			：（在命令行中适用　括号时，一定要
			#tcpdump host 210.27.48.1 and / (210.27.48.2 or 210.27.48.3 /)
			
			
			L 如果想要获取主机210.27.48.1除了和主机210.27.48.2之外所有主机通信的ip包，使用命令：
			　#tcpdump ip host 210.27.48.1 and ! 210.27.48.2
			
			
			M 如果想要获取主机210.27.48.1接收或发出的telnet包，使用如下命令：
			　#tcpdump tcp port 23 host 210.27.48.1
		
		第三种是协议的关键字，主要包括fddi,ip ,arp,rarp,tcp,udp等类型
			除了这三种类型的关键字之外，其他重要的关键字如下：gateway, broadcast,less,
			greater,还有三种逻辑运算，取非运算是 'not ' '! ', 与运算是'and','&&';或运算 是'o
			r' ,'||'；
		
		第二种是确定传输方向的关键字，主要包括src , dst ,dst or src, dst and src ,
			如果我们只需要列出送到80端口的数据包，用dst port；如果我们只希望看到返回80端口的数据包，用src port。 
			#tcpdump –i eth0 host hostname and dst port 80  目的端口是80
			或者
			#tcpdump –i eth0 host hostname and src port 80  源端口是80  一般是提供http的服务的主机
			如果条件很多的话  要在条件之前加and 或 or 或 not
			#tcpdump -i eth0 host ! 211.161.223.70 and ! 211.161.223.71 and dst port 80
		如果在ethernet 使用混杂模式 系统的日志将会记录
			May  7 20:03:46 localhost kernel: eth0: Promiscuous mode enabled.
			May  7 20:03:46 localhost kernel: device eth0 entered promiscuous mode
			May  7 20:03:57 localhost kernel: device eth0 left promiscuous mode
			tcpdump对截获的数据并没有进行彻底解码，数据包内的大部分内容是使用十六进制的形式直接打印输出的。显然这不利于分析网络故障，通常的解决办法是先使用带-w参数的tcpdump 截获数据并保存到文件中，然后再使用其他程序进行解码分析。当然也应该定义过滤规则，以避免捕获的数据包填满整个硬盘。

### starce 命令

	strace 显示这些调用的参数并返回符号形式的值。
	strace 从内核接收信息，而且不需要以任何特殊的方式来构建内核。

	strace命令说明：
		
		-c 统计每一系统调用的所执行的时间,次数和出错的次数等. 
		-d 输出strace关于标准错误的调试信息. 
		-f 跟踪由fork调用所产生的子进程. 
		-ff 如果提供-o filename,则所有进程的跟踪结果输出到相应的filename.pid中,pid是各进程的进程号. 
		-F 尝试跟踪vfork调用.在-f时,vfork不被跟踪. 
		-h 输出简要的帮助信息. 
		-i 输出系统调用的入口指针. 
		-q 禁止输出关于脱离的消息. 
		-r 打印出相对时间关于,,每一个系统调用. 
		-t 在输出中的每一行前加上时间信息. 
		-tt 在输出中的每一行前加上时间信息,微秒级. 
		-ttt 微秒级输出,以秒了表示时间. 
		-T 显示每一调用所耗的时间. 
		-v 输出所有的系统调用.一些调用关于环境变量,状态,输入输出等调用由于使用频繁,默认不输出. 
		-V 输出strace的版本信息. 
		-x 以十六进制形式输出非标准字符串 
		-xx 所有字符串以十六进制形式输出. 
		-a column 
		设置返回值的输出位置.默认 为40. 
		-e expr 
		指定一个表达式,用来控制如何跟踪.格式如下: 
		[qualifier=][!]value1[,value2]... 
		qualifier只能是 trace,abbrev,verbose,raw,signal,read,write其中之一.value是用来限定的符号或数字.默认的 qualifier是 trace.感叹号是否定符号.例如: 
		-eopen等价于 -e trace=open,表示只跟踪open调用.而-etrace!=open表示跟踪除了open以外的其他调用.有两个特殊的符号 all 和 none. 
		注意有些shell使用!来执行历史记录里的命令,所以要使用\\. 
		-e trace=set 
		只跟踪指定的系统 调用.例如:-e trace=open,close,rean,write表示只跟踪这四个系统调用.默认的为set=all. 
		-e trace=file 
		只跟踪有关文件操作的系统调用. 
		-e trace=process 
		只跟踪有关进程控制的系统调用. 
		-e trace=network 
		跟踪与网络有关的所有系统调用. 
		-e strace=signal 
		跟踪所有与系统信号有关的 系统调用 
		-e trace=ipc 
		跟踪所有与进程通讯有关的系统调用 
		-e abbrev=set 
		设定 strace输出的系统调用的结果集.-v 等与 abbrev=none.默认为abbrev=all. 
		-e raw=set 
		将指 定的系统调用的参数以十六进制显示. 
		-e signal=set 
		指定跟踪的系统信号.默认为all.如 signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO信号. 
		-e read=set 
		输出从指定文件中读出 的数据.例如: 
		-e read=3,5 
		-e write=set 
		输出写入到指定文件中的数据. 
		-o filename 
		将strace的输出写入文件filename 
		-p pid 
		跟踪指定的进程pid. 
		-s strsize 
		指定输出的字符串的最大长度.默认为32.文件名一直全部输出. 
		-u username 
		以username 的UID和GID执行被跟踪的命令
	常用：
		strace -o output.txt -T -tt -e trace=all -p 28979
		
		1 用strace调试程序
		2 解决库依赖问题
		3 限制strace只跟踪特定的系统调用
			
			
###ldd

	查看库的依赖问题
	ldd xxx


###ar
	
	创建静态库。a文件
		　参数r：在库中插入模块（替换）。当插入的模块名已经在库中存在，则替换同名的模块。
		　		如果若干模块中有一个模块在库中不存在，ar显示一个错误消息，
		　		并不替换其他同名模块。
		　		默认的情况下，新的成员增加在库的结尾处，可以使用其他任选项来改变增加的位置。
		　参数c：创建一个库。不管库是否存在，都将创建。
		　参数s：创建目标文件索引，这在创建较大的库时能加快时间。
		　		（补充：如果不需要创建索引，可改成大写S参数；如果。a文件缺少索引，
		　		可以使用ranlib命令添加）
	例子
		ar t libxxx.a显示库文件中有哪些目标文件，只显示名称。

###sar  
	
	sar命令是Linux下系统运行状态统计工具，它将指定的操作系统状态计数器显示到标准输出设备。sar工具将对系统当前的状态进行取样，然后通过计算数据和比例来表达系统的当前运行状态。它的特点是可以连续对系统取样，获得大量的取样数据。取样数据和分析的结果都可以存入文件，使用它时消耗的系统资源很小。
	
		-A：显示所有的报告信息； 
		-b：显示I/O速率； 
		-B：显示换页状态；
		-c：显示进程创建活动；
		-d：显示每个块设备的状态； 
		-e：设置显示报告的结束时间； 
		-f：从指定文件提取报告； 
		-i：设状态信息刷新的间隔时间； 
		-P：报告每个CPU的状态；
		-R：显示内存状态；
		-u：显示CPU利用率； 
		-v：显示索引节点，文件和其他内核表的状态；
		-w：显示交换分区状态；
		-x：显示给定进程的状态。

	间隔时间次数
		sar 60 10
	
	-u  cpu
		sar -u  3 1
		%usr：CPU处在用 户模式下的时间百分比。 
	　　 %sys：CPU处在系统模式下的时间百分比。 
	　　 %wio：CPU等待输入输出完成时间的百分比。 
	　　 %idle：CPU空闲时间百分比。 
	
	 -v：进程、I节点、文件和锁表状态。 
		proc-sz：目前核心中正在使用或分配的进程表的表 项数，由核心参数MAX-PROC控制。
		inod-sz：目前核心中正在使用或分配的i节点表的表项数，由核心参数 MAX- INODE控制。 
		file-sz： 目前核心中正在使用或分配的文件表的表项数，由核心参数MAX-FILE控制。 
		ov：溢出出现的次数。 
		Lock-sz：目前核心中正在使用或分配的记录加锁的表项数，由核心参数MAX-FLCKRE控制。 
	
	-d 显示每个块设备的状态； 
		sar -d 3 1
		device： sar命令正在监视的块设备的名字。 
	　　 %busy： 设备忙时，传送请求所占时间的百分比。 
	　　 avque： 队列站满时，未完成请求数量的平均值。 
	　　 r+w/s： 每秒传送到设备或从设备传出的数据量。 
	　　 blks/s： 每秒传送的块数，每块512字节。 
	　　 avwait： 队列占满时传送请求等待队列空闲的平均时间。 
	　　 avserv： 完成传送请求所需平均时间（毫秒）。 
	
	-b 显示I/O速率；
		
		bread/s： 每秒从硬盘读入系统缓冲区buffer的物理块数。 
		lread/s： 平均每秒从系统buffer读出的逻辑块数。 
		%rcache： 在buffer cache中进行逻辑读的百分比。 
		bwrit/s： 平均每秒从系统buffer向磁盘所写的物理块数。 
		lwrit/s： 平均每秒写到系统buffer逻辑块数。 
		%wcache： 在buffer cache中进行逻辑读的百分比。 
		pread/s： 平均每秒请求物理读的次数。 
		pwrit/s： 平均每秒请求物理写的次数。
	
	-m 报告进程间的通信活动（IPC消息和信号灯活动）情况
		msg/s 每秒消息操作的次数（包括发送消息的接收信息）。 
		sema/s 每秒信号灯操作次数。 ＊信 号灯和消息作为进程间通信的工具，如果在系统中运行的应用过程中没有使用它们，那么由sar命令报告的msg 和sema的值都将等于0.00。
		
	-p 报告分页活动
		vflt/s 每秒进行页面故障地址转换的数量（由于有效的页面当前不在内存中）。 
		pflt/s 每秒来自由于保护错误出现的页面故障数量
			（由于对页面的非法存，取引起的页面故障）。 
		pgfil/s 每秒通过”页—入”满足vflt/s的数量。 
		rclm/s 每秒由系统恢复的有效页面的数量。有效页面被增加到自由页面队列上。 
		＊如 果vflt/s的值高于100，可能预示着对于页面系统来说，
			应用程序的效率不高，也可能分页参数需要调整，或者内存配置不太合适。
		
		
	
	
	
	
	
	
	
	
	
	
			
			