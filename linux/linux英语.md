# 一、关机重启
sync=synchronization/ˌsɪŋkrənaɪ'zeɪʃ(ə)n/同步
shutdown-h=halt
shutdown-r=reboot
shutdown-c=cancel
shutdown-P=poweroff

# 二、登录注销
su=switch user

# 三、用户管理
useradd -d=directory
useradd -g=group
userdel -r=remove
usermod=user modify
passwd=password
id=identity

# 四、运行级别
init=initialization
systemctl=sysytem control
default用户缺失或省略选择时，系统默认的选择

# 五、帮助
man=manual

# 六、文件目录类
pwd=print working directory
ls=list
ls -a=all
ls -l=long
ls -h=human-readable
ls -R=recursive
cd=change directory
mkdir=make directory
mkdir -v=verbose详细
rmdri=remove diretory
mkdir -p=parents
cp=copy
cp -r 或 cp -R=recursive
rm -f=force
cat=concatenate连接
cat -n=number
ln=link
ln -s=symbolic
# 七、时间日期指令
cal=calendar

# 八、搜素查找类
updatedb=update database
grep=global regular expression print全局正则表达式打印
grep -n=line number
grep -i=ignore-case
grep -r=recursive
grpe -v=invert match反向匹配
grep -l=files with match
grep -c=count
grep -w=word regexp
grep -E=extended regexp
grep -o=only-matching


# 九、压缩和解压缩类
gzip =GUN zip
gunzip=GUN unzip
tar=tape archive磁带归档
tar -c=create创建新的归档文件
tar -x=extract从归档中提取文件
tar -z=zip/gzip使用gzip压缩/解压
tar -v=verbose显示处理过程
tar -f=file指定归档文件名
tar -C=change directory

# 十、组管理与权限管理
chmod=change mode

# 十一、定时任务调度
crontab=cron table时间计划表
crontab -e=edit
crontab -l=list
crontab -r=remove
atq=at queue
atrm =at remove
at -m=mail
at -l=list
at -d=delete
at -v=version
at -c=concatenate
at -q=queue
at -f=file
at -t=time

# 十二、liunx磁盘分区、挂载
lsblk=list block devices
fdisk=format disk修改分区表（磁盘级）
m=manual
p=print
n=new
d=delete
w=write
q=quit
mkfs=make filesystem格式化（分区级）
mkfs -t=type
/mnt=mount 挂载
fstab=filesysytems table文件系统表
df=disk free磁盘空闲空间
du=disk usage磁盘使用情况
du -s=summary
yum=yellowdog updater, modified黄狗更新器改进版

# 十三、网络配置
ipconfig=internet protocol configuration  IP地址配置
ifconfifg=interface configuration网络接口配置
ping=packet internet groper


# 十四、进程管理
1. ps=process status
2. ps -a=all
3. ps -e=every
4. ps -u=user
5. ps -f=full
6. MEM=memory内存
7. VSZ=Virtual Memory Size
8. RSS=Resident Set Size
9. TTY=teletype终端
10. sshd=secure shell daemon提供安全的远程登录服务
11. gedit=GNOME Editor
12. init.d=initialization directory初始化进程目录

13. telnet=teletypewriter network电传打字机网络，用于网络端口测试
14. cmd=command prompt命令提示符

15. top=Table Of Processes
16. top -d=delay interval设置刷新延迟
17. top -i=idle processes忽略空闲进程
18. top -p=process ID
19. top交互按键P=Percent CPU
20. top交互按键N=Numeric PID
21. zombie 僵尸
22. ni=Niced进程的CPU使用率
23. id=idle空闲的
24. PR=priority动态优先级（用户不可设置）
25. NI=nice静态优先级（用户可设置，影响PR）
26. VIRT=virtual memory size虚拟内存
27. RES=resident set size常驻内存
28. SHR=shared memory size共享内存

29. netstat=network statistics网络统计，显示各种网络连接和网络状态信息
30. netstat -a=all
31. netstat -n=numeric
32. netstat -p=program
# 十五、RPM与YUM
1. noarch=no architecture与架构无关

2. RPM=Red Hat Package Manager红帽软件包管理器
3. rpm -q=query查询
4. rpm -qi=information
5. rpm -ql=list
6. rpm -qr=requires依赖
7. rpm -qf=file
8. rpm -e=erase
9. rpm -e --nodeps=no dependencies忽略依赖关系
10. rpm -h=进度条
11. rpm -i=install
12. rpm -v=verbose提示
# 十六、搭建javaEE环境

mysql=My Structured Query Language结构化查询语言
# 十七、Shell编程
expr=expression
# 十八、Ubuntu
# 十九、APT软件管理和远程登录
apt=Advanced Packaging Tool
# 二十、Centos8.1的使用
# 二十一、高级篇--日志管理

1. var=variable可变数据目录
2. cups=Commom UNIX Printing System通用打印服务日志
3. dmesg=diagnostic message内核诊断信息
4. btmp=bad login attempts错误登录记录

5. conf=configuration配置
6. rsyslog=Rocket-fast system log
7. auth=authentication认证
8. authpriv=authentication private
9. kern=kernel内核
10. lpr=line printer打印服务
11. info=information
12. crit=critical
13. emerg=emergency

14. log rotate=日志轮替
15. dateext=date extension

# 二十二、定制自己的linux系统
grub=Grand Unified Bootloader大统一启动加载器

# 二十三、linux内核源码和内核升级

wget=web get网络获取
uname=Unix Name显示系统信息
uname -a=all

# 二十四、备份与恢复
1. yum -y=yes
2. dump -u=update
3. dump -j=bzip2
4. dump -z=gzip
5. dump -v=verbose
6. dump -n=notify
7. dump -W=what
8. restore -i=interactive
9. restore -t=Table of contents
10. restore -C=Compare
11. restore -r=recover
12. restore -x=extract

# 二十五、webin和bt运维工具
