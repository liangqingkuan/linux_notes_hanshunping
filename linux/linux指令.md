```sh
Ctrl+L(等价于指令clear)清屏
Ctrl+C强制中断
Ctrl+D正常结束
Tab自动补全
```

选项卡可叠加，一般无顺序要求；
多个带参数选项要单独写；


```sh
su默认行为，切换到root用户
cd默认行为，切到当前用户的家目录
head默认行为，看前10行
tail默认行为，看后10行
history默认行为，显示全部指令
top默认行为，3秒更新
默认行为当前：ls，date，cal，du
```
# 文件
```sh
/etc/paasswd用户账户数据库（除了密码）
/etc/shadow加密密码与账户策略库
/etc/group组信息数据库
/etc/fstab永久挂载，定义系统启动时自动挂载的文件系统信息
/etc/hostname修改主机名
/etc/hosts指定主机映射
ll /etc/init.d或/etc/rc.d/init.d 查看service管理的服务
/usr/lib/systemd/system查看systemctl管理的服务
/etc/profile环境变量配置文件
```

# 一、关机重启
```sh
sync      将内存中的数强制写入磁盘
```

```sh
halt不一定断电
poweroff
shutdown -h +N（+可省略，其中0可换为now，+1等同于shutdown）
shutdown  -h HH:MM
shutdown -c取消
```

```sh
reboot
shutdown -r +N（+可省略，其中0可换为now）
shutdown  -r HH:MM
shutdown -c取消

```
# 二、登录注销

```sh
su username     # 切换到指定用户但不加载其环境变量
su - username   # 切换到指定用户并加载其环境变量
su              # 切换到 root 用户（默认不切换环境变量）
su -            # 切换到 root 用户并加载其环境变量（推荐方式）
```

```sh
logout 或 exit
```
# 三、用户管理
```sh
groupadd 组名

groupdel 组名
```

```sh
useradd 用户名
useradd -d 地址 用户名 # 指定用户的家目录路径（地址写到用户名子目录）
useradd -g 组名 用户名

userdel 用户名
userdel -r 用户名 # 删除用户同时删除家目录和相关文件

usermod -g 组名 用户名
```

```sh
passwd 用户名
id 用户名
whoami
who am i
```

/etc/paasswd用户账户数据库（除了密码）
/etc/shadow加密密码与账户策略库
/etc/group组信息数据库
# 四、运行级别
## (一)init
`init 运行级别数字`
(常用:0关机，3多用户有网，5图形化，6重启)
## (二)systemctl
```sh
systemctl get-default
systemctl set-default 运行级别.target
```
(poweroff.target，multi-user.target，graphical.target，reboot.target)
# 五、帮助
## (一)man
`man 指令`
## (二)help
`help 指令`
这些不能用man查看
（1）目录操作
cd, pwd, dirs, pushd, popd
（2）作业控制
jobs, fg, bg, wait
（3）变量操作
set, unset, export, readonly, declare
（4）其他
alias, unalias, echo, printf, test
source, ., exit, logout

# 六、文件目录类
## (一)pwd显示绝对路径&&目录树见十二(四)

```shell
# 1. 先确保网络能通（如果ping不通114，说明网卡没开）
sed -i 's/ONBOOT=no/ONBOOT=yes/' /etc/sysconfig/network-scripts/ifcfg-*
systemctl restart network
echo "nameserver 8.8.8.8" > /etc/resolv.conf

# 2. 一键切换阿里源（核心步骤）
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 3. 重建缓存并安装tree
yum clean all && yum makecache
yum install -y tree
```
## (二)ls 选项 文件或目录
```sh
-a          # 显示所有，包括 . 和 ..
-l           # 单行详细显示
-h          # 人类可读
-R          # 递归显示
```

## (三)cd 选项 目录（选项很少用）
```sh
cd /home/user      # 绝对路径
cd Documents       # 相对路径
cd ..              # 上一级目录
cd ../..           # 上两级目录
cd ~               # 家目录（当前用户）
cd ~username       # 指定用户的家目录
cd -               # 返回上一个工作目录
```
## (四)mkdir、rmdir、touch
```sh
mkdir 选项 目录
-p            # 多级目录
-v            # 详细
例：mkdir -p a/b/c
创建a/(如果不存在)->a/b/(如果不存在)->a/b/c/
```

```sh
rmdir 选项 目录
-p            # 多级目录
例：rmdir -p a/b/c
删除顺序：c(如果c空)->b(如果b空)->a(如果a空)
```
`touch 选项 文件`
## (五)cp复制
```sh
cp 选项 源文件或目录 目标文件或目录
-r 或 -R             # 递归复制整个文件夹（目录）
-f                   # 强制
```
覆盖：
（1）目标文件已存在（2）目标目录中有同名文件
==强制覆盖不提示\cp==
## (六)rm删除
```sh
rm 选项 文件或目录
-r 或 -R             # 递归删除整个文件夹（目录）
-f                       # 强制删除不提示
例子：
rm -rf a              # 删除a下所有文件

```
 rm 处理符号链接（软链接）时：
1.遇到 link（无斜杠）→ 删除链接本身
2.遇到 link/（有斜杠）→ 进入目录删除内容
3.-r 选项对链接本身无意义（链接不是目录）
## (七)mv移动
```
mv 旧 新
```
移动整个目录无需遍历，也没有-r选项卡
存在目标目录移动，不存在目标目录移动并重命名；存在目标文件覆盖，不存在目标文件重命名
## (八)查看
### 1、cat 选项 文件
```sh
-n                      # 显示行号
方便浏览，加上管道命令 | more
```

### 2、more 文件
```sh
空格        # 翻一页
enter        # 翻一行
q               # 立刻离开more，
Ctrl+F        # 向下滚动一屏
Ctrl+B        # 返回上一屏   
等号            # 输出当前行号
:f                 # 输出文件名与当前行行号
```

### 3、less 文件
```sh
空格                   # 向下翻一页
[pagedown]       # 向下翻一页
[pageup]            # 向上翻一页
/字串                  # 向下搜寻，n向下查找，N向上
？字串                 # 向上搜寻，n向上查找，N向下
q                          # 离开less

```
### 4、echo 选项 输出内容
$PATH , $HOSTNAME
"内容"

