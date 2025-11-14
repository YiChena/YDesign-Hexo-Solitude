---
title: Linux
date: 2022-10-01 16:17:28
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Linux.png
tags:  
    - 技术
    - 工具
    - Linux
---

# Linux

# linux常用命令

## Linux 系统目录结构

- **/bin**：
  bin 是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。

- **/boot：**
  这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。

- **/dev ：**
  dev 是 Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。

- **/etc：**
  etc 是 Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录。

- **/home**：
  用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的，如上图中的 alice、bob 和 eve。

- **/lib**：
  lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。

- **/lost+found**：
  这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

- **/media**：
  linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下。

- **/mnt**：
  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。

- **/opt**：
  opt 是 optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

- **/proc**：
  proc 是 Processes(进程) 的缩写，/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
  这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：

  ```
  echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
  ```

- **/root**：
  该目录为系统管理员，也称作超级权限者的用户主目录。

- **/sbin**：
  s 就是 Super User 的意思，是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里存放的是系统管理员使用的系统管理程序。

- **/selinux**：
  这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

- **/srv**：
  该目录存放一些服务启动之后需要提取的数据。

- **/sys**：

  这是 Linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。

  sysfs 文件系统集成了下面3种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。

  该文件系统是内核设备树的一个直观反映。

  当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

- **/tmp**：
  tmp 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。

- **/usr**：
  usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

- **/usr/bin：**
  系统用户使用的应用程序。

- **/usr/sbin：**
  超级用户使用的比较高级的管理程序和系统守护程序。

- **/usr/src：**
  内核源代码默认的放置目录。

- **/var**：
  var 是 variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

- **/run**：
  是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。

在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。

**/etc**： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。

**/bin, /sbin, /usr/bin, /usr/sbin**: 这是系统预设的执行文件的放置目录，比如 **ls** 就是在 **/bin/ls** 目录下的。

值得提出的是 **/bin**、**/usr/bin** 是给系统用户使用的指令（除 root 外的通用用户），而/sbin, /usr/sbin 则是给 root 使用的指令。

**/var**： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在 /var/log 目录下，另外 mail 的预设放置也是在这里。



## 常用快捷键

【tab】键：它将自动完成你输入的内容，或为你显示全部可能的结果。



【ctrl+c】键：它将立刻终止运行的程序。如果你想要停止使用一个正在后台运行的程序，只需按下这对组合键。



【ctrl+D】键：该快捷键将移动光标到所在行首。回到段落的开头。



【ctrl+U】键：输入了错误的命令？ 代替用退格键来丢弃当前命令，使用 Linux 终端中的 Ctrl+U 快捷键。 该快捷键会擦除从当前光标位置到行首的全部内容。



【ctrl+K】键：这对和 Ctrl+U 快捷键有点像。 唯一的不同在于不是行首，它擦除的是从当前光标位置到行尾的全部内容。



【ctrl+W】键： 用 Ctrl+W 快捷键。使用 Ctrl+W 快捷键，你可以擦除光标位置前的单词。 如果光标在一个单词本身上，它将擦除从光标位置到词首的全部字母。最好的方法是用它移动光标到要删除单词后的一个空格上， 然后使用 Ctrl+W 键盘快捷键。



【ctrl+Y】键：这将粘贴使用 Ctrl+W，Ctrl+U 和 Ctrl+K 快捷键擦除的文本。 如果你删除了错误的文本或需要在某处使用已擦除的文本，这将派上用场。



【ctrl+P】键：你可以使用该快捷键来查看上一个命令。 你可以反复按该键来返回到历史命令。 在很多终端里，使用 PgUp 键来实现相同的功能。



【ctrl+N】键：你可以结合 Ctrl+P 使用该快捷键。Ctrl+N 显示下一个命令。 如果使用 Ctrl+P 查看上一条命令，你可以使用 Ctrl+N 来回导航。 许多终端都把此快捷键映射到 PgDn 键。



【ctrl+R】键：你可以使用该快捷键来搜索历史命令。



【ctrl+左右键】： 在单词之间跳转。



【ait - d】：由光标位置开始，往右删除单词。往行尾删。



## 常用命令

##### 文件管理

1. cat （英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上。

   ```linux
   cat[-nbs] [-help] [-version] fileName
   ```

   参数说明：

   -n或--number：由1开始对所有输出的行数编号。

   -b或--number-ninblank：和-n相似，只不过对于空白行不编号。

   -s或--squeeze-blank：当遇到连续2行以上的空白就代换为一行的空白行。

   实例：
   把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：

   ```linux
   cat -n textfile1 > textfile2
   ```

   把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：

   ```linux
   cat -b textfile1 textfile2 >> textfile3
   ```

