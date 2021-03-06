# 基本

+ 硬链接和软连接：
  + 硬链接： 在Linux系统中，多个文件名指向同一索引节点(Inode)是正常且允许的。一般这种链接就称为硬链接。硬链接的作用之一是允许一个文件拥有多个有效路径名，这样用户就可以建立硬链接到重要的文件，以防止“误删”源数据
  + 软连接：   软链接（也叫符号链接），类似于windows系统中的快捷方式，与硬链接不同，软链接就是一个普通文件，只是数据块内容有点特殊，文件用户数据块中存放的内容是另一文件的路径名的指向，通过这个方式可以快速定位到软连接所指向的源文件实体。软链接可对文件或目录创建。
  
+ Linux中>代表表示覆盖原文件内容（文件的日期也会自动更新），>>表示追加内容（文件的日期也会更新）

+ 权限：如`-rwxrwxrwx`或`drwxrwxrwx`
  + -代表是一个文件，d代表是一个目录
  + 从第二个开始每三个分别表示：所有者用户权限、同组的用户权限、系统中其他用户的权限
  + r代表读，数字4代表
  + w表示写，数字2代表
  + x代表执行，数字1代表
  
+ &和&&，|和||
  + &：表示任务在后台执行
  + &&：表示前一条指令执行成功，才执行后一条命令
  + |：表示管道，上一条命令的输出作为下一条命令参数
  + ||：表示上一条命令失败后，才执行下一条命令
  
+ Linux的系统组成：

  + Linux内核
  + Linux Shell
  + Linux文件系统
  + Linux应用程序

+ 操作系统的接口：

  + 命令行用户接口
  + 图形用户接口
  + 程序接口

+ Linux常用目录：

  + | 目录        |                                                              |
    | ----------- | ------------------------------------------------------------ |
    | /bin        | 存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里。 |
    | /etc        | 存放系统管理和配置文件                                       |
    | /home       | 存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示 |
    | /usr        | 用于存放系统应用程序，比较重要的目录/usr/local 本地系统管理员软件安装目录（安装系统级的应用）。这是最庞大的目录，要用到的应用程序和文件几乎都在这个目录。/usr/x11r6 存放x window的目录/usr/bin 众多的应用程序 /usr/sbin 超级用户的一些管理程序 /usr/doc linux文档 /usr/include linux下开发和编译应用程序所需要的头文件 /usr/lib 常用的动态链接库和软件包的配置文件 /usr/man 帮助文档 /usr/src 源代码，linux内核的源代码就放在/usr/src/linux里 /usr/local/bin 本地增加的命令 /usr/local/lib 本地增加的库 |
    | /opt        | 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里。 |
    | /proc       | 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息。 |
    | /root       | 超级用户（系统管理员）的主目录（特权阶级^o^）                |
    | /sbin       | 存放二进制可执行文件，只有root才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如ifconfig等。 |
    | /dev        | 用于存放设备文件。                                           |
    | /mnt        | 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统。 |
    | /boot       | 存放用于系统引导时使用的各种文件                             |
    | /lib        | 存放跟文件系统中的程序运行所需要的共享库及内核模块。共享库又叫动态链接共享库，作用类似windows里的.dll文件，存放了根文件系统程序运行所需的共享文件。 |
    | /tmp        | 用于存放各种临时文件，是公用的临时文件存储点。               |
    | /var        | 用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等。）等。 |
    | /lost+found | 这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里 |
    
  