### 5、head与tail
```sh
head 选项 文件
-n 数字                  # 看前几行
tail 选项 文件
-n 数字                   # 看后几行
-f                       # 实时追踪该文档所有更新，Ctrl+C退出

```
### 6、输出重定向> 和 追加>>
```sh
1) Is -l > 文件
（功能描述：列表的内容写入文件a.txt中（覆盖写）)
2) Is -al >> 文件
（功能描述：列表的内容追加到文件aa.txt的未尾）
3) cat 文件1 > 文件2　
（功能描述：将文件1的内容覆盖到文件2)
4) echo "内容" >> 文件
（追加）
```
## (九)链接
```sh
ln -s 原文件或目录 软链接目录
```
无s是硬链接
## (十)指令历史记录
```sh
history           # 显示全部指令
history 数字    # 最近几个指令
! 数字             # 某历史编号的指令
上下箭头查看上一条和下一条指令，回车执行
```


# 七、时间日期指令
## (一)date
```sh
1) date （功能描述：显示当前时间）
2) date +%Y （功能描述：显示当前年份）
3) date +%m （功能描述：显示当前月份）
4) date +%d （功能描述：显示当前是哪一天）
5) date "+%Y-%m-%d %H:%M:%S"（功能描述：显示年月日时分秒）
date -s                           # 设置日期
```
## (二)cal 选项 月份 年份

# 八、搜素查找类
## (一)find查找文件或目录，输出路径
```sh
find [搜索范围] [匹配条件] [处理动作]

匹配条件：
-name 文件名
-user 用户名
-size 文件大小(+大于，-小于，单位c,k,M,G)
-type 文件类型(l 软链接，d 目录，c 字符设备(如终端)文件，b 块设备(如硬盘)文件，f普通文件，s套接字，p命名管道)
-mtime 时间(n:正好n天前修改;+n:n天之前修改;-n:n天之内修改)
```
```sh
处理动作（默认打印）：
-print  打印路径
-delete 删除
-exec   执行其他命令
-ok     执行前询问
```

## (二)locate查找文件或目录，输出路径
```
locate 文件名
```
由于locate指令基于数据库进行查询，所以第一次运行前，必须使用updatedb指令创建locate数据库。
## (三)which查找命令，输出命令完整路径
```
which 指令
```
看指令在哪个目录下
## (四)grep查找文件内容，输出匹配到的文本行
```sh
grep [选项] 查找内容 [查找范围]
-n                   # 显示匹配行及行号
-i                    # 忽略字母大小写
-r 或 -R                # 递归搜索目录
-v                     # 反向匹配，显示不包含的
-l                     # 只显示文件名，不显示内容
-c                     # 统计匹配行数
-w                     # 单词匹配
-E                     # 使用扩展正则表达式
-o                    # 只显示匹配的部分
```
例子：
cat /home/test.txt | grep -n "yes"
grep -n "yes" /home/test.txt


基本正则表达式：
	 查找包含 test 的行
	 `grep 'test' file.txt`
	 查找 test 或 text（| 需要转义）
	 `grep 'test\|text' file.txt`
	 查找 test 开头的行（^ 不需要转义）
	 `grep '^test' file.txt`
	 查找 test 或 text（| 和 括号需要转义）
	 `grep '\(test\|text\)' file.txt`
	 
扩展正则（-E，推荐）：
	查找 test 或 text（ | 不需要转义）
	`grep -E 'test|text' file.txt`
	查找 test 或 text（| 和 括号不需要转义）
	`grep -E '(test|text)' file.txt`
	量词语法不需要转义
	`grep -E 'a{1,3}' file.txt`    # 找到a连续重复1~3次的地方
	`grep -E 'colou?r' file.txt`  # 查找color 或 colour
	
Perl正则（-P）：
	数字匹配
	`grep -P '\d+' file.txt`      # 一个或多个数字
	 单词边界
	`grep -P '\btest\b' file.txt` # 精确匹配test单词
	 非贪婪匹配
	`grep -P 'a.*?b' file.txt`    # 匹配a...b，最短匹配
字符类：
```sh
	.        # 任意单个字符（换行符除外）
	[abc]    # a、b或c中的任意一个
	[^abc]   # 除了a、b、c的任意字符
	[a-z]    # a到z的任意小写字母
	[0-9]    # 任意数字
	\w       # 单词字符（字母、数字、下划线）- 需要 -E 或 -P
	\W       # 非单词字符
	\d       # 数字 - 需要 -P
	\D       # 非数字
	\s       # 空白字符（空格、制表符等）
	\S       # 非空白字符
```
位置锚定：
```sh
	^        # 行首
	$        # 行尾
	\<       # 单词开头
	\>       # 单词结尾
	\b       # 单词边界 - 需要 -E 或 -P
```
量词：
```sh
	*        # 前一个字符0次或多次
	+        # 前一个字符1次或多次 - 需要 -E 或 -P
	?        # 前一个字符0次或1次 - 需要 -E 或 -P
	{n}      # 恰好n次 - 需要 -E 或 -P
	{n,}     # 至少n次 - 需要 -E 或 -P
	{n,m}    # n到m次 - 需要 -E 或 -P
```


# 九、压缩和解压缩类
## (一)gzip与gunzip
```sh
gzip 文件
gunzip 文件(.gz)
```
## (二)zip与unzip
```sh
zip [选项] 压缩包名(.zip) 被压缩文件或目录
-r                      # 递归压缩
unzip [选项] 压缩包名(.zip) [-d 目录]
-d 目录               # 将解压缩文件放入指定目录
```
## (三)tar
```sh
tar  
-c               创建新的归档文件      # 加.tar
-x               从归档中提取文件      # 去掉.tar
-z               使用gzip压缩/解压    # 加或去掉.gz
-v               显示处理过程
-f 归档文件名     指定归档文件名
-C               指定解压到目录
tar -czvf 归档文件名.tar.gz 要打包的文件或目录
tar -zxvf 归档文件名.tar.gz [-C 解压到目录]
```

# 十、文件组管理与权限管理
## (一)所有者与所在组

修改所有者 `chown 用户名 文件或目录`
修改所有者和所在组 `chown 用户名:组名 文件或目录`
-R                # 对目录下所有子文件或目录生效


查看文件目录所在组`ls -l` 或 `ll`
修改所在组` chgrp 组名 文件或目录`


修改用户登录的初始目录 `usermod -d 目录 用户名`

## (二)权限：
第一列10位：
1）第0位决定文件类型：l 软链接，d 目录，c 字符设备文件，b 块设备文件，- 普通文件，s套接字，p命名管道
2）第1-3位确定所有者（该文件的所有者）拥有该文件的权限。--User 
3）第4-6位确定所属组（同用户组的）拥有该文件的权限，---Group 
4）第7-9位确定其他用户拥有该文件的权限---Other
r可读read;w可写write;x可执行execute

