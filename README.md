# Linux

**笔记较早，没有进行md规范**

Linux中快捷键：
clear		清屏(ctrl+l)
ctrl+c 		终止命令


1 VMware 简介：虚拟PC的软件
官网：http://www.wmware.com

2 磁盘分区：
	主分区：最多有4个；
	扩展分区：只能有1个，不能写入数据，不能格式化，只能容纳逻辑分区；
	主分区+扩展分区最多只能有4个；
	逻辑分区：无论怎么分，第一个逻辑分区都是编号5，即/dev/sda5,即使主分区和扩
	展分区不足4个；
	
	block：数据块，4Kb，并且用I节点号标记（I节点不是很清楚）
备注：
必须分区：
		/ (根分区)
		swap分区		(交换分区，内存2倍，不超过2GB)
推荐分区：
		/boot		(启动分区，200MB，包含启动文件，其实也是必须)

3 格式化：写入文件系统，又称逻辑格式化，它是指根据用户选定的文件系统(如 FAT16，FAT32，NTFS,EXT2，EXT3，EXT4等)，在磁盘的特定区域写入特定数据，在分区中划出一片用于存放文件分配表、目录表等用于文件管理的磁盘空间；

4 定义分区设备文件名：给每个分区定义设备文件名；		
硬件							设备名
IDE硬盘						/dev/hd[a-d]
SCSI/SATA/USB硬盘			/dev/sd[a-p]
光驱							/dev/cdrom或dev/sr0
软盘							/dev/fd[0-1]
打印机(25针)					/dev/lp[0-2]
打印机（USB）				/dev/usb/lp[0-15]
鼠标							/dev/mouse
备注2：linux硬盘有设备文件名，分区也有设备文件名(即在设备文件名后加数字，如 /dev/sda1 就是SCSI硬盘或者SATA硬盘接口，现在都用这个)

4 挂载：给每个分区分配挂载点(挂载点必须是空目录)；
备注：windows使用：分区-格式化-分配盘符
	  Linux	使用 ：分区-格式化-为分区建立设备文件名(系统自动检测)-挂载(分配
	  盘符)

	  
5 Linux安装：
特别注意：计算机默认是通过硬盘启动，但是还没有操作系统时，硬盘是找不到操作系统的，所以我们需要(猜测：按F2进入BIOS)
a 在 BIOS-BOOT 中调整启动顺序为 CD-ROM Drive 优先；
b 在 BIOS-EXIT 中保存 Exit Saving Changes	;但虚拟机可以选择Exit Discarding Changes;
c 真实机中，还需要在安装完成后，第一次启动再在 BIOS-BOOT 中调整(可能是按F2)启动顺序为 Hard Drive 优先；

备注：安装日志
/root/install.log			存储了安装在系统中的软件包及版本信息
/root/install.log.syslog 	存储了安装过程中留下的事件记录；
/root/anaconda-ks.cfg:		以Kcikstart配置文件的格式记录安装过程中设置的
							选项信息；

6 虚拟机的模拟远程登录链接方式：虚拟机/设置/网络适配器/(右边...)
A 选择网络连接方式

a 桥接:虚拟机利用真实计算机的真实网卡，设置与真实windows同一个网段的IP地址，就
可以实现虚拟机与真实机以及同一个网段的其他真实机通信，缺点：会占用一个IP地址；

b NAT：虚拟机采用VMnet8这个假网卡来进行通信，只能和自己的真实机通信，如果真实
机可以访问互联网，虚拟机也可以访问；

c HOST-ONLY:虚拟机采用VMnet1这个假网卡来进行通信，只能和自己的真实机通信，如果
真实机可以访问互联网，虚拟机确不可以访问；
备注： 从192.168.0.1到192.168.0.255着之间就是一个网段

B 查看真实机的IP地址，注意：虽说是真实机的IP地址，但是是根据不同的网络连接方式
是查看不同的网卡；桥接方式是查看真实机的有线或者无线网卡（没安装虚拟机时候就有的）；NAT方式是查看VMnet8网卡的IP号，HOST-ONLY查看VMnet1的IP号；

C 在Linux系统中设置网卡的IP号，注意：要跟B步骤的查看的IP地址要同网段（无论任何
网络连接方式，都要同网段）；
设置网卡命令
命令1	ifconfig 	显示网卡信息
命令2	ifconfig eth0 192.168.1.156		设置网卡信息（只是临时生效，重启后
										失效，eth表示网卡名 eth0表示第一个网卡）
D 在Windows运行中输入CMD，在黑屏中输入:
ping 192.168.150.2			(Linux系统中设置的IP地址)
备注：观察是否有反馈，提示成功则成功；

备注：有些时候windows和Linux设置同一个网段的IP地址，但还是无法连接，可能是因为
虚拟机中编辑/虚拟网络编辑器/桥接到  自动 ，只要更改为具体网卡即可；


7 远程管理工具 secureCRT 	备注：不用这个了，用xShell
A 设置secureCRT的字符为中文
步骤：options/session options/Terminal/Appearance 界面右边 Current color scheme 选择为Traditional,并点击Font...按钮，弹出界面选择一个中文字符，并把
字符集调整为GB2312，确认并回到session options窗口，设置Character encoding为utf-8；

B 设置外观:options/session options/Terminal/Emulation 界面右边Terminal:linux （其实随意，只不过linux习惯些）

C 快捷键：
Ctrl + l	 清空屏幕（或者输入 clear命令）

8 文件拷贝工具 WinSCP

9 linux注意事项：
A linux系统严格区分大小写，所有命令都是小写；
B linux所有内容以文件形式保存，包括硬件，如：硬盘文件是/dev/sd[a-p],光盘文件
是/dev/sr0等；
C linux不靠扩展名区分文件，依靠文件的权限区分；
有些文件为了管理员易区分，有扩展名（但不是必须，即使没有扩展名，linux系统也能区分）
压缩包："*.gz"、"*.bz2"、"*.tar.bz2"、"*.tgz"
二进制软件包：".rpm"
网页文件："*.html"、"*.php"
脚本文件:"*.sh"
配置文件："*.conf"
D linux所有的存储设备都必须挂载之后才能使用，包括硬盘、U盘和光盘；
E windows下的程序不能直接在linux中安装和运行；
F 隐藏文件以 .文件名 

10 服务器注意事项
A 远程服务器不允许关机，只能重启
B 重启时应该关闭服务；
C 不要在服务器访问高峰运行高负载命令(如：大数据压缩、查找等)
D 远程配置防火墙时不要把自己踢出服务器(防火墙是过滤作用，与杀毒软件不同)
E 指定合理的密码，并定期更新
F 合理分配权限
G 定期备份重要的数据和日志；（一定要做）

11 linux常用命令
命令格式：命令 [-选项] [参数]
	 例： ls -la /etc
备注：-选项 作用是调整命令功能；参数是命令的对象；
备注2：个别命令不遵守此格式；
	   当有多个选项时，可以写在一起；
	   简化选项和完整选项	，如 -a等同于 --all

	  
	4.1 文件处理命令
	  
A 	命令：ls		英文：list		命令所在路径：/bin/ls	执行权限：所有用户
功能：	显示所有目录文件
语法：	ls 选项[-ald] [文件或目录] 
选项：	-a	显示所有文件，包括隐藏文件
		-l	详细信息显示(是字母小写的L)，说明：
		-d	查看目录属性（即查看目录本身）；
通用选项 -h	以人类易读方式显示
		-i	查看文件或目录ID号
备注：关于 -l 选项的结果分析：
-rw-r--r--.	1 root	root	26420	May	27 20:06 	install.log	
内容				位置		含义
	
1        		第2位	文件引用计数
root			第3位	文件所有者（只能有一个）
root			第4位	文件所有组（一类可以使用该文件的使用者，只有一个组）
26420			第5位	文件大小，可以通过-h选项，使之显示为人类易读方式
May	27 20:06	第6位	文件上次修改时间
install.log		第7位	文件名
-rw-r--r--.		第1位	再分析；
	-		rw-				r--				r--.
文件类型		u所有者权限		g所属组权限		o其他人权限

备注：文件类型常用类型：- 代表普通文件，d代表目录，l代表软链接；
备注2：权限分类：r是读，w是写，x是执行；

B 	命令		英文					命令所在路径		权限			
	mkdir	make directories	/bin/mkdir		所有用户
语法：mkdir	[-p]	目录名
功能：创建新目录，选项-p是递归创建，即可以在不存在的目录下创建目录；
例：mkdir /tmp/boduo /tem/aichuan
例：mkdir /tmp/japan/longze
	
C 	命令		英文					命令所在路径			权限			
	cd	change directory		shell内置命令		所有用户
语法：cd [目录]
功能：切换到指定目录；
例：cd /tmp/japan/boduo		切换到指定目录
	cd ..					切换到上级目录；
	
D 	命令		英文						命令所在路径			权限			
	pwd	print working directory		bin/pwd			所有用户
语法：pwd
功能：显示当前完整目录；
例：pwd 

E 	命令		英文							命令所在路径			权限			
	rmdir	remove empty directory		bin/rmdir			所有用户
语法：rmdir	[目录名]
功能：删除 空 目录；
例：rmdir /tmp/japan/boduo

F 	命令		英文							命令所在路径			权限			
	cp		copy						bin/cp				所有用户
语法：cp -rp [源文件或目录] [目标目录或文件]
		 -r **复制目录**
		 -p 保留文件属性（如：保留上次修改时间和访问权限等）
功能：复制文件或目录；
例：cp /root/install.log /root/install.log.syslog /tmp
备注：复制文件可以不用选项，可以同时复制多个文件，只要最后一个写上目标目录；
例2：cp -r /tmp/japan/cangjing /root121212
备注：复制目录，相应的下面的文件也会复制哦；
例3：cp -r /tmp/japan /tmp/new
备注：如果目标目录或目标文件不存在，则功能是复制并重命名；
例4:cp /root/install.log /tmp/new/12.log
备注：将install.log文件复制到/tmp/new文件夹下，并重命名为12.log

G 	命令		英文							命令所在路径			权限			
	mv		move						bin/mv				所有用户
语法：mv	[原文件或目录] [目标目录]
功能：剪切或者改名；
例：mv /tmp/longze /tmp/hot

H 	命令		英文							命令所在路径			权限			
	rm		remove						bin/rm				所有用户
语法：rm	-rf [文件或目录]
		-r 删除目录
		-f 强制删除（将不再询问是否删除，而是直接删除）
功能：删除文件或者目录(可以非空)；
例：		rm /tmp/install.log		//删除文件
例2：	rm -r /tmp/japan		//删除目录
例3:	rm -rf /tmp/japan		//强制删除，不用询问；(这个无论目录或文件，都删除)

I 	命令		英文							命令所在路径			权限			
	touch	touch						bin/touch			所有用户
语法：touch [文件名]
功能：创建空文件；
例：touch lovestory

J 	命令		英文							命令所在路径			权限			
	cat		cat							bin/cat				所有用户
语法：cat [-n] [文件名]
		   -n 显示行号
功能：显示文件内容，适合查看文件内容比较短小的文件；
例：cat /etc/issue

K 	命令		英文							命令所在路径			权限			
	tac		...							/usr/bin/tac		所有用户
语法：tac [文件名]
功能：作用cat相同都是显示小文件内容，但这个行数是从大到小显示，即与cat相反；
例：tac /etc/issue		//没有选项-n
备注：这个命令不太重要

L 	命令		英文							命令所在路径			权限			
	more	more						bin/more			所有用户
语法：more [文件名]
说明：进入more浏览状态时，可以
	(空格)或f	翻页
	(Enter)		换行
	q或Q		退出
	
功能：分页显示文件内容；
例：more /etc/services
备注：more不能查看已经浏览过的内容，即不能往上翻页，只能往下；

M 	命令		英文							命令所在路径			权限			
	less	less						/usr/bin/less		所有用户
语法：less [文件名] 
功能：分页显示文件内容（可以向上翻页）；	
说明：进入less浏览状态时，可以
	(空格)或f	翻页
	(Enter)		换行
	q或Q		退出
	home		回到首页
	end			回到尾页
	pg up		上一页
	pg dn		下一页
	↑			上一行
	↓			下一行
	/检索内容	在文件中检索，处于检索状态时，n代表下一个匹配，N代表上一个匹配
例：less /etc/services

N 	命令		英文							命令所在路径			权限			
	head	head						/usr/bin/head		所有用户
语法：head [-n] [文件名] 
			-n	指定行数
功能：显示文件前几行；
例：head /etc/services			//省略显示行数，则默认显示前10行；	
例2：head -7 /etc/services		//显示前7行；

O 	命令		英文							命令所在路径			权限			
	tail	tail						/usr/bin/tail		所有用户
语法：tail [-n] [文件名] 
			-n	指定行数
			-f	动态显示文件末尾(便于查看日志更新信息等)
功能：显示文件最后几行；
例：tail /etc/services			//省略显示行数，则默认显示最后10行；
例：tail /-n -18 /etc/services
例：tail -f /var/log/messages

P 	命令		英文							命令所在路径			权限			
	ln		link						/bin/ln				所有用户
语法：ln -s [原文件] [目标文件]
		-s	创建软链接
功能：生成链接文件；
例：ln -s /etc/issue /tmp/issue.soft			//生成软链接文件
备注1：软链接相当于windows下的快捷方式；
用ls命令显示：
lrwxrwxrwx. 1 root root     10 5月  28 11:32 issue.soft -> /etc/issue
软链接特征：
a 所有软链接文件类型和权限都是lrwxrwxrwx，l只代表软链接，不包含硬链接；
b 文件大小只是符号链接大小；
c issue.soft -> /etc/issue箭头指向原文件；

例2：ln /etc/issue /tmp/issue.hard			//生成硬链接文件
用ls命令显示：
-rw-r--r--. 2 root root     47 10月 23 2014 issue.hard
硬链接特征：
a 作用相当于：cp -p + 同步更新
b 可以通过i节点识别（原文件和硬链接具有相同的i节点），且原文件删除，硬链接可以
照常访问
c 不能跨分区
d 不能针对目录使用


	4.2 权限管理命令
				文件目录权限总结
代表字符		权限		对文件的含义					对目录的含义

r			读		可以查看文件内容，			可以列出目录中的内容
					r:cat/more/less/head/tail	r:ls
					
w			写		可以修改文件内容		  	 可以在目录中创建和删除文件	
					w:vim						touch/mkdir/rmdir/rm
					
x			执行		可以执行文件					可以进入目录
					x:script command			x:cd
特别注意：删除文件是只需要相应 目录 有写w权限即可；
备注：对于Linux系统中目录rx权限几乎都是同时出现的；
	
4.2.1 权限管理命令chmod
		
Q 	命令		英文										命令所在路径 	权限
	chmod	change the permissions mode of a file 	/bin/chmod 	  所有用户
语法：chmod [{ugoa}{+-=}{rwx}] [文件或目录]
			[mode=421] [文件或目录]
			-R 递归修改 (将目录和目录下的所有文件都更改权限)
功能：改变文件或目录的权限；
例：chmod g+w /tmp/hot			//为hot目录的所属组增加一个w权限；
例：chmod g+x,o-w /tmp/hot		//为hot目录的所属组增加执行权限，为其他人减少
								  一个写权限；
例：chmod g=rw /tmp/hot			//注意    =rw，不设置e权限，则不写，
								  而不是  =rw-;	
备注：权限的数字表示：
r -- 4 	w -- 2  e -- 1
如：rwxrw-r-- 对应的权限数字就是 764
例：chmod 750 /tmp/hot	

备注：能够改变目录或文件权限的只有管理员root和所有者；

		
4.2.2 其他权限管理命令
R 	命令		英文										命令所在路径 	权限
	chown	change file ownership				 	/bin/chown 	  所有用户
语法：chown [用户] [文件或目录]
功能：改变文件或目录的所有者；
特别注意：只有管理员才能改变所有者，而原来的所有者本身是无权更改的，且软链接的所有者和所属组不能更改；
例：chown neo /tmp/hot
例2：[root@localhost ~]# chown root:student_group /tmp/project
备注：例2写法是同时更改所有者和所属者；


S 	命令		英文										命令所在路径 	权限
	chgrp	change file group ownership				/bin/chgrp 	  所有用户
语法：chgrp [用户组] [文件或目录]
功能：改变文件或目录的所属组；
特别注意：只有管理员才能改变所属组，而原来的所有者本身是无权更改的；
例：chgrp lampbrother /tmp/hot

T 	命令		英文										命令所在路径 	权限
	umask	the user file-creation mask				shell内置命令 所有用户
语法：umask [-S]
			-S 以rwx形式显示新建文件缺省权限；
功能：显示、设置文件的缺省权限（猜测：即在创建目录时没有设置权限时的默认权限）
例：		umask -S
显示：	u=rwx,g=rx,o=rx
备注：文件的缺省权限时在目录缺省权限(即上一行显示的权限)的基础上，去掉所有人的执行权限；
备注2：设置缺省权限，语法：umask 数字1；
	   解析：这个数字是用 777-要设置的数字权限
例：要设置缺省权限为：-rwxr-xr-- 即数字权限为754;则数字1为777-754=023
代码：umask 023


4.3 文件搜索部分的学习

4.3.1 文件搜索命令find
说明：尽量把目录结构整理清晰，文件规整放置，减少搜索，因为会消耗大量系统资源；
	
U 	命令		英文						命令所在路径 			权限
	find	find					/bin/find			 所有用户
语法：find [搜索范围] [匹配条件]
功能：文件搜索
选项解析：
a 	-name		严格（精确）查找文件名
例：find /etc -name 'init'			//只查询到名为init文件，猜测：应该最好加上引号
例：find /etc -name '*init*'		//*代表任意字符任意次数；
例：find /etc -name 'init???'		//?匹配单个字符

b 	-iname		不区分大小写的查找
例：find /etc -iname 'init'

c 	-size		根据文件大小来查找
例：find / -size +204800			//最后数字单位为数据块，一个数据块为0.5Kb
例：find / -size -204800			//搜索比100Mb小的文件
例：find / -size 204800			//搜索等于100Mb小的文件

d 	-user		根据文件所有人查找
例：find /home -user neo

e	-group		根据文件所属组查找
例：find /tmp -group root

f 	-amin		访问时间access,后面接 -分钟数，表示查找在分钟数内访问过的文件
				，+分钟数 表示查找超过分钟数访问过的文件；
	-cmin		文件属性change，后面接 -分钟数，表示查找在分钟数内文件、
				属性被改变过的文件，+分钟数 表示查找超过分钟数访问过的文件；
				文件属性是指，用ls -l 显示出来的7大属性；
	-mmin		文件内容modify，后面接 -分钟数，表示查找在分钟数内文件内容
				改变过的文件，+分钟数 表示查找超过分钟数文件内容被改变过的文
				件；
例： find /tmp -cmin -30

g 	-a			两个条件都同时满足
	-o			两个条件满足其中一个
例：find /etc -size +163840 -a -size -204800
解释：在目录/etc下查找文件大小在80Mb到100Mb之间的文件；

h 	-exec 命令 {} \;		对搜索结果直接进行操作
	-ok 命令 {} \;		对搜索结果进行操作，但是要询问；
例：find /etc -name inittab -exec ls -l {} \;
例：find /etc -name inittab -ok ls -l {} \;

i	-type		根据文件类型查找，f代表文件，d代表目录，l代表软链接	
例：find /etc -type f

j	-inum		根据i节点查找
例：find . -inum 31531 -exec rm {} \;

4.3.2 其他搜索命令

V 	命令		英文						命令所在路径 			权限
	locate	locate					/usr/bin/locate			所有用户
语法：locate 文件名
功能：在文件资料库中查找文件，速度比find快很多，但是可能资料库没更新，有些最
新文件找不到，弥补方式，手动更新命令：updatedb；且locate资料库不收录/tmp文件
夹下的文件；
例：
updatedb
locate new_name
locate -i	new_name 		//选项 -i 不区分大小写；

W 	命令		英文						命令所在路径 			权限
	which	which					/usr/bin/which			所有用户
语法：which 命令
功能：搜索命令所在的目录及别名信息
例：which ls

X 	命令		英文						命令所在路径 			权限
	whereis	whereis					/usr/bin/whereis		所有用户
语法：whereis [命令名称]
功能：搜索命令所在的目录及帮助文档路径
例：whereis ls

Y 	命令		英文						命令所在路径 			权限
	grep	grep					/bin/grep				所有用户
语法：grep -iv [指定字串] [文件]
		   -i	不区分大小写
		   -v 	排除指定字串（默认是某一行出现该字串，就排除）
功能：在文件中搜索字串匹配的行，并输出
例：grep -i multiuser /etc/inittab		//在文件inittab中不区分大小写查询
										multiuser行
例：grep -v ^# /etc/inittab				//在文件inittab中排除行首字母为#的行
										并输出，这里就不是默认了，而是设置了
										^ ,尾部用$
4.4 帮助命令
Z	命令		英文						命令所在路径 			权限
	man		manual					/usr/bin/man			所有用户
语法：man 命令或配置文件
功能：获得除shell命令的帮助信息
例：man ls			//查看ls命令帮助信息
例：man services		//查看配置文件services的帮助信息，特别注意：配置文件只能
					 是文件名，不能有路径
备注：还有一些查询帮助信息中指定内容的命令，如：
whatis 命令			//只显示帮助信息中name描述信息
apropos	配置文件(只是文件名，不能写绝对路径)//获得配置文件相关信息	

好像上面两个命令这个系统不能使用
		
命令 --help			//功能：显示该命令的主要选项信息；例：ls --help
info 命令			//与man近似；

AA  命令		英文						命令所在路径 			权限
	help	help					shell内置命令			所有用户
语法：help 命令
功能：获得shell内置命令的帮助信息，与上面的man对应；
例：help umask			//查看umask的帮助信息

4.5 用户管理命令
	
AB  命令		英文						命令所在路径 			权限
	useradd	useradd					/usr/sbin/useradd		root
语法：useradd 用户名
功能：添加新用户；
例：useradd neo

AB  命令		英文						命令所在路径 			权限
	passwd	password				/usr/bin/passwd			所有用户
语法：passwd 用户名
功能：设置用户密码；
例：passwd neo
备注：passwd默认是要用终端作为标准输入,加上--stdin表示可以用任意文件做标准输入
所以当然也可用管道作为标准输入：
password=123
echo password | /usr/sbin/passwd --stdin $name$i &>/dev/null

AC  命令		英文						命令所在路径 			权限
	who		who						/usr/bin/who			所有用户