+ 命名空间：Linux命名空间是Linux提供的一种内核级别环境隔离的方法，Linux支持七种不同类型的命名空间。使得用户创建的进程能与系统分离的更加彻底。使用`fork()`或`clone()`系统调用可以使用特定的选项来指定和父进程共享命名空间还是建立新的命名空间。[Linux命名空间详解](https://cloud.tencent.com/developer/article/1339557)

  + 命名空间分类：
    + CLONE_NEWPID 进程命名空间。空间内的PID 是独立分配的，意思就是命名空间内的虚拟 PID 可能会与命名空间外的 PID 相冲突，于是命名空间内的 PID 映射到命名空间外时会使用另外一个 PID。比如说，命名空间内第一个 PID 为1，而在命名空间外就是该 PID 已被 init 进程所使用。
    + CLONE_NEWIPC 进程间通信(IPC)的命名空间，可以将 SystemV 的 IPC 和 POSIX 的消息队列独立出来。
    + CLONE_NEWNET 网络命名空间，用于隔离网络资源（/proc/net、IP 地址、网卡、路由等）。后台进程可以运行在不同命名空间内的相同端口上，用户还可以虚拟出一块网卡。
    + CLONE_NEWNS 挂载命名空间，进程运行时可以将挂载点与系统分离，使用这个功能时，我们可以达到 chroot 的功能，而在安全性方面比 chroot 更高。
    + CLONE_NEWUTS UTS 命名空间，主要目的是独立出主机名和网络信息服务（NIS）。
    + CLONE_NEWUSER 用户命名空间，同进程 ID 一样，用户 ID 和组 ID 在命名空间内外是不一样的，并且在不同命名空间内可以存在相同的 ID。
    + CLONE_NEWCGROUP：用于隔离物理资源，如CPU、内存、磁盘IO等。
  + 操作命名空间的系统调用：
    + `clone()`系统调用：clone系统调用类似fork系统调用，可以在创建进程的时候同时创建namespace
    + `setns()`系统调用：加入一个已经存在的namespace，docker种的exec命令使用的就是此系统调用。
    + `unshare()`系统调用：在原来的进程上进行namespace的隔离。

+ 

# 常用命令

## 文件

### 文件操作

#### rm

+ 作用：用于删除一个文件或者目录。
+ 格式：`rm [options] name...`
+ 参数：
  + `-f`：强制删除
  + `-i`：每次删除前询问
  + `-r`：递归删除文件夹
  + `-d`：删除空文件夹

#### rmdir

+ 作用：一个目录中删除一个或多个子目录项，删除某目录时也必须具有对其父目录的写权限。
+ 参数：
  + `-p`：当 parent 子目录被删除后使它也成为空目录的话，则顺便一并删除
+ 注意：不能删除非空目录

#### mv

+ 作用：移动文件或修改文件名

+ 格式：

  + `mv [options] source dest`
  + `mv [options] source... directory`

+ 参数：

  + `-b`：如果覆盖则留下一个备份
  + `-i`：覆盖前确认
  + `-f`：强制覆盖
  + `-n`：不覆盖任何文件
  + `-u`：当当前文件比覆盖文件新或者不存在的时候才移动
  + `-v`：显示移动的文件

+ 实例：

  + ```shell
    mv test.log test1.txt # 将文件 test.log 重命名为 test1.txt
    mv llog1.txt log2.txt log3.txt /test3 # 将文件 log1.txt,log2.txt,log3.txt 移动到根的 test3 目录中
    mv * ../ # 移动当前文件夹下的所有文件到上一级目录
    ```

#### cp

+ 作用：将源文件复制至目标文件，或将多个源文件复制至目标目录。
+ 格式：
  + `cp [options] source dest`
  + `cp [options] source... directory`
+ 参数：
  + `-a`：保留链接、文件属性、并且复制目录下所有内容。相当于dpR的组合
  + `-b`：如果覆盖则留下一个备份
  + `-d`：复制时保留链接，相当于windows下的快捷方式
  + `-f`：强制覆盖
  + `-i`：覆盖时确认
  + `-n`：不覆盖任何文件
  + `-p`：除复制文件的内容外，同时复制修改时间和访问权限
  + `-r`：递归复制文件夹
  + `-l`：不复制文件只是进行硬链接

#### tar

+ 作用：用来压缩和解压文件。tar 本身不具有压缩功能，只具有打包功能，有关压缩及解压是调用其它的功能来完成。

+ 参数：

  + `-c`：建立新的压缩文件
  + `-f`：指定压缩文件
  + `-r`：添加文件到已压缩文件包中
  + `-u`：添加改了和现有的文件到压缩包中
  + `-x`：从压缩包中抽取文件
  + `-t`：显示压缩文件中的内容
  + `-z`：支持gzip压缩
  + `-j`：支持bzip2压缩
  + `-Z`：支持compress压缩
  + `-v`：显示操作过程

+ 实例：

  + ```shell
    tar -cvzf package.zip *.txt # 将当前目录下的txt文件夹打包到package.zip
    tar -xvzf package.zip -C ./unpackage # 将package.zip中的文件夹解压到unpackage文件夹中
    ```

  + 

### 文件查看

#### cat

+ 作用：用于连接文件并打印到标准输出设备上。

+ 格式：`cat [-AbeEnstTuv] [--help] [--version] fileName`

+ 参数：

  + `-n`：从1开始对输出的行数进行编号
  + `-b`：从1开始对输出的行数进行编号，但是不会对空白行编号
  + `-s`：当遇到两个以上空白行，替换成一个空白行
  + `-E`：在每行结束的地方显示$
  + `-T`：显示tab字符为^|

+ 实例：

  + ```shell
    cat file1 file2 > file3 # 将file1和file2添加到file3
    cat -n file1 file2 
    cat > filename # 创建一个文件，并写入内容，不能修改文件。按ctrl + z退出
    ```

#### more

+ 作用：功能类似cat，会一页一页的阅读。不能后退

#### less

+ 作用：功能类似cat，more不能后退，less可以后退

#### head

+ 作用：显示前n行
+ 参数：
  + `-n`：指定显示行数

#### tail

+ 作用：显示末尾n行，常用于查看日志文件
+ 参数：
  + `-n`：指定显示行数
  + `-f`：循环读取

#### grep

+ 作用：用于查找文件里符合条件的字符串。

+ 格式：`grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]`

+ 参数：

  + `-A n`：显示匹配字符后n行
  + `-B n`：显示匹配字符前n行
  + `-C n`：显示匹配字符前后n行
  + `-c`：计算符合的列数
  + `-i`：忽略大小写
  + `-l`：只列出符合的文件名称
  + `-f`：从文件中取得匹配到的值
  + `-e`：使用正则表达式来匹配
  + `-n`：显示匹配内容所在文件中的行数
  + `-R`：递归查找文件夹

+ 实例：

  + ```shell
    ps -ef | grep svn # 查找指定进程
    ps -ef | grep svn -c # 查找进程个数
    cat test1.txt | grep -f key.log # 从文件中读取关键字
    grep -lR '^grep' /tmp # 从文件夹中递归查找以grep开头的行，并只列出文件
    ```

#### wc

+ 作用：文件中统计字符总数
+ 格式：`wc [option] file..`
+ 参数：
  + `-c`：统计字节数
  + `-l`：统计行数
  + `-m`：统计字符数
  + `-w`：统计词数

#### sort

+ 作用：用于将文本文件内容加以排序。
+ 格式：`sort [-bcdfimMnr][-o<输出文件>][-t<分隔字符>][+<起始栏位>-<结束栏位>][--help][--verison][文件]`
+ 参数：
  + -b 忽略每行前面开始出的空格字符。
  + **-c 检查文件是否已经按照顺序排序。**
  + -d 排序时，处理英文字母、数字及空格字符外，忽略其他的字符。只考虑空格、字母和数字
  + -f 排序时，将小写字母视为大写字母。
  + -i 排序时，除了040至176之间的ASCII字符外，忽略其他的字符。只考虑可打印字符。
  + -m 将几个排序好的文件进行合并。
  + -M 将前面3个字母依照月份的缩写进行排序。
  + **-n 依照数值的大小排序；对指定的列进行排序，+0表示第一列，以空格或制表符作为列的间隔符。**
  + -o<输出文件> 将排序后的结果存入指定的文件。
  + -u 去重，配合-c，严格校验排序；不配合-c，则只输出一次排序结果，一般用uniq代替。
  + **-r 倒序（降序）以相反的顺序来排序。**
  + **-t<分隔字符> 指定排序时所用的栏位分隔字符。例如：-t. 表示按点号分隔域，类似于awk -F或cut -d**
  + **-k指定第几列或第几列的第几个字符。与-t配合使用**
  + +<起始栏位>-<结束栏位> 以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位。

### 文件查找

#### which

+ 作用：会在环境变量$PATH设置的目录里查找符合条件的文件。

+ 注意：只能查找PATH中的命令而不能查找内建命令。如cd

+ 格式：`which [文件...]`

+ 参数：

  + `-n<文件名长度>`：指定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。
  + `-p<文件名长度>`：与-n参数相同，但此处的<文件名长度>包括了文件的路径。

+ 实例：

  + ```shell
    which bash # 查看bash指令的路径
    ```

#### whereis

+ 作用：只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）
+ 参数：
  + `-b`：搜索二进制文件
  + `-B<dir>`：指定搜索二进制文件路径
  + `-m`：搜索帮助文件
  + `-s`：搜索源代码文件
  + `-u`：搜索除帮助文件、二进制文件、源代码文件以外的其他文件

#### locate

+ 作用：locate 通过搜寻系统内建文档数据库达到快速找到档案，数据库由 updatedb 程序来更新，updatedb 是由 cron daemon 周期性调用的。默认情况下 locate 命令在搜寻数据库时比由整个由硬盘资料来搜寻资料来得快，但较差劲的是 locate 所找到的档案若是最近才建立或 刚更名的，可能会找不到，在内定值中，updatedb 每天会跑一次，可以由修改 crontab 来更新设定值 (etc/crontab)。

#### find

+ 作用：用于在文件树中查找文件，并作出相应的处理。

+ 格式：`find pathname -options [-print -exec -ok ...]`

+ 参数：

  + `-print`：将匹配到的文件输出到标准输出
  + `-exec`：对匹配到的文件执行shell命令。相应命令的形式为`'command' {  } \;`，注意{   }和\；之间的空格。
  + `-ok`：和-exec差不多，不过每执行一个命令之前都会给出提示

+ option：

  + ```shell
    -name 按照文件名查找文件
    -perm 按文件权限查找文件
    -user 按文件属主查找文件
    -group  按照文件所属的组来查找文件。
    -type  查找某一类型的文件，诸如：
       b - 块设备文件
       d - 目录
       c - 字符设备文件
       l - 符号链接文件
       p - 管道文件
       f - 普通文件
    
    -size n :[c] 查找文件长度为n块文件，带有c时表文件字节大小
    -amin n   查找系统中最后N分钟访问的文件
    -atime n  查找系统中最后n*24小时访问的文件
    -cmin n   查找系统中最后N分钟被改变文件状态的文件
    -ctime n  查找系统中最后n*24小时被改变文件状态的文件
    -mmin n   查找系统中最后N分钟被改变文件数据的文件
    -mtime n  查找系统中最后n*24小时被改变文件数据的文件
    (用减号-来限定更改时间在距今n日以内的文件，而用加号+来限定更改时间在距今n日以前的文件。 )
    -maxdepth n 最大查找目录深度
    -prune 选项来指出需要忽略的目录。在使用-prune选项时要当心，因为如果你同时使用了-depth选项，那么-prune选项就会被find命令忽略
    -newer 如果希望查找更改时间比某个文件新但比另一个文件旧的所有文件，可以使用-newer选项
    ```

+ 实例：

  + ```shell
    find ./ -name "file*" -exec rm -f -i {} \; # 查找当前目录下file开头的文件并删除
    find /opt -perm 777 # 查找opt目录下权限为777的文件
    find -size +1000c # 查找大于1k的文件
    ```

### 文件权限

#### chmod

+ 作用：用于改变 linux 系统文件或目录的访问权限。用它控制文件或目录的访问权限。该命令有两种用法。一种是包含字母和操作符表达式的文字设定法；另一种是包含数字的数字设定法。

+ 参数：

  + `-c`：当发生改变时，报告处理信息
  + `-R`：处理指定目录以及子目录下的所有文件

+ 权限范围：

  + u：目录或文件的当前的用户
  + g：目录或文件的当前的群组
  + o：除了目录或文件当前用户或群组以外的用户或群组
  + a：所有用户及群组

+ 权限代号：

  + r：读权限，用数字4代表
  + w：写权限，用数字2代表
  + x：执行权限，用数字1代表
  + -：删除权限，用数字0代表
  + s：特殊权限

+ 实例：

  + ```shell
    chmod u+r,g+r,o+r -R text/ -c # 将 test 目录及其子目录所有文件添加可读权限
    chmod 751 t.log -c（或者：chmod u=rwx,g=rx,o=x t.log -c) # 给 file 的属主分配读、写、执行(7)的权限，给file的所在组分配读、执行(5)的权限，给其他用户分配执行(1)的权限
    ```

#### chown

+ 作用：将文件的拥有者改为指定的用户或组。用户可以是用户名或者用户 ID；组可以是组名或者组 ID；文件是以空格分开的要改变权限的文件列表，支持通配符。

+ 参数：

  + `-c`：当发生改变时，报告处理信息
  + `-R`：处理指定目录以及子目录下的所有文件

+ 实例：

  + ```shell
    chown -c mail:mail log2012.log # 改变拥有者和群组 并显示改变信息
    chown -c :mail t.log # 改变文件群组
    chown -cR mail: test/ # 改变文件夹及子文件目录属主及属组为 mail
    ```


#### chgrp

+ 作用：改变文件的组

+ 实例：

  + ```shell
    chgrp root ping.log # 将ping.log的组改为root
    ```

  + 


### 文件目录

#### cd

+ 作用：切换当前工作目录。

+ 格式：`cd [dirName]`

+ 实例：

  + ```shell
    cd ~ #跳到home目录
    cd - #返回上次目录
    cd !$ #把上个命令的参数作为cd参数使用。
    ```

#### pwd

+ 作用：查看当前工作目录路径。

#### ls

+ 作用：用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)。
+ 格式：` ls [-alrtAFR] [name...]`
+ 参数：
  + `-a`：显示所有文件及目录 (**.** 开头的隐藏文件也会列出)
  + `-A`：和-a差不多，但是不列出.和..
  + `-l`：除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
  + `-r`：倒序列出
  + `-t`：时间排序
  + `-R`：如果目录下有文件将文件也列出