第二列：文件的硬链接数 或 目录的子目录数
第三列：所有者
第四列：所在组
第五列：文件大小（字节）
第六七八列：最后修改日期
第九列：文件名

修改权限 chmod
```shell
第一种：
u所有者，g所在组，o其他组，a所有人
通过+、-、=变更权限
chmod u=rwx,g=rx,o=x 文件或目录
chmod o+w,a-x 文件或目录

第二种：
r=4,w=2,x=1
chmod 751 文件或目录

```
# 十一、定时任务调度
## (一)crontab
```shell
crontab 选项
-e                    # 编辑crontab定时任务，用vim打开文本文件
-l                     # 查询crontab任务，列出当前有哪些任务调度
-r                     # 删除当前用户所有的crontab任务，终止任务调度
```
### 1、时间表示
`* * * * * ls -l /etc/ > /tmp/to.txt`
一小时中的第几分钟0-59
一天中的第几小时0-23
一月当中的第几天1-31
一年中的第几月1-12
一周中的星期几0-7
==分时日月星==
`*`表示所有时间，如第一个`*`代表一小时中每分钟
`,`分隔不连续的时间`0 8,12,16 * * *`
`-`代表连续时间范围`0 5 * * 1-6`
`*/n`代表每隔多久执行一次`*/1等同于*`
### 2、例：
1）`crontab -e` 输入`*/1 * * * * date >> /tmp/mydate`
2）`vim /home/my.sh` 输入 `date >> /home/mycal` 和 `cal >> /home/mycal`；
`chmod u+x /home/my.sh`；`crontab -e` 输入 `*/1 * * * * /home/my.sh`

`service crond restart`      # 重启任务调度

## (二)at定时任务
`at [选项] 时间`，回车，显示"at>"，一条条输入指令
Ctrl+D结束at命令输入
创建类选项：
```shell
-m                             # 指定任务完成后将给用户发送邮件，即使没有标准输出
-v                               # 显示任务将被执行的时间
-c                               # 打印任务的内容到标准输出
-V                              # 显示版本信息
-q 队列                       # 显示指定的队列
-f 文件                        # 从指定文件读入内容而不是从标准输入读入
-t 时间参数                  # 以时间参数的形式提交要运行的内容
```
管理类选项：
```shell
-l                                # atq别名，查看系统中没有执行的工作任务
-d 编号                       # atrm别名，删除已经设置的任务
```
### 1、指定时间的方法：
1）`hh:mm`，假如当天时间已过，到第二天执行
2）midnight、noon、teatime(16:00)
3）`数字am`（上午）、`数字pm`（下午）
4）指定具体日期：mm/dd/yy 或 mm dd yy 或 yy-mm-dd（不写年份默认今年或明年）
5）`now + 数 单位`，单位可以是minutes,hours,days,weeks
6）today、tomorrow、next week、next monday
### 2、例：
```shell
[tom@hspEdu01 ~]$ at 18:00
at> echo "成功"
at> <EOT>   ###这里按了Ctrl+D
job 2 at Sun Feb  1 18:00:00 2026
[tom@hspEdu01 ~]$ atq
2	Sun Feb  1 18:00:00 2026 a tom
[tom@hspEdu01 ~]$ atrm 2
[tom@hspEdu01 ~]$ atq
[tom@hspEdu01 ~]$ 
```


# 十二、liunx磁盘分区、挂载、查询
## (一)查看所有设备挂载情况lsblk 或 lsblk -f
## (二)挂载：
1）设置/==添加==/改磁盘大小/重启
2）==分区==：`fdisk /dev/sdb`，然后n，p，两次回车，w
```shell
m    # 显示命令列表
p      # 显示磁盘分区，同fdisk -l
n      # 新增分区
d       # 删除分区
w       # 写入并退出
q        # 不保存退出
```
3）==格式化==磁盘：`mkfs -t ext4 /dev/sdb1`
`-t`是type，`ext4`是<文件系统类型>，`/dev/sdb1`是<设备路径>
4）==挂载==：`mount 设备名称 挂载目录`
卸载：`umount 设备名称或挂载目录`
用命令行挂载重启后会失效
5）==永久挂载==：`vim /etc/fstab`，复制一份UUID换成/dev/sdb1，挂到/newdisk，最后00不备份不检测
## (三)查询使用情况
```shell
df 查询系统整体磁盘使用情况
-h                    # 换算计量单位，人类容易读
```

```shell
du 查询指定目录磁盘使用情况，默认当前磁盘
-s                    # 指定目录占用大小汇总
-h                    # 换算计量单位，人类容易读
-a                    # 含文件
--max-depth=1           # 子目录深度
-c                     # 列出明细的同时，增加汇总值
```
## (四)目录树
以树状显示目录结构，如果没有tree，则使用`yum install tree` 来安装
`tree 目录路径`
```shell
# 1. 先确保网络能通（如果ping不通114，说明网卡没开）
sed -i 's/ONBOOT=no/ONBOOT=yes/' /etc/sysconfig/network-scripts/ifcfg-*
systemctl restart network
echo "nameserver 8.8.8.8" > /etc/resolv.conf

# 2. 一键切换阿里源（核心步骤）
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 3. 重建缓存并安装tree
yum clean all && yum makecache
yum install -y tree
```

# 十三、网络配置
## (一)查看
```shell
查看windows环境中VMnet8网络配置
ipconfig
查看linux的配置
ifconfig
```
## (二)测试网络链接ping（见十四）

## (三)设置静态ip
1）`vim /etc/sysconfig/network-scripts/ifcfg-ens33`
2）	修改：BOOTPROTO="static"
添加：
	IPADDR=192.168.xxx.100
	NETMASK=255.255.255.0
	GATEWAY=192.168.xxx.2
	DNS1=192.168.xxx.2
	DNS2=8.8.8.8
3）`sudo systemctl restart network`或者`service network restart`

## (四)设置主机名和hosts映射

`hostname`查看主机名
`vim /etc/hostname`修改主机名

windows在C:\Windows\System32\drivers\etc\hosts指定
linux在/etc/hosts指定

# 十四、进程管理
## (一)ps查看进程
```shell
-a              # 显示当前终端所有进程信息
-e              # 显示所有进程，包括系统进程即无终端进程
-u              # 以用户的格式显示进程信息
-x              # 显示后台进程运行的参数
-f               # 全格式
```

