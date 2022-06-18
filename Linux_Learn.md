Linux简介：一种操作系统用于管理计算机资源的系统软件Linux注重安全性和稳定性，高并发处理能力，没有优异的可视化界面
Linux主要发行版本：Ubuntu(乌班图),Redhat(红帽),CentOS

## 安装Linux操作系统

1. 虚拟机：可以用软件的方法模拟出一套具有完整的硬件系统功能的，运行在一个完全隔离的系统环境的完整计算机系统。

+ Linux的文件目录结果
  + Bin： 存放的是命令的文件 用户的可执行文件可放在 bin->usr/bin
  + user/local/bin：存放用户自己的可执行文件 和 上面的大差不差
  + etc：放的是配置文件 最重要的是环境变量。etc/profile
  + opt：这是给Linux文件额外安装的软件的目录 比如：JDK Tomcat Mysql 相当于Windows中的program
  + 快照的使用：使用快照可以实现对系统的回滚 类似于sql的事务管理

  

## Linux远程操作？

+ Xshell 远程控制台
+ Xftp 传输文件 ftp可能会有中文乱码 可以再文件中修改编码模式为UTF-8
+ 虚拟机的迁移和删除：





## 使用vim开发一个简单的java程序

+ 正常模式：可以编辑 删除之类的
+ 插入模式：按i进入，可以添加自己的内容
   命令行模式：	先输入esc表示退出vim 再输入： 进入命令行模式
+ 退出：:wq 表示写入并退出(write Quite) :q 退出 :q! 强制退出不保存

## 快捷键的使用练习：

1. 拷贝当前行 yy，当前行向下5行 5dd 黏贴p
2. 删除当前行 dd，删除当前行向下的5行 5dd
3. 在文件中查找字符串 按“/”进入命令行输入自己要找的字符串 输入n可以到下一个匹配支付串处
4. 设置文件的行号(:set nu)，取消文件的行号(set nonu)
5. 编辑/etc/profile文件，使用快捷键到该文件的最末尾G和最首行gg
6. 在文件中输入“hello”，回到一般模式（按esc）然后撤销这个动作 u 
7. 编辑 /etc/profile文件，并将光标移动到 20行 shift+g 需要注意的是 不要使用小键盘 因为回到是进入编辑模式

+ 关机 重启命令
  + shutdown -h now 立即关机 -h中的h表示halt表示停止
  + shutdown -h 1 一分钟后关机
  + shutdown -r now  现在重启计算机
  + halt
  + reboot
  + sync  把内存的数据同步到磁盘
+ 注意细节：
  1. 不管是重启还是关闭系统，首先要运行sync命令 把内存中的数据写到磁盘中
  2. 目前的shutdown/reboot/halt 等命令均已经在关机前进行sync，老韩提醒：小心驶得万年船
+ 用户的登录和注销：
  + 基本介绍
    + 登陆时尽量不使用root账号登录，因为他的权限比较高，避免失误操作。可以利用普通用户登录，登陆后再用“ su - 用户名命令“ 来切换成管理员身份
    + 在提示符下输入logout即可注销用户

## 用户管理

+ Linux是一个多用户的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统

### 添加用户

+ useradd
+ 应用案例
  + 添加一个用户sea
    + 细节说明：
      1. 当前用户创建成功后，会自动创建用户名同名的home文件
      2. 也可以通过useradd -d 指定文件目录 新的用户名，给新创建的用户指定home目录
      3. 需要注意的是 root下才能有这样高的权限哦
  + 指定/修改密码：
    + 基本语法
      + passwd 用户名 xxxx
    + 给Ocean用户指定密码
      + pwd 显示当前用户所在的用户 print working directory
      + 

### 删除用户

+ userdel 用户名
+ 应用案例：
  1. 删除用户Ocean，但是要保留home目录 userdel Ocean  保留目录
  2. 删除用户以及用户主目录 userdel -r admin 删除了目录
+ 细节说明：
  + 是否能够保留home目录的讨论？

###  用户的统一管理