## 系统管理

### 磁盘

#### fdisk

+ 作用：一个创建和维护分区表的程序，它兼容DOS类型的分区表、BSD或者SUN类型的磁盘列表。
+ 格式：`fdisk [必要参数][选择参数]`
+ 参数：
  + `-l`：列出所有分区表
  + `-u`：搭配-l使用，显示分区数目

#### mkfs

+ 作用：特定分区上建立Linux文件系统

+ 格式：`mkfs [-V] [-t fstype] [fs-options] filesys [blocks]`

+ 参数：

  + `-t<格式>`：指明格式化格式
  + `-c`：检查是否有坏轨

+ 实例：

  + ```shell
    mfks -t ext3 /dev/sda6   # 格式化sda6为ext3
    ```

#### mount

+ 作用：挂载Linux系统外的文件。

#### unmount

+ 作用：卸载Linux系统外的文件。

### 进程

#### ps

+ 作用：显示当前进程的状态，类似于 windows 的任务管理器。
+ 格式：`ps [options] [--help]`
+ 参数：
  + `-a`：显示所有用户的所有进程(包括其它用户)
  + `-u`：显示用户
  + `-x`：显示无控制终端的进程
  + `-e`：显示所有用户的进程此参数的效果和指定"a"参数相同
  + `-f`：用ASCII字符显示树状结构，表达程序间的相互关系