-u的首行：`USER     PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND`
USER进程执行用户
PID进程号
%CPU占用CPU的百分比
%MEM占用物理内存的百分比
VSZ占用虚拟内存
RSS占用物理内存
TTY终端信息
STAT运行状态（S表示sleep休眠，R表示run正在运行，D短期等待，Z僵死进程）
SATRT执行开始时间
TIME占用CPU时间
COMMAND进程名称
PPID父进程的进程号

## (二)终止进程
```shell
kill [选项] 进程号
-9                 # 强迫进程立即停止
killall [选项] 进程名称（同名进程全部终止）
-9                 # 强迫进程立即停止  
```

1）踢掉某个非法登录用户
`ps -aux | grep sshd`
`kill PID`
2）重启sshd服务
`/bin/systemctl start sshd.service`
3）终止多个gedit
`killal gedit`

## (三)进程树
```shell
pstree 选项
-p                    # 显示进程PID
-u                    # 显示进程的所属用户
```
## (四)服务管理

前备知识：
1）后台程序，即守护程序，即服务
2）runlevel
	运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
	运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
	运行级别2：多用户状态（没有NFS)，不支持网络
	运行级别3：完全的多用户状态（有NFS)，无界面，登陆后进入控制台命令行模式
	运行级别4：系统未使用，保留
	运行级别5：X11控制台，登陆后进入图形GUI模式
	运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动
3）开机流程：开机->BIOS->/boot->systemd进程1->运行级别->运行级对应的服务

### 1、sevice
#### (1)`service 服务名 动作`
1）service管理的服务在/etc/init.d查看，或用setup查看；
2）动作有`start、stop、restart、reload、status`

```shell
[root@hspEdu01 ~]# ll /etc/init.d
lrwxrwxrwx. 1 root root 11 1月  28 20:36 /etc/init.d -> rc.d/init.d
[root@hspEdu01 ~]# ll /etc/rc.d/init.d
总用量 40
-rw-r--r--. 1 root root 18281 5月  22 2020 functions
-rwxr-xr-x. 1 root root  4569 5月  22 2020 netconsole
-rwxr-xr-x. 1 root root  7928 5月  22 2020 network
-rw-r--r--. 1 root root  1160 10月  2 2020 README
```
==符号链接中的相对路径是基于链接所在目录解析的，不是基于当前工作目录！==
#### (2)service管理的服务自启动设置：
1）查看各个运行级别自启动情况：
`chkconfig 服务名 --list` 或 `chkconfig --list 服务名`（等价于`chkconfig --list | grep 服务名`）
2）设置自启动：
`chkconfig --level 运行级别数字 服务名 on`
`chkconfig --level 运行级别数字 服务名 off`
（需要重启生效）
### 2、systemctl
#### (1)`sysytemctl 动作 服务名`
1）在/usr/lib/systemd/system查看systemctl管理的服务，如firewalld.service
2）动作有`start、stop、restart、status`
#### (2)systemctl管理服务的自启动设置：
1）查看自启动情况：
`systemctl list-unit-files | grep 服务名`
`systemctl is-enabled 服务名（.service可以省略）`
2）设置：
`systemctl enable 服务名`
`systemctl disable 服务名`


## (五)top动态监控进程
top显示正在执行的进程
```shell
-d 秒数                # 指定top指令每隔几秒更新，默认3秒
-i                         # 使top不显示任何闲置或僵死进程
-p                        # 通过监控进程ID来仅仅监控某个进程状态
```

```shell
top - 16:06:57 up 55 min,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 183 total,   1 running, 182 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  1.0 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  2027896 total,  1161212 free,   488792 used,   377892 buff/cache
KiB Swap:  2096124 total,  2096124 free,        0 used.  1377876 avail Mem 

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND 
```
16:06:57当前时间
55 min系统执行时间
load average: 0.00, 0.01, 0.05负载值
zombie僵死进程
id=idle空闲的

交互操作按键
```shell
P                  # 以CPU使用率排序，默认就是此项
M                 # 以内存的使用率排序
N                  # 以PID排序
q                  # 退出
u                  # 监视特定用户
k                  # 终止指定进程
```
## (六)监控网络状态
```shell
netstat
-a              # 显示所有网络连接
-n                # 显示数字形式的地址和端口
-p                # 查看连接对应的程序PID/Program name
```

`检测主机连接命令ping`
## (七)firewall指令
`netstat -anp | more可以查看端口对应的协议`
`telnet ip 端口号                # 服务端口测试`

firewall格式：
`firewall-cmd --动作-对象=值 --zone=区域 --permanent` 
四个核心动作：
```shell
--add           
--remove
--query
--list
```
三个主要对象：
```shell
-service  服务（如ssh,http）
-port     端口（如80/tcp）
-rich-rule 富规则（复杂规则）
```
打开端口：firewall-cmd --permanent --add-port=端口号/协议
关闭端口：firewall-cmd --permanent --remove-port=端口号/协议
重新载入，才能生效：firewall-cmd --reload
查询端口是否开放：firewall-cmd --query-port=端口号/协议
# 十五、RPM与YUM
## (一)rpm包的管理
rpm包名基本格式：
名称-版本号.适用操作系统
i686、i386表示32位系统，noarch表示通用

查询已安装的rpm列表
rpm指令
```shell
-q           # 查询模式
-a           # 查询模式下查询所有已安装包
-i           # 查询模式下查询软件包信息
-l           # 查询模式下查询软件包中的文件
-r           # 查询模式下查询文件所属软件包
-e          # 卸载
--nodeps    #增加参数强制删除，一般不推荐
-i           # 安装
-v         # 提示
-h          # 进度条、
```
```shell
rpm -q 软件包名称                 # 查询是否安装
rpm -qa | grep 软件包名称
rpm -qi 软件包名称                # 查询软件包信息
rpm -ql 软件包名称                # 查询软件包中的文件
rpm -qf 文件全路径            # 查询文件所属软件包
rpm -e 软件包名称
rpm -hiv 文件全路径              # 安装包
```
## (二)yum
Yum 是一个Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。
`yum [选项] 命令 包名`
常用命令：
```shell
install
remove
update
search
info
list
provides #查文件归属
clean all #清理缓存
makecache #生成缓存
```
查询yum服务器是否有需要安装的软件
`yum list | grep 软件包名称`
安装指定yum包
`yum install 软件包名称`
# 十六、搭建javaEE环境
```shell
[root@hspEdu01 home]# updatedb
[root@hspEdu01 home]# locate hsp.html
/opt/tomcat/apache-tomcat-10.1.52/webapps/ROOT/hsp.html
[root@hspEdu01 jdk]# locate jdk-25_linux-x64_bin.tar.gz
/opt/jdk/jdk-25_linux-x64_bin.tar.gz

```
jdk解压缩文件在`/usr/local/java/jdk-25.0.2`