语法：who
功能：查看登录用户信息；
例：who
结果：
root     tty1         2015-05-28 05:47
root     pts/1        2015-05-28 18:59 (192.168.150.1)
备注：tty代表本地终端，pts代表远程终端；后面是登录时间和终端id

AC  命令		英文						命令所在路径 			权限
	w		...						/usr/bin/w				所有用户
语法：w
功能：查看登录用户详细信息；
例：w

4.5 压缩解压命令


AD  命令		英文						命令所在路径 			权限
	gzip	GNUzip					/bin/gzip				所有用户
语法：gzip [文件]
功能：压缩文件（只能压缩文件，不能压缩目录），不保留原文件；
压缩后文件格式：.gz
例：gzip hehe	

AE  命令		英文						命令所在路径 			权限
	gunzip	GNUunzip				/bin/gunzip				所有用户
语法：gunzip [压缩文件]		//另一种形式为gzip -d [压缩文件]
功能：解压格式为.gz的压缩文件，不保留原压缩文件；
压缩后文件格式：.gz
例：gunzip hehe.gz

AF  命令		英文						命令所在路径 			权限
	tar 	...						/bin/tar				所有用户
语法：tar 选项[-zcf] [压缩后文件名] [目录]
			  -c 打包
			  -v 显示详细信息
			  -f 指定文件名（该选项一定要在最后）
			  -z 压缩或者解压缩(是压缩还是解压缩取决于是-c还是-x,采用gzip方式)
			  -j 压缩或者解压缩(是压缩还是解压缩取决于是-c还是-x,采用bzip2方式)
			  -x 解包（选项-c和-x一条语句中只能有一个）
			  
功能：打包(解压)目录(其实文件也适用)，原文件保留；
压缩后文件格式：.tar.gz
例：tar -czf japan.tar.gz japan			//压缩
例：tar -xzf japan3.tar.gz				//解压，注意解压还原的文件是压缩前的
										文件名，而不是japan3.tar.gz中japan3
特别注意：-f选项一定要在最后，不然要出错；
备注：可以同时压缩几个文件到一个压缩文件中，被压缩文件用空格隔开；

AF  命令		英文						命令所在路径 			权限
	zip		...						/usr/bin/zip			所有用户
语法：zip 选项[-r] [压缩后文件名] [文件或目录]
			  -r 压缩目录
功能：压缩文件或目录
压缩后文件格式：.zip
备注：这个命令压缩比不高；

AG  命令		英文						命令所在路径 			权限
	unzip	...						/usr/bin/unzip			所有用户
语法：unzip [压缩文件]
功能：解压.zip的压缩文件
例：unzip test.zip

AH  命令		英文						命令所在路径 			权限
	bzip2	...						/usr/bin/bzip2			所有用户
语法：bzip2 选项[-k] [文件]
				-k 产生压缩文件后保留原文件；
功能：压缩文件(相比zip压缩比要高很多)
例：bzip2 -k boduo
	tar -cjf japan.tar.bz2 japan	//选项中j代表bzip2方式压缩，以前的z代表
		吃减肥						gzip压缩

AI  命令		英文						命令所在路径 			权限
	bunzip2	...						/usr/bin/bunzip2		所有用户
语法：bunzip2 选项[-k] [压缩文件]
				  -k 解压缩后保留原文件
功能：解压缩
例：bunzip2 -k boduo.tar.bz2
例：tar -xjf japan.tar.bz2	
		想减肥


4.6 网络命令
AJ  命令		英文						命令所在路径 			权限
	write	write					/usr/bin/write			所有用户
语法：write <用户名>
功能：给在线用户发信息，以ctrl+d 结束，删除已写内容，ctrl+backspace
例：write neo
hello neo!

AK  命令		英文						命令所在路径 			权限
	wall	write all				/usr/bin/wall			所有用户
语法：wall [message]
功能：发广播信息
例：wall notice:liuzuochao will be the richest man!

AL  命令		英文						命令所在路径 			权限
	ping	ping					/bin/ping				所有用户
语法：ping 选项 IP地址
			-c 指定发送次数(默认一直发送，按ctrl+c才能终止)
功能：测试网络连通性
例：ping 192.168.150.1
例：[root@localhost ~]# ping -c 4 www.baidu.com

AM  命令		 英文						命令所在路径 			权限
	ifconfig interface configure		/sbin/ifconfig			root
语法：ifconfig 网卡名称 IP地址
功能：查看和设置网卡信息
例：config eth0 192.168.150.2

AN  命令		 英文						命令所在路径 			权限
	mail 	 mail						/bin/mail				所有用户
语法：mail [用户名]
功能：查看发送电子邮件(即使用户不在线)
例：mail root
备注：mail同时也有查看邮件功能，进入查看状态后，输入邮件编号回车就可以查看邮件
内容，输入h回车，就可以显示邮件列表，输入d 邮件编号 就可以删除相应文件；
输入q回车退出；
备注：按下Ctrl+D 键或. 键结束正文。连按两次Ctrl+C键则中断工作，不送此信件。
Cc( Carbon copy) : 复制一份正文，给其他的收信人。

AO  命令		 英文						命令所在路径 			权限
	last 	 last						/usr/bin/last			所有用户
语法：last
功能：列出目前与过去登入系统的用户信息
例：last

AP  命令		 英文						命令所在路径 			权限
	lastlog  lastlog					/usr/bin/lastlog		所有用户
语法：lastlog
功能：检测某特定用户上次登录的时间
例：lastlog
例：lastlog -u 502 		//502代表用户uid

AQ  命令		 英文						命令所在路径 			权限
	traceroute  						/bin/traceroute			所有用户
语法：traceroute
功能：显示数据包到主机间的路径
例：traceroute www.lampbrother.net

AR  命令		 英文						命令所在路径 			权限
	netstat  							/bin/netstat			所有用户
语法：netstat [选项]
			  -t 		TCP协议
			  -u		UDP协议
			  -l		监听
			  -r		路由
			  -n		显示IP地址和端口号
功能：显示网络相关信息
例：netstat -ptlun 		查看本机监听端口（最常用）
例：netstat -an			查看本机所有的网络连接（网关）
例：netstat -rn 			查看本机路由表

AS  命令		 英文						命令所在路径 			权限
	setup	 setup  					/usr/bin/setup			root
语法：setup
功能：配置网络（永久的）
例：setup
备注：改变后永久生效，且需要输入命令：service network restart		//重启才行

AT  命令		 英文						命令所在路径 			权限
	mount	 mount  					/bin/mount				所有用户
语法：mount [-t 文件系统] 设备文件名 挂载点(即空目录)
功能：为设备挂载
例：mount -t iso9660 /dev/sr0 /mnt/cdrom		//挂载点需要自己创建
例：mount /dev/sr0 /mnt/cdrom				//两条代码功能一样
备注1：就可以进入光盘 cd /mnt/cdrom
备注2：在光盘目录下是无法卸载挂载点哦！
备注3：卸载挂载的语法：umount /dev/sr0 

4.8 关机重启命令
AU  命令		 英文						命令所在路径 			权限
	shutdown	  												
语法：shutdown [选项] 时间
				-c	取消前一个关机命令
				-h	关机
				-r	重启
功能：关机或者重启
例：shutdown -h 5			//五分钟后关机
例：shutdown -r 5			//五分钟后重启
例：shutdown -c 			//取消关机或者重启
备注：其他关机命令：
halt
poweroff
init 0
备注：其他重启命令：
reboot
init 6
备注：系统运行级别（可以查看cat /etc/inittab）
0		关机
1		单用户						//相当于windows的安全模式
2		不完全多用户，不含NFS服务
3		完全多用户
4		未分配
5		图形界面
6		重启
说明1：更改级别命令：init 级别			例：	init 4
说明2：显示前一次级别和当下级别命令：runlevel		//显示：4 3

AV  命令		 英文						命令所在路径 			权限
	logout	logout  												
语法：logout
功能：退出登录
备注：操作完成后，一定要退出；


5 vim编辑器

5.1 vim工作模式
A 命令：vim filename 	
环境：在Linux命令行中使用
功能：进入命令模式，无论filename原来存不存在

B 命令：wq
环境：在vim命令模式中使用,即输入冒号 :wq
功能：保存并退出vim编辑器

C1 命令：a
环境：在vim命令模式中使用
功能：在光标所在的字符后插入

C2 命令：A
环境：在vim命令模式中使用
功能：在光标所在的行尾后插入

C3 命令：i
环境：在vim命令模式中使用
功能：在光标所在的字符前插入

C4 命令：I
环境：在vim命令模式中使用
功能：在光标所在的行首插入

C5 命令：o
环境：在vim命令模式中使用
功能：在光标下插入新行

C6 命令：O
环境：在vim命令模式中使用
功能：在光标上插入新行

D 命令：esc
环境：在vim的插入模式中使用
功能：退出vim插入模式

E 命令：

F 命令：enter
环境：在vim编辑模式中使用
功能：结束运行

G 命令：set nu
环境：在vim命令模式下使用（这个模式下意味着，先输入冒号:）
功能：设置行号

H 命令：set nonu
环境：在vim命令模式下使用（这个模式下意味着，先输入冒号:）
功能：设置行号

I 系列命令：
gg		到第一行
G		到最后一行
nG		到第n行(不采用)
:n		到第n行(采用，n就是行号)
$		移至行尾
0		移至行首
备注：以上命令只是移动光标，并未进入编辑状态；

J 删除系列命令(在命令模式，没有进入编辑模式)
x 		删除光标所在的字符
nx		删除光标所在处后n个字符
dd		删除光标所在行，ndd删除n行
dG		删除光标所在行到文件末尾的内容
D		删除光标所在处到行尾内容
:n1,n2d	删除指定范围的行，例：  :222,333d			//直接输入的
d$      删除光标到行末
d0      删除光标到行前

K 复制和剪切系列命令
yy		复制当前行
nyy		复制当前行以下n行
dd		剪切当前行(也是删除)
ndd		剪切当前行以下n行，当前行不算；
p、P	粘贴在光标所在当前行下或上

L 替换和取消命令
r		取代光标所在处字符
R		从光标所在处开始替换字符，按Esc结束
u		取消上一步操作

M 搜索和搜索替换命令
/string	搜索指定字符串，如果要搜索时忽略大小写，设置 :set ic
n		显示搜索匹配的下一个字符串,N就是上一匹配的字符串
:%s/old/new/g		全文替换指定字符串(最后的g可以换成c，作用是替换时要询问)
:n1,n2s/old/new/g	在一定范围内替换指定字符串(同上g也可以换成c)

N 保存和退出命令
:w						保存修改
:w new_filename			另存为指定的文件
:wq						保存修改并退出（快捷键：ZZ）
:q!						不保存修改退出（注意：没有直接的q）
:wq!				保存修改并退出(在没有写权限时，文件所有者和root强制保存)

	5.2 vim使用技巧
O 导入命令：r 文件名
功能：将文件内容导入光标所在位置后
例：  :r /etc/services
备注：这是在底部命令行输入的:r

P 命令(底部命令行输入)：!
功能：暂时跳出vim编辑，输入Linux命令
例：:!which ls
备注：r !同时使用，导入命令执行结果
例：:r !date

Q 命令(底部命令行输入):map
功能：map 快捷键 触发命令
例： :map ^W	 I#<ESC>
例： :map ^E 0x
备注：真实操作是，输入:map 然后按下ctrl+v，再按下ctrl+快捷键（这是字体会高亮）
，最后输入要执行的命令；
备注2：退出登录后，快捷键作用消失
备注3：连续行注释：
:n1,n2s/^/#/g
:n1,n2s/^#//g					//这里的^符号表示只针对行首的#进行替换；
:n1,n2s/\/\//g					//这里需要转义


R 命令：ab 						//这是vim里面的命令
语法：:ab 输入内容 变成内容 
功能：将
例：:ab myemail 445354187@qq.com

备注：以上快捷键，以及ab的设置并不是永久生效的，永久生效需要写入配置文件中
A 命令：:vim /root/.vimrc			//编辑.vimrc文件，无论原来存不存在
B 写入：
set nu
ab myemail 445354187@qq.com
map ^W I#<ESC>
map ^E 0x

备注：如果是普通用户则在 /home/username/.vimrc中编辑；
备注：当然map操作也是根据其用法来的


6 软件包管理

	6.1 软件包管理简介
A 安装包简介
a 源码包：可以直接看到源代码，例如C语言（其中包含脚本安装包，有安装界面的那种）
优点：
开源，如果有能力，可以修改源代码
可以自由选择所需的功能
软件是编译安装，更加适合自己的系统，更加稳定也效率更高
卸载方便（就是直接删除安装目录）

缺点：
安装步骤较多，尤其安装较大的软件集合时（如LAMP环境搭建），容易出现拼写错误
编译时间过长，安装时间比二进制安装时间长
因为是编译安装，安装过程中一旦报错，新手很难解决


b 二进制包(windows下为exe，Linux下为RPM包和系统默认包)：为机器语言01的安装包
优点：
包管理系统简单，通过几个命令就可以实现包的安装、升级、查询和卸载
安装速度比源码包快很多

缺点：
经过了编译，不能再看到源代码
功能选择不如源码包灵活
依赖性

	6.2 RPM包管理-rpm命令管理
A RPM包命名原则
httpd-2.2.15-15.el6.centos.1.i686.rpm
httpd 			软件包名
2.2.15			软件版本
15				软件发布次数
el6.centos		适合的Linux平台
i686			适合的硬件平台
rpm				rpm包扩展名

B RPM包依赖性
树形依赖：a->b->c				//解决方式：先安装依赖包
环形依赖：a->b->c->a				//解决方式：一条命令安装三个包
模块依赖：依赖的文件并不在Linux安装文件中，而是其他软件中，安装了其他软件，自然
这个依赖就解决了，www.rpmfind.net可以查询依赖的软件（也是文件）；
安装包时出现
error: Failed dependencies:
        libodbcinst.so.2 is needed by mysql-connector-odbc-5.1.5r1144-7.el6.i686
备注：出现 libodbcinst.so.2 说明出现了模块依赖，

C 安装升级与卸载
包全名：操作的包时没有安全的软件包时（安装和升级），使用全名，而且要注意路径
包名：操作已经安装的软件包时（查询和卸载），使用包名。是搜索/var/lib/rpm中的数据库

C1 RPM安装
语法：rpm -ivh 包全名
		  -i(install)		安装
		  -v(verbos)		显示安装信息
		  -h(hash)			显示进度
		  --nodeps			不检测依赖性(基本不采用)
例：rpm -ivh httpd-2.2.15-39.el6.centos.i686.rpm
备注：上面文件安装有依赖;选项为hiv


C2 RPM包升级
语法：rpm -Uvh 包全名
		  -U(update)	升级
		  
C3 卸载
语法：rpm -e 包名
		  -e(erase)			卸载
		  --nodeps			不检查依赖性
例：rpm -e apr
例：rpm -e httpd --nodeps
备注：安装如果不在安装包里安装，则需要绝对路径，但无论在哪儿，只要输入包名就能卸载



D 查询

D1 语法：rpm -q 包名
			 -q 		查询
			 -qa		所有
功能：查询某包是否安装，已经安装会显示包名
例：rpm -q httpd
例;rpm -qa

D2 语法：rpm -qi 包名
			 -i 		查询软件信息(information)
			 -p			查看未安装包信息(package)，注意：这儿要用包全名
功能：查询软件包详细信息
例：rpm -qi libXres
例：rpm -qip httpd-2.2.15-39.el6.centos.i686.rpm

D3 语法：rpm -ql 包名
			 -l 		列表(list)，更像location
			 -p			查询未安装包将会安装在哪儿(package)，注意用包全名
功能：查询包中文件安装位置；
例：rpm -qlp httpd-2.2.15-39.el6.centos.i686.rpm 

D4 语法：rpm -qf 系统文件名
			 -f 		查询系统文件属于哪个包(file)
功能：查询系统文件是哪个包安装出来的
例：rpm -qf /etc/yum.conf

D5 语法：rpm -qR 包名
			 -R 		查询软件包的依赖性(requires)
			 -p			查询未安装包的信息(package)，注意用包全名
功能：查询软件包的依赖性
例：rpm -qRp httpd-2.2.15-39.el6.centos.i686.rpm

E RPM包校验

E1 语法：rpm -V 已安装的包名(包全名)
			 -V 		校验指定RPM包中的文件(verify)
功能：查看已经安装的包文件是否被更改过			
例：rpm -V setup-2.8.14-20.el6_4.1.noarch
S.5....T.  c		 /etc/services	
--部分1-- -部分2-	 -文件路径-
备注：验证内容（部分1）中的8个信息的具体内容如下：
S		文件大小改变
M		文件的类型或文件的权限(rwx)被改变
5 		文件MD5校验被改变(可以看成文件内容是否被改变)
D		设备的中，从代码是否改变
L		文件路径被改变
U		文件的属主(所有者)改变
G		文件的属组被改变
T		文件的修改时间改变
部分2解析：
c		配置文件(config file)
d		普通文档(documentation)
g		"鬼"文件(ghost file)，很少见，就是该文件不应该被这个RPM包包含
l 		授权文件(license file)
r		描述文件(read me)
备注:当没有做任何修改时，执行代码不会有任何输出

F 语法：rpm2cpio 包全名 | \
cpio -idv .文件绝对路径
功能：从包中提取某个文件，注意：上面的\转义符只是代表代码没有写完，换一行
解析：
rpm2cpio	//将rmp包转化为cpio格式的命令
cpio		//是一个标准的工具，它用于创建软件档案文件和从档案文件中提取文件
例：
```shell script
# rpm -qf /bin/ls			//查询ls命令属于哪个软件包
# mv /bin/ls /tmp			//造成ls误删假象
# rpm2cpio /mnt/cdrom/Packages/coreutils-8.4-37.el6.i686.rpm | cpio -div ./bin/ls					 //提取RPM包中ls命令放到当前目录的/bin/ls下
# cp /root/bin/ls /bin		//把ls命令复制到/bin目录下，修复文件丢失
```



F1 命令：cpio 选项 <文件|设备>
		   -i		copy-in 模式，还原
		   -d		还原自动新建目录
		   -v		显示还原过程
功能：通过重定向的方式将文件进行打包备份，还原恢复的工具，它可以解压以“.cpio”或者“.tar”结尾的文件。 

IP地址、子网掩码、网关和DNS解析

A  IP地址：是给每个连接在Internet上的主机分配的一个32bit地址。地址有两部分
组成，一部分为网络地址，另一部分为主机地址。

B  子网掩码：作用就是将某个IP地址划分成网络地址和主机地址两部分。长度也是32位
，左边是网络位，用二进制数字“1”表示；
右边是主机位，用二进制数字“0”表示。例如IP地址为“192.168.1.1”
和子网掩码为“255.255.255.0”。
其中，“1”有24个，代表与此相对应的IP地址左边24位是网络号；
“0”有8个，代表与此相对应的IP地址右边8位是主机号。

备注1：求网络地址：
把IP地址和子网掩码都用二进制表示，然后各位做相与运算（即只有1+1=1，其他都是0）....得到的结果就是网络地址；
备注2：求主机号：
先把他们都转化为二进制，
然后子网掩码的二进制求反，最后IP的二进制和求反后的二进制进行与运算得到即为主机号。
例如：IP：192.168.200.13 子网掩码：255.255.255.0
ip二进制：11000000.00001001.1100100.00001101
子网掩码：1111111.11111111.11111111.00000000
反：0000000.0000000.0000000.11111111
主机号即为：0.0.0.13

备注3：网络地址相同，而主机号不同在局域网中就能通信；

C 网关：又称网间连接器、协议转换器。网关在网络层以上实现网络互连，是最复杂的
网络互连设备，仅用于两个高层协议不同的网络互连。网关既可以用于广域网互连，也可以用于局域网互连。 网关是一种充当转换重任的计算机系统或设备。使用在不同的通信协议、数据格式或语言，甚至体系结构完全不同的两种系统之间，网关是一个翻译器。与网桥只是简单地传达信息不同，网关对收到的信息要重新打包，以适应目的系统的需求

进一步解析：网关实质上是一个网络通向其他网络的IP地址。比如有网络A和网络B，网络A的IP地址范围为“192.168.1.1~192.168.1.254”，子网掩码为255.255.255.0；网络B的IP地址范围为“192.168.2.1~192.168.2.254”，子网掩码为255.255.255.0。在没有路由器的情况下，两个网络之间是不能进行TCP/IP通信的，即使是两个网络连接在同一台交换机（或集线器）上，TCP/IP协议也会根据子网掩码（255.255.255.0）判定两个网络中的主机处在不同的网络里。而要实现这两个网络之间的通信，则必须通过网关。如果网络A中的主机发现数据包的目的主机不在本地网络中，就把数据包转发给它自己的网关，再由网关转发给网络B的网关，网络B的网关再转发给网络B的某个主机（如附图所示）。网络A向网络B转发数据包的过程。
备注：猜测：通常理解的IP地址唯一，应该是指网关的IP地址；
备注：设置的ip地址要与网关地址在同一个网段内；


DNS：DNS（Domain Name System，域名系统），因特网上作为域名和IP地址相互映射的一个分布式数据库，能够
使用户更方便的访问互联网，而不用去记住能够被机器直接读取的IP数串。通过主机名，最终得到该主机名对应的IP地址的过程叫做域名解析（或主机名解析）。DNS协议运行在UDP协议之上，使用端口号53。在RFC文档中RFC 2181对DNS有规范说明，RFC 2136对DNS的动态更新进行说明，RFC 2308对DNS查询的反向缓存进行说明。
备注：以上内容都可以百度百科

6.3 RPM包管理-yum在线管理
	
6.3.1 IP地址配置和网络yum源
A 设置IP地址
a 使用setup工具，设置
   名称     eth0
│ 设备            eth0________________ 
│ 使用 DHCP       [ ]                    //空格可将其设置成*
│ 静态 IP         192.168.1.250_______ 
│ 子网掩码        255.255.255.0_______ 
│ 默认网关 IP     192.168.1.1_________ 
│ 主 DNS 服务器   61.139.2.69_________ 
│ 第二 DNS 服务器 ____________________

备注：真实无线卡IP为：192.168.1.126 		(通过windows的ipconfig查看)
备注：主 DNS 是成都电信的DNS

b vim /etc/sysconfig/network-scripts/ifcfg-eth0 		//更改文件中
ONBOOT=no 更改为 ONBOOT=yes								//保存退出

c service network restart								//重启网络服务
备注：测试一下是否连通，ping www.sina.com