​      清空 /etc/test.txt 文档内容：  

```linux
cat /dev/null > /etc/test.txt
```



2. more，more 命令类似 cat ，不过会以一页一页的形式显示，更方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能（与 vi 相似）

   语法格式：
   more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames…]
   参数说明：
   -num 一次显示的行数
   -d 提示使用者，在画面下方显示 [Press space to continue, ‘q’ to quit.] ，如果使用者按错键，则会显示 [Press ‘h’ for instructions.] 而不是 ‘哔’ 声
   -l 取消遇见特殊字元 ^L（送纸字元）时会暂停的功能
   -f 计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
   -p 不以卷动的方式显示每一页，而是先清除萤幕后再显示内容
   -c 跟 -p 相似，不同的是先显示内容再清除其他旧资料
   -s 当遇到有连续两行以上的空白行，就代换为一行的空白行
   -u 不显示下引号 （根据环境变数 TERM 指定的 terminal 而有所不同）
   +/pattern 在每个文档显示前搜寻该字串（pattern），然后从该字串之后开始显示
   +num 从第 num 行开始显示
   fileNames 欲显示内容的文档，可为复数个数

   实例：
   逐页显示 testfile 文档内容，如有连续两行以上空白行则以一行空白行显示。

   ```linux
   more -s testfile
   ```

   从第 20 行开始显示 testfile 之文档内容。

   ```linux
   more +20 testfile
   ```

   3. rm命令用于删除一个文件或者目录。
      语法规则：
      rm [options] name…
      参数说明：
      -i 删除前逐一询问确认。
      -f 即使原档案属性设为唯读，亦直接删除，无需逐一确认。
      -r 将目录及以下之档案亦逐一删除。
      实例：
      删除文件可以直接使用rm命令，若删除目录则必须配合选项"-r"，例如：

   ```linux
   rm  test.txt
   
   rm  -r  homework  
   ```

   删除当前目录下的所有文件及目录，命令行为：

   ```linux
   rm  -r  * 
   ```

3. cp命令主要用于复制文件或目录。
   语法规则：
   cp [options] source dest 或 cp [options] source… directory
   参数说明：
   -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
   -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
   -f：覆盖已经存在的目标文件而不给出提示。
   -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
   -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
   -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
   -l：不复制文件，只是生成链接文件。
   实例：
   使用指令 cp 将当前目录 test/ 下的所有文件复制到新目录 newtest 下，输入如下命令：

```linux
    $ cp –r test/ newtest  
```

5. read
   命令用于从标准输入读取数值。read 内部命令被用来从标准输入读取单行数据。这个命令可以用来读取键盘输入，当使用重定向的时候，可以读取文件中的一行数据。
   语法规则：
   read [-ers] [-a aname] [-d delim] [-i text] [-n nchars] [-N nchars] [-p prompt] [-t timeout] [-u fd] [name …]
   参数说明：
   -a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
   -d 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
   -p 后面跟提示信息，即在输入前打印提示信息。
   -e 在输入的时候可以使用命令补全功能。
   -n 后跟一个数字，定义输入文本的长度，很实用。
   -r 屏蔽\，如果没有该选项，则\作为一个转义字符，有的话 \就是个正常的字符了。
   -s 安静模式，在输入字符时不再屏幕上显示，例如login时输入密码。
   -t 后面跟秒数，定义输入字符的等待时间。
   -u 后面跟fd，从文件描述符中读入，该文件描述符可以是exec新开启的。


##### 磁盘管理

1. cd：命令用于切换当前工作目录。其中 dirName 表示法可为绝对路径或相对路径。若目录名称省略，则变换至使用者的 home 目录 (也就是刚 login 时所在的目录)。另外，~ 也表示为 home 目录 的意思， . 则是表示目前所在的目录， … 则表示目前目录位置的上一层目录。
   语法规则：
   cd [dirName]
   实例：
   跳到 /usr/bin/ :

   ```linux
    cd /usr/bin
   ```

   跳到自己的 home 目录 :

   ```linux
   cd ~
   ```

   跳目前目录的上上两层 :

   ```linux
   cd ../..
   ```

   2. mkdir命令用于创建目录。
      语法规则：

      ```linux
      mkdir [-p] dirName
      ```

      参数说明：
      -p 确保目录名称存在，不存在的就建一个。
      实例：
      在工作目录下，建立一个名为 runoob 的子目录 :

      参数说明：
      -p 确保目录名称存在，不存在的就建一个。
      实例：
      在工作目录下，建立一个名为 runoob 的子目录 :

      ```linux
      mkdir runoob
      ```


      在工作目录下的 runoob2 目录中，建立一个名为 test 的子目录。
      若 runoob2 目录原本不存在，则建立一个。（注：本例若不加 -p 参数，且原本 runoob2 目录不存在，则产生错误。）
    
      在工作目录下的 runoob2 目录中，建立一个名为 test 的子目录。
      若 runoob2 目录原本不存在，则建立一个。（注：本例若不加 -p 参数，且原本 runoob2 目录不存在，则产生错误。）
    
      ```linux
      mkdir -p runoob2/test
      ```