查看mysql的root密码：
`grep "password" /var/log/mysqld.log`


# 十七、Shell编程
## (一)Shell脚本的执行
脚本格式要求：
1）以`#!/bin/bash`开头
2）有可执行权限

脚本常用执行方式：
1）输入脚本绝对路径或相对路径
例：`./hello.sh`==千万别漏了./==
`/root/shcode/hello.sh`
2）sh 脚本名
`sh hello.sh`
说明：不用赋予x权限
## (二)Shell的变量
变量分为系统变量和用户自定义变量。
系统变量：`HOME,PWD,SHELL,USER,PATH`等等。
显示当前Shell中的所有系统变量：`set`
定义变量：`变量=值`（不要加空格，一般是大写）
变量的值用`$变量`表示
==${} 的用途：==
1. 明确边界：${var}_suffix
2. 设置默认：${var:-default}
3. 字符串操作：${#var} ${var:0:5}
4. 数组操作：${array[@]}
==什么时候用？==
- 变量后有字符时：${name}_xxx
- 需要特殊操作时：${var:-默认值}
- 处理数组时：${fruits[0]}
- 不确定时：都可以用，更安全！
撤销变量：`unset 变量`
声明静态变量：`readonly 变量`
echo后：
1. 双引号“安全模式”：变量展开、命令替换、特殊字符保护，
2. 单引号“字面模式”：变量不展开、命令不替换，
3. 无引号“完全展开模式”：特殊字符不保护
将命令的返回值赋给变量
1）``A=`date`
2）`A=$(date)`
## (三)设置环境变量
```shell
1）export 变量名=变量值     #将shell变量输出为环境变量/全局变量
2）source 配置文件         #让修改后的配置信息立即生效
3）echo $变量名            #查询环境变量的值
```
单行注释：`#`
多行注释：
```shell
:<<!
注释
!
```
## (四)设置参数变量

`$n`（功能描述：n为数字，`$0`代表命令本身，`$1`-`$9`代表第一到第九个参数，十以上的参数需要用大括号包含，如`${10}`)
`$*`（功能描述：这个变量代表命令行中所有参数的==字符串==，把所有的参数看成一个整体）
`$@`（功能描述：这个变量代表命令行中所有参数的==列表==，把每个参数区分对待)
`$#`（功能描述：这个变量代表命令行中所有参数的个数)

运行时就按照$0 $1 $2······的顺序输入
## (五)预定义变量
```shell
$$（功能描述：当前进程的进程号(PID))
$!（功能描述：后台运行的最后一个进程的进程号(PID))
$?（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了
```
## (六)运算符
expr后运算式要空格，乘法用`\*`，如`expr $x \* $y`赋值时整体还要加反引号或$()；
`$(())`变量前可不加$
```shell
A="值"（有空格、shell特殊字符，以波折号开头需要加）
A=数字或布尔值或字符串
A=$变量
A=$(command)
A=`command`
A=$((运算式))
A=$[运算式]
```
关于引号：
1. 双引号“安全模式”：变量展开、命令替换、特殊字符保护，
2. 单引号“字面模式”：变量不展开、命令不替换，
3. 无引号“完全展开模式”：特殊字符不保护
## (七)条件判断
常用测试运算符：
1）= 字符串比较
2）两个整数的比较：
	-lt 小于
	-le 小于等于
	-eq 等于
	-gt 大于
	-ge 大于等于
	-ne 不等于
3）按照文件权限判断：
	-r,-w,-x
4）按照文件类型判断：
	-f 文件存在并且是一个常规文件
	-e 文件存在
	-d 文件存在并且是一个目录
`[ 23 -gt 22  ]`
`[ -f /home/tom/try/code1.sh ]`
`[ ]`为false，非空返回true
`[右边`和`]左边`，测试运算符与参数之间必须是空格，空格多少无所谓，必须要有
## (八)流程控制
```shell
if [ 条件判断式 ]
then
	代码
elif [ 条件判断式 ]
then
	代码
fi
```

```shell
case $变量名 in
"值1")
如果变量的值等于值1，执行此程序
;;
*)
如果变量的值不是以上值，执行此程序
;;
esac
```

```shell
for 变量 in 值1 值2 值3 ···
do
程序
done
```
将值1，值2，值3依次赋给变量，执行循环体
```shell
for (( 初始化;循环控制条件;变量变化 ))
do
代码
done
```

```shell
while [ 条件判断式 ]
do
代码
done
```
## (九)read读取控制台输入
read 选项 参数
-p            # 指定读取值时的提示符（提示文字）
-t            # 指定读数时等待的时间，没有在指定的时间=输入就不再等待了
参数指定读取值的变量名
`read -t 数字 -p "字" 变量名`
==注意是变量名，不是$==
## (十)函数
### (1)系统函数
```shell
[tom@hspEdu01 try]$ basename /home/tom/try/code1.sh
code1.sh
[tom@hspEdu01 try]$ basename /home/tom/try/code1.sh .sh
code1
[tom@hspEdu01 try]$ dirname /home/tom/try/code1.sh
/home/tom/try
```
### (2)自定义函数
```shell
function 函数名(){
	代码
}
```
调用函数和在命令行差不多，函数名 值1 值2······
### (3)定时备份数据库案例
```shell
[tom@hspEdu01 ~]$ mysql -u root -p
mysql> show databases;
mysql> create database 数据库名;
mysql> use 数据库名;

mysql> show tables;
mysql> create table 表名(列名1 数据类型 约束, 列名2 数据类型 约束, ······);
mysql> insert into 表名 values(第一个数据, 第二个数据······);

mysql> select * from 表名
```
例如：create table myorder(id int, name varchar(32));
insert into myorder values(100, 'dog');

```shell
#!/bin/bash
#备份目录
BACKUP=/data/backup/db
#当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)
echo $DATETIME
#数据库的地址
HOST=localhost
#数据库的用户名
DB_USER=root
#数据库的密码
DB_PW=YOUR_PASSWORD
#备份的数据库
DATABASE=hspedu

#创建备份目录，如果不存在就创建
[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"

#备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --databases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz

#将文件处理成.tar.gz
cd ${BACKUP}
tar -czvf $DATETIME.tar.gz ${DATETIME}
#删除对应的备份目录
rm -rf ${BACKUP}/${DATETIME}

#删除10天前的备份文件
find ${BACKUP} -ctime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份数据库${DATABASE} 成功"
```
`[tom@hspEdu01 ~]$ crontab -e`
`30 2 * * * /usr/sbin/mysql_db_backup.sh`