备注2：虚拟机本身的设置为：
网络适配器：桥接(没有勾选复制物理网络连接状态)
虚拟网络编辑器：VMnet信息采用 桥接到 Broadcom 802.11g Network Adapter


B 网络yum源
a vim /etc/yum.repos.d/CentOS-Base.repo
备注：该文件中已经配置好了网络yum源，基本不用改，会看就行

[base]		容器名称，一定要放在[]中
name		容器说明，可以自己随便写
mirrorlist	镜像站点
baseurl		我们的yum源服务器的地址。默认是CentOS官方的yum源服务器，是可以使
			用的，如果你觉得慢可以改成你喜欢的yum源地址
enabled		此容器是否生效，如果不写或写成enable=1都是生效的，写成
			enable=0就不生效
gpgcheck	如果是1则代表RPM数字证书生效，如果是0则不生效
gpgkey		数字证书的公钥文件保存位置，不用修改		


6.3.2 yum命令

说明：yum中都是包名，包全名只在手工RPM管理才有；		
A yum list 查询远程服务器中有哪个软件包可以安装；		

B yum search 关键字(如：包名)
功能:搜索服务器上与关键字相关的包

C yum -y install 包名
	  install	安装
	  -y		自动回答yes
功能：安装包

D yum -y update 包名
	  update	升级
	  -y		自动回答yes
功能：升级软件包
特别注意：不能省略包名，如果省略，则表示升级所有，包括Linux内核，而内核需要本地调整参数，不能远程设置；

E yum -y romove 包名
	  remove	卸载
	  -y		自动回答yes
功能：卸载；
备注：卸载时会自动把依赖包也卸载，但很可能该依赖包还被其他软件依赖，容易系统崩溃，所以安装最小化，轻易别卸载，最好别用yum卸载；

F yum grouplist
功能：列出所有可用的软件组列表

G yum groupinstall 软件组名(必须是英文，如果组名有空格，必须用双引号)
功能：安装指定软件组，组名可以由grouplist查出来

H yum groupremove 软件组名
功能：卸载指定软件组

I 光盘yum源搭建步骤
a 挂载光盘 mount /dev/cdrom /mnt/cdrom

b 让网络yum源文件失效(通过改变文件后缀，让其无法识别)
```shell script
# cd /etc/yum.repos.d/
# mv CentOS-Base.repo CentOS-Base.repo.bak
# mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
# mv CentOS-fasttrack.repo CentOS-fasttrack.repo.bak
# mv CentOS-Vault.repo CentOS-Vault.repo.bak
```

c 
```shell script
[c6-media]    
name=CentOS-$releasever - Media
baseurl=file:///mnt/cdrom						
#        file:///media/cdrom/
#        file:///media/cdrecorder/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
```
备注：baseurl地址为光盘挂载地址，并且把下面俩地址注释，将enabled值改为1；就完成了；

6.4 源码包管理
		
6.4.1 源码包和RPM包的区别
穿越：RPM包安装位置
```
/etc			配置文件安装目录
/usr/bin		可执行的命令安装目录
/usr/lib		程序所使用的函数库保存位置
/usr/share/doc	基本的软件使用手册保存位置
/usr/share/man	帮助文档保存位置	
```

源码包安装位置：一般在/usr/local/软件名/

说明：由于安装位置不同带来的影响
RPM包安装的服务可以使用系统服务管理命令(service)来管理，例如RPM包安装的Apache
启动方式是：
	`/etc/rc.d/init.d/httpd start`
或者service httpd start	
备注：service命令是red hat系列才有的命令
备注2：这儿可以通过在主机浏览器中输入虚拟机IP来访问Apache，如果主机能ping通虚拟机，但是不能访问，就把Linux的防火墙关掉就行，service iptables stop
备注：Apache的网页默认在/var/www/html/下；
备注：源码包安装的网页在/usr/local/apache2/htdocs/内

备注3：源码包安装的服务则不能被服务管理命令管理，因为没有安装到默认的路径中，所以只能用绝对路径进行服务管理，如：
	`/usr/local/apache2/bin/apachectl start`
	