+ 根据权限的不同划分不同的组

  + 用户组

    + 介绍：

      + 类似于角色，系统可以有共性/权限的多个用户进行统一的管理

    + 新增组

      + 指令groupadd xxxx  -----创建xxxx组

    + 删除组

      + 指令：groupdel xxxx --------删除xxxx组

    + 增加用户时直接加上组

      + 指令：useradd -g 用户组 用户名

        注意：Linux中没有消息就是好消息！ 细节：如果吗，没有指定用户的组会创建一个和用户同名的组 并将其放入

      + 如何修改用户的组呢？

        + usermod -g spring ocean 
        + 组里面有什么样的权限不用着急 后面会讲

      + 用户和组相关文件

        + /etc/passwd
          + 用户的配置文件，记录了用户的各种信息
          + 每行的含义：用户名：口令：用户标识号（UID):组标识号（GID）：解释性描述：主目录：登录Shell、
        + /etc/shadow文件
          + 口令的配置文件
          + 每行的含义：登录名：加密口令：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间：失效时间：标志
        + /etc/group文件
          + 组（group）的配置文件，记录Linux包含的组的文件的信息每行含义：组名：口令：组标识号：组内用户列表

## 常用的使用指令

+ 基本介绍:

  0. 关机
  1. 单用户（找回丢失密码）
  2. 多用户状态没有网络服务
  3. 多用户状态有网络服务
  4. 系统未使用保留给用户
  5. 图形界面
  6. 系统重启

  +  常用的运行级别是3和5 也可以指定默认的运行级别，后面演示

+ 应用实例

  命令：init[0123456]应用案例：通过init来切换不同的运行级别，比如动5->3，然后关机