# 十八、Ubuntu
sudo systemctl restart NetworkManager
sudo nmcli networking off && sudo nmcli networking on
ping -c 4 baidu.com
# 十九、APT软件管理和远程登录
## (一)
`apt [选项] 命令 包名`
常用命令：
```shell
install
remove
purge彻底卸载
reinstall
autoremove
update更新软件源列表
upgrade升级所有包
search
show显示包信息
list
policy查看版本策略
depends查看依赖
rdepends查看被谁依赖
clean清理下载的包缓存
autoclean
```
例子：
```sh
sudo apt update
sudo apt install vim -y
sudo apt-get install vim
sudo apt-get remove vim
sudo apt-cache show vim

```

## (二)远程登录
```sh
apt install net-tools
netstat -anp | more

sudo apt-get install openssh-server         #安装SSH服务端和客户端
service ssh restart 或 systemctl ssh restart #启动sshd服务
service ssh status 或 systemctl ssh status
ssh 用户名@IP                                 #远程登录

```

# 二十、Centos8.1的使用

# 二十一、高级篇--日志管理

## (一)系统常用日志

| ==/var/log/boot.log== | 系统启动日志                                                                                |
| --------------------- | ------------------------------------------------------------------------------------- |
| ==/var/log/cron==     | 记录与系统定时任务相关的日志                                                                        |
| /var/log/cups         | 记录打印信息的日志                                                                             |
| /var/log/dmesg        | 记录了系统在开机时内核自检的信总。可以使用dmesg命令直接查看内核自检信息                                                |
| /var/log/btmp         | 记录错误登录的日志，这个文件是二进制文件，不能直接用vi查看，而要使用lastb命令查看。                                         |
| ==/var/log/lastlog==  | 记录系统中所有用户最后一次的登录时间的日志。这个文件也是二进制文件，要使用==lastlog==命令查看                                  |
| ==/var/log/maillog==  | 记录邮件信息的日志                                                                             |
| ==/var/log/message==  | 记录系统重要信息的日志。这个日志文件中会记录LInux系统的绝大多数重要信息。如果系统出现问题首先要检查的就是这个日志文件                         |
| ==/var/log/secure==   | 记录验证和授权方面的信息，只要涉及账户和密码的程序都会记录，比如系统的登录、ssh的登录、su切换用户、sudo授权，甚至添加用户和修改i用户密码都会记录在这个日志文件中 |
| /var/log/wtmp         | 永久记录所有用户的登录、注销信息，同时记录系统的启动、重启、关机事件。是二进制文件，要使用last命令查看                                 |
| ==/var/run/utmp==     | 记录已经登录的用户的信息。这个文件会随着用户的登录和注销而不断变化，只记录当前登录用户的我信息，这个文件不能用vi查看，而使用w、who、users等命令查看       |


在日志文件中包含四栏：时间，主机名，服务或程序，描述
## (二)日志服务管理rsyslogd
查询是否自启动
ps aux | grep "rsyslog" | grep -v "grep"
查询自启动状态
systemctl list-unit-files | grep rsyslog


配置文件：/etc/rsyslog.conf
编辑文件的格式：日志类型.日志级别
（1）日志类型：
1. auth                             ##pam产生的日志
2. authpriv                       ##ssh、ftp等登录信息的验证信息
3. cron                             ##时间任务相关
4. kern                             ##内核
5. lpr                                ##打印
6. mail                              ##邮件
7. mark(syslog)-rsyslog    ##服务内部的信息，时间标识
8. news                              ##新闻组
9. user                                ##用户程序产生的相关信息
10. uucp                             ##unix to unix copy主机之间相关的通信
11. loacl 1-7                       ##自定义的日志设备
（2）日志级别：
12. debug           ##有调试信息的，日志通信最多
13. info               ##一般信息日志，最常用
14. notice           ##最具有重要性的普通条件的信息
15. warning        ##警告级别
16. err                ##错误级别，阻止某个功能或模块不能正常工作的信息
17. crit               ##严重级别，阻止整个系统或整个软件不能正常工作的信息
18. alert            ##需要立刻修改的信息
19. emerg         ##内核崩溃等重要信息
20. none           ##什么都不记录
注意： 从上到下，级别从低到高，记录信息越来越少

## (三)日志轮替
配置文件：/etc/logrotate.conf
默认：
```ps
[tom@hspEdu01 log]$ cat /etc/logrotate.conf | more
# see "man logrotate" for details
# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
dateext

# uncomment this if you want your log files compressed
#compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d

# no packages own wtmp and btmp -- we'll rotate them here
/var/log/wtmp {
    monthly
    create 0664 root utmp
	minsize 1M
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}

# system-specific logs may be also be configured here.

```