6.4.2 源码包的安装过程
A 安装准备
a 安装c语言编译器（如：gcc,通过 rpm -q gcc查看是否安装，安装会显示包全名）
b 下载源码包(http://mirror.bit.edu.cn/apache/httpd)

B 安装注意事项
a 源代码保存位置：/usr/local/src/
b 软件安装位置：/usr/local/
c 如果确定安装过程报错：安装过程停止，且出现error、warning和no的提示

C 源码包安装过程
a 下载源码包
b 解压下载的源码包
c 进入解压缩目录

D `./configure` 软件配置与检查
a 定义需要的功能选项(如：设置安装目录，代码 
`# ./configure --prefix=/usr/local/apache2 )`
b 检测系统环境是否符合安装要求（如：检测gcc是否安装）
c 把定义好的功能选项和检测系统环境的信息都写入Makefile文件（文件全名就是
Makefile），用于后续的编辑；

E `# make`				//编译（调用gcc，把源码包编译成机器语言），如果编译
						出错，用 # make clean 即可清除安装产生的垃圾文件
  `# make install`		//编译安装；
  
F 启动Apache
  `# /usr/local/apache2/bin/apachectl start`
  
备注：有些时候还是不能访问，可以尝试关闭防火墙：
`[root@localhost conf]# /etc/rc.d/init.d/iptables stop`
防火墙配置文件：`/etc/sysconfig/iptables`

  
G 卸载：直接删除安装目录就行；




安装出错以及解决方法：

不知道这一步具体有没有作用(先别执行)：
`yum remove apr-util-devel apr apr-util-mysql apr-docs apr-devel apr-util apr-util-docs`

错误1 ：在步骤 D-a 中显示错误：
错误信息：`configure: error: APR not found. Please read the documentation`
解决方法：
a 在http://apr.apache.org/download.cgi下载apr-1.5.2.tar.gz
b 上传到/root目录下
c `#tar -zxf apr-1.5.2.tar.gz` ,在/root下解压该文件，得到目录apr-1.5.2
d 进入该目录
e 输入命令：`# ./configure --prefix=/usr/local/apr`
f 可能会出现错误2
g `# make `
h `# make install `
i 进入正常安装(需看下面 5 注意)

 
错误2：
错误信息：rm: cannot remove `libtoolT': No such file or directory
解决方法：
a # cd apr-1.5.2			//进入该目录
b # vim configure			//打开configure文件
c : /$RM "$cfgfile"			//所有$RM "$cfgfile"，然后将这一行注释掉，
							即行首加#
d 保存退出

错误3：
错误信息：configure: error: APR-util not found.  Please read the documentation.
解决方法：
a 在http://apr.apache.org/download.cgi下载 apr-util-1.5.4.tar.gz
b 上传到/root目录下
c `#tar -zxf apr-util-1.5.4.tar.gz` ,在/root下解压该文件，
得到目录apr-util-1.5.4
d 进入该目录 #cd apr-util-1.5.4
e 输入命令：# ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr/bin/apr-1-config
f `# make && make install`

错误4：
错误信息：configure: error: pcre-config for libpcre not found. PCRE is required and available from http://pcre.org/
解决方法：
a 在http://ftp.exim.llorien.org/pcre/下载pcre-6.5.tar.gz文件
b `[root@localhost ~]# tar -zxf pcre-6.5.tar.gz`
c `[root@localhost ~]# cd pcre-6.5`
d `[root@localhost pcre-6.5]# ./configure --prefix=/usr/local/pcre`
e `[root@localhost pcre-6.5]# make && make install`

5 注意：解决错误后不再是采用D-a中代码，而是
`# ./configure --prefix=/usr/local/apache2 --with-apr=/usr/local/apr/ --with-apr-util=/usr/local/apr-util/ --with-pcre=/usr/local/pcre`
备注：错误1出现加第一个with，错误3出现加第二个with，错误4出现加第三个with

备注：草泥马，干你娘，好像httpd-2.2.29.tar.gz可以安装，而httpd-2.4.12.tar.gz不能在虚拟机上安装；

说明：
a	脚本安装包并不是独立的软件包类型，常见安装的是源码包
b 	是人为的把安装过程写成了自动安装的脚本，只要执行脚本，定义简单的参数，就可
	以完成安装
c 	非常类似于windows下的安装方式
说明2：这种安装方式并不推荐，所以只简单说一下：
安装过程：
a 下载软件：http://sourceforge.net/projects/webadmin/files/webmin/
b 解压缩并进入压缩目录
c 执行安装脚本





总备注：
1 命令：cp、mv、ln 操作对象 目标对象；
说明：注意其操作对象和目标对象位置，与大多数命令操作对象在最后是不同的；



7 用户和用户组管理
	
7.1 用户配置文件
	
7.1.1 用户信息文件 /etc/passwd
A /etc/passwd
备注：用vim /etc/passwd 进入文件；用man 5 passwd 查看帮助信息；
内容解析：
root	:	x	:	0	:	0	:	root	:	/root	:	/bin/bash
用户名	 密码标志  UID	   GID	  用户说明 		家目录	 登录之后的shell
进一步解析：
x 			密码标志，真实密码密文存在shadow文件中，如果没有x标志，则会认为该
			用户没有密码，不过没有密码只允许本机登录；

UID(用户ID)：
0			超级用户
1-499		系统用户（伪用户）	
500-65535	普通用户
备注：把普通用户变为超级用户，只需要把UID改为0，其他啥都不变；

GID(用户初始组ID):用户一登录就拥有这个用户组的相关权限，每个用户的初始组只能有一个，一般就是和这个用户的用户名相同的组名作为这个用户的初始组；
附加组：指用户可以加入多个其他的用户组，并拥有这些组的权限，附加组可以有多个；

家目录：
	普通用户：/home/用户名/
	超级用户：/root/

shell：Linux的命令解释器，在/etc/passwd当中，除了标准的Shell是标准的/bin/bash
之外，还可以写其他的，如：/sbin/nologin

		

7.1.2 影子文件/etc/shadow
文件内容部分：
root:$6$b.ett8MQ$VuIGRSjH3xUeibzmZRnCJFuT/yF4ZvLaWLLCZVkQ4liCDvR2JVYOilm.b.firtWKR35Hh27dW8uQJjL9M92Na0:16583:0:99999:7:::
第一字段：用户名
第二字段：加密密码（sha512散列加密法），如果密码位是'!!'或'*'表示没有密码，不能登录
第三字段：密码最后一次修改的日期(使用1970/1/1到现在的天数作为时间戳)
第四字段：两次密码的修改间隔(和第三字段相比)
第五字段：密码有效期(和第三个字段相比)
第六字段：密码修改到期前的警告天数(和第五个字段相比)
第七字段：过期之后的宽限天数（0代表密码过期后立即失效，-1代表密码永远不会失效）
第八字段：账号失效时间（也用时间戳表示）
第九字段：保留

备注：
时间戳换算成人类识别时间
`# date -d '1970-01-01 16666 days'`
将日期换算为时间戳
`# echo $(($(date --date="2014/01/06" +%s)/86400+1)) `
		
7.1.3 组信息文件/etc/group和组密码文件/ect/gshadow
A 组信息文件/etc/group
部分内容：
root:x:0:lydia

第一字段：组名
第二字段：组密码标志
第三字段：GID
第四字段：组中附加用户

B 组密码文件/ect/gshadow
部分内容：
root:::lydia
第一字段：组名
第二字段：组密码
第三字段：组管理员用户名
第四字段：组中附加用户
	
7.2 用户管理相关文件
A 用户家目录
a 普通用户：/home/用户名/，所有者和所属组都是此用户，权限是700
b 超级用户：/root/,所有者和所属组都是root用户，权限时550	

B 用户邮箱：/var/spool/mail/用户名/	

C 用户模板目录	/etc/skel/
作用：在该目录下的文件，会在创建用户时	，直接复制到该用户的家目录下；
例：
```shell script
[root@localhost ~]# cd /etc/skel
[root@localhost skel]# vim warning.txt
[root@localhost skel]# useradd user1
[root@localhost skel]# passwd					//设置密码
[root@localhost skel]# cd /home/user1
```

备注：就会显示` warning.txt`

    

7.3 用户的管理命令
A 命令：useradd [选项] 用户名
				-u UID			手工指定用户的UID号
				-d 家目录		手工指定用户的家目录
				-c 用户说明		手工指定用户的说明
				-g 组名			手工指定用户的初始组(一般不采用)
				-G 组名			指定用户的附加组(采用)
				-s shell		手工指定用户的登录shell，默认为/bin/bash
功能：添加新用户
例：
```shell script
[root@localhost user1]# useradd -u 666 -d /lydia -c "这是一个测试用户" -G root,bin -s /bin/bash lydia
[root@localhost user1]# grep lydia /etc/passwd

lydia:x:666:666:这是一个测试用户:/lydia:/bin/bash
```


B 命令：passwd [选项] 用户名
			    -S			查询用户密码的密码状态，仅root用户可用
				-l			暂时锁定用户（用户不能登录），仅root用户可用
				-u			解锁用户，仅root用户可用
				--stdin		可以通过管道符输出的数据作为用户的密码
功能：设置密码		
例：`passwd username`
备注：超级用户可以passwd username 而普通用户只需输入passwd直接回车即可
例：
```shell script
[root@localhost ~]# passwd -S lydia
lydia LK 2015-05-28 0 99999 7 -1 (密码已被锁定。)
```
例：
`[root@localhost ~]# echo '123' | passwd --stdin lydia`
备注：管道符作用是将第一个输出结果作为第二个的操作对象；


C 命令：usermod [选项] 用户名
				-u UID			手工指定用户的UID号
				-d 家目录		手工指定用户的家目录
				-c 用户说明		手工指定用户的说明
				-g 组名			手工指定用户的初始组(一般不采用)
				-G 组名			指定用户的附加组(采用)
				-s shell		手工指定用户的登录shell，默认为/bin/bash
				-L				临时锁定用户(Lock)
				-U				解锁用户锁定(Unlock)
功能：修改用户，除了锁定密码，其他都是修改/etc/passwd文件
例：usermod -c '测试用户' lydia


D 命令：chage [选项] 用户名
				-l			列出用户的详细密码状态
				-d 日期		修改密码最后一次更改的日期(shadow3字段)
				-m 天数		两次密码修改间隔(4字段)
				-M 天数		密码有效期(5字段)
				-W 天数		密码过期前警告天数(6字段)
				-I 天数		密码过期后宽限天数(字段)
				-E 日期		账号失效时间(8字段)
重要例子：				
例：`chage -d 0 lydia	`	//该命令作用是把密码修改日期设为0，这样用户一登录就要
						求改密码（这个很重要）

E 命令：`userdel [-r] 用户名`
				-r 			删除用户的同时删除用户的家目录

功能：删除用户


F 命令：id 用户名
功能：查看用户的uid和gid
例：id lydia

G 命令：su [选项] 用户名
			 -			选项只使用'-'代表连带用户的环境变量一起切换（不能省）
			 -c 		仅执行一次命令，而不切换用户身份
功能：切换用户	
例：
```shell script
[root@localhost ~]# su - lydia			//root用户切换成普通用户不需要密码
```
例：
`[lydia@localhost ~]$ su - root -c "useradd haha"`	//该例作用是普通用户
利用root用户执行一条只有root才能执行的命令，引号中为命令；

H 添加默认用户操作，影响到的文件
```shell script
[root@localhost user1]# useradd hehe
[root@localhost user1]# grep hehe /etc/passwd
[root@localhost user1]# grep hehe /etc/shadow
[root@localhost user1]# grep hehe /etc/group
[root@localhost user1]# grep hehe /etc/gshadow
[root@localhost ~]# ll -d /home/hehe
[root@localhost ~]# ll /var/spool/mail/hehe
```



I 用户默认文件	/etc/default/useradd
```shell script
GROUP=100				#用户默认组
HOME=/home				#用户家目录
INACTIVE=-1				#密码过期宽限天数(shadow文件7字段)
EXPIRE=					#密码失效时间(8，没有表示永久不失效)
SHELL=/bin/bash			#默认shell
SKEL=/etc/skel			#模板目录
CREATE_MAIL_SPOOL=yes	#是否建立邮箱
```

J 另一个默认文件 /etc/login.defs
```shell script
PASS_MAX_DAYS 99999		#密码有效期(5)
PASS_MIN_DAYS 0 		#密码修改间隔(4)
PASS_MIN_LEN  5			#密码最小位数(显示采用PAM，最小是8位)
PASS_WARN_AGE 7			#密码到期警告(6)
UID_MIN		  500		#最小和最大的UID范围
GID			  60000		#用户初始组最大值
ENCRYPT_METHOD SHA512	#加密方式
```


	
	7.4 用户组管理命令
A 命令：`groupadd [选项] 组名`
				 -g GID			指定组ID
功能：添加新组
例：`[root@localhost ~]# groupadd test_group`		//添加新组
例：`[root@localhost ~]# groupadd -g 888 test2`	//设置新组的GID为888；				 

B 命令：groupmod [选项] 新组名 旧组名
				 -g GID			修改组ID
				 -n 新组名		修改组名
功能：修改组的信息
例：`[root@localhost ~]# groupmod -n new_group test_group`

备注：修改用户和组名，涉及到其他的很多信息，尽量别修改，如果要换用户的组，还不如直接新建一个；

C 命令：groupdel 组名
功能：删除组
备注：如果组中有初始用户，则用户不能删除，如果只是有其他用户，则可以删除；

D 命令：gpasswd 选项 组名
				-a 用户名		把用户加入组
				-d 用户名		把用户从组中删除
功能：给用户添加附加组
例：`[root@localhost ~]# gpasswd -a lydia root`	//给lydia用户添加root组
例：`[root@localhost ~]# gpasswd -d lydia root`	//给lydia用户删除root组

穿越：# df -h		//作用查看分区使用情况；



8 权限管理

8.1 ACL权限
	
8.1.1 ACL权限简介与开启

说明：ACL权限主要是解决所有者、所属组和其他人角色不够的情况下，给具体的用户分配某目录或文件的相应权限（主角是目录或者文件，即目录或文件允许用户或组对自身有什么权限）		
A 查看分区ACL权限是否开启（其实默认就是开启的）
`[root@localhost ~]# dumpe2fs -h /dev/sda1`
							 -h	仅显示超级块中信息，不显示磁盘块组的详细信息
功能：查询指定分区详细文件系统信息的命令选项
内容：`Default mount options:    user_xattr acl`	//说明开启

B 临时开启分区ACL权限
语法：`[root@localhost ~]# mount -o remount,acl /`
功能：重新挂载分区，并加入ACL权限（重启系统，将无效）；

C 永久开启分区ACL权限
a `[root@localhost ~]# vim /etc/fstab `
b `UUID=fac6b26e-b796-45bf-b2a6-ce8a2b41a04e /  `                     ext4    defaults        1 1
备注：在要添加ACL权限的分区所在的行 defaults更改为defaults,acl
c `[root@localhost ~]# mount -o remount /`
备注：重新挂载文件系统或重启系统，使修改生效；
							 
									
		8.1.2 查看与设定ACL权限
		
A 命令：getfacl 文件名
功能：查看acl权限


B 命令：setfacl 选项 文件名
				-m			设定ACL权限
				-x			删除指定的ACL权限
				-b 			删除所有的ACL权限
				-d			设定默认的ACL权限
				-k			删除默认的ACL权限
				-R			递归设定ACL权限
功能：设置目录或文件给用户和组设置acl权限
例：`[root@localhost tmp]# setfacl -m u:test:rx project`
备注：u:test:rx中u是代表为用户设置acl权限，test表示用户名，rx代表权限
例：
				
C 为具体用户创建ACL权限
```shell script
[root@localhost ~]# mkdir /tmp/project			//创建project目录
[root@localhost ~]# useradd student_1
[root@localhost ~]# passwd						
[root@localhost ~]# useradd student_2
[root@localhost ~]# passwd						//创建两个学生用户
[root@localhost ~]# groupadd student_group		//创建学生组
[root@localhost ~]# gpasswd -a student_1 student_group //为用户添加附加组
[root@localhost ~]# gpasswd -a student_2 student_group //为用户添加附加组
[root@localhost ~]# chown root:student_group /tmp/project 
//更改目录的所有者和所属组
[root@localhost tmp]# chmod 770 project			//更改目录权限；
[root@localhost tmp]# useradd test
[root@localhost tmp]# passwd					//添加一个测试用户（只分配读和执行权限）
[root@localhost tmp]# setfacl -m u:test:rx project//为具体用户设置具体权限
```

备注：查看目录信息（可以看出权限后有+，表示设置ACL权限成功）
信息：drwxrwx---+ 2 root student_group   4096 5月  29 00:43 project		
备注：切换到test用户，测试用户在/tmp/project中的权限
```shell script
[root@localhost tmp]# su - test
[test@localhost ~]$ cd /tmp/project
[test@localhost project]$ touch test.txt
```
touch: 无法创建"test.txt": 权限不够

D 为组设置ACL权限
```shell script
[root@localhost tmp]# groupadd test_group
[root@localhost tmp]# setfacl -m g:test_group:rx project
[root@localhost tmp]# getfacl test_group
```

	
				
		8.1.3 最大有效权限与删除ACL权限
A 用户的真实得到的权限：由最大有效权限mask与指定的ACL权限'相与'得到的；
备注：'相与'例子：mask中有r权限，指定有r权限，则用户真实权限有r，其余情况用户真实权限没有r；
		
B 修改最大有效权限
代码：`[root@localhost ~]# setfacl -m m:rx /tmp/project`
备注：修改最大有效权限为rx；


C 删除ACL权限
a 删除指定用户ACL权限
语法：setfacl -x u:用户名 文件名	
例：`[root@localhost ~]# setfacl -x u:test /tmp/project`

b 删除指定组ACL权限
语法：setfacl -x g:组名 文件名
例:`[root@localhost ~]# setfacl -x g:test_group /tmp/project`

c 删除文件的所有ACL权限
语法：setfacl -b 文件名
例：`[root@localhost ~]# setfacl -b /tmp/project/`
		
8.1.4 默认ACL权限与递归ACL权限
		
A 递归ACL权限（只针对现有子目录）
a 递归是父目录在设定ACL权限时，所有 现有 的子文件和子目录也会拥有相同的ACL
权限。
设置递归ACL权限语法：
`# setfacl -m u:用户名:权限 -R 文件名(即目录)`
备注：-R只能在那个位置；
例：`[root@localhost project]# setfacl -m u:test:rx -R /tmp/project/`

B 默认ACL权限（只针对新建子目录）
a 默认ACL权限作用是如果给父目录设定了ACL权限，那么父目录中所有 新建 的子文件
都会继承父目录的ACL权限
语法：setfacl -m d:u:用户名:权限 文件名
例1：`[root@localhost project]# setfacl -m d:u:test:rx -R /tmp/project/`
例2：`[root@localhost project]# setfacl -m d:u:test:rx /tmp/project/`
备注：例1中虽然有递归，但是也是对新建子目录才有效；


8.2 文件特殊权限

8.2.1 SetUID
		
A SetUID 功能
a 只有可执行的二进制程序才能设定SUID权限
b 命令执行者要对该程序拥有x(执行)权限
c 命令执行者在执行该程序时获得该程序文件所有者的身份(在执行程序的过程中灵魂
附体为文件的所有者，且这种身份对命令执行对象(其他文件)有效)
d SetUID权限只在该程序执行过程中有效，也就是说身份改变只在程序执行过程中有效；
例：-rwsr-xr-x. 1 root root 25980 2月  22 2012 /usr/bin/passwd
备注：注意所有者权限中的s就是指SUID，当时所属组中有s代表SGID

B 设定SetUID方法
a 4代表SUID
	chmod 4755 文件名
	chmod u+s 文件名
例：chmod 4755 test			//为文件test设置SUID权限以及其他权限
备注：如果文件原来没有执行权限x，就直接设定SUID，那么这个设定是无效的；
-rwSr--r--. 1 root root               0 5月  29 03:51 aa
备注：无效的设定用S表示；


b 取消SUID
	chmod 755 文件名
	chmod u-s 文件名
例：chmod 755 test

C 危险的SUID
a 关键目录应严格控制写权限。比如：'/'、'/etc'等
b 用户的密码设置要严格遵守密码三原则
c 对系统中默认应该具有SetUID权限的文件做一列表，定时检查有没有这之外的文件被
设置了SetUID权限；
		
8.2.2 SetGID

A SetGID 针对文件的作用
a 只有可执行的二进制程序才能设置SGID权限
b 命令执行者要对该程序有x(执行)权限
c 命令执行时，执行者的组身份升级为该程序文件的所属组
d SetGID权限同样只在该程序执行过程中有效；
用处不多，例子：
`[root@localhost tmp]# ll /usr/bin/locate`
-rwx--s--x. 1 root slocate 35548 10月 10 2012 /usr/bin/locate

`[root@localhost tmp]# ll /var/lib/mlocate/mlocate.db`
-rw-r-----. 1 root slocate 1707172 5月  29 03:34 /var/lib/mlocate/mlocate.db	
备注：locate命令实际上搜索mlocate.db，而普通用户是没有r读权限的，真实原理是普通用户执行locate命令，所属组升级为slocate，而mlocate.db的所属组就是slocate，所以普通用户可以查看mlocate.db中的内容了；

B SetGID针对目录的作用：
a 普通用户必须对此目录拥有r和x权限，才能进入此目录
b 普通用户在此目录中的有效组此目录的所属组
c 若普通用户对此目录拥有w权限时，新建的文件的默认所属组是这个目录的所属组；

C 设定SetGID
 2代表SGID
语法1：chmod 2755 文件名
语法2：chmod g+s 文件名

D 取消SetGID
语法1：chmod 755 文件名
语法2：chmod g-s 文件名

8.2.3 L文件特殊权限-Sticky BIT
		
A SBIT粘着位作用
a 粘着位目前只对目录有效
b 普通用户对该目录拥有w和x权限，即普通用户可以在此目录拥有写入权限
c 如果没有粘着位，因为普通用户拥有w权限，所以可以删除此目录下所有文件，包括其他
用户建立的文件。一旦赋予的粘着位，除了root可以删除所有文件，其余普通用户就算拥有w权限，也只能删除自己建立的文件，但是不能删除其他用户建立的文件
例：
`[root@localhost ~]# ll -d /tmp`
drwxrwxrwt. 5 root root 4096 5月  29 06:13 /tmp
备注：可以看出其他人权限t，就是SBIT权限标志

B 设置和取消粘着位
a 设置粘着位
语法1：chmod 1755 目录名
语法2：chmod o+t 目录名

b 取消粘着位
语法1：chmod 777 目录名
语法2：chmod o-t 目录名

	8.3 文件系统属性chattr权限（作用：防止用户误操作删除文件）
A 命令：chattr
语法：chattr [+-=] [选项] 文件或目录名
			  +				增加权限
			  -				删除权限
			  =				等于某权限
选项：
	i 	如果对文件设置i属性，那么不允许对文件进行删除、改名，也不能添加和修改数
	    据；如果对目录设置i属性，那么只能修改目录下的文件数据，但不允许建立和删
		除文件，也不允许删除该目录；（这些现在针对root用户也同样有效；）
	
	a 	如果对文件设置a属性，那么只能在文件中增加数据，但是不能删除也不能修改
		数据	；如果对目录设置了a属性，那么只允许在目录中建立和修改文件，但不允
		许删除；
备注：为文件或目录设置chattr权限；		
		
例：`[root@localhost tmp]# chattr +i /test`		//针对root用户也有效
例2：`[root@localhost tmp]# chattr -i /test`	//删除i属性后可以更改了；	

B 命令：lsattr
语法：lsattr 选项 文件名
			 -a 		显示所有文件和目录
			 -d 		若目标是目录，仅列出目录本身的属性，而不是子文件的；
功能：查看文件系统属性
例：lsattr -d /test	


	8.4 系统命令sudo权限
A sudo权限
a root把本来只能超级用户执行的命令赋予普通用户执行
b sudo操作的对象是系统命令

B sudo使用
语法：`[root@localhost tmp]# visudo`
备注：上面语法也是例子，实际修改的是/etc/sudoers文件
文件中有使用sudo例子：
root    ALL=(ALL)       ALL					//98行
位置1	root 		被赋权限用户名 
位置2 	ALL			被管理主机的地址（为Linux系统主机ip,也可以写ALL）
位置3 	ALL			可以使用的身份（可省略，省略表示可使用root身份）
位置4 	ALL			授权命令(命令绝对路径)
例：（在/etc/sudoers文件中照着上面写）
lydia	192.168.1.250= /sbin/shutdown -r now
备注：命令可以限制具体一些，那么被赋予用户权限小一些；
```shell script
# %wheel        ALL=(ALL)       ALL
# %组名			...(与上面相同)
```


C 普通用户执行sudo赋予命令
```shell script
[root@localhost ~]# su - lydia			//转换为普通用户
[lydia@localhost ~]$ sudo -l			//查看该普通用户被赋予可执行哪些命令
										(不过要先输入该用户密码)
[lydia@localhost ~]$ sudo /sbin/shutdown -r now	//普通用户用sudo命令执行被赋予可执行的命令（其实这是就是用户升级为root执行该命令，前面位置3中声明的），如果直接执行命令，则还是该用户身份；

```
千万注意：不要把/usr/bin/vim等编辑命令赋予普通用户，因为那样普通用户将借助vim命令可以任何文件；

权限总结：
1 rwx权限
2 acl权限
3 SUID
4 SGID
5 SBIT
6 chattr（文件系统属性）
7 sudo





9 文件系统管理
	
9.1 回顾分区和文件系统
	
A 分区类型
a 主分区：总共最多只能分4个
b 扩展分区：只有一个，也是主分区的一种，就是说主分区加扩展分区最多有四个，但
扩展分区不能存储数据和格式化，必须在划分成逻辑分区才能使用；
c 逻辑分区：逻辑分区是在扩展分区中划分的；如果是IDE硬盘，Linux最多支持59个逻辑
分区，如果是SCSI硬盘Linux最多支持11个逻辑分区
	分区的设备文件名
主分区1				/dev/sda1
主分区2				/dev/sda2
主分区3				/dev/sda3
扩展分区				/dev/sda4
逻辑分区1			/dev/sda5		//第一个逻辑分区始终是5开始，即使主分区
逻辑分区2			/dev/sda6		//不足4个
逻辑分区3			/dev/sda7

B 文件系统
a 现在CentOS6以后采用ext4:它是ext3的升级版本；ext4在性能、伸缩性和可靠性方
面进行了大量的改进；最大支持1EB文件系统和16TB文件、无限数量子目录等等；
格式化：就是在小分区中打入隔断，其实就是写入文件系统；	
	
9.2 文件系统常用命令
			
9.2.1 df命令、du命令、fsck命令和dump2fs命令
		
A 命令：df [选项] [挂载点]		
		  -a 			显示所有文件系统信息，包括特殊文件系统，如/proc,
						/sysfs	
		  -h			使用习惯单位显示容量，如KB、MB和GB等
		  -T			显示文件系统类型
		  -m			以MB为单位显示容量
		  -k			以KB为单位显示容量。默认就是以KB为单位；
功能：查看分区使用情况(挂载点可以省略，即查看全部)
例：[root@localhost ~]# df -h

B 命令：`du [选项] [目录或文件名]`
			-a 		显示每个子文件的磁盘占用量，默认只统计子目录的磁盘占用量
			-h		使用习惯单位显示磁盘占用量，如KB、MB、GB等
			-s		统计总占用量，而不列出子目录和子文件的占用量
功能：统计目录或文件大小（其实统计文件常用ls）		  
例：`[root@localhost ~]# du -sh`
备注：du命令和df命令的区别
df命令是从文件系统考虑的，不光要考虑文件占用的空间，还要统计被命令或程序占用的空间（最常见的就是文件已经删除，但程序并没有释放空间，也就是说df命令统计的更准确）
du命令是面向文件的，只会计算文件或目录占用的空间；

C 命令：fsck [选项] 分区设备文件名
			  -a			不用显示用户提示，自动修复文件系统
			  -y			自动修复，和-a作用一样，不过有些文件系统只支持-y
功能：底层修复分区，其实启动系统时，自动执行，并不需要人工执行（人工执行反而可能出错）；
例：fsck -y /dev/sda1

D 命令：dumpe2fs 分区设备名
功能：显示磁盘状态
功能：`[root@localhost ~]# dumpe2fs /dev/sda5`
		
		9.2.2挂载命令
A 命令：mount [-l]		
功能：查询系统中已经挂载的设备，-l会显示卷标名称
例：[root@localhost ~]# mount 

  命令：mount -a 
功能：依据配置文件/etc/fstab的内容，自动挂载；  
 
B 命令：mount [-t文件系统] [-L卷标名] [-o特殊选项] 设备文件名 挂载点
			-t文件系统	加入文件系统类型来指定挂载的类型，可以ext3、ext4、
						、iso9660等文件系统
			-L 卷标名	挂载指定卷标的分区，而不是安装设备文件名挂载
			-o 特殊选项	可以指定挂载的额外选项
特殊选项o特别多（以后详查）：常用的
	参数				说明
exec/noexec		执行/不执行，设定是否允许在文件系统中执行可执行文件，默认为
				exec允许	
remount			重新挂载已经挂载的文件系统，一般用于指定修改特殊权限
例：mount -o remount,noexec /home/		//重新挂载分区，并使用noexec权限
备注：可以考虑把用户上次的分区设置为noexec，防止病毒；改回去就把上例中noexec改为exec即可；

		9.2.3 挂载光盘和U盘
A 挂载光盘
a `[root@localhost mnt]# mkdir cdrom`		//建立挂载点
b 将光盘放入光驱
c `[root@localhost mnt]# mount -t iso9660 /dev/cdrom /mnt/cdrom/` //挂载光盘

B 卸载命令
a 语法：umount 设备文件名或卸载点
例：	`[root@localhost ~]# umount /mnt/cdrom`
注意：卸载挂载点，必须要先退出挂载点目录；		

C 挂载U盘
a 先进入虚拟机，再插入u盘(不然u盘是被windows识别)
b `[root@localhost ~]# fdisk -l`			//查看U盘设备文件名
内容：
Disk /dev/sdb: 15.5 GB, 15538716672 bytes
68 heads, 4 sectors/track, 111577 cylinders
Units = cylinders of 272 * 512 = 139264 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xa2560c90

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1              30      111578    15170496    c  W95 FAT32 (LBA)
这里是设备文件名										FAT32挂载时要用-t vfat

d `[root@localhost ~]# mount -t vfat /dev/sdb1 /mnt/usb`   //挂载
注意:Linux中把FAT16分区识别为fat ，把FAT32识别为vfat，fat和vfat是针对-t选项
注意2：Linux默认不支持NTFS文件系统（形式的硬或U盘）

	9.2.4 支持NTFS文件系统
A 下载NTFS-3G插件
http://www.tuxera.com/community/ntfs-3g-download/
	
B 安装NTFS-3G
a # tar -zxvf ntfs-3g_ntfsprogs-2013.1.13.tgz		//解压
b # cd ntfs-3g_ntfsprogs-2013.1.13				//进入解压目录
c # ./configure			    //编译前准备，没有指定安装目录，安装到默认位置
d # make					//编译
e # make install			//编译安装

C #mount -t ntfs-3g 分区设备文件名 挂载点

9.3 fdisk分区
	
9.3.1 fdisk命令分区过程
A 添加新硬盘(需要关机才能添加，但是好像我的虚拟机要开启系统才行)		

B 查看硬盘命令：fdisk -l 

C 使用fdisk进行分区：fdisk /dev/sdb 
备注：/dev/sdb后面没有数字编号；
		fdisk交互指令说明 
	命令			说明
	 a			设置引导标记	
	 b			编辑bsd磁盘标签
	 c			设置DOS操作系统兼容标记
	 d 			删除一个分区(记)
	 l			显示已知的文件系统类型，82为Linux swap分区，83为Linux分区(记)
	 m			显示帮助菜单(记)
	 n			新建分区(记)
	 o 			建立空白DOS分区表
	 p			显示分区列表
	 q			不保存退出
	 s			新建空白SUN磁盘标签
	 t			改变一个分区的系统ID(记)
	 u			改变显示记录单位
	 v			验证分区表
	 w			保存退出(记)
	 x			附加功能(仅专家)
在fdisk命令中的详细步骤：
创建主分区
Command (m for help): n				//输入n，新建分区
Command action
   e   extended
   p   primary partition (1-4)
p									//输入p，新建主分区
Partition number (1-4): 1   		//分区号
First cylinder (1-652, default 1):  //直接回车，从默认1开始
Using default value 1
Last cylinder, +cylinders or +size{K,M,G} (1-652, default 652): +1G	
									//输入+1G，分配存储大小；
创建扩展分区
Command (m for help): n 
Command action
   e   extended
   p   primary partition (1-4)
e
Partition number (1-4): 
Value out of range.
Partition number (1-4): 2
First cylinder (133-652, default 133): 
Using default value 133
Last cylinder, +cylinders or +size{K,M,G} (133-652, default 652): 
Using default value 652
创建逻辑分区
Command (m for help): n           
Command action
   l   logical (5 or over)
   p   primary partition (1-4)
l
First cylinder (133-652, default 133): 
Using default value 133
Last cylinder, +cylinders or +size{K,M,G} (133-652, default 652): 
Using default value 652									
保存退出
Command (m for help): w
	
D 重新读取分区信息命令：partprobe
例：`[root@localhost ~]# partprobe`
备注：这个命令执行后会有警告，不用理会；

E 格式化分区：
```shell script
[root@localhost ~]# mkfs -t ext4 /dev/sdb1
[root@localhost ~]# mkfs -t ext4 /dev/sdb5
```

F 建立挂载点并挂载
```shell script
[root@localhost ~]# mkdir /mnt/sdb1
[root@localhost ~]# mkdir /mnt/sdb5
[root@localhost ~]# mount /dev/sdb1 /mnt/sdb1
[root@localhost ~]# mount /dev/sdb5 /mnt/sdb5
```


备注：以上进行的分区挂载等，在重启之后有需要手动挂载；

	
		9.3.2 分区自动挂载与fatab文件修复
A 编辑/etc/fstab文件
部分内容	：	
UUID=fac6b26e-b796-45bf-b2a6-ce8a2b41a04e  /     ext4    defaults   1 1
解析：
第一字段：分区设备文件名或UUID(硬盘通用唯一标识码，推荐用这个，即使更改硬盘顺序也无所谓，查看UUID，命令：[root@localhost /]# dumpe2fs -h /dev/sda1 )
第二字段：挂载点
第三字段：文件系统名称
第四字段：挂载参数（挂载参数跟mount中特殊权限o的值是一样）
第五字段：指定分区是否被dump备份，0代表不备份，1代表每天备份，2代表不定期备份（在分区下的lost+found文件夹下）
第六字段：指定分区是否被fsck检测，0代表不检测，其他数字代表检测优先级，数字越小优先级越高，如：1的优先级大于2；

B 按照上面格式，将需要自动挂载的分区写入/etc/fstab文件（注意：千万别写错了，写
错后，系统将可能启动不了）
UUID=2c0bacae-fc4e-4c38-bcdc-81064fb4c677 /mnt/sdb1              ext4    defaults         1 2 

C 为了防止写错，用命令
[root@localhost /]# mount -a //作用：重新自动挂载分区，如果有错，好即时看见

D /etc/fstab文件修复
备注：只能修改这个文件中除根分区挂载行错误，如果根分区挂载行错误，就不能得到提示信息，也不能采用这种方式修复了；
a 在提示错误后，输入root密码，进入操作系统；
b 输入：[root@localhost /]# mount -o remount,rw / 
c 更改/etc/fstab文件中的错误；

	
	9.4 分配swap分区
A 命令：free -m
功能：显示内存和swap使用状况(选项m表示以MB显示)	
例：free -m
备注：
cached(缓存)：是指把读取出来的数据保存在内存当中，当再次读取时，不用读取硬盘而是直接从内存当中读取，加速了数据的读取过程；
buffer(缓冲)：是指在写入数据时，先把分散的写入操作保存到内存当中，当达到一定程度再集中写入硬盘，减少了磁盘碎片和硬盘的反复寻道，加速了数据的写入过程；

B 添加swap分区的详细步骤
提示：开始之前可以通过# fdisk -l 查看分区以及剩余空间(空间不够，要先调整其他
分区的空间，删除和调整都行)
a 	`[root@localhost ~]# fdisk /dev/sdb`
b 	
Command (m for help): n					//新建分区
Command action
   l   logical (5 or over)
   p   primary partition (1-4)
l										//新建逻辑分区
First cylinder (301-652, default 301):  //从默认cylinder期
Using default value 301
Last cylinder, +cylinders or +size{K,M,G} (301-652, default 652): +500M
										//设置500M分区	
Command (m for help): t					//更改文件系统
Partition number (1-6): 6				//输入wamp分区编号
Hex code (type L to list codes): 82		//输入wamp系统编号
Changed system type of partition 6 to 82 (Linux swap / Solaris)

Command (m for help): w					//保存退出
The partition table has been altered!


c `[root@localhost ~]# partprobe`
备注：partprobe可能没有生效，即输入后面格式化命令，提示找不到相对于目录，则需要重启系统；

d 格式化
`[root@localhost ~]# mkswap /dev/sdb6`

f 加入swap分区
`[root@localhost ~]# swapon /dev/sdb6`		//加入swap分区
`[root@localhost ~]# swapoff /dev/sdb6	`	//取消swap分区(在这个步骤中不执行)

备注：如果是通过以上步骤把分区加入swap，需要每次开机后手工执行加入swap操作；
显然我们需要自动加入swap，所以，在分好要加入swap的分区后，通过更改/etc/fstab来进行自动加入；

C swap分区开机自动挂载
前提：先要分配好要加入swap的分区
a  	`[root@localhost ~]# vim /etc/fstab`
b	输入内容:
/dev/sdb6      swap         swap    defaults         0 0
c	`[root@localhost ~]# mount -a		//测试有没有把文件写错；


10 Shell

10.1 Shell概述
A Shell是一个命令行解释器，它为用户提供一个向Linux内核发送请求以便运行程序的界
面系统级程序，用户可以用Shell来启动、挂起、停止甚至是编写一些程序；	

B Shell还是一个功能相当强大的编程语言，易编写，易调试，灵活性较强。Shell是解释
执行的脚本语言，在Shell命令中可以直接调用Linux系统语言；

C Shell有很多种，Linux采用Bash类型；在/etc/shells文件中可以查看Linux支持的
shell

	
10.2 Shell脚本执行方式
	
A 命令：echo [选项] [输出内容]
			  -e			支持输出内容中反斜线控制的字符转换
控制字符				作用
\\					输出\本身
\a					输出警告音
\b 					退格键，也就是向左删除键
\c					取消输出行末的换行符。和'-n'选项一致
\e					ESCAPE键
\f					换页符
\n					换行符
\r					回车键
\t					制表符，也就是tab键
\v					垂直制表符
\0nnn				按照八进制ASCII码表输出字符。其中0为数字0，nnn是三位
					八进制数			  
\xhh				按照十六进制ASCII码表输出字符，其中hh是两位二进制数
例：`[root@localhost sh]# echo -e 'a\tb'`
例2：`[root@localhost sh]# echo -e '\e[1;31m hehe\e[0m'	`
备注：例2输出红颜色字体hehe， 注意 \e[1; 和 \e[0m开启和关闭颜色标记
30m = 黑色，31m = 红色，32m=绿色 ，33m=黄色
34m = 蓝色，35m = 洋红，36m=青色 ，37m=白色
	
B 第一个脚本
```shell script
#!/bin/bash							//这里不是注释，而是shell脚本标记；
#writer:neo 
#2015年 05月 29日 星期五 15:28:33 CST
echo 'hello world'
```


C 脚本执行
方式一：赋予执行权限，直接执行；(采用)
chmod 755 hello.sh
./hello.sh 					//必须要加上前面的 ./ 才行

方式二：通过bash调用脚本
bash hello.sh

D 命令：dos2unix 文件名
功能：将文件由windows格式转化为Linux下文本编辑格式；
备注：该命令有些Linux系统没有安装，可以通过命令：
`[root@localhost ~]# yum -y install dos2unix`
备注：当然unix2dos命令也同理；


10.3 Bash的基本功能
		
10.3.1 历史命令和命令补全
A 历史命令：history [选项] [历史命令保存文件]
					-c			清空历史命令，只会清除内存中的历史命令；
					-w			把缓存中的历史命令写入历史命令保存文件：
								用户家目录/.bash_histroy
功能：显示历史命令或把历史命令写入文件等
备注：每个用户的历史命令在用户退出登录时，存放入自己家目录的.bash_histroy(隐藏文件)，退出之前放在内存中；
备注2：历史命令默认会保存1000条，可以在环境变量配置文件/etc/profile中进行修改，找到文件中HISTSIZE=1000行修改即可；

历史命令的调用：
a 使用上、下箭头调用以前的命令
b 使用'!n'重复执行第n条历史命令
c 使用'!!'重复执行上一条历史命令
d 使用'!字串'重复执行最后一条以该字串开头的命令
例：`[root@localhost ~]# !vi

B 命令与文件补全
在Bash中，命令与文件补全是非常方便与常用的功能，我们只要在输入命令或文件时，按
'Tab'键就能自动进行补全(如果命令或文件不止一个，需要按两次，进行选择)

		
10.3.2 命令别名与常用快捷键
		
A 命令别名：alias 别名='原命令'		
功能：设定命令别名
例： `alias mv='mv -i'`

B 命令：`alias`
功能：查询命令别名

C 命令优先级
a 第一顺位执行绝对路径或相对路径执行的命令
b 第二顺位执行别名
c 第三顺位执行Bash的内部命令
d 第四顺位执行安装$PATH环境变量定义的目录查找顺序找到的第一个命令；

D 别名永久生效：编辑用户家目录下的.bashrc文件
备注：最好不要让别名永久生效；

E 删除别名：unalias 别名
备注：执行清除命令设置的别名，并不能清除文件.bashrc中设置的别名；

F bash常用快捷键
快捷键				作用
ctrl+a  		把光标移动到命令行开头。如果我们输入的命令过长，想要把光标移动到命令
				行开头使用
ctrl+e			把光标移动到命令行结尾		
ctrl+c			强制终止当前的命令				
ctrl+l			清屏，相当于clear				
ctrl+u			删除或剪切光标之前的命令			
ctrl+k			删除或剪切光标之后的内容；	
ctrl+y			粘贴ctrl+u和ctrl+y剪切的内容			
ctrl+r			在历史命令中搜索相关命令			
ctrl+d			退出登录，相当于logout				
ctrl+z			暂停，并放入后台	
ctrl+s			暂停屏幕输出	
ctrl+q			恢复屏幕输出	



10.3.3 输入输出重定向
A 标准输入和输出设备
设备			设备文件名			文件描述符			类型
键盘			/dev/stdin				0				标准输入
显示器			/dev/stdout				1				标准输出
显示器			/dev/stderr				2				标准错误输出

B 输出重定向
类型				符号					作用
			命令 > 文件			以覆盖的方式，把命令的正确输出输出到指定的文件或
标准输出		    				设备当中	
重定向		命令 >> 文件			以追加的方式，把命令的正确输出输出到指定的文件或
								设备当中	
			
			错误命令 2> 文件		以覆盖的方式，把命令的错误输出输出到指定的文件或
标准错误							设备当中	(注意：2>连在一起的)
输出重定									
向			错误命令 2>> 文件	以追加的方式，把命令的错误输出输出到指定的文件或
								设备当中
例：`[root@localhost ~]# ls >>test.txt`			//test.txt不存在会自动创建；
例：`[root@localhost ~]# ls2 2>>error.txt`			//把错误输出加入error.txt文件中


C 将正确和错误输出加入同一文件

				命令 > 文件 2>&1		以覆盖的方式，把正确输出和错误输出都保存到同
									一文件当中
									
				命令 >> 文件 2>$1	以追加的方式，把正确输出和错误输出都保存到同
正确输出和							一文件当中

错误输出同		命令 &> 文件			以覆盖的方式，把正确输出和错误输出都保存到同
时保存								一文件当中

				命令 &>> 文件		以追加的方式，把正确输出和错误输出都保存到同
									一文件当中(常用)
									
			命令>>文件1 2>>文件2		把正确的输出追加到文件一，把错误的输出追加到
									文件2(常用)
例：`[root@localhost ~]# ls &>>test.txt`

D 输入重定向（用的不多）
举例命令：wc [选项] [文件名]
			 -c		统计字节数
			 -w		统计单词数
			 -l		统计行数
备注：显示输入的行、单词和字节；			 
例：[root@localhost ~]# wc </etc/profile			 
			 

		10.3.4 多命令顺序执行与管道符
		
A 多命令顺序执行
多命令执行符		 格式				 作用
	;		 命令1 ；命令2			多个命令顺序执行，命令之间没有任何的逻辑关系
	
	&&		 命令1 && 命令2			逻辑与  当命令1正确执行，则命令2执行；
											当命令1执行不正确，则命令2不执行
	||		 命令1 || 命令2			逻辑或  当命令1执行不正确，则命令2执行
											当命令1执行正确，则命令2不执行
例：`# ./configure && make && make install`
备注：判断命令是否正确执行：命令 && echo yes || echo no

恶补：
命令：dd if=输入文件 of=输出文件 bs=字节数 count=个数
		if=输入文件		指定源文件或源设备			
		of=输出文件		指定目标文件或目标设备
		bs=字节数		指定一次输入/输出字节，即把这些字节看成一个数据块
		count=个数		指定输入或输出多少个数据块
例：`[root@localhost ~]# date;dd if=/dev/zero of=/root/testfile bs=1k count=100000;date`			//这条代码作用是创建一个100MB文件需要多少时间

B 管道符
a 命令格式：命令1 | 命令2
功能：命令1的正确输出，作为命令2的操作对象；
例：`[root@localhost ~]# ll -a /etc | less`	//ll命令结果作为less的操作对象；

b 恶补命令：grep [选项] '搜索内容' 文件名
				 -i				忽略大小写
				 -n				输出行号
				 -v				反向查找
				 --color=auto	搜索出的关键字特殊颜色标记
功能：显示搜索到的行；
例：`[root@localhost ~]# grep -n --color=auto 'root' /etc/passwd`	//在/etc/passwd中搜索root并显示其所有在行用特殊颜色显示；

配合管道符例子：
例：netstat -an | grep 'ESTABLISHED'		//作用：显示远程连接


10.3.5 通配符与其他特殊符号
		
A 通配符：匹配文件名的；
通配符			作用
 ？			匹配一个任意字符
 *			匹配0个或任意多个字符，也就是可以匹配任意内容
 []			匹配中括号中任意一个字符。例如：[abc]代表一定匹配一个字符，或者是a，或者
			是b，或者是c
[-]			匹配中括号中任意一个字符，代表一个范围。例如：[a-z]代表匹配一个小写字母

[^]			逻辑非，表示匹配不是中括号内的一个字符，例如：[^0-9]代表匹配一个不是
			数字的字符
			
例：`rm -fr /tmp/*	`		//删除除隐藏文件外的所有文件；			
 
B bash中其他特殊符号
符号				作用
 ''			单引号。在单引号中所有的特殊符号，如'$'、'`'(反引号)都没有特殊含义意义
 
 ""			双引号。在双引号中特殊符号都没有特殊含义，但是"$"、"`"和"\"是例外，拥有
			调用的值、引用命令和转义符的特殊含义。
			
 ``			反引号。反引号括起来的内容是系统命令，在bash中会先执行它和$()作用一样，
			不过推荐使用$()，因为反引号容易看错
			
 $()		和反引号作用一样，用来引用系统命令
 
 #			在shell脚本中，#开头行代表注释(除首行) 
 
 $			用于调用变量的值，如调用变量name的值时，需要用$name的方式得到变量的值
 
 \			转义符，在跟\之后的特殊符号将失去特殊含义，变为普通字符，如\$；
例：
```shell script
[root@localhost ~]# name=neo 			//给变量name赋值，注意变量和值要紧挨着=
[root@localhost ~]# echo $name
[root@localhost ~]# echo "$name is a man" //输出：neo is a man
[root@localhost ~]# echo "$(date)"		//输出：时间值
```



10.4 Bash的变量
	
10.4.1 用户自定义变量
				
A 说明
a 在bash中变量默认为字符串型，如果要进行数值运算，则必须指定变量类型为数值型	
b 变量和值要紧挨着=，等号左右两侧不能有空格
c 变量的值如果有空格，需要用单引号或者双引号括起来；
d 如果需要增加变量的值，可以进行变量叠加，不过变量名需要用双引号包含"$变量"或
  用${变量}包含
e 如果是把命令的结果作为值赋值给变量，则需要使用反引号或$()包含命令
f 环境变量名建议大写，便于区分；
例：`[root@localhost ~]# name=${name}1234`
`[root@localhost ~]# echo $name`
2015年 05月 29日 星期五 19:49:08 CST1234

B 变量分类
a 用户自定义变量
b 环境变量：这种变量中主要保存的是和系统操作环境相关的数据；
c 位置参数变量：这种变量主要用来向脚本当中传递参数或数据的，变量名不能自定义，变量作
用是固定的
d 预定义变量：在bash中已经定义好的变量，变量名不能自定义，变量作用也是固定；

恶补：
命令:set 		
功能：查看系统中所有变量(包含以上四种变量)
例：`[root@localhost ~]# set`

命令:unset 变量名
功能：删除变量
例：`[root@localhost ~]# unset name`

		
10.4.2 环境变量

A 环境变量：用户自定义变量只在当前的shell中生效，而环境变量会在当前shell和这个shell
的所有子shell(执行bash等类型的shell，就进入了子shell，用exit退出子shell)当中生效。如果把环境变量写入相应的配置文件，那么这个环境变量会在所有shell中生效；

恶补：
命令：pstree
功能：确定进程树(可以看出在哪个shell)；

B 设置环境变量
命令；export 变量名=变量值
功能：声明环境变量(当然export 环境变量 可以把已经有的变量转化为环境变量)		

命令:env
功能：查询环境变量

命令：unset 变量名
功能：删除变量

C 系统常见环境变量
a PATH：系统查找命令的路径
```shell script
[root@localhost ~]# echo $PATH
/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```

备注：即我们输入命令时，之所以不用输入路径，是因为去了以上目录的搜索匹配执行文件，然后执行；我们也可以将执行文件复制到上面目录中，也可以像命令一样执行执行文件了；

b PATH=${PATH}:/root/sh
备注：通过变量叠加把目录/root/sh加入命令查找路径中，当然重启之后失效；

D PS1：定义系统提示符的变量(不能通过env查看，PS1默认是数字一)
\d 			显示日期，格式为"星期 月 日"
\h			显示简写主机名。如默认主机名'localhost'
\t			显示24小时制的时间，格式为"HH:MM:SS"
\T			显示12小时制的时间，格式为"HH:MM:SS"
\A			显示24小时制的时间，格式为"HH:MM"			
\u			显示当前用户名
\w			显示当前目录的完整名称
\W			显示当前目录的最后一个目录
\#			显示执行的是第几条命令
\$			提示符，如果是root用户显示'#'，普通用户显示'$'
例：[root@localhost ~]# PS1='[\u@\t \w]\$ '		//注意：等号右边一定要有引号


		10.4.3 位置参数变量
		
A 位置参数变量
位置参数变量				作用
 $n					n为数字，$0代表命令本身，$1-$9代表第一个到第九个参数，十个以上
					的参数需要用大括号包含，如${10}
					
 $*					这个变量代表命令行中所有的参数，$*把所有的参数
					看成一个整体，感觉把输入的参数数组组合成了一个，是以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个
					
 $@					这个变量代表命令行中所有的参数，$*把所有的参数
					看成一个整体，感觉就是一个输入参数数组，是传给脚本的所有参数的列表
 
 $#					这个变量代表命令行中所有参数的个数

例：
```shell script
#!/bin/bash
#2015年 05月 29日 星期五 21:40:06 CST
sum=$(($1+$2+$3+$4))			//注意等号两侧不能有空格；
echo $sum
echo '************' 
for i in "$*"					//$*这儿一定要用双引号括起来才行(单引号都不行)，没有声明过变量i哦
        do  
                echo $i
        done
echo '************'
for y in "$@"					//$*这儿一定要用双引号括起来才行(单引号都不行)
        do  
                echo $y
        done
echo '************'
echo $#  
```
执行结果：           
[root@localhost sh]# ./test_weizhi.sh  1 2 3 4
10
1 2 3 4
1
2
3
4
4 
		
		10.4.4 预定义变量
A 预定义变量

预定义变量			作用
 $?				最后一次执行命令的返回状态（并不是命令执行结果）。如果这个变量的值
				为0，表示上一条命令正确执行，如果这个变量值非0（具体哪个数由命令自
				己决定），则表示上一条命令执行不正确；
 
 $$				当前进程的进程号(PID)
 
 $!			
 后台运行的最后一个进程的进程号(PID)
例：`[root@localhost sh]# echo $?`
0									//表示上一条命令正确执行
例：
`[root@localhost sh]# vim test_yudingyi.sh`
```shell script
#!/bin/bash
echo "the current process is $$"		//当前的PID也就是该脚本本身运行时的PID
echo "************"
find /etc -name profile &				// &的作用：就是将命令放到后台执行
echo "the last one Daemon process is $!"
```


B 接收键盘输入
命令：read [选项] [变量名]
			-p "提示信息"		在等待read输入时，输出提示信息
			-t 秒数				read命令会一直等待用户输入，使用此选项可以指定等待
								时间
			-n 字符数			设置read命令接收指定的字符数后，就直接执行（不需
								要enter）			
			-s					隐藏输入数据(不再输入的屏幕上显示)
功能：提示用户输入，把输入赋值到变量；			
例：
```shell script
[root@localhost sh]# vim read.sh       
#!/bin/bash
read -p "enter your name: " name
echo $name
read -s -p "enter your age:" age
echo $age
read -n 1 -s -p "enter your sex(B/G): " sex
echo $sex
```

		
10.5 Bash的运算符
	
10.5.1 数值运算与运算符
		
A 命令：declare [+/-] [选项] 变量名
				-			给变量设定类型属性
				+			取消变量的类型属性
				-i			将变量声明为整数型(integer)
				-x			将变量声明为环境变量
				-p			显示指定变量的被声明的类型
功能：声明或查看具体类型变量	

B 数值运算方法：
a 方法一：declare			
例：
```shell script
[root@localhost sh]# a=1
[root@localhost sh]# b=2
[root@localhost sh]# declare -i c=$a+$b			//步骤1
[root@localhost sh]# echo $c
```
3
备注：步骤1可以更改为：
```shell script
[root@localhost sh]# declare -i c
[root@localhost sh]# c=$a+$b
```

例：`[root@localhost sh]# declare +i c`			//取消变量c的数值属性；

b 方法二：expr或let数值运算工具
`[root@localhost sh]# e=$(expr $a + $b)`			//符号+两侧必须有空格

c 方法三："$((运算式))"或"$[运算式]"      //采用
```shell script
[root@localhost sh]# a=1
[root@localhost sh]# b=2
[root@localhost sh]# echo $(($a+$b))			//符号+两侧就不严格要求空格与否
```


备注：$后面分析
$变量			变量值
$(命令)			执行命令返回其其结果
$((运算式))		返回运算结果
${变量}			变量值，主要用于和其他字符串叠加，如：echo ${a}12345,如果是
				echo $a12345 是不能输出的；

C 运算符
优先级		运算符			说明
 13		 	-、+			单目负、单目正
 12			!、~			逻辑非、按位取反或补码
 11			*、\、%			乘、除、取模
 10			+、-			加、减
 9			<<、>>			按位左移、按位右移
 8		<=、>=、<、>			小于等于、大于等于、小于、大于	
 7			==、!=			等于、不等于
 6			&				按位与
 5			^				按位异或
 4			|				按位或
 3			&&				逻辑与
 2			||				逻辑或
 1			=、+=、-=、*=、/=、%=、&=、^=、|=、<<=、>>=		赋值、运算且赋值

例： `[root@localhost sh]# echo $(($a+$b*3))`
例2：
`[root@localhost sh]# echo $(( 1&&0 ))`			//同时为1，则为1，其余为0
0
`[root@localhost sh]# echo $(( 1&&1 ))`			//当然||就相反了；
1


10.5.2 变量测试与内容替换
		
A 变量置换表格(基本不咋用)
变量置换方式			变量y没有设置		变量y为空值			变量y设置值
x=${y-新值}				x=新值			x为空				x=$y
x=${y:-新值}				x=新值			x=新值				x=$y
x=${y+新值}				x为空			x=新值				x=新值
x=${y:+新值}				x为空			x为空				x=新值
x=${y=新值}			x=新值，y=新值		x为空，y值不变		x=$y,y值不变
x=${y:=新值}			x=新值，y=新值		x=新值，y=新值		x=$y,y值不变
x=${y?新值}			新值输入到标准		x为空				x=$y
					错误输出(屏幕)
x=${y:?新值}			新值输出到标准		新值输出到标准		x=$y
					错误输出				错误输出

备注：
例：
```shell script
[root@localhost ~]# unset y
[root@localhost ~]# x=${y-345}
[root@localhost ~]# echo $x
345
[root@localhost ~]# y=12
[root@localhost ~]# x=${y-345}	
[root@localhost ~]# echo $x
12	
```
			
		
	
	
10.6 环境变量配置文件
		
10.6.1 环境变量配置文件简介
A 命令：source 配置文件			
功能：让单独配置文件重启(减少重启系统时间)		
备注：source可以简写为 .   ,注意.和文件名的空格；

B 简介：环境变量配置文件中主要是定义对系统的操作环境生效的系统默认环境变量，比如
PATH、HISTSIZE、PSI、HOSTNAME等默认环境变量

C 配置文件
```shell script
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
~/.bash_profile
~/.bashrc
```
备注：写在/etc目录下环境变量对所有用户生效；后两个在家目录中的文件只对自己用户生效；

D 配置文件调用顺序
a `/etc/profile -> /etc/profile.d/*.sh -> /etc/profile.d/lang.sh -> /etc/sysconfig/i18n`

b `/etc/profile -> ~/.bash_profile -> ~/.bashrc -> /etc/bashrc（分支到：命令提示符） -> /etc/profile.d/*.sh -> /etc/profile.d/lang.sh -> /etc/sysconfig/i18n`

备注：
/etc/profile的作用：
	USER变量
	LOGNAME变量
	MAIL变量
	PATH变量
	HOSTNAME变量
	HISTSIZE变量
	umask:
	调用/etc/profile.d/*.sh文件
~/.bash_profile的作用：
	调用了~/.bashrc文件
	在PATH变量后面加入了":$HOME/bin"这个目录
~/.bashrc的作用：
	定义默认别名
	调用/etc/bashrc
/etc/bashrc(没有登录时以下声明才起作用)
	PS1
	umask
	PATH变量
	调用/etc/profile.d/*.sh文件
	

10.6.2 环境变量配置文件作用
		
查看上节配置文件就行	
	
10.6.3 其他配置文件和登录信息

A 注销时生效的环境变量配置文件:~/.bash_logout

B 其他配置文件：~/.bash_history

C Shell登录信息
本地终端欢迎信息：/etc/issue
	转义符				作用
	\d					显示当前系统日期
	\s					显示操作系统名称
	\l					显示登录的终端号，这个比较常用
	\m					显示硬件提醒结构，如i386，i686等
	\n					显示主机名
	\o					显示域名
	\r					显示内核版本
	\t					显示当前系统时间
	\u					显示当前登录用户的序列号
例：
CentOS release 6.6 (Final)
Kernel \r on an \m	
\l
备注：上面转义符的特殊信息只对/etc/issue文件有效，且只对本地登录(登录前)有效；

D 远程终端欢迎信息：/etc/issue.net			
a C中转义符在/etc/issue.net中不起作用(只能写纯文本信息)
b 是否显示此欢迎信息，由ssh的配置文件/etc/ssh/ssh_config决定，加入"
Banner /etc/issue.net "行才能显示(记得重启SSH服务，命令：service sshd restart)
例：
步骤1：编辑 /etc/issue.net；
步骤2：编辑 /etc/ssh/ssh_config ，并写入 Banner /etc/issue.net  //先搜索到Banner然后取消隐藏；
步骤3：service sshd restart
备注：以上更改只对远程登录有效（登录前）；

E 不管远程登录还是本地登录都有效的登录后信息配置文件:/etc/motd


11 Shell编程

11.1 基础正则表达式
	
A 正则表达式与通配符
正则表达式用来在文件中匹配符合条件的字符串，正则是包含匹配。grep、awk、sed等命令可以支持正则表达式
通配符用来匹配符合条件的文件名，通配符是完全匹配。ls、find、cp这些命令不支持正则表达式，所以只能使用shell自己的通配符进行匹配；

B 基础正则表达式
元字符				作用
 *					前一个字符匹配0次或任意多次
 .					匹配除了换行符外任意一个字符
 ^					匹配行首。例如：^hello会匹配以hello开头的行
 $					匹配行尾。例如：hello&会匹配hello结尾的行
 []					匹配中括号中指定的任意一个字符，只匹配一个字符。
 [^]				匹配除中括号的字符以外的任意一个字符。
 \					转义符，取消特殊符号的含义；
 \{n\}				表示其前面的字符恰好出现n次
 \{n,\}				表示其前面的字符出现不小于n次
 \{n,m\}			表示其前面的字符至少出现n次，最多出现m次；

例：
[root@localhost ~]# grep "HOST*" /etc/profile 
[root@localhost ~]# grep "^$" /etc/profile 		//作用：匹配空白行
[root@localhost ~]# grep '10\{3\}' /etc/profile
HISTSIZE=1000
	
11.2 字符截取命令

11.2.1 cut字段提取命令
		
命令：cut [选项] 文件名
		   -f 列号			提取第几列
		   -d 分隔符			按照指定分隔符分割列(默认是水平制表符，即tab)
功能：提取相应的列；
例：[root@localhost ~]# grep "/bin/bash" /etc/passwd | cut -d ":" -f 1,3

备注：cut的局限性，下面代码就没有作用，因为df命令输出是 空格 隔断，不是制表符；
[root@localhost ~]# df -h | cut -d ' ' -f 5
		   
		
11.2.2 printf命令
		
A printf '输出类型输出格式' 输出内容
			%ns				输出字符串。n是数字，代表最少输出几个字符，字串本身
							字符数大于等于n，那么直接输出字串，如果字串字符数
							小于n，则用空格不起(下同)
			%ni				输出整数。n是数字，只代输出几个数字
			%m.nf			输出浮点数。m和n是数字，分别指代输出的整数位和小数位
输出格式：
\a			输出警告音
\b			输出退格键，也就是Backspace键
\f			清除屏幕
\n			换行								(常用)
\r			回车，也就是Enter				(常用)
\t			输出水平制表符，就是tab键			(常用)
\v			输出垂直制表符，就是tab键(没弄懂)

功能：按格式输出	
例：
`[root@localhost ~]# printf '%s %s %s\n' 12 23 34 45 56 67`	
例：
`[root@localhost tmp]# printf "%s\t%s\t%s\n" $(cat /tmp/test.txt)`
备注：printf不能接受管道符传递的数据，只能用这种；其实printf并不管原先数据采用什么符号分割，包括空格也一样；
	
备注2：在awk命令中支持printf和print命令
print：会在每个输出后自动加入换行符(Linux中默认并没有print命令，只能在awk中使用)
printf：不会自动加入换行符；

11.2.3 awk命令
命令：awk '条件1{动作1}条件2{动作2}' 文件名	
说明1：条件一般是关系表达式作为条件，如：x>10
说明2：格式化输出、流程控制语言
说明3：格式的引号中用$0表示行数据，$n,n为数字，表示n列的数据；
说明4：awk默认分隔符为水平制表符或空格；

功能：字符串分割

例：`[root@localhost ~]# df -h | grep 'sda5' | awk '{printf $5 "\n"}' | cut -d "%" -f 1`

条件：BEGIN
作用：在awk命令取值之前执行，只要配合FS
`[root@localhost ~]# df -h | grep 'sda5' | awk 'BEGIN{printf "this is begin\n"}{printf $5 "\n"}' | cut -d "%" -f 1`
备注：条件END用法一样；

动作中：	FS内置变量
作用：声明分隔符
`[root@localhost ~]# cat /etc/passwd | grep "/bin/bash" | awk 'BEGIN{FS=":"}{printf $1 "\t" $2 "\n"}'
`
		
11.2.4 sed命令

说明：sed是一种几乎包括在所有UNIX平台(包括Linux)的轻量级流编辑器。sed主要用来将数据进行选取、替换、删除、新增的命令

A 命令：sed [选项] '[动作]' 文件名			//动作一定要引号括起来
	选项：
			-n		一般sed命令会把所有的数据都输出到屏幕上，如果加入此选择，则只会
					经过sed命令处理的行输出到屏幕上
			-e		运行对输入数据应用多条sed命令编辑
			-i		用sed的修改结果直接修改读取数据的文件，而不是由屏幕输出(慎重)；
			
	动作：
			a\		追加，在当前行后增加一行或多行。增加多行时，除最后一行外，每行
					末尾需要用"\"来代替数据没有完成；
					
			c\		行替换，用c后面的字符串替换原数据行，替换多行时，除最后一行外，
					每行末尾需要用"\"来代替数据没有完成；
					
			i\		插入，在当行前插入一行或多行。插入多行时，除最后一行外，
					每行末尾需要用"\"来代替数据没有完成；
					
			d		删除指定的行
			
			p		打印指定的行
			
			s 		字串替换，用一个字符串替换另一个字符串。格式为
					"行范围s/旧字串/新字串/g" （和vim替换格式类似）
					
功能：主要用来将数据进行选取、替换、删除、新增的命令
例：
```shell script
[root@localhost ~]# df -h | sed -n '2p'					//打印第2行
[root@localhost ~]# df -h | sed -n '2,4p'				//打印2到4行
[root@localhost ~]# df -h | sed '1a this is a test '	//在第1行后插入
[root@localhost ~]# df -h | sed  "2i this is \n \
a test"
[root@localhost ~]# df -h | sed -e "2s/7.2/100/g;3s/shm/test/g"	//执行两个替换；

```


	11.3 字符处理命令
A 命令：sort [选项] 文件名
			  -f 		忽略大小写
			  -n		以数值型进行排序，默认使用字符串型进行排序
			  -r		反向排序
			  -t		指定分隔符，默认分隔符是制表符
			  -k n[,m]	按照指定的字段范围排序，从第n字段开始，m字段结束(默认到行尾
功能：对文件内容或者输出内容进行 行排序；
例：
```shell script
[root@localhost ~]# sort /etc/passwd
[root@localhost ~]# sort -r /etc/passwd
[root@localhost ~]# sort -t ":" -k 3,3 -n /etc/passwd
```

B 命令：wc [选项] 文件名
			-l			只统计行数
			-w			只统计单词数
			-m			只统计字符数
功能：统计内容信息；
例：`[root@localhost ~]# wc /etc/passwd`

	
	11.4 条件判断
	
A 判断两种格式：
a test [选项] 文件名
b [ 选项 文件名 ]

B 判断类型(即选项)
a 按照文件类型进行判断
测试选项				作用
-b 文件				判断文件是否存在，并且是否为块设备文件(是块设备文件为真)

-c 文件				判断文件是否存在，并且是否为字符设备文件(是字符设备文件为真)

-d 文件				判断文件是否存在，并且是否为目录文件(是目录文件为真)  （常用）

-e 文件				判断文件是否存在，(存在为真)						   （常用）

-f 文件				判断文件是否存在，并且是否为普通文件(是普通文件为真)   （常用）

-L 文件		判断文件是否存在，并且是否为符号链接文件(是软链接文件为真)   （常用）

-p 文件				判断文件是否存在，并且是否为管道文件(是管道文件为真)

-s 文件				判断文件是否存在，并且是否为空(非空为真)

-S 文件				判断文件是否存在，并且是否为套接字文件(是套接字文件为真)
例：
```shell script
[root@localhost ~]# test -d /tmp && echo yes || echo no
[root@localhost ~]# [ -e /tmp ] && echo yes || echo no
```
特别注意：[ -e /tmp ]中括号边界[]与内容之间必须有空格；

b 按照文件权限进行判断
测试选项				作用
-r 文件		常用		判断该文件是否存在，并且是否该文件拥有读权限(有读选项就为真，
					不区分哪个有，下同)			
					
-w 文件		常用		判断该文件是否存在，并且是否该文件拥有写权限(有写权限为真)

-x 文件		常用		判断该文件是否存在，并且是否该文件拥有执行权限(有执行权限为真)

-u 文件				判断该文件是否存在，并且是否该文件拥有SUID权限(有SUID权限为真)

-g 文件				判断该文件是否存在，并且是否该文件拥有SGID权限(有SGID权限为真)

-k 文件				判断该文件是否存在，并且是否该文件拥有SBit权限(有SBit权限为真)

例：`[root@localhost ~]# [ -w /tmp  ] && echo yes || echo no`


c 两个文件之间进行比较
测试选项				作用
文件1 -nt 文件2		判断文件1的修改时间是否比文件2的新(如果新则为真)

文件1 -ot 文件2		判断文件1的修改时间是否比文件2的旧(如果旧则为真)

文件1 -ef 文件2		判断文件1是否和文件2的Inode号一致，可以理解为两个文件是否为同一
					个文件，这个经常用于判断硬链接
					
例：`[root@localhost tmp]# [ hehe -nt test.txt  ] && echo yes || echo no`

d 两个整数比较
测试选项				作用
整数1 -eq 整数2		判断整数1是否和整数2相等(相等为真)

整数1 -ne 整数2		判断整数1是否和整数2不相等(不相等为真)

整数1 -gt 整数2		判断整数1是否大于整数2(大于为真)

整数1 -lt 整数2		判断整数1是否小于整数2(小于为真)

整数1 -ge 整数2		判断整数1是否大于等于整数2(大于等于为真)

整数1 -le 整数2		判断整数1是否小于等于整数2(小于等于为真)
例：
```shell script
[root@localhost tmp]# [ 22 -gt 11  ] && echo yes || echo no
[root@localhost tmp]# [ 22 -ge 11  ] && echo yes || echo no
```


e 字符串
测试选项				作用
-z 字符串			判断字符是否为空(为空返回真)，基本相当于查看变量为空

-n 字符串			判断字符是否为非空(非空返回真)，相当于查看变量是否为非空

字串1==字串2			判断字符串1是否和字符串2相等(相等返回真)

字串1 != 字串2		判断字符串1是否和字符串2不相等(不相等返回真)
例：
```shell script
[root@localhost tmp]# aa=123
[root@localhost tmp]# [ aa==123  ] && echo yes || echo no
yes
[root@localhost tmp]# [ -z $aa  ] && echo yes || echo no       
no

```
f 多重条件判断
条件选项				作用
判断1 -a 判断2		逻辑与，两个判断都成立，则返回结果为真

判断1 -o 判断2		逻辑或，只要一个判断成立，则返回结果为真
 
 ! 判断				逻辑非，使原始的判断式取反，特别注意!后有空格
 
例：`[root@localhost tmp]# [ 22 -gt 20  -a  -d /tmp  ] && echo yes || echo no `



	
11.5 流程控制
		
11.5.1 if语句
A 单分支if条件语句
```shell script
if [ 条件判断式 ];then
	程序
fi
或者
if [ 条件判断式 ]
	then
		程序
fi
```
	
例：判断分区的使用率
```shell script
[root@localhost sh]# vim rate.sh
#!/bin/bash
#author:neo 
#e015年 05月 30日 星期六 11:12:40 CST
rate=$(df -h | grep /dev/sda5 | awk '{print $5}' | cut -d '%' -f 1)
if [ $rate -ge 30  ];then			#注意if后面必须有空格
#真实项目中是发邮件
    echo "存储不足30%"
fi
```

B 双分支if条件语句
if
例：类似可以备份mysql数据；
```shell script
#!/bin/bash
#2015年 05月 30日 星期六 12:01:28 CST
#测试if语句，进行默认数据备份，如mysql备份

date=$(date +%y%m%d)
size=$(du -sh /etc)

if [ -d /tmp/dbbak  ]
         then
                 echo "Date:$date" > /tmp/dbbak/db.txt
                 echo "Size:$size" >> /tmp/dbbak/db.txt		
                 cd /tmp/dbbak
				#同时压缩两个文件(重定向不止有echo，任何有输出信息的都可以)
                 tar -zcf etc_$date.tar.gz /etc db.txt &>/dev/null	
                 rm -r /tmp/dbbak/db.txt
         else
                 mkdir /tmp/dbbak
                 echo "Date:$date" > /tmp/dbbak/db.txt
                 echo "Size:$size" >> /tmp/dbbak/db.txt
                 cd /tmp/dbbak
                 tar -zcf etc_$date.tar.gz /etc db.txt &>/dev/null
                 rm -r /tmp/dbbak/db.txt
fi
```

例2：判断Apache是否启动
```shell script
#!/bin/bash
#2015年 05月 30日 星期六 12:48:53 CST
 
#判断Apache是否启动
date=$(date +%y%m%d)
# nmap命令默认没有安装，安装：yum -y install nmap
port=$(nmap -sT 192.168.1.250 | grep tcp | grep http | awk '{print $2}')
if [ "$port" == 'open'  ]
         then
                 echo "$date httpd is ok !" >> /tmp/httpd.stat.log
         else
				#这儿是源代码安装的启动；
                 /usr/local/apache2/bin/apachectl start &>/dev/null
                 echo "$(date) restart httpd!" >> /tmp/http.err.log
fi
 
```
恶补：可以通过 ntpdate [选项] server_id 手动调整服务器时间（调整不是设置）
备注：本命令默认可能没有安装，可以：[root@localhost tmp]# yum -y install ntp
例：(同步时间)
`[root@localhost tmp]# ntpdate 202.112.10.36`

C 多分支if条件语句
```shell script
if [ 条件判断式1 ]
	then
		条件1成立时，执行程序1
elif [ 条件判断式2 ]
	then
		条件2成立时，执行程序2
...
else
	当所有条件都不成立时，最后执行程序
fi
```
例：
```shell script
#!/bin/bash
#2015年 06月 06日 星期六 19:59:59 CST
#接收用户输入值，赋值到变量file
read -p "请输入一个文件：" file
#判断文件是否存在
if [ ! -e $file  ]
        then 
                echo "请输入一个文件"
                exit 1
#判断是否是文件
elif [ -f $file  ]
        then
                echo "输入的是文件"     
#判断是否是路径
elif [ -d $file  ]
        then 
                echo "输入的是路径"
else
        echo "输入的既不是文件，也不是路径"
fi
```

		
11.5.2 case语句
A 语法：
case $变量名 in
	"值1")
		如果变量的值等于值1，则执行程序1
		;;
	"值2")
		如果变量的值等于值2，则执行程序2
		;;	
	...
	*)
		如果变量的值都不是以上的值，则执行此程序
		;;
esac
		
例：
```shell script
#!/bin/bash
#2015年 06月 06日 星期六 20:31:22 CST
#输入提示
read -p "请输入您的选择：1 代表 苍井空 \n 2 代表 愛川美里菜 \n 3 代表 濑亚美莉 4 代表 长泽锌 " input
#进行输入判断
case "$input" in
        "1")
                echo "您已经选择苍井空，注意查收!"
                ;;  
        "2")
                echo "您已经选择愛川美里菜，注意查收!"
                ;;  
        "3")
                echo "您已经选择濑亚美莉，注意查收!"
                ;;  
        "4")
                echo "您已经选择长泽锌，注意查收!"
                ;;  
        *)  
                echo "木鱼脑袋，这都不选！"     
esac
	
```
	
11.5.3 for循环
	
A 语法1：

```
for 变量 in 值1 值2 值3...
	do
		程序
	done
```
	
备注：值有多少个，循环多少次，每次循环前，先会把值赋给变量；
例：(批量解压文件)
```shell script
#!/bin/bash
#批量解压缩脚本,用于搭建lamp环境
#把所有lamp环境的压缩包放在/lamp内
cd /lamp
ls *.tar.gz > ls.log
for i in $(cat ls.log)
	do
		tar -zxf $i &> /dev/null
	done
rm -rf /lamp/ls.log

B 语法2：
for ((初始值;循环控制条件;变量编号))
	do
		程序
	done
```


例：
```shell script
#!/bin/bash
#2015年 06月 06日 星期六 21:53:39 CST
#批量添加指定用户
#提示
read -t 30 -p "请输入用户名：" username
read -t 30 -p "请输入用户数量：" usernum
read -t 30 -p "请输入用户密码：" password
if [ ! -z "$username" -a ! -z "$usernum" -a ! -z "$password"  ]
        then
                y= echo "$usernum" | sed "s/^[0-9]*$//g"
                if [ -z "$y"  ]
                        then
                                for ((i=1;i<=$usernum;i=i+1))
                                        do  
                                                /usr/sbin/useradd $username$i &>/dev/null
                                                echo password | /usr/sbin/passwd --stdin $name$i &>/dev/null
                                        done
                fi  

fi	
```
		
11.5.4 while循环和until循环
 
A while语法：
```shell script
while [条件判断式]	
	do
		程序
	done
```
例：
```shell script
[root@localhost sh]# vim while.sh
#!/bin/bash
#2015年 06月 06日 星期六 22:45:42 CST
#while例子
i=1
sum=0
while [ $i -le 100 ]
        do
                sum=$(($sum + $i))
                i=$(($i+1))
        done
echo $sum
```

B until语法：
until [条件判断式]	
	do
		程序
	done
备注：添加判断式不成立，则循环，一旦成立则终止；
例：
```shell script
[root@localhost sh]# vim while.sh
#!/bin/bash
#2015年 06月 06日 星期六 22:45:42 CST
#while例子
i=1
sum=0
until [ $i -le 100 ]
        do
                sum=$(($sum + $i))
                i=$(($i+1))
        done
echo $sum

```

12 服务分类
	
12.1 服务简介与分类

A Linux服务：
a RPM包默认安装的服务：a1 独立的服务；b1 基于xinetd服务(xinitd本身是独立的)
b 源码包安装的服务

B 启动与自启动
服务启动：就是在当前系统中让服务运行，并提供功能；
服务自启动：是指让服务在开机或重启之后，随着系统的启动而自动启动的服务；

C 查询已经安装的服务
a RPM包安装的服务
命令：chkconfig --list
功能：查看所有RPM包安装的服务，查看服务是否是自启动，但无法判断是否现在在运行；
备注：查看现在在运行的服务命令：ps aux 

b 查询源码包安装的服务
一般直接到/usr/local/目录下查看，如果相应目录存在说明安装了该服务；
备注：RPM包和源码包的区别就是安装目录不同；

	
12.2 RPM包安装服务的管理
	
12.2.1 独立服务管理

A RPM包的安装目录：
/etc/init.d 		启动脚本位置(独立服务的启动脚本,/etc/rc.d/init.d内容是一样的)
/etc/sysconfig/		初始化环境配置文件位置
/etc/				配置文件位置
/etc/xinetd.conf	xinetd配置文件
/etc/xinetd.d/		基于xinetd服务的启动脚本
/var/lib/			服务产生的数据放在这里
/var/log/			日志

B 独立服务的启动
a /etc/init.d/独立服务名 start|stop|status|restart|
备注：推荐这个方法；/etc/init.d 是文件 /etc/rc.d/init.d 的软链接


b service 独立服务名 start|stop|status|restart|
备注：service是红帽子才有；
备注2：命令：service --status-all  功能：查询所有RPM包安装的服务的状态；

C 独立服务的自启动
a 命令：`chkconfig [--level 运行级别] [独立服务名] [on|off]`
功能：改变独立服务名的自启动
例：
```shell script
# chkconfig --level 2345 httpd on			//自启动Apache
# chkconfig httpd off		//关闭Apache的自启动，省略--level 2345,其实默认也是它
```
b 修改/etc/rc.d/rc.local文件，该文件会在系统启动后，用户登录前，执行该文件，也就是
说该文件中的所有脚本会被执行；所以可以添加内容：
/etc/rc.d/init.d/httpd start
备注：这个方法推荐(并且也适合源码包安装服务的自启动)
/usr/local/apache2/bin/apachectl start 				#源码包的apaceh启动
/usr/local/apache2/bin/apachectl restart 			#源码包的apaceh重启
/usr/local/apache2/bin/apachectl stop 	 			#源码包的apaceh停止

c 使用ntsysv命令管理自启动(可以管理独立服务和xinetd服务，红帽子专有命令)
备注：前面有*，表示自启动

如何增加一个服务：
1.服务脚本必须存放在/etc/init.d/目录下；
2.chkconfig --add servicename
    在chkconfig工具服务列表中增加此服务，此时服务会被在/etc/rc.d/rcN.d中赋予K/S入口了；
3.chkconfig --level 35 mysqld on
    修改服务的默认启动等级。
		
		12.2.2 基于xinetd服务的管理(了解)

A 安装xinetd与telnet(telnet基本不用),这两个默认是没有安装的
例：
```shell script
[root@localhost ~]# yum -y install xinetd
[root@localhost ~]# yum -y install telnet-server	
```

B xinetd服务的启动
a 更改/etc/xinetd.d/telnet
`#vim /etc/xinetd.d/telnet`
server telnet			←服务的名称为telnet
{
	flags 		=REUSE	←标志位REUSE，设定TCP/IPsocket可重用
	socket_type	=stream ←使用TCP协议数据包
	wait		=no		←允许多个连接同时连接
	user		=root	←启动服务的用户为root
	server 		=/usr/sbin/in.telnetd		←服务的启动程序
	log_on_failure	+=USERID				←登录失败后，记录用户ID
	#只用更改这儿
	disable		=no							←服务启动(no为启动，yes为不启动)

}
b 重启服务：service xinetd restart 	
	
C xinetd服务的自启动
方式一：chkconfig telnet on

方式二：ntsysv

备注：xinetd的启动和自启动是相同的；即启动了就自启动，自启动就启动了；
	
	
12.3 源码包安装服务的管理
	
A 源码包安装服务的启动：使用绝对路径，调用启动脚本来启动。不同的源码包的启动脚本不同
可以查看源码包的安装说明(进入源码包，vim INSTALL 查看)，查看启动脚本的方法：
部分内容：
```shell script
    $ ./configure --prefix=PREFIX
    $ make
    $ make install
    $ PREFIX/bin/apachectl start
```
	
例：`[root@localhost ]# /usr/local/apache2/bin/apachectl start|stop`

B 源码包安装服务的自启动：在/etc/rc.d/rc.local文件中加入：
`/usr/local/apache2/bin/apachectl start`


C 让源码包服务被服务管理命令service识别(不推荐)
原理：利用软链接
例：让源码包Apache服务能被service命令管理启动
`ln -s /usr/local/apache2/bin/apachectl /etc/init.d/apache`

D 让源码包的Apache服务能被chkconfig与ntsysv命令管理自启动(不推荐)
a 需要步骤C，创建软链接

b 更改/etc/init.d/apache，添加内容：
vim /etc/init.d/apache
```shell script
#chkconfig:35 86 76
#指定httpd脚本可以被chkconfig命令管理。格式是：chkconfig:运行级别 启动顺序 关闭顺序
#description:source package apache
#内容随意
```

c chkconfig --add apache			//添加后就可以管理了，--del可以删除；


备注：可以把上面添加内容的中文删去，但是添加内容必须是注释，也就是有#
备注：启动顺序和关闭顺序，在目录/etc/rc.d 中：
rc0.d  rc1.d  rc2.d  rc3.d  rc4.d  rc5.d  rc6.d		
//数字代表着运行级别，目录里是运行服务的启动和关闭属性，S开头为启动，紧接着数字代表第几个启动，K代表关闭顺序，紧接着数字代表第几个关闭；


	
12.4 服务管理总结

总结：
所有服务(除xinetd服务)的启动：绝对路径 start
所有服务(除xinetd服务)的自启动：在/etc/rc.d/rc.local文件中添加：绝对路径 start

	
13 Linux系统管理
	
13.1 进程管理
		
13.1.1 进程查看
A 进程：正在执行的一个程序或命令，每一个进程都是一个运行的实体，都有自己的地址空间，
并占用一定的资源

B 进程管理的作用
a 判断服务器健康状态
b 查看系统中的所有进程
c 杀死进程（先正常关闭，实在不行，才杀死）		

C 命令1：ps aux
功能：查看系统中所有进程，这是使用BSD操作系统格式；
  命令2：ps -le
功能：查看系统中所有进程，这是使用Linux标准命令格式；  
备注：两个都可以在Linux系统中运行；
运行结果分析：
USER		该进程由哪个用户产生的
PID			进程ID号
%CPU		该进程占用CPU资源的百分比，占用越高，进程越耗费资源
%MEM		该进程占用物理内存的百分比，占用越高，进程越耗费资源
VSZ			该进程占用虚拟内存的大小，单位为KB
RSS			该进程占用实际物理内存的大小，单位为KB
TTY			该进程是在哪个终端中运行的，其中tty1-tty7代表本地控制台终端，tty1-tty6
			是本地的字符界面终端，tty7是图形终端。pts/0-255代表虚拟终端(即远程连接)
STAT		进程状态。常见的状态有：R：运行、S：睡眠、T：停止状态、s:包含子进程、
			+：位于后台
START		该进程启动时间
TIME		该进程占用CPU运算时间，注意不是系统时间
COMMAND		产生此进程的命令名

D 命令：top [选项]
			-d 秒数		指定top命令每隔几秒更新。默认3秒
说明：在top命令的交互模式当中可以执行的命令：
	?或h		显示交互模式的帮助
	P			以CPU使用率排序，默认就是此项
	M			以内存的使用率排序
	N			以PID排序
	q			退出top
备注：显示Linux系统状态信息，以判断服务器健康状态
结果分析：
第一行信息：
内容					说明
04:24:36			系统当前时间
up 1 day, 10:53		系统已经连续运行的时间
2 users				当前登录了两个用户

load average: 0.00, 0.00, 0.00		系统在之前1分钟、5分钟、15分钟的平均负载。一般
**重点观察**	认为小于计算机核数(如果4核计算机就小于4)，负载较小，反之，则大

第二行信息：
Tasks:  96 total	系统中总进程数
1 running			正在运行的进程数
95 sleeping			睡眠的进程
0 stopped			正在停止的进程
0 zombie 			僵尸进程，如果不是0，需要手工检查僵尸进程(猜测：手工关闭)

第三行信息：
Cpu(s):  0.3%us		用户模式占用的CPU百分比
0.0%sy				系统模式占用的CPU百分比
0.0%ni				改变过优先级的用户进程占用的CPU百分比
99.7%id				空闲CPU的CPU百分比重点观察
0.0%wa				等待输入/输出的进程的占用CPU百分比
0.0%hi				硬中断请求服务占用CPU的百分比
0.0%si				软中断请求服务占用CPU的百分比
0.0%st				st (Steal time)虚拟时间百分比。就是当有虚拟机时，虚拟CPU等待
					实际CPU的时间百分比
第四行信息：
Mem:   1030492k total	物理内存的总量，单位KB
633804k used			已经使用的物理内存数量
396688k free			空闲的物理内存数量重点观察
71060k buffers 			作为缓冲的内存数量

第五行信息：
Swap:  2058072k total	交换分区(虚拟内存)的总大小
0k used					已经使用的交换分区大小
2058072k free			空闲交换分区的大小
465100k cached			作为缓存的交换分区大小
					


13.1.2 终止进程

A 命令：kill -l				//这是是小写L
功能：查看可用的进程信号
常用信号：
信号代号		信号名称			说明
	1		SIGHUP			该信号让进程立即关闭，然后重新读取配置文件之后重启
	9		SIGKILL			用来立即结束程序的运行，表示强制终止
	15		SIGTERM			正常结束进程信号，kill命令的默认信号
例:
`[root@localhost ~]# pstree -p`				//查看PID
`[root@localhost ~]# kill -9 8260`			//强制终止进程8260

B 命令：killall [选项] [信号] 进程名
				-i		交互式，询问是否要杀死某个进程
				-I		忽略进程名的大小写
功能：按照进程名杀死进程
例：[root@localhost ~]# killall httpd

C 命令：pkill [选项] [信号] 进程名
				-t 终端号		按照终端号剔除用户				
功能：按照进程名终止进程或按照终端号剔除用户
例：
```shell script
[root@localhost ~]# w							//显示登录终端
[root@localhost ~]# pkill -9 -t tty1			//剔除本地登录用户tty1
```


13.2 工作管理
A 把进程放入后台
方式一：命令	&
说明：这种方式，进程在后台还在执行
例：tar -zcf etc.tar.gz /etc &

方式二：在命令执行过程中，按ctrl+z快捷键，进程放入后台，但进程是暂停的；
例：`[root@localhost ~]# top`			//然后按ctrl+z

B 查看后台工作
命令：jobs [-l]			
			-l		显示工作的PID
功能：查看后台工作
备注："+"代表最近一个放入后台的工作，也是恢复工作时默认的工作，"-"号代表倒数第二个放入后台的工作 

C 将后台暂停的工作恢复到前台执行
命令：fg %工作号
		 %工作号			%号可以省略，但是注意工作号和PID的区别
例：`[root@localhost ~]# fg %1`

D 把后台暂停的工作恢复到后台执行
命令：bg %工作号
注意：后台恢复执行的命令，是不能和前台有交互的，否则不能恢复到后台执行；
例：bg %1

	
13.3 系统资源查看
	
A 命令：vmstat [刷新延时 刷新次数]	
功能：监控系统资源
例：	[root@localhost tmp]# vmstat 1 3
	
B 命令：dmesg
功能：显示开机时内核检测信息
例：`[root@localhost tmp]# dmesg | grep CPU`		//显示CPU信息
例：`[root@localhost tmp]# dmesg | grep eth0`		//显示网卡信息

C 命令：free [选项]
			 -b			以字节为单位显示
			 -k			以KB为单位显示，默认就是以KB为单位显示
			 -m			以MB为单位显示
			 -g			以GB为单位显示
功能：查看内存使用状态
例：`[root@localhost tmp]# free -m`

D 命令：cat /proc/cpuinfo 
功能：查看CPU信息
例：`[root@localhost ~]# cat /proc/cpuinfo`

E 命令：uptime
功能：显示系统的启动时间和平均负载，也就是top命令的第一行。w命令也可以看到这个数据
备注：就是top第一行

F 命令：uname [选项]
				-a		查看系统所有相关信息
				-r		查看内核版本
				-s		查看内核名称
功能：查看系统和内核相关信息
例：[root@localhost ~]# uname -a


G 命令：file /bin/ls
功能：判断当前系统位数
例：`[root@localhost ~]# file /bin/ls`

H 命令：lsb_release -a
功能：查询当前的Linux发行版本
`[root@localhost ~]# lsb_release -a
`
I 命令：lsof [选项]
			  -c 字符串			只列出以字符串开头的进程打开的文件
			  -u 用户名			只列出某个用户的进程打开的文件
			  -p pid			列出某个PID进程打开的文件
功能：列出进程打开或使用的文件信息			  
例：`[root@localhost ~]# lsof -c http`
例：`[root@localhost ~]# lsof -u root`
例：[root@localhost ~]# lsof -p 14047` 		

	
	13.4 系统定时任务
	
A crond服务管理与访问控制
```shell script
[root@localhost ~]# service crond start		//启动crond服务

[root@localhost ~]# chkconfig crond on		//设置crond服务为自启动，默认为自启动

```
B 命令：crontab [选项]
				-e		编辑crontab任务
				-l		查询crontab任务
				-r		删除当前用户所有的crontab任务
功能：用户的crontab设置
说明：crontab -e			#进入crontab编辑界面，会打开vim编辑你的工作。
内容格式：
`* * * * *` 命令
项目				含义						范围
第一个 *		一个小时当中的第几分钟		0-59
第二个 *		一天当中的第几小时			0-23
第三个 *		一个月当中的第几天			1-31
第四个 *		一年当中的第几月				1-12
第五个 *		一周当中的星期几				0-7(0和7都代表星期日)
进一步说明：
特殊符号			含义
* 			代表任何时间，比如第一个"*"就代表一个小时中每分钟都执行一次；
,			代表不连续的时间。比如"0 8,12,16 * * * 命令"，代表每天的8:00,12:00
			16:00都执行一次
-			代表连续的时间范围，比如"0 5 * * 1-6 命令"代表周一到周六凌晨5:00
			执行命令
*/10		代表每隔多久执行一次，如"*/10 * * * * 命令",代表每个10分钟就执行一遍
例：
45 22 * * * 命令			在22:45执行命令
0 17 * * 1 命令			每周星期一的17:00执行
0 5 1,15 * * 命令		每月1号和15号的5:00执行命令
40 4 * * 1-5 命令		每周一到周五4:40执行命令
*/10 4 * * * 命令		每天凌晨4点，每隔10分钟执行一次命令
0 0 1,15 * 1 命令		每月1号和15号，和每周星期1的0:00都执行一次命令，注意：最好
						星期几不要和几号同时出现，容易造成混乱；
						
例：
```shell script
[root@localhost tmp]# crontab -e
 * * * * * echo 123 >> /tmp/test.txt
* * * * * /sh/hello.sh
```
备注：hello.sh内容：
```shell script
#!/bin/bash
#writer:neo 
#2015年 05月 29日 星期五 15:28:33 CST
date=$(date +\%y\%m\%d)			# %在crontab中特殊含义，即使是在任务文件中也要加
echo "hello world $date"		# 转移符\
```

备注：在定时任务中单纯的echo并不会输出到屏幕，所以需要重定向以检测定时任务是否执行，如：
``` * * * * * echo hehe >> /tmp/test.txt ```
备注：crontab中并没有相关环境变量，解决办法就是在脚本中打日志,另外 *默认将所有的命令采用全路径的* 方式.
例：
``` 
1 7 * * * php /mnt/www/oa/Application/Common/curl/createRecord.php    #不会执行，因为找不到p hp 执行命令
1 7 * * * /alidata/server/php-5.4.27/bin/php /mnt/www/oa/Application/Common/curl/createRecord.php    #会执行
```




14 日志管理

14.1 日志管理简介
A 在CentOS 6.x中日志服务已经由rsyslogd取代了原先的syslogd服务；

B 确定服务启动
`# ps aux | grep rsyslogd`		#查看服务是否启动

`# chkconfig --list | grep rsylog` #查看服务是否自启动	

C 常见的日志作用
日志文件				作用
/var/log/cron		记录了系统定时任务相关的日志	
/var/log/cups/		记录打印信息的日志
/var/log/dmesg		记录了系统在开机时内核自检的信息。	也可以使用dmsg命令直接查看
					内核自检信息
/var/log/btmp		记录错误登录的日志。这个文件是二进制文件，不能直接vim查看，
					而是通过lastb命令查看，命令如下：# lastb	
/var/log/lastlog	记录系统中所有用户最后一次登录时间的日志。这个文件也是二进制
					文件，不能直接vim，而要使用lastlog命令查看	
/var/log/mailog		记录邮件信息	
/var/log/messages	记录系统重要信息的日志。这个日志文件会记录Linux系统的
					绝大多数重要信息，如果系统出现问题时，首先要检测这个文件	
/var/log/secure		记录验证和授权方面信息，只要涉及账户和密码的程序都会记录。
					比如说系统登录，ssh的登录，su切换用户，sudo授权，
					甚至添加用户和修改用户密码都会记录在这个日志文件中	
/var/log/wtmp		永久记录所有用户登录、注销信息，同时记录系统的启动、重启、
				关机事件。同样这个文件也是一个二进制文件，不能直接用vim，而是
					需要用last命令查看
/var/log/utmp		记录当前已经登录的用户信息，这个文件会随着用户的登录和注销
				而不断变化，只记录当前登录用户的信息，同样这个文件
					不能直接vim，而是要使用w、who、users等命令来查询	
	
备注：除系统默认日志外，通过RPM包方式安装的系统服务也会默认把日志放在/var/log/目录下(源码包安装的不会)	，不过这些日志不是由rsyslogd服务来记录和管理的，而是各个服务使用
自己的日志管理文档来管理
	
14.2 rsyslogd日志服务
	
A 日志文件格式
	a 事件产生时间	
	b 发生事件的服务器主机名
	c 产生事件的服务名或程序名
	d 事件的具体信息

B /etc/rsyslog.conf配置文件
部分内容解析：
authpriv.*                                              /var/log/secure
服务名称[连接符号]日志等级								日志记录位置
该内容就是：认证相关服务.所有日志等级			记录在/var/log/secure日志中

服务名称
服务名称				说明
auth				安全和认证相关信息(不推荐使用authpriv替代)
authpriv			安全和认证相关信息(私有的)
cron				系统定时任务cront和at产生的日志
daemon				和各个守护进程相关的日志
ftp					ftp守护进程产生的日志	
kern				内核产生的日志(不是用户进程产生的)
local0-local7		为本地使用预留的服务
lpr					打印产生的日志
mail				邮件收发信息
news				与新闻服务器相关的日志
syslog				有syslogd服务产生的日志(虽然服务名已经改为rsyslogd)
user				用户等级类别的日志信息
uucp				uucp子系统的日志信息，uucp是早期的Linux系统进行数据传递的协议
					，后来也常在新闻组服务中
					
连接符号可以识别为：
"*"代表所有日志等级，比如："authpriv.*"代表authpriv认证信息服务产生的日志，所有的日志等级都记录
"."代表只要比后面等级高的(包含该等级)日志都记录下来。比如："cron.info"代表cron服务产生的日志，只要日志等级大于等于info级别，就记录
".="代表只记录所需等级的日志，其他等级都不记录。比如："*.=emerg"代表人和日志服务产生的日志，只要等级是emerg等级就记录。这种很少见，了解就好
".!"代表不等于，也就是除了该等级外的日志都记录

日志等级				说明
debug				一般的调试信息说明
info				基本的通知信息
notice				普通信息，但是有一定的重要性
warning				警告信息，但是还不会影响到服务或系统的运行
err					错误信息，一般达到err等级的信息可以影响到服务或系统的运行了
crit				临界状况信息，比err等级还要严重
alert				警告状态信息，比crit还要严重，必须立即采取行动
emerg				疼痛等级信息，系统已经无法使用了

C 日志记录的位置
a 日志文件的绝对路径，如：/val/log/secure
b 系统设备文件，如/dev/lp0
c 转发给远程主机，如"@192.168.1.127"				
d 发送给在线用户，如"root"
e 忽略或丢弃日志，如"~"


14.3 日志轮替(日志的分割和更替后删除)

A 日志文件命名规则
a 如果配置文件(/etc/logrotate.conf)中拥有“dateext”参数，那么日志会用日期作为日志
文件的后缀，例如："secure-20150608"。这样的话日志文件名不会重叠，所以也就不需要日志
文件改名，只需要保存指定的日志个数，删除多余的日志文件即可；(采用)

b 如果配置文件(/etc/logrotate.conf)中没有"dateext"参数，那么日志文件就需要进行改名
了，当第一次进行日志轮替时，当前的"secure"日志会自动改名为"secure.1"，然后新建"secure"日志，用来保存新的日志。当第二次进行轮替时，当前的"secure.1"会自动改名为"secure.2",当前的"secure"日志会自动改名为"secure.1"，然后再新建"secure"日志，保存新的日志，一次类推；

B logrorate配置文件(/etc/logrorate.conf)
参数				参数说明
daily			日志的轮替周期是每天
weekly			日志的轮替周期是每周
monthly			日志的轮替周期是每月
rotate 数字		保留的日志文件个数。0指没有备份
compress		日志轮替时，旧的日志进行压缩
create mode owner group	建立新日志，同时指定新日志的权限与所有者和所属组，如：
				create 0600 root utmp
mail address	当日志轮替时，输出内容通过邮件发送到指定的邮件地址。如：
				mail 445354187@qq.com
missingok		如果日志不存在，则忽略该日志的警告信息
notifempty		如果日志为空文件，则不进行日志轮替
minsize 大小		日志轮替最小值，也就是日志一定要达到这个最小值才会轮替，否则就算时间
				达到也不轮替
size 大小		日志只有大于指定大小才进行日志轮替，而不是按照时间轮替，如：
				size 100k							
dateext			使用日期作为日志轮替文件的后缀。如secure-20150608

C 把源码包安装的Apache日志加入轮替(PRM包安装的服务自动就有轮替)

`[root@localhost logs]# vim /etc/logrotate.conf`

#Apache的access_log轮替参数
```shell script
/usr/local/apache2/logs/access_log{
        daily
        rotate 30
        compress
        mail 445354187@qq.com		#好像没作用
}
```
#Apache的error_log
```
/usr/local/apache2/logs/error_log{
        daily
        rotate 30
        compress
        mail 445354187@qq.com
}
```

D 命令：logrotate [选项] 文件配置名
说明：如果此命令没有选项，则会按照配置文件中的条件进行日志轮替
					-v		显示日志轮替过程。加入-v选项，会显示日志的轮替的过程
					-f		强制进行日志轮替。不管日志轮替的条件是否已经符合，强制
							配置文件中所有的日志进行轮替
功能：实现日志轮替相关功能							
例：
```shell script
[root@localhost log]# logrotate -f /etc/logrotate.conf
[root@localhost log]# logrotate -v /etc/logrotate.conf
```
				

				
15 启动管理

15.1 CentOS 6.x 启动管理
	
15.1.1 系统运行级别		
A 运行级别
运行级别			含义
	0			关机
	1			单用户模式，可以想象为windows的安全模式，主要用于系统修复
	2 			不完全命令行模式，不含NFS服务
	3			完全的命令行模式，就是标准的字符界面
	4 			系统保留
	5			图形模式
	6 			重启动
	
B 运行级别命令
`[root@localhost ~]# runlevel`		#查看运行级别

`[root@localhost ~]# init 运行级别`	#改变运行级别

C 系统默认运行级别
`[root@localhost ~]# vim /etc/inittab`	#开机后直接进入哪个运行级别	
id:3:initdefault:
		
15.1.2 系统启动过程(了解)

15.2 启动引导程序grub
	
15.2.1 Grub配置文件
A Grub中分区表示
硬盘					分区				Linux中设备文件名		Grub中设备文件名	

第一块SCSI硬盘	第一个主分区				/dev/sda1				hd(0,0)
				第二个主分区				/dev/sda2				hd(0,1)
				扩展分区					/dev/sda3				hd(0,2)
				第一个逻辑分区			/dev/sda5				hd(0,4)

第二块SCSI硬盘	第一个主分区				/dev/sdb1				hd(1,0)
				第二个主分区				/dev/sdb2				hd(1,1)
				扩展分区					/dev/sdb3				hd(1,2)
				第一个逻辑				/dev/sdb5				hd(1,4)	
				
B Grub配置文件
`vi /boot/grub/grub.conf(或者软链接 /etc/grub.conf)`

```shell script
default=0 			#默认启动第一个系统
timeout=5 			#等待时间，默认是5秒，选择系统的时间
splashimage=(hd0,0)/grub/splash.xpm.gz
					#这里是指定grub启动时的背景图像文件的保存位置的
hiddenmenu 			#隐藏菜单				
#多一个系统，就会多下面四行
title CentOS (2.6.32-279.el6.i686) title	#就是标题的意思
root (hd0,0) 								#是指启动程序的保存分区
kernel /vmlinuz-2.6.32-279.el6.i686 ro root=UUID=b9a7a1a8-767f-4a87-8a2b-a535edb362c9 rd_NO_LUKS KEYBOARDTYPE=pc KEYTABLE=us rd_NO_MD crashkernel=auto LANG=zh_CN.UTF-8 rd_NO_LVM rd_NO_DM rhgb quiet 									#定义内核加载时的选项
initrd /initramfs-2.6.32-279.el6.i686.img
					#指定了initramfs内存文件系统镜像文件的所在位置
					
```

				
15.2.2 Grub加密与字符界面分辨率调整

A grub加密 
a [root@localhost ~]# grub-md5-crypt 		#生成加密字符串，输入该命令后再输入密码，会产生加密字符串
b 将生成的加密字符串
c vim /etc/grub.conf
default=0 
timeout=5 
password --md5 $1$zCQEH$H5Xz7/7DL1mcHCaLhXgXF1		#注意插入位置，原本没有这一行
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu
...

B 纯字符界面分辨率调整
a [root@localhost ~]# grep "CONFIG_FRAMEBUFFER_CONSOLE" /boot/config-2.6.32-504.el6.i686		#查询内核是否支持分辨率修改
结果有：CONFIG_FRAMEBUFFER_CONSOLE=y		#说明支持

b vim /etc/grub.conf
kernel /vmlinuz-2.6.32-504.el6.i686 ro root=UUID=fac6b26e-b796-45bf-b2a6-ce8a2b41a04e rd_NO_LUKS  KEYBOARDTYPE=pc
     KEYTABLE=us rd_NO_MD crashkernel=auto LANG=zh_CN.UTF-8 rd_NO_LVM rd_NO_DM rhgb quiet vga=791
initrd /initramfs-2.6.32-504.el6.i686.img
注意：更改的是在quiet后面添加了vga=791

色深		640*480		800*600		1024*768		1280*1024
8位		769			771			773				775
15位	784			787			790				793
16位	785			788			791				794
32位	786			789			792				795
备注：有时可能不接受十进制，那么把上面数字转化为16进制

15.3 系统修复模式

A 单用户模式常见的错误修复
a 遗忘root密码
步骤：
步骤一：系统重启，在倒数5秒界面，按任意键(方向键)
步骤二：如果grub设置了密码，那么就按下p，输入密码；
步骤三：用光标选择要启动的系统，按e键编辑，在调整光标选择，kernel...行，再按e键编辑，在默认添加  1    然后按enter键退出；
步骤四：在grub界面，再按b键，启动系统
步骤五：# passwd root			#更改root密码

b 修复系统默认运行级别(如：真把默认运行级别改为0或6)
步骤一：和a 遗忘root密码修改前四步是一样的，都先进入单用户模式
步骤二：#vim /etc/inittab		#在文件中更改默认级别好就行

B 光盘修复模式
步骤一：	在Linux中放入光盘
步骤二：在启动界面(虚拟机是显示VMware界面)一直按F2键，进入BIOS
步骤三：调整启动顺序为光盘启动
步骤四：选择Rescue installed system		#安全模式
步骤五：选择英文界面语言(即使有中文)，并选择美式标准键盘us，选择Local CD/DVD，网络需要就设置（可以不要）；一直ok，到选择shell Start shell
步骤六：显示 bash-4.1# ，这个并不是系统根目录，需要将硬盘挂载(但不是mount)
步骤七：
bash-4.1# chroot /mnt/sysimage			#改变主目录(可以通过命令:pwd查看)
备注：如果要删除grub密码：就直接vim /etc/grub.conf 删除密码行即可；

备注2：如果把重要文件删除，如：/etc/inittab删除了，
sh-4.1# cd /root
sh-4.1# rpm -qf /etc/inittab		#通过其他正常系统查看或者百度，查询下/etc/inittab文件属于哪个包。
sh-4.1# mkdir /mnt/cdrom				#建立挂载点
sh-4.1# mount /dev/sr0 /mnt/cdrom		#挂载光盘
sh-4.1# rpm2cpio /mnt/cdrom/Packages/initscripts-8.45.3-1.i386.rpm \
| cpio -idv ./etc/inittab 				#提取inittab文件到当前目录
sh-4.1# cp etc/inittab /etc/inittab		#复制inittab文件到指定位置

C Linux安全性

拔出主板电池		破解		BIOS加密->可以保护光盘修复
光盘修复模式		破解		Grub加密->可以保护单用户模式
单用户模式		破解		用户密码


16 备份与恢复

16.1 备份概述
A Linux系统需要备份的数据
/root/目录
/home/目录
/var/spool/mail/目录
/var/log/
/etc/目录
其他目录		

B 安装服务的数据
Apache需要备份的数据
配置文件、网页主目录、日志文件

mysql需要备份的数据
源码包安装的mysql:/usr/local/mysql/data/
RPM包安装的mysql：/var/lib/mysql/

C 备份策略
a 完全备份：把所有需要备份的数据完全备份，当然完全备份可以备份整个硬盘，整个分区或
某个具体的目录。优点：恢复快。缺点：占用空间大(常用)

b 增量备份：第一次完全备份，以后每次备份前一次没有的数据。优点：占用空间小。缺点：
恢复慢(常用)

c 差异备份：第一次完全备份，以后每次备份第一次备份中没有的数据；优缺点都居中；

16.2 dump和restore命令

A dump命令
语法：dump [选项] 备份之后的文件名 原文件或目录
			-level		0-9十个备份级别(-0代表完全备份，-1代表增量备份)
			-f 文件名	备份之后的文件名
			-u			备份成功之后，把备份的时间记录在/etc/dumpdates文件
			-v			显示备份过程中更多的输出信息
			-j			调用bzlib库压缩备份文件，其实就是把备份文件压缩为bz2格式
			-W			显示允许被dump的分区的备份等级及备份时间
功能：备份文件或目录
备注：默认dump命令没有安装，要手动安装
```shell script
[root@localhost ~]# rpm -qa | grep dump 		#查看dump命令是否安装
[root@localhost ~]# yum -y install dump			#没有，则安装；
```
例：（备份分区）
dump -0uj -f /root/boot.bak.bz2 /boot/
							#备份命令。先执行一次完全备份，并压缩和更新备份时间
cat /etc/dumpdates			#查看备份时间文件
cp install.log /boot/		#复制日志文件到/boot分区
dump -1uj -f /root/boot.bak1.bz2 /boot/		
	
#增量备份/boot分区，并压缩(猜测：先要参考目标目录下已经有的备份)
dump –W					#查询分区的备份时间及备份级别的
备注：只有分区备份才能1备份等级，文件和目录只能使用0

例2：(备份文件或目录)
dump -0j -f /root/etc.dump.bz2 /etc/
#完全备份/etc/目录，只能使用0级别进行完全备份，而不再支持增量备份


B 命令restore
语法：restore [模式选项] [选项]
模式选项：restore命令常用的模式有以下四种，这四个模式不能混用（不能同时存在）。
						-C：比较备份数据和实际数据的变化
						-i： 进入交互模式，手工选择需要恢复的文件。
						-t： 查看模式，用于查看备份文件中拥有哪些数据。
						-r： 还原模式，用于数据还原。
						选项：
						-f： 指定备份文件的文件名
例：restore -C -f /root/boot.bak.bz2		#只需要备份文件名，因为备份文件中有关于原文件的信息，所以不需要再说明；
例2：
#还原boot.bak.bz2分区备份
#先还原完全备份的数据
mkdir boot.test
cd boot.test/
restore -r -f /root/boot.bak.bz2			#解压缩，注意会自动解压到当前文件夹；
restore -r -f /root/boot.bak1.bz2			#恢复增量备份数据



添加：

A 远程登录服务SSH

a.登陆linux系统，打开终端命令。输入 rpm -qa |grep ssh 查找当前系统是否已经安装
b.如果没安装，命令:
`# yum -y install ssh`
c.启动命令:
`# /etc/init.d/sshd start`
d.一定要设置开机自启动，查看命令：
`[root@localhost ~]# chkconfig --list | grep sshd`
sshd            0:关闭  1:关闭  2:启用  3:启用  4:启用  5:启用  6:关闭
设置命令：
`[root@localhost ~]# chkconfig sshd on`
e.查看或编辑SSH服务配置文件，如 vi /etc/ssh/sshd.config   
 如果要修改端口，把 port 后面默认的22端口改成别的端口即可（注意前面的#号要去掉）
f.如果需要远程连接SSH，需要把22端口在防火墙上开放。关闭防火墙，或者设置22端口例外


B url重写(针对CI)
a 在和index.php同目录下建立.htaccess文件，把上面的内容复制进去了保存
.htaccess内容：
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d        //此处有大坑。加上这两句可保证一般css、js文件正常加载。(注意删掉这句注释哦)
RewriteCond $1 !^(index\.php|images|robots\.txt)
RewriteRule ^(.*)$ /index.php/$1 [L]

注意:如果你的项目不在根目录请把上面这一句改为：RewriteRule ^(.*)$ index.php/$1 [L]
在上面的例子中，可以实现任何非 index.php、images 和 robots.txt 的 HTTP 请求都被指向 index.php。

b 修改config.php ,$config['index_page']='';

c 开启Apache的mod_rewrite模块
c1 打开Apache配置文件： vim /etc/httpd/httpd.conf
c2 更改
<Directory />
  Options FollowSymLinks
  AllowOverride None  
</Directory>
将上面的None改为All

c3 更改
<Directory "/usr/local/apache2//htdocs">
...
 AllowOverride None
...
</Directory>
将上面的None改为All

c4 将Apache重启：/usr/local/apache2/bin/apachectl restart


额外知识：

在Linux CentOS系统上安装完php和MySQL后，为了使用方便，需要将php和mysql命令加到系统命令中，如果在没有添加到环境变量之前，执行“php -v”命令查看当前php版本信息时时，则会提示命令不存在的错误，下面我们详细介绍一下在linux下将php和mysql加入到环境变量中的方法（假设php和mysql分别安装在/usr/local/php/和/usr/local/mysql/中）。

方法一：直接运行命令export PATH=$PATH:/usr/local/php/bin 和 export PATH=$PATH:/usr/local/mysql/bin

使用这种方法，只会对当前会话有效，也就是说每当登出或注销系统以后，PATH 设置就会失效，只是临时生效。

方法二：执行vi ~/.bash_profile修改文件中PATH一行，将/usr/local/webserver/php/bin 和 /usr/local/webserver/mysql/bin 加入到PATH=$PATH:$HOME/bin一行之后

这种方法只对当前登录用户生效

方法三：修改/etc/profile文件使其永久性生效，并对所有系统用户生效，在文件末尾加上如下两行代码
PATH=$PATH:/usr/local/php/bin:/usr/local/mysql/bin
export PATH

最后：执行 命令source /etc/profile或 执行点命令 ./profile使其修改生效，执行完可通过echo $PATH命令查看是否添加成功。


 

1001 获取进程号，并传递信号到该进程

kill -SIGHUP $(ps aux | grep 'syslog' | awk '{print $2}')
备注：上面的都是英文单引号
传递信号简单用法：killall
获取进程号简单用法：pidof '进程名'

1002 查询SUID/SGID文件
find / -perm +6000

1003 文件与进程的相关操作
A 查看文件被哪些进程使用：fuser
B 查看进程使用了哪些文件：lsof
C 查看进程号：pidof 

1004 让已连接的用户下线
ps aux | grep pts
kill xxxx
备注：xxxx表示显示出的用户连接进程号(第二列)




1005 更改服务端口，以更改sshd端口为例

A 修改/etc/ssh/sshd_config配置文件 （特别注意这里是/etc/ssh/sshd_config，而不是/etc/ssh/ssh_config）
```shell script
[root@localhost ssh]# more sshd_config 
#       $OpenBSD: sshd_config,v 1.69 2004/05/23 23:59:53 dtucker Exp $
# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.
# This sshd was compiled with PATH=/usr/local/bin:/bin:/usr/bin
# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options change a
# default value.
Port 22             	//先把22保留，防止更改未成功，而原来的页连接不上了
port 12321            //添加一个新的端口
#Protocol 2,1
```

B 重启ssh服务让修改的端口号生效
```shell script
[root@localhost ~]# service sshd restart
Stopping sshd:[  OK  ]
Starting sshd:[  OK  ]
```

1006 配置防火墙

事實上，我們在設定防火牆的時候，不太可能會一個一個指令的輸入，通常是利用 shell scripts 來幫我們達成這樣的功能吶！底下是利用上面的流程圖所規劃出來的防火牆腳本，你可以參考看看， 但是你需要將環境修改成適合你自己的環境才行喔！此外，為了未來修改維護的方便，鳥哥將整個 script 拆成三部分，分別是：

iptables.rule：設定最基本的規則，包括清除防火牆規則、載入模組、設定服務可接受等；
iptables.deny：設定抵擋某些惡意主機的進入；
iptables.allow：設定允許某些自訂的後門來源主機！
鳥哥個人習慣是將這個腳本放置到 /usr/local/virus/iptables 目錄下，你也可以自行放置到自己習慣的位置去。 那底下就來瞧瞧這支腳本是怎麼寫的吧！

```shell script
[root@www ~]# mkdir -p /usr/local/virus/iptables
[root@www ~]# cd /usr/local/virus/iptables
[root@www iptables]# vim iptables.rule
```
`#!/bin/bash`

請先輸入您的相關參數，不要輸入錯誤了！
  EXTIF="eth0"             # 這個是可以連上 Public IP 的網路介面
  INIF="eth1"              # 內部 LAN 的連接介面；若無則寫成 INIF=""
  INNET="192.168.100.0/24" # 若無內部網域介面，請填寫成 INNET=""
  export EXTIF INIF INNET

第一部份，針對本機的防火牆設定！
1. 先設定好核心的網路功能：
```shell script
  echo "1" > /proc/sys/net/ipv4/tcp_syncookies
  echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts
  for i in /proc/sys/net/ipv4/conf/*/{rp_filter,log_martians}; do
        echo "1" > $i
  done
  for i in /proc/sys/net/ipv4/conf/*/{accept_source_route,accept_redirects,\
send_redirects}; do
        echo "0" > $i
  done
```

2. 清除規則、設定預設政策及開放 lo 與相關的設定值
```shell script
  PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/sbin:/usr/local/bin; export PATH
  iptables -F
  iptables -X
  iptables -Z
  iptables -P INPUT   DROP
  iptables -P OUTPUT  ACCEPT
  iptables -P FORWARD ACCEPT
  iptables -A INPUT -i lo -j ACCEPT
  iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
```

3. 啟動額外的防火牆 script 模組
```shell script
  if [ -f /usr/local/virus/iptables/iptables.deny ]; then
        sh /usr/local/virus/iptables/iptables.deny
  fi
  if [ -f /usr/local/virus/iptables/iptables.allow ]; then
        sh /usr/local/virus/iptables/iptables.allow
  fi
  if [ -f /usr/local/virus/httpd-err/iptables.http ]; then
        sh /usr/local/virus/httpd-err/iptables.http
  fi
```

4. 允許某些類型的 ICMP 封包進入
```shell script
  AICMP="0 3 3/4 4 11 12 14 16 18"
  for tyicmp in $AICMP
  do
    iptables -A INPUT -i $EXTIF -p icmp --icmp-type $tyicmp -j ACCEPT
  done
```

5. 允許某些服務的進入，請依照你自己的環境開啟
```shell script
# iptables -A INPUT -p TCP -i $EXTIF --dport  21 --sport 1024:65534 -j ACCEPT # FTP
# iptables -A INPUT -p TCP -i $EXTIF --dport  22 --sport 1024:65534 -j ACCEPT # SSH
# iptables -A INPUT -p TCP -i $EXTIF --dport  25 --sport 1024:65534 -j ACCEPT # SMTP
# iptables -A INPUT -p UDP -i $EXTIF --dport  53 --sport 1024:65534 -j ACCEPT # DNS
# iptables -A INPUT -p TCP -i $EXTIF --dport  53 --sport 1024:65534 -j ACCEPT # DNS
# iptables -A INPUT -p TCP -i $EXTIF --dport  80 --sport 1024:65534 -j ACCEPT # WWW
# iptables -A INPUT -p TCP -i $EXTIF --dport 110 --sport 1024:65534 -j ACCEPT # POP3
# iptables -A INPUT -p TCP -i $EXTIF --dport 443 --sport 1024:65534 -j ACCEPT # HTTPS
iptables -A INPUT -p TCP -i lo --dport 6379 --sport 1024:65534 -j ACCEPT # REDIS，供服务器自己访问，以后甚至可以指定sip(即来源ip)和dip(目标ip)

```
第二部份，針對後端主機的防火牆設定！
1. 先載入一些有用的模組
```shell script
modules="ip_tables iptable_nat ip_nat_ftp ip_nat_irc ip_conntrack 
ip_conntrack_ftp ip_conntrack_irc"
  for mod in $modules
  do
      testmod=`lsmod | grep "^${mod} " | awk '{print $1}'`
      if [ "$testmod" == "" ]; then
            modprobe $mod
      fi
  done
```

2. 清除 NAT table 的規則吧！
```shell script
iptables -F -t nat
iptables -X -t nat
iptables -Z -t nat
iptables -t nat -P PREROUTING  ACCEPT
iptables -t nat -P POSTROUTING ACCEPT
iptables -t nat -P OUTPUT      ACCEPT
```

3. 若有內部介面的存在 (雙網卡) 開放成為路由器，且為 IP 分享器！
```shell script
  if [ "$INIF" != "" ]; then
    iptables -A INPUT -i $INIF -j ACCEPT
    echo "1" > /proc/sys/net/ipv4/ip_forward
    if [ "$INNET" != "" ]; then
        for innet in $INNET
        do
            iptables -t nat -A POSTROUTING -s $innet -o $EXTIF -j MASQUERADE
        done
    fi
  fi
  # 如果你的 MSN 一直無法連線，或者是某些網站 OK 某些網站不 OK，
  # 可能是 MTU 的問題，那你可以將底下這一行給他取消註解來啟動 MTU 限制範圍
  # iptables -A FORWARD -p tcp -m tcp --tcp-flags SYN,RST SYN -m tcpmss \
  #          --mss 1400:1536 -j TCPMSS --clamp-mss-to-pmtu

```
4. NAT 伺服器後端的 LAN 內對外之伺服器設定
```shell script
# iptables -t nat -A PREROUTING -p tcp -i $EXTIF --dport 80 \
#          -j DNAT --to-destination 192.168.1.210:80 # WWW

# 5. 特殊的功能，包括 Windows 遠端桌面所產生的規則，假設桌面主機為 1.2.3.4
# iptables -t nat -A PREROUTING -p tcp -s 1.2.3.4  --dport 6000 \
#          -j DNAT --to-destination 192.168.100.10
# iptables -t nat -A PREROUTING -p tcp -s 1.2.3.4  --sport 3389 \
#          -j DNAT --to-destination 192.168.100.20
```

6. 最終將這些功能儲存下來吧！
  `/etc/init.d/iptables save`
特別留意上面程式碼的特殊字體部分，基本上，你只要修改一下最上方的介面部分， 應該就能夠運作這個防火牆了。不過因為每個人的環境都不相同， 因此你在設定完成後，依舊需要測試一下才行喔！不然，出了問題不要怪我啊！.... 再來看一下關於 iptables.allow 的內容是如何？假如我要讓一個 140.116.44.0/24 這個網域的所有主機來源可以進入我的主機的話，那麼這個檔案的內容可以寫成這樣：

```shell script
[root@www iptables]# vim iptables.allow
#!/bin/bash
# 底下則填寫你允許進入本機的其他網域或主機啊！
iptables -A INPUT -i $EXTIF -s 140.116.44.0/24 -j ACCEPT

# 底下則是關於抵擋的檔案設定法！
[root@www iptables]# vim iptables.deny
#!/bin/bash
# 底下填寫的是『你要抵擋的那個咚咚！』
iptables -A INPUT -i $EXTIF -s 140.116.44.254 -j DROP

[root@www iptables]# chmod 700 iptables.*
```
將這三個檔案的權限設定為 700 且只屬於 root 的權限後，就能夠直接執行 iptables.rule 囉！ 不過要注意的是，在上面的案例當中，鳥哥預設將所有的服務的通道都是關閉的！ 所以你必須要到本機防火牆的第 5 步驟處將一些註解符號 (#) 解開才行。 同樣的，如果有其他更多的 port 想要開啟時，一樣需要增加額外的規則才行喔！

不過，還是如同前面我們所說的，這個 firewall 僅能提供基本的安全防護，其他的相關問題還需要再測試測試呢！ 此外，如果你希望一開機就自動執行這個 script 的話，請將這個檔案的完整檔名寫入 /etc/rc.d/rc.local 當中，有點像底下這樣：

```shell script
[root@www ~]# vim /etc/rc.d/rc.local
....(其他省略)....
# 1. Firewall
/usr/local/virus/iptables/iptables.rule
```
事實上，這個腳本的最底下已經加入寫入防火牆預設規則檔的功能，所以你只要執行一次，就擁有最正確的規則了！ 上述的 rc.local 僅是預防萬一而已。 ^_^！上述三個檔案請你不要在 Windows 系統上面編輯後才傳送到 Linux 上運作，因為 Windows 系統的斷行字元問題，將可能導致該檔案無法執行。建議你直接到底下去下載，傳送到 Linux 後可以利用 dos2unix 指令去轉換斷行字元！就不會有問題！

http://linux.vbird.org/download/index.php?action=detail&fileid=43
這就是一個最簡單、陽春的防火牆。同時，這個防火牆還可以具有最陽春的 IP 分享器的功能呢！ 也就是在 iptables.rule 這個檔案當中的第二部分了。 這部分我們在下一節會再繼續介紹的。


1007 更改sshd服务端口(其他服务类似)
步骤一 vim /etc/ssh/sshd_config

找到
`# Port 22`
更改为
Port 22
Port 12321 		#先同时存在，防止改错了，自己登不上去；

步骤二 service sshd restart 测试是否能登陆，然后注释掉 Port 22


1008 查看进程使用资源情况
方式一：top 
方式二：ps
ps命令常用用法（方便查看系统进程）

1）ps a 显示现行终端机下的所有程序，包括其他用户的程序。
2）ps -A 显示所有进程。
3）ps c 列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示。
4）ps -e 此参数的效果和指定"A"参数相同。
5）ps e 列出程序时，显示每个程序所使用的环境变量。
6）ps f 用ASCII字符显示树状结构，表达程序间的相互关系。
7）ps -H 显示树状结构，表示程序间的相互关系。
8）ps -N 显示所有的程序，除了执行ps指令终端机下的程序之外。
9）ps s 采用程序信号的格式显示程序状况。
10）ps S 列出程序时，包括已中断的子程序资料。
11）ps -t<终端机编号> 　指定终端机编号，并列出属于该终端机的程序的状况。
12）ps u 　以用户为主的格式来显示程序状况。
13）ps x 　显示所有程序，不以终端机来区分。
最常用的方法是ps -aux,然后再利用一个管道符号导向到grep去查找特定的进程,然后再对特定的进程进行操作。


运行 ps aux 的到如下信息：

```shell script
root:# ps aux
USER      PID       %CPU    %MEM    VSZ    RSS    TTY    STAT    START    TIME    COMMAND
smmsp    3521    0.0    0.7    6556    1616    ?    Ss    20:40    0:00    sendmail: Queue runner@01:00:00 f
root    3532    0.0    0.2    2428    452    ?    Ss    20:40    0:00    gpm -m /dev/input/mice -t imps2
htt    3563    0.0    0.0    2956    196    ?    Ss    20:41    0:00    /usr/sbin/htt -retryonerror 0
htt    3564    0.0    1.7    29460    3704    ?    Sl    20:41    0:00    htt_server -nodaemon
root    3574    0.0    0.4    5236    992    ?    Ss    20:41    0:00    crond
xfs    3617    0.0    1.3    13572    2804    ?    Ss    20:41    0:00    xfs -droppriv -daemon
root    3627    0.0    0.2    3448    552    ?    SNs    20:41    0:00    anacron -s
root    3636    0.0    0.1    2304    420    ?    Ss    20:41    0:00    /usr/sbin/atd
dbus    3655    0.0    0.5    13840    1084    ?    Ssl    20:41    0:00    dbus-daemon-1 --system

```

Head标头：

USER    用户名
UID    用户ID（User ID）
PID    进程ID（Process ID）
PPID    父进程的进程ID（Parent Process id）
SID    会话ID（Session id）
%CPU    进程的cpu占用率
%MEM    进程的内存占用率
VSZ    进程所使用的虚存的大小（Virtual Size）
RSS    进程使用的驻留集大小或者是实际内存的大小，Kbytes字节。
TTY    与进程关联的终端（tty）
STAT    进程的状态：进程状态使用字符表示的（STAT的状态码）
R 运行    Runnable (on run queue)            正在运行或在运行队列中等待。
S 睡眠    Sleeping                休眠中, 受阻, 在等待某个条件的形成或接受到信号。
I 空闲    Idle
Z 僵死    Zombie（a defunct process)        进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放。
D 不可中断    Uninterruptible sleep (ususally IO)    收到信号不唤醒和不可运行, 进程必须等待直到有中断发生。
T 终止    Terminate                进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行。
P 等待交换页
W 无驻留页    has no resident pages        没有足够的记忆体分页可分配。
X 死掉的进程
< 高优先级进程                    高优先序的进程
N 低优先    级进程                    低优先序的进程
L 内存锁页    Lock                有记忆体分页分配并缩在记忆体内
s 进程的领导者（在它之下有子进程）；
l 多进程的（使用 CLONE_THREAD, 类似 NPTL pthreads）
+ 位于后台的进程组 
START    进程启动时间和日期
TIME    进程使用的总cpu时间
COMMAND    正在执行的命令行命令
NI    优先级(Nice)
PRI    进程优先级编号(Priority)
WCHAN    进程正在睡眠的内核函数名称；该函数的名称是从/root/system.map文件中获得的。
FLAGS    与进程相关的数字标识


### logrotate说明

logrotate对日志进行轮替，配置文件在/etc/logrotate.conf 和/etc/logrotate.d/中
如：vim /etc/logrotate.d/admin
```
/var/log/admin.log{
	monthly 	#每个月进行一次
	size=10M 	#文件容量大于10M则开始处置
	rotate 5 	#保留5个
	compress 	#进行压缩
	sharedscripts
	prerotate	#轮替前进行动作
		/usr/bin/chattr -a /var/log/admin.log
	endscript
	postrotate	#轮替后进行动作
		/usr/bin/killall -HUP syslogd
		/usr/bin/chattr +a /var/log/admin.log
	endscript
}
```

### 备份说明
/etc 		#整个目录
/home 		#整个目录
/var/spool/mail 	
/boot
/root
/var/www 	#不一定在这里，最好把nginx配置也备份
/var/lib/mysql 	#也不一定在这里，最好把mysql配置也备份