+ 如何指定一个默认的init 级别呢？

  + /tec/inittab文件中

    在CentOS7之前，/etc/inittab文件中。

    进行了简化，如下：

    ​	multi-user.target:analogous to runlevel 3

    ​	graphical.target:analogous to runlevel 5

    #To view current default target,run:
    	systemctl get-default

    #To set a default target,run:
    	systemctl set-default TARGET.target

  

  + 如何找回用户密码？
    1. 在开机快速按e 进入edit界面
    2. 在UTF-8后面加入以下的字符
    3. ![pWxuB.png](https://s1.328888.xyz/2022/06/14/pWxuB.png)
    4. 使用快捷键Ctrl+X进入单用户模式
    5. 接着，在光标闪烁的位置中输入：mount -o remount, rw / ,完成后按键盘的回车键。
    6. 在新的一行最后面输入：passwd，完成后按回车键。输入密码，然后再次确认密码即可，密码修改成功以后，会显示password。。。。的样式，说明修改成功
    7. 输入touch /.autorelabel            Enter
    8. 输入exec /sbin/init          Enter 时间较长 上来还会不过一个enable 不管他

  ## 手册

  + man 获取帮助文件

    + 基本语法：man[命令或者配置文件] 功能获取帮助信息
    + 案例：查看ls命令的帮助信息 man xx （xx表示目标命令）
      + 需要特别指出的是在Linux中，选项是可以组合使用的 比如ls -a 和ls -l 组合变为ls-al
    + 在Linux下

  + help指令：

    + 基本语法：help 命令 

    + 功能描述：获取shell内部的命令的帮助信息

      

  ### 目录操作

  + pwd 显示当前文件目录的绝对路径

  + ls 显示当前目录下的所有子目录和文件

    + -a 显示所有 包含隐藏的
    + -l 显示一些信息

  + mkdir是默认创建单极目录

    + 如果需要使用多级目录使用选项 mkdir -p /home/dog

  + 目录的删除 rmdir remove directory

    + 默认是删除空目录，如果不是空目录则无法删除
    + 提示：如果删除非空目录，需要使用 rm -rf xxxx（要删除的目录）

  + touch指令

    + touch指令创建空文件
      + 基本语法
        + touch的文件名称
      + 应用实例
        + 在/home目录下，创建一个空文件

  + cp指令

    + cp fuckname.txt /home/ 复制fuckname.txt 到 home文件夹下
    + 递归拷贝：cp -r /home/bbb /opt/
    + 使用细节：强制覆盖： \cp  ---->   \cp -r /home/bbb /opt/

  + mv

    + mv cat.txt pig.txt 等价于重命名
    + mv pig.txt /root 移动 剪切
    + mv pig.txt /root/donkey.txt 移动并且重命名
    + mv移动整个目录 将opt移动到home下

  + cat指令

    + 查看文件内容

    + cat查看文件更安全 只能查看 不能修改

    + cat -n Hello.java          -n显示行号

      + 管道命令 为了方便，一般会带上管道命令  | more
      + example: cat -n /etc/profile | more
      + 使用more查看文件 more /opt/profile

    + less指令

      + less指令用来分屏查看文件内容，他的功能与more指令类似，但是比more指令更加强大，支持各种显示终端less指令在显示文件内容时，并不是一次将整个文件全部加载之后才显示，而是根据显示需要加载内容，对于显示具有较高的效率
      + 实时的

    + 基本语法

      + less 要查看的文件

        ​	操作： 

         	1. 空格键     向下翻页
        	2. pagedown 向下翻页
        	3. pageup 向上翻页
        	4. /字符串 匹配
        	5. ？支付串 匹配
        	6. q 离开less程序

      + echo指令

        + echo输出内容到控制台
        + 基本语法
          + echo -(?) databaseLearn.md
            + 实例：
            + 使用echo指令输出环境变量 比如$PATH
            + 使用echo指令输出Hello，world

      + Head指令

        + head用于显示文件的开头内容，默认情况下head指令显示文件的前十行
        + 基本语法
        + head 文件
        + head -n 文件
          + 案例
            + 查看/etc/profile 的前面5行代码
            + [root@192 ~]# head -n 5 databaseLearn.md 

      + tail指令

        + tail database.md 查看最后的10行（默认）
        + tail -n 15 database.md 指定行数查看尾部
        + tail -f database.md 功能描述：实现追踪文档的所有更新

      + />指令和>>指令

        + />输出重定向和>>追加
        + 基本语法
          + ls -l > Helllo.java 列表的内容写到Hello.java （覆盖写）
          + ls -al >>Hello.java 列表的内容追加到文件Hello.java中
          + cat database.md > Hello.java   将文件1的内容覆盖到文件2中
          + echo "Hello" >>Hello.java 将内容追加到文件中 
            + 注意退出监控是Ctrl+C

      + ln指令

        + 软连接也称为符号链接，类似于windows里面的快捷方式，主要存放了链接其他文件的路径
        + 基本语法
          + ln -s 源文件目录 软连接名 （给源文件创建一个软连接）
          + 当我们使用pwd是显示的是软连接所在的路径
          + 需要注意的是不用形成回路

      + history指令

        + 查看已执行过的历史命令，也可以执行历史命令
        + 基本语法
          + history 功能描述:查看已执行的历史命令



## 日期指令

 + date指令显示日期
   1. date
   2. date +%Y
   3. date +%m
   4. date +%d
   5. date "+%Y-%m-%d %H:%M:%S" 
   6. date -s "2022-11-03 20:02:10" 设置时间

## 搜索查找类

+ find指令
  + find指令将从指定的目录向下递归各个子目录，将满足条件的文件或者目录现在在终端
  + 基本语法：
    + find [搜索范围][选项]
      + 选项说明
        + -name<查询方式>
        + -user<用户名>
        + -size<文件大小>
  + eg: find /home -name admin

+ locate指令
  + locate指令可以快速地定位文件路径，locate指令利用实现建立的系统中所有文件名称及其路径的local数据库实现快速定位给定的文件。local指令无需遍历整个文件系统，查询速度较快。为了保证查询结果的

+ which

  + 查看某一个指令的地址
  + which ls

+ grep指令

  + grep过滤查找，管道符，“|” ，表示将一个命令的处理结果输出传递给后面的命令处理

    + 效果展示

    + [root@192 ~]# cat data.xml | grep "Hello"
      Hello
      Hello this is a row used test by grep command

      -------------------

    + [root@192 ~]# cat data.xml | grep -n "Hello"

      1:Hello

      3:Hello this is a row used test by grep command

      --------------------------

+ 压缩和解压类

  + gzip/gunzip指令

    + gzip用于压缩文件，gunzip用于解压

  + 基本语法

    + gzip 文件
    + gunzip 文件.gz

  + zip/unzip

    + zip用于压缩文件，unzip用于解压文件 在项目中打包发布中很有用的

      + zip -选项 xxx.zip

      + unzip -选项 xxx.zip

      + 常用选项

        + -r 递归压缩，即压缩目录
        + -d 指定解压文件的存放目录

        [root@192 Hello]# zip -r  home.zip /home/

        [root@192 Hello]# unzip -d /opt/tmp home.zip 

  + tar 指令

    + 即可打包压缩文件也可以压缩文件夹
    + 可以压缩 也可以解压缩
    + [root@192 home]# tar -zcvf pc.tar.gz /home/pig /home/cat 压缩
    + [root@192 home]# tar -zxvf myhome.tar.gz /home/ 解压

  + ls -alh

    + a表示all
    + h表示human 大小表示为人可读的
    + l表示list 列出

  + chown指令

    + 修改文件的所有者
      + chown 用户名 文件名
        + 所有者亦需要root权限

  + 组的管理

    + 

      [root@192 home]# groupadd master
      [root@192 home]# useradd -g master jerry

    + [root@192 home]# id jerry
      uid=1004(jerry) gid=1005(master) 组=1005(master)

    + 所在组？

      + 一个文件被创建以后 文件所有者所在的组就是文件的所在组

      + -rw-r--r--. 1 jerry master 0 6月  16 19:07 readme.md jerry创建的文件

      + 修改文件所在组chgrp 目标组 文件名

      + 权限      所有者 所在组 大小    修改时间  文件名  

      + chgrp spring orange.txt
        -rw-r--r--. 1 root spring    0 6月  16 19:21 orange.txt

      + r代表读 w代表写 x代表执行

      + 第一列 一共十位 

        + 第0为是文件类型	
          + d目录 	
          + l 链接文件 
          + c字符设备文件 鼠标键盘 
          + b块设备  U盘 打印机
          + -

        + 第1-3位 确定文件的所有者拥有的权限 ---user
        + 第4-6位确定所属组拥有该文件的权限 --group
        + 第7-9位确定其他组用户拥有该文件的权限 --Other

    + 其他组

      + 除了文件所在的组 其他的用户都是文件的其他组
      + 改变用户的所在组
        + usermod -g 新组名 用户名
        + usermod -d 目录名 用户名 改变用户的 初始目录。特别说明：用户需要有进入新目录的权限

    + 修改-chmod

      + u user g group o Other Person a all
      + chmod u=rwx,g=rx,o=x 文件目录名
      + chomod o+w 文件目录名
      + chmod a-x 文件目录名
      + +-= +：添加权限 -：删除权限 =：赋予权限

      [root@192 home]# chmod u=rwx,g=rx,o=x mycal 
      [root@192 home]# ls -hal

      -rwxr-x--x.  1 root  root    644 6月  15 18:21 mycal

    + 使用数字代表权限：r-4 w-2 x-1  注意   1:x 2:w 3:wx 4:r 5:rx 6:rw 7:rwx

+ crontab

  + crond 定时调用：脚本 shell 比如对数据库的增删改查
  + 任务调度快速入门
    + 设置任务调度文件: /etc/crontab
    + 设置个人任务调度: 执行crontab -e 命令
    + 接着输入任务到调度文件
    + 如：*/1 * * * * ls -l /etc/ > /tmp/to.txt

  ```java
  *min *h *d *m *week    
  */1 * * * *  每一分钟来一次
  ```

  + ​     。/ 注意 前面有空格

  + 任务调度

    + 现在.sh 文件中写脚本

    + 注意给目标用户执行权限 没有权限的话会执行失败的

    + crontable -e 写时间表达式 

      corntab -r 终止任务调度

      crontab -l 列出当前有哪些任务调度

      service crond restart 重启任务

  + at定时任务

    + 基本介绍
      1. at命令是一次性定时计划任务，at的守护进程atd会以后台模式运行，检查作业队列来运行
      2. 默认情况下，atd守护进程的每60s检查工作队列，有作业时，会检查作业的运行时间，如果时间与当前时间匹配，者作业运行
      3. at命令是一次性定时计划任务，执行完一个任务后就不再执行该任务了
      4. 在使用at命令的时候，一定要保证atd进程的启动，可以使用相关的指令来查看
    + at命令格式
      + at 【选项】 时间
      + ctrl+D 结束at的命令输入 2次
      + ps -ef 打印所有的进程 ps -ef | grep atd 过滤
      	 1	Sun Jun 19 17:00:00 2022 a root
        2	Sun Jun 19 17:00:00 2022 a root
          [root@192 home]# atrm 1  演示删除1号atd
          [root@192 home]# atq
        2	Sun Jun 19 17:00:00 2022 a root

  

  ## Linux 磁盘分区 管理

  + Linux来说无论分几个分区，分给哪一个目录用，它归根结底只有一个根目录，一个独立的唯一的文件结构，Linux中每一个分区都是用来组成整个文件系统的一部风
  + Linux采用了一种叫做载入的处理方案，他的整个文件系统中包含了一套文件和目录，并且一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得
  + lsblk 查看分区的情况
  + 分区命令 fdisk /dev/sdb  dev表示设备 device
    + 开始对/sdb分区
      + m显示命令列表
      + p显示磁盘分区 同 fdisk -l
      + n 新增分区
      + d 删除分区
      + w 写入并退出
    + 说明：开始分区输入n，新增分区p，分区类型为主分区。两次回车默认剩余全部空间。最后输入w写入分区并退出，若不保存退出输入q。
    + mkfs -t ext4 /dev/sdb1       格式化磁盘
    + 挂载：
      + mount /dev/sdb1 /newdisk 挂载分区
      + umount /dev/sdb1 卸载分区
      + 这种挂载 在reboot只会会失效 需要重新挂载
    + 如何自动挂载 永久挂载呢？
      + 通过修改/etc/fstab 实现挂载
      + 添加完成后执行mount -a 立即生效
      + du -nac --max -depth=1 /opt

  

  

  ## Linux中网络管理

  + 找到网络管理的文件
  + [root@192 etc]# cd sysconfig
  + [root@192 sysconfig]# cd network-scripts/
  + [root@192 network-scripts]# vim ifcfg-ens33 
  + 第一种方式
    + 说明：登陆后，通过界面来设置主动获取IP 特点：Linux启动后会主动获取IP 缺点是每次自动获取的IP地址可能不一样
  + 设置主机名和hosts映射
    1. 为了方便记忆，可以给Linux系统设置一个主机名，也可以根据需要修改主机名
    2. 指令hostname：查看主机名
    3. 修改文件在/etc/hostname指定
    4. 修改后，重启生效

  

  ## Linux的进程管理

  + 基本介绍
    + ps命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状态。可以不加任何参数
    + PID 进程表示符
    + TTY 终端记号
    + TIME 此进程所消耗CPU时间
    + CMD 正在执行的命令或者进程名
    + ps -a：显示终端的所有进程信息
    + ps -u： 以用户的格式显示进程的信息
    + ps -x 显示后台进程的运行参数
    + ps -aux | grep sshd
    + R-运行 D-短期等待 Z-僵死进程
    + VSZ 虚拟内存占用
    + RSS 物理内存占用
  + 应用实例
    + 以全格式显示当前所有的进程，查看进程的父进程
      + ps -ef 是以全格式显示当前所有的进程
      + -e 显示所有进程 -f 全格式
      + ps -ef | grep xxx

  user       PID     FPID

  root       1235      1  0 10:09 ?        00:00:00 /usr/sbin/sshd -D
  root       1682   1235  0 10:09 ?        00:00:00 sshd: root@pts/0
  root       3156   2973  0 10:16 pts/0    00:00:00 grep --color=auto sshd

  + 终止进程

    + 若是某个进程执行一半需要停止，或是已经消耗了大量系统资源时，需要考虑停止进程。使用kill命令来完成此项任务

  + 基本语法

    + kill 选项 进程号 通过进程号杀死进程
    + killall 进程名称 通过进程名称杀死进程，也支持通配符，这在系统因为负载过大而变得很慢时很有用

  + 常用选项

    + -9：表示强制终止进程

  + 最佳实践：

    + 踢掉某一个非法的登录用户

      + [root@seaOcean ~]# ps -aux | grep sshd
        root       1235  0.0  0.2 112900  4312 ?        Ss   10:09   0:00 /usr/sbin/sshd -D
        root       1682  0.0  0.2 158856  5560 ?        Ss   10:09   0:00 sshd: root@pts/0

        #### root       3674  0.3  0.2 158856  5496 ?        Ss   10:25   0:00 sshd: monkeyone [priv]

        monkeyo+   3683  0.0  0.1 158856  2504 ?        S    10:25   0:00 sshd: monkeyone@pts/1
        root       3770  0.0  0.0 112828   976 pts/0    S+   10:26   0:00 grep --color=auto sshd

        ### [root@seaOcean ~]# kill 3674

    + 终止远程登录

      + [root@seaOcean ~]# ps -aux | grep sshd
        root       1235  0.0  0.2 112900  4312 ?        Ss   10:09   0:00 /usr/sbin/sshd -D
        root       1682  0.0  0.2 158856  5560 ?        Ss   10:09   0:00 sshd: root@pts/0
        root       3674  0.3  0.2 158856  5496 ?        Ss   10:25   0:00 sshd: monkeyone [priv]
        monkeyo+   3683  0.0  0.1 158856  2504 ?        S    10:25   0:00 sshd: monkeyone@pts/1
        root       3770  0.0  0.0 112828   976 pts/0    S+   10:26   0:00 grep --color=auto sshd
        [root@seaOcean ~]# kill 3674
        [root@seaOcean ~]# kill 1235
        [root@seaOcean ~]# ps -aux | grep sshd
        root       1682  0.0  0.2 158856  5560 ?        Ss   10:09   0:00 sshd: root@pts/0
        root       3847  0.0  0.0 112828   972 pts/0    S+   10:30   0:00 grep --color=auto sshd

    + 如何重启sshd服务？

      + [root@seaOcean ~]# /bin/systemctl start sshd.service
        [root@seaOcean ~]# ps -aux | grep sshd
        root       1682  0.0  0.2 158856  5560 ?        Ss   10:09   0:00 sshd: root@pts/0
        root       3871  0.1  0.2 112900  4316 ?        Ss   10:32   0:00 /usr/sbin/sshd -D
        root       3881  0.0  0.0 112828   976 pts/0    S+   10:33   0:00 grep --color=auto sshd

    + 终止多个gedit，演示killall

      + killall gedit

    + 强制杀掉一个终端：

      + kill -9 xxxx： 强制杀死xxxx进程

    + 查看进程树

      + pstree 选项 可以直观的查看进程的信息
      + pstree -p 给进程号
      + pstree -u 显示进程的拥有者

    + service管理

      + service本质就是进程，但是时运行在后台的，通常会监听某个端口，等待其他程序的请求，比如(mysql,sshd防火墙等)因此我们又称守护进程，是Linux中非常重要的知识点

    + top 动态监控

      + top与ps命令相似，都是用来监控进程的，Top会自动更新而已
      + 基本语法
        + top 选项
          + -d 秒数	指定top命令每隔几秒更新。默认是3秒
          + -i 使top不显示任何闲置或者僵死进程
          + -p 通过指定监控进程ID来仅仅监控某个进程的状态
        + 动态监控的交互操作
          + P 以CPu的使用率来排序
          + M 以内存的使用了排序
          + N 以进程号排序
          + q 退出

    + 查看系统网络状况：

      + 基本语法
        + netstat 【选项】
      + 选项说明
        + -an 按照一定的顺序排列输出
        + -p 显示哪一个进程在调用
      + rpm包的管理
        + rpm包名的基本格式
        + rpm包的其他查询指令
          + rpm -qa：查询所安装的所有rpm软件包
          + rpm -qa | more
          + rpm -qa | grep xxxx  query all
          + rpm -q 软件包名 ： 查询软件是否安装
          + rpm -q firefox   query
          + rpm -qi 软件包名 ： 查询软件包信息    query information
          + rpm -ql 软件包名 查询软件包中的文件 
          + rpm -qf 文件全路径名  查询文件所属的软件包 query 
      + 什么是rpm？
        百度百科：RPM是Red-Hat Package Manager（RPM软件包管理器）的缩写，这一文件格式名称虽然打上了RedHat的标志，但是其原始设计理念是开放式的，现在包括OpenLinux、S.u.S.E.以及Turbo Linux等Linux的分发版本都有采用，可以算是公认的行业标准了。一种用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。与Dpkg类似。
        简而言之，就是linux中的软件包。
      + 软件包管理 resource package manager

    + 卸载rpm包

      + 基本语法：

        + rpm -e rpm包的名称 
        + 会有警告但是还是会被删除

        

    + 安装rpm包

      + 基本语法
        + rpm -ivh rpm包全路径名称
        + 参数说明：
          + i=install
          + v=verbose 提示
          + h=hash 进度条

    + yum

      + yun是一个shell前端软件包管理器。基于RPM包管理，能够从指定的服务器中自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有的软件包

    + yum的基本指令

      + yum list | grep xx 软件列表

      + 安装指定的yum包

        [root@seaOcean ~]# yum list | grep firefox
        firefox.i686                              91.10.0-1.el7.centos         updates  
        firefox.x86_64                            91.10.0-1.el7.centos         updates  

      + yum install xxx 下载安装

    + 使用yum安装firefox


​    

# Linux之JavaEE定制篇

## 搭建JavaEE环境

+ 如果需要在Linux下进行JavaEE的开发，我们需要安装如下的软件

+ 安装JDK

  1. mkdir /opt/jdk
  2. 通过xftp6上传到/opt/jdk下.
  3. cd /opt/jdk
  4. 解压 tar -zxvf jdk-8u261-linux-x64.tar.gz
  5. mkdir /usr/local/java
  6. mv /opt/jdk/jdk-8u261 /usr/local/java
  7. 配置环境变量的配置文件vim /etc/profile
  8. export JAVA_HOME=/usr/local/java/jdk1.8.0_261
  9. `export PATH=$JAVA_HOME/bin:$PATH`
  10. source /etc/profile 
  11. 测试安装成功

+ 安装tomcat

  1. 上传安装文件，并解压到/opt/tomcat
  2. 进入解压目录/bin，启动tomcat  ./startup.sh
  3. 开放端口 8080
  4. firewall -cmd --permanent add-port=8080/tcp
  5. firewall -cmd --reload

  测试：在Linux下访问http://linuxip:8080

+ idea2020的安装

+  步骤：

  1. 下载地址：idea官网 注意不要下载windows版本的
  2. 解压缩到/opt/idea
  3. 启动idea bin目录下 ./idea.sh 配置jdk
  4. 编写Hello world程序执行测试

+ mysql的安装

  1. 新建文件夹/opt/mysql 并cd进去

  2. 运行wget http://dev.mysql.com/get/mysql-5.7.26             下载安装包

     ps：centOS7.6自带mysql数据库是mariadb，会和mysql冲突，要先删除

  3. 运行 tar -vxf mysqlxxxx

  4. 运行 rpm -qa|grep mari 查询mariadb相关安装包

  5. 运行rpm  -e --nodeps mariadb-libs 卸载

  6. 然后开始真正的安装mysql 依次运行一下几条

  https://blog.csdn.net/p445098355/article/details/124338426



























