2. pwd命令用于显示工作目录。执行pwd指令可立刻得知目前所在的工作目录的绝对路径名称。

   语法规则：

   pwd [-help] [-version]

   参数说明：

   -help

   -version显示版本信息。

   实例：

   查看当前所在目录：

   ```linux
   pwd/root/test           #输出结果
   ```

   

3. redir命令删除空的目录。
   语法规则：
   rmdir [-p] dirName
   参数说明：
   -p 是当子目录被删除后使它也成为空目录的话，则顺便一并删除。
   实例
   将工作目录下，名为 AAA 的子目录删除 :

   ```linux
   rmdir AAA
   ```

   在工作目录下的 BBB 目录中，删除名为 Test 的子目录。若 Test 删除后，BBB 目录成为空目录，则 BBB 亦予删除。

   ```linux
   rmdir -p BBB/Test
   ```





5. ls命令用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)。
   语法规则：

   ```linux
   ls [-alrtAFR] [name…]
   ```

   参数说明：

   -a 显示所有文件及目录 (. 开头的隐藏文件也会列出)
   -l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
   -r 将文件以相反次序显示(原定依英文字母次序)
   -t 将文件依建立时间之先后次序列出
   -A 同 -a ，但不列出 “.” (目前目录) 及 “…” (父目录)
   -F 在列出的文件名称后加一符号；例如可执行档则加 “*”, 目录则加 “/”
   -R 若目录下有文件，则以下之文件亦皆依序列出
   实例：
   列出根目录()下的所有目录：

   ```linux
   #ls /
   bin               dev   lib         media  net   root     srv  upload  www
   boot              etc   lib64       misc   opt   sbin     sys  usr
   home  lost+found  mnt    proc  selinux  tmp  var
   
   ```

   列出目前工作目录下所有名称是 s 开头的文件，越新的排越后面 :

   ```linux
   ls -ltr s*
   ```

   ​        将 /bin 目录以下所有目录及文件详细资    料列出 : 

   ```linux
   ls -lR /bin
   ```

   ​	列出目前工作目录下所有文件及目录；目录于名称后加 “/”, 可执行档于名称后加 “*” :

   ```linux
   ls -AF
   ```

#####  网络通信

1. telnet命令用于远端登入。执行telnet指令开启终端机阶段作业，并登入远端主机。
   语法规则：
   telnet [-8acdEfFKLrx][-b<主机别名>][-e<脱离字符>][-k<域名>][-l<用户名称>][-n<记录文件>][-S<服务类型>][-X<认证形态>][主机名称或IP地址<通信端口>]
   参数说明：
   -8 允许使用8位字符资料，包括输入与输出。
   -a 尝试自动登入远端系统。
   -b<主机别名> 使用别名指定远端主机名称。
   -c 不读取用户专属目录里的.telnetrc文件。
   -d 启动排错模式。
   -e<脱离字符> 设置脱离字符。
   -E 滤除脱离字符。
   -f 此参数的效果和指定"-F"参数相同。
   -F 使用Kerberos V5认证时，加上此参数可把本地主机的认证数据上传到远端主机。
   -k<域名> 使用Kerberos认证时，加上此参数让远端主机采用指定的领域名，而非该主机的域名。
   -K 不自动登入远端主机。
   -l<用户名称> 指定要登入远端主机的用户名称。
   -L 允许输出8位字符资料。
   -n<记录文件> 指定文件记录相关信息。
   -r 使用类似rlogin指令的用户界面。
   -S<服务类型> 设置telnet连线所需的IP TOS信息。
   -x 假设主机有支持数据加密的功能，就使用它。
   -X<认证形态> 关闭指定的认证形态。

   实例：
   登录远程主机

   ```linux
   telnet 192.168.1.2     //登录IP为 192.168.1.2的远程主机
   ```