+ 显示内容解析：
  + UID：启动这些进程的用户
  +  PID：进程的进程ID
  + PPID：父进程的进程号（如果该进程是由另一个进程启动的）
  + C：进程生命周期中的CPU利用率
  + STIME：进程启动时的系统时间
  + TTY：进程启动时的终端设备
  + TIME：运行进程需要的累计CPU时间
  + CMD：启动的程序名称
  + VSZ：占用虚拟内存的大小
  + RSS：占用内存的大小
  + STAT：该进程的状态
    + D：收到信号不唤醒和不可运行, 进程必须等待直到有中断发生，不可中断，通常是因为等待IO。
    + S：休眠中, 受阻, 在等待某个条件的形成或接受到信号。中断。
    + Z：进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放。
    + R：正在运行或在运行队列中等待。
    + T：进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行。
    + X：死掉的进程
    + <：优先级高的进程
    + N：优先级较低的进程
    + L：有些页被锁进内存；
    + s：进程的领导者（在它之下有子进程）；
    + I：多进程的
    + +：位于后台的进程组；

#### kill

+ 作用：Linux kill 命令用于删除执行中的程序或工作。
+ 常用信号：
  + -9：用来立即结束程序的运行. 本信号不能被阻塞、处理和忽略。如果管理员发现某个进程终止不了，可尝试发送这个信号。
  + -15：程序结束(terminate)信号, 与SIGKILL不同的是该信号可以被阻塞和处理。通常用来要求程序自己正常退出，shell命令kill缺省产生这个信号。如果进程终止不了，我们才会尝试SIGKILL。