每周对日志文件进行一次轮替；
保留最近4周的日志归档文件；
创建新的空的日志文件在日志轮替后；
使用日期作为日志轮替文件的后缀；
如果希望日志文件被压缩，请取消这一行(#compress)的注释；
BPM软件包会将日志轮转的配置信息放置到此目录下，主配置文件通过include指令将其包含进来；
wtmp和btmp这两个日志文件不属于任何软件包，所以我们在这里直接配置它们的轮转规则。

```sh
[tom@hspEdu01 ~]$ cd /etc/logrotate.d
[tom@hspEdu01 logrotate.d]$ ls
bootlog  chrony  cups  firewalld  glusterfs  iscsiuiolog  libvirtd  libvirtd.qemu  mysql  numad  ppp  psacct  samba  sssd  syslog  wpa_supplicant  yum
[tom@hspEdu01 logrotate.d]$ cat bootlog
/var/log/boot.log
{
    missingok
    daily
    copytruncate
    rotate 7
    notifempty
}
```

参数说明：
daily
weekly
monthly
rotate 数字                               # 保留的日志文件个数，0指没有备份、
compress                                 # 日志轮替时旧的日志进行压缩
create mode owner group       # 建立新日志同时指定新日志的权限与所有者和所属组
mail address                              # 当日志轮替时，输出内容通过邮件发送到指定的邮件地址
missingok                                  # 如果日志不存在，则忽略该日志的警报信息
notifempty                                 # 如果日志为空文件，则不进行日志轮替
minsize 大小                               # 日志轮替的最小值，也就是日志一定要到达这个最小值才会轮替，否则就算时间达到也不轮替
size 大小                                      # 日志只有大于指定大小才进行日志轮替， 而不是按照时间轮替
dateext                                        # 使用日期作为日志轮替文件的后缀
sharedscripts                               # 在此关键字之后的脚本只执行一次
prerotate/endscript                     # 在日志轮替之前执行脚本命令
postrotate/endscript                   # 在日志轮替之后执行脚本命令

## (四)查看内存日志

`journalctl                     # 查看全部`(查看的内存日志，重启清空)
`-n 3                           # 查看最新3条`
`--since 19:00 --until 19:10:10 # 查看起始时间到结束时间的日志可加日期`
`-p err                         # 报错日志`
`-o verbose                     # 日志详细内容`
`_PID=1245 _COMM=sshd          # 查看包含这些参数的日志`

# 二十二、定制自己的linux系统


`[root@hspEdu01 ~]# `==fdisk /dev/sdb==
`欢迎使用 fdisk (util-linux 2.23.2)。`

`更改将停留在内存中，直到您决定将更改写入磁盘。`
`使用写入命令前请三思。`

`Device does not contain a recognized partition table`
`使用磁盘标识符 0x1b1ae43a 创建新的 DOS 磁盘标签。`

`命令(输入 m 获取帮助)：`==n==
`Partition type:`
   `p   primary (0 primary, 0 extended, 4 free)`
   `e   extended`
`Select (default p): `==p==
`分区号 (1-4，默认 1)：`==此处回车==
`起始 扇区 (2048-41943039，默认为 2048)：`==此处回车==
`将使用默认值 2048`
`Last 扇区, +扇区 or +size{K,M,G} (2048-41943039，默认为 41943039)：`==+500M==
`分区 1 已设置为 Linux 类型，大小设为 500 MiB`

`命令(输入 m 获取帮助)：`==n==
`Partition type:`
   `p   primary (1 primary, 0 extended, 3 free)`
   `e   extended`
`Select (default p):` ==p==
`分区号 (2-4，默认 2)：`==此处回车==
`起始 扇区 (1026048-41943039，默认为 1026048)：`==此处回车==
`将使用默认值 1026048`
`Last 扇区, +扇区 or +size{K,M,G} (1026048-41943039，默认为 41943039)：`==此处回车==
`将使用默认值 41943039`
`分区 2 已设置为 Linux 类型，大小设为 19.5 GiB`

`命令(输入 m 获取帮助)：`==w==
`The partition table has been altered!`

`Calling ioctl() to re-read partition table.`
`正在同步磁盘。`

```ps
[root@hspEdu01 ~]# mkfs -t ext4 /dev/sdb1
[root@hspEdu01 ~]# mkfs -t ext4 /dev/sdb2
[root@hspEdu01 ~]# mkdir -p /mnt/boot /mnt/sysroot
[root@hspEdu01 ~]# mount /dev/sdb1 /mnt/boot
[root@hspEdu01 ~]# mount /dev/sdb2 /mnt/sysroot
[root@hspEdu01 ~]# grub2-install --roote-directory=/mnt /dev/sdb
[root@hspEdu01 ~]# rm -rf /mnt/boot/*
[root@hspEdu01 ~]# cp -rf /boot/* /mnt/boot/
[root@hspEdu01 ~]# cd /mnt/boot/grub2
[root@hspEdu01 grub2]# ls
device.map  fonts  grub.cfg  grubenv  i386-pc  locale
[root@hspEdu01 grub2]# vim grub.cfg

[root@hspEdu01 grub2]# mkdir -pv /mnt/sysroot/{etc/rc.d,usr,var,proc,sys,dev,lib,lib64,bin,sbin,boot,srv,mnt,media,home,root}
[root@hspEdu01 grub2]# cp /lib64/*.* /mnt/sysroot/lib64/
[root@hspEdu01 grub2]# cp /bin/bash /mnt/sysroot/bin/

[root@hspEdu01 ~]# mount /dev/sdb2 /mnt/sysroot
[root@hspEdu01 ~]# cp /bin/ls /mnt/sysroot/bin/
[root@hspEdu01 ~]# cp /sbin/reboot /mnt/sysroot/sbin/


```

# 二十三、linux内核源码和内核升级
## (一)查看内核源码
### 1.树结构
```sh
[root@hspEdu01 ~]# cd /opt/linuxsrc
[root@hspEdu01 linuxsrc]# ls
linux-0.01
[root@hspEdu01 linuxsrc]# tree linux-0.01/
linux-0.01/
└── linux
    ├── boot
    │   ├── boot.s
    │   └── head.s
    ├── fs
    │   ├── bitmap.c
    │   ├── block_dev.c
    │   ├── buffer.c
    │   ├── char_dev.c
    │   ├── exec.c
    │   ├── fcntl.c
    │   ├── file_dev.c
    │   ├── file_table.c
    │   ├── inode.c
    │   ├── ioctl.c
    │   ├── Makefile
    │   ├── namei.c
    │   ├── open.c
    │   ├── pipe.c
    │   ├── read_write.c
    │   ├── stat.c
    │   ├── super.c
    │   ├── truncate.c
    │   └── tty_ioctl.c
    ├── include
    │   ├── a.out.h
    │   ├── asm
    │   │   ├── io.h
    │   │   ├── memory.h
    │   │   ├── segment.h
    │   │   └── system.h
    │   ├── const.h
    │   ├── ctype.h
    │   ├── errno.h
    │   ├── fcntl.h
    │   ├── linux
    │   │   ├── config.h
    │   │   ├── fs.h
    │   │   ├── hdreg.h
    │   │   ├── head.h
    │   │   ├── kernel.h
    │   │   ├── mm.h
    │   │   ├── sched.h
    │   │   ├── sys.h
    │   │   └── tty.h
    │   ├── signal.h
    │   ├── stdarg.h
    │   ├── stddef.h
    │   ├── string.h
    │   ├── sys
    │   │   ├── stat.h
    │   │   ├── times.h
    │   │   ├── types.h
    │   │   ├── utsname.h
    │   │   └── wait.h
    │   ├── termios.h
    │   ├── time.h
    │   ├── unistd.h
    │   └── utime.h
    ├── init
    │   └── main.c
    ├── kernel
    │   ├── asm.s
    │   ├── console.c
    │   ├── exit.c
    │   ├── fork.c
    │   ├── hd.c
    │   ├── keyboard.s
    │   ├── Makefile
    │   ├── mktime.c
    │   ├── panic.c
    │   ├── printk.c
    │   ├── rs_io.s
    │   ├── sched.c
    │   ├── serial.c
    │   ├── sys.c
    │   ├── system_call.s
    │   ├── traps.c
    │   ├── tty_io.c
    │   └── vsprintf.c
    ├── lib
    │   ├── close.c
    │   ├── ctype.c
    │   ├── dup.c
    │   ├── errno.c
    │   ├── execve.c
    │   ├── _exit.c
    │   ├── Makefile
    │   ├── open.c
    │   ├── setsid.c
    │   ├── string.c
    │   ├── wait.c
    │   └── write.c
    ├── Makefile
    ├── mm
    │   ├── Makefile
    │   ├── memory.c
    │   └── page.s
    └── tools
        └── build.c

12 directories, 88 files
```

### 2.文件夹
`[root@hspEdu01 linux]# ll`
`总用量 36`
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 boot`和系统引导相关的代码
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 fs`存放linux支持的文件系统代码
`drwxr-xr-x. 5 root root 4096 2月  15 09:42 include`存放Linux核心需要的头文件，比如asm,linux,sys
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 init`
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 kernel`和系统内核相关的源码
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 lib`存放库代码
`-rw-r--r--. 1 root root 2186 2月  15 09:42 Makefile`
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 mm`和内存管理相关的代码
`drwxr-xr-x. 2 root root 4096 2月  15 09:42 tools`

### 3.main函数
`void main(void)		/* This really IS void, no error here. */`
`{			/* The startup routine assumes (well, ...) this */`
`/*`
 * `Interrupts are still disabled. Do necessary setups, then`
 * `enable them`
 `*/`
	`time_init();`初始化运行时间
	`tty_init();`tty初始化
	`trap_init();`陷阱门（硬件中断向量）初始化
	`sched_init();`调度程序初始化
	`buffer_init();`缓冲管理初始化
	`hd_init();`硬盘初始化
	`sti();`所有初始化工作完成，开启中断
	`move_to_user_mode();`进入到用户模式
	`if (!fork()) {		/* we count on this going ok */`
		`init();`
	`}`
`/*`
 *   `NOTE!!   For any other task 'pause()' would mean we have to get a`
 * `signal to awaken, but task0 is the sole exception (see 'schedule()')`
 * `as task 0 gets activated at every idle moment (when no other tasks`
 * `can run). For task0 'pause()' just means we go check if some other`
 * `task can run, and if not we return here.`
 `*/`
	`for(;;) pause();`
`}`
## (二)内核升级
查看内核最新版本：https://www.kernel.org/
下载解压最新版本：
`wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.18.10.tar.gz`
v6.x里6是大版本号；6.18.10选稳定版本号
`tar -zxvf linux-6.18.10.tar.gz`
`uname -a` 查看当前的内核版本
`yum info kernel -q` 检测内核版本，显示可以升级的内核
`yum update kernel` 升级内核
`yum list kernel -q` 查看已经安装的内核

# 二十四、备份与恢复
yum -y install dump
yum -y install restore
## (一)dump
### 1.备份操作
`dump -[核心信息1:级别+选项] -[其他选项信息] [核心信息2:要备份的设备或目录]`
```sh
核心信息1：
备份级别：0-9，0表示完全备份，1-9表示增量备份,不写默认9级
-u：                   备份成功后更新/etc/dumpdates备份记录文件
-j：                  用bzllib压缩备份（生成.dump.bz2格式）
-z：                  用zlib压缩备份（生成.dump.gz格式）
-v：                  显示详细信息
-n：                  备份完成时通知所有operator组用户
```
```sh
其他选项：
-f 备份后文件名：        指定备份后文件名或设备
-T 日期：              指定备份时间
```

例子：
dump -0uj -f /opt/boot.bak.bz2 /boot
dump -1uj -f /opt/boot.bak1.bz2 /boot
如果备份文件或目录，不支持增量备份，只能使用0级别备份
dump -0j -f /opt/etc.bak.bz2 /etc/
### 2.显示查看
`dump -W`                   显示需要备份的文件以及最后一次备份的层级、时间、日期
`cat /etc/dumpdates`    查看备份时间文件
## (二)restore
`restore [核心信息1:模式+参数] -[其他选项信息] [核心信息2:指定位置]
```sh
模式（只能选1个）：
-i：                  交互模式，可选，指定恢复到的目录；
-t：                  查看模式，不需要，指定位置
-C：                  对比模式，可选，指定要比较的目录
-r：                 完整恢复模式，可选，指定恢复到的目录
-x 文件或目录：         提取模式，可选，指定提取到的目录
```
```sh
其他选项：
-f 备份文件名：         指定备份文件
-T 日期：              指定恢复的时间点
-v：                   显示详细信息
```
例子：
`restore -C -f boot.bak1.bz2`没有指定位置默认当前目录
`restore -t -f boot.bak0.bz2`
`restore -r -f /opt/boot.bak0.bz2`没有指定位置默认当前目录
# 二十五、webin和bt运维工具
## (一)webmin

下载地址：https://download.webmin.com/download/yum/
安装：
	1.安装缺失的 Perl 模块
	`yum install -y perl-Digest-MD5 perl-Digest-SHA perl-Net-SSLeay` 
	2.重新安装 Webmin
	`rpm -ivh webmin-2.621-1.noarch.rpm`
重置密码：`[root@hspEdu01 webmin]# /usr/libexec/webmin/changepass.pl /etc/webmin root YOUR_PASSWORD`
修改webmin的服务端口（==不必要，有可能连不上，用10000就行==）：
`[root@hspEdu01 webmin]# vim /etc/webmin/miniserv.conf`
`port=6666`
`listen=6666`
重启webmin：
	/etc/webmin/restart
	/etc/webmin/start
	/etc/webmin/stop
防火墙开放6666端口：
```ps
[root@hspEdu01 webmin]# firewall-cmd --zone=public --add-port=6666/tcp --permanent
[root@hspEdu01 webmin]# firewall-cmd --reload
[root@hspEdu01 webmin]# firewall-cmd --zone=public --list-ports
8080/tcp 6666/tcp
```
登录webmin：http://192.168.24.100:6666

## (二)bt
`[root@hspEdu01 bt]# yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh`
```ps
========================面板账户登录信息==========================

 【云服务器】请在安全组放行 29040 端口
 外网ipv4面板地址: https://YOUR_SERVER_IP:29040/819caa99
 内网面板地址:     https://192.168.24.100:29040/819caa99
 username: YOUR_USERNAME
 password: YOUR_PASSWORD
 

 浏览器访问以下链接，添加宝塔客服
 https://www.bt.cn/new/wechat_customer
==================================================================

```
查看用户名和密码：`bt default`