2. ping命令用于检测主机。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。
   语法规则：
   ping [-dfnqrRv][-c<完成次数>][-i<间隔秒数>][-I<网络界面>][-l<前置载入>][-p<范本样式>][-s<数据包大小>][-t<存活数值>][主机名称或IP地址]
   参数说明：
   -d 使用Socket的SO_DEBUG功能。
   -c<完成次数> 设置完成要求回应的次数。
   -f 极限检测。
   -i<间隔秒数> 指定收发信息的间隔时间。
   -I<网络界面> 使用指定的网络接口送出数据包。
   -l<前置载入> 设置在送出要求信息之前，先行发出的数据包。
   -n 只输出数值。
   -p<范本样式> 设置填满数据包的范本样式。
   -q 不显示指令执行过程，开头和结尾的相关信息除外。
   -r 忽略普通的Routing Table，直接将数据包送到远端主机上。
   -R 记录路由过程。
   -s<数据包大小> 设置数据包的大小。
   -t<存活数值> 设置存活数值TTL的大小。
   -v 详细显示指令的执行过程。
   实例：
   检测是否与主机连通ping www.w3cschool.cc //ping主机

   ```linux
   PING aries.m.alikunlun.com (114.80.174.110) 56(84) bytes of data.64 bytes from 114.80.174.110: icmp_seq=1 ttl=64 time=0.025 ms64 bytes from 114.80.174.110: icmp_seq=2 ttl=64 time=0.036 ms64 bytes from 114.80.174.110: icmp_seq=3 ttl=64 time=0.034 ms64 bytes from 114.80.174.110: icmp_seq=4 ttl=64 time=0.034 ms64 bytes from 114.80.174.110: icmp_seq=5 ttl=64 time=0.028 ms64 bytes from 114.80.174.110: icmp_seq=6 ttl=64 time=0.028 ms64 bytes from 114.80.174.110: icmp_seq=7 ttl=64 time=0.034 ms64 bytes from 114.80.174.110: icmp_seq=8 ttl=64 time=0.034 ms64 bytes from 114.80.174.110: icmp_seq=9 ttl=64 time=0.036 ms64 bytes from 114.80.174.110: icmp_seq=10 ttl=64 time=0.041 ms
   --- aries.m.alikunlun.com ping statistics ---10 packets transmitted, 30 received, 0% packet loss, time 29246ms
   rtt min/avg/max/mdev = 0.021/0.035/0.078/0.011 ms
   //需要手动终止Ctrl+C
   ```

   

3. ifconfig命令用于显示或设置网络设备。ifconfig可设置网络设备的状态，或是显示目前的设置。
   语法规则：
   ifconfig [网络设备][down up -allmulti -arp -promisc][add<地址>][del<地址>][<hw<网络设备类型><硬件地址>][io_addr][irq<IRQ地址>][media<网络媒介类型>][mem_start<内存地址>][metric<数目>][mtu<字节>][netmask<子网掩码>][tunnel<地址>][-broadcast<地址>][-pointopoint<地址>][IP地址]
   参数说明：
   add <地址> 设置网络设备IPv6的IP地址。
   del<地址>  删除网络设备IPv6的IP地址。
   down 关闭指定的网络设备。
   <hw<网络设备类型><硬件地址>  设置网络设备的类型与硬件地址。
   io_addr <I/O地址>  设置网络设备的I/O地址。
   irq<IRQ地址>  设置网络设备的IRQ。
   media<网络媒介类型>  设置网络设备的媒介类型。
   mem_start<内存地址>  设置网络设备在主内存所占用的起始地址。
   metric<数目>  指定在计算数据包的转送次数时，所要加上的数目。
   mtu<字节>  设置网络设备的MTU。
   netmask<子网掩码>  设置网络设备的子网掩码。
   tunnel<地址> 建立IPv4与IPv6之间的隧道通信地址。
   up  启动指定的网络设备。
   -broadcast<地址>  将要送往指定地址的数据包当成广播数据包来处理。
   -pointopoint<地址>  与指定地址的网络设备建立直接连线，此模式具有保密功能。
   -promisc  关闭或启动指定网络设备的promiscuous模式。
   [IP地址]  指定网络设备的IP地址。
   [网络设备] 指定网络设备的名称
   实例：
   显示网络设备信息ifconfig        

   ```linux
   eth0   Link encap:Ethernet HWaddr 00:50:56:0A:0B:0C 
        inet addr:192.168.0.3 Bcast:192.168.0.255 Mask:255.255.255.0
        inet6 addr: fe80::250:56ff:fe0a:b0c/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
        RX packets:172220 errors:0 dropped:0 overruns:0 frame:0
        TX packets:132379 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000 
        RX bytes:87101880 (83.0 MiB) TX bytes:41576123 (39.6 MiB)
        Interrupt:185 Base address:0x2024 
   
   ```