### 用户管理

#### useradd

+ 作用：建立用户帐号。
+ 格式：`useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]`
+ 参数：
  + `-c<备注> `：加上备注文字。备注文字会保存在passwd的备注栏位中。
  + `-d<登入目录>`：指定用户登入时的起始目录。
  + `-D`：变更预设值
  + `-e<有效期限>`：指定账号的有效期限
  + `-f<缓冲天数>`：指定密码过期多少天后关闭账号
  + `-g<群组>`：指定群组
  + `-G<群组>`：指定附加群组
  + `-m`：自动建立用户登录目录
  + `-M`：不建立用户登录目录
  + `-n`：取消建立以用户名称为名的群组．
  + `-r`：建立系统账号
  + `-s<shell>`：指定用户登陆后使用的shell
  + `-u<UID>`：指定用户ID

#### usermod

+ 作用：修改用户账号
+ 格式：`usermod [-LU][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-l <帐号名称>][-s <shell>][-u <uid>][用户帐号]`
+ 

# 常用操作

## 磁盘挂载

```shell
fdisk -l # 查看空闲硬盘
fdisk /dev/xvdb # 创建分区
mkfs.ext4 /dev/xvdb1 # 格式化分区
mkdir /data # 创建挂载目录
mount /dev/xvdb1 /data # 挂载分区
vi /etc/fstab # 设置开机自动挂载
# 输入：/dev/xvdb1 /data ext4 defaults 0 0


```

