# 第1章  Linux快速入门

### 32位与64位操作系统的区别

学习Linux操作系统之前，需要理解计算机基本的常识，计算机内部对数据的传输和储存都是使用二进制，二进制是计算技术中广泛采用的一种[数制](http://baike.baidu.com/item/%E6%95%B0%E5%88%B6)，而Bit（比特）则表示二进制位，[二进制数](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%95%B0)是用0和1两个[数码](http://baike.baidu.com/item/%E6%95%B0%E7%A0%81)来表示的数。基数为2，进位规则是“逢二进一”，0或者1分别表示一个Bit二进制位。

Bit位是计算机最小单位，而字节是计算机中数据处理的基本单位，转换单位为：1Byte=8Bit，4Byte=32Bit。

随着计算机技术的发展，尤其是中央处理器（Central Processing Unit，CPU）技术的变革，CPU的位数指的是通用寄存器（General-Purpose Registers， GPRs）的数据宽度，也就是处理器一次可以处理的数据量多少。

目前主流CPU处理器分为32位CPU处理器和64位CPU处理器，32位CPU处理器可以一次性处理4个字节的数据量。而64位处理器一次性处理8个字节的数据量（1Byte=8bit），64位CPU处理器对计算机处理器在RAM里（随机存取储存器）处理信息的效率比32位CPU做了很多优化，效率更高。

X86_32位操作系统和X86_64操作系统也是基于CPU位数的支持，具体区别如下：

32位操作系统表示32位CPU对内存寻址的能力；

64位操作系统表示64位CPU对内存寻址的能力；

32位的操作系统安装在32位CPU处理器和64位CPU处理器上；

64位操作系统只能安装64位CPU处理器上；

32位操作系统对内存寻址不能超过4GB；

64位操作系统对内存寻址可以超过4GB，企业服务器更多安装64位操作系统，支持更多内存资源的利用；

 64位操作系统是为高性能处理需求设计，数据处理、图片处理、实时计算等领域需求；

 32位操作系统是为普通用户设计，普通办公、上网冲浪等需求。

### **Linux内核命名规则**

Linux内核是Linux操作系统的核心，一个完整的Linux发行版包括进程管理、内存管理、文件系统、系统管理、网络操作等部分。

Linux内核官网可以下载Linux内核版本、现行版本，历史版本，可以了解版本与版本之间的特性。

     Linux内核版本命名在不同的时期有其不同的命名规范，其中在2.X版本中，X如果为奇数表示开发版、X如果为偶数表示稳定版，从2.6.X以及3.X，内核版本命名就没有严格的约定规范。

从Linux内核1994年发布1.0发布到目前主流3.X、4.x版本，最新稳定版本是5.14，查看Linux操作系统内核如图1-2所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817648284-2abf3beb-b8e4-49b7-8def-da1adcff4119.png)

图1-2操作系统内核

Linux内核命名格式为 “R.X.Y-Z”，其中R、X、Y、Z命名意义如下：

q  数字R表示内核版本号，版本号只有在代码和内核有重大改变的时候才会改变，到目前为止有4个大版本更新。

q  数字X表示内核主版本号，主版本号根据传统的奇偶系统版本编号来分配，奇数为开发版，偶数为稳定版。

q  数字Y表示内核次版本号，次版本号是无论在内核增加安全补丁、修复Bug、实现新的特性或者驱动时都会改变。

q  数字Z表示内核小版本号，小版本号会随着内核功能的修改、Bug修复而发生变化。

官网内核版本如图1-3所示，Mainline表示主线开发版本，Stable表示稳定版本，稳定版本主要由mainline测试通过而发布。Longterm表示长期支持版，会持续更新及Bug修复，如果长期版本被标记为EOL（End of Life），则表示不再提供更新。

![官网内核版本](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817648501-1fbed42d-46cb-4573-be98-7aaeb3786722.png)


## 目录结构

### Linux 文件系统总体结构（FHS 标准）

Linux 遵循一个叫 **FHS（Filesystem Hierarchy Standard）** 的标准，它规定了：各个目录该放什么类型的文件。理解 FHS，你就能看懂几乎所有 rpm、deb 包安装后文件都去哪儿。

---
### 常见目录作用

| 目录                          | 主要内容                 | 示例                                                                              |
| --------------------------- | -------------------- | ------------------------------------------------------------------------------- |
| **/bin**                    | 基本命令（所有用户都能用）        | `ls`<br><br>, `cp`<br><br>, `mv`<br><br>, `cat`                                 |
| /boot                       | 开机时用到的文件核心文件         | 开机菜单及配置文件核心文件开机菜单及配置文件                                                          |
| **/sbin**                   | 系统管理命令（root 专用）      | `reboot`<br><br>, `mount`<br><br>, `ifconfig`                                   |
| **/usr/bin**                | 普通应用程序               | `nginx`<br><br>, `mysql`<br><br>, `python3`                                     |
| **/usr/sbin**               | 系统级应用程序              | `sshd`<br><br>, `httpd`<br><br>, `zabbix_server`                                |
| **/usr/lib**                | 程序运行依赖的库文件           | `/usr/lib/libzabbix.so`                                                         |
| **/usr/lib/systemd/system** | systemd 服务单元文件       | `nginx.service`<br><br>, `zabbix-agent.service`                                 |
| **/etc**                    | 系统和软件的配置文件           | `/etc/nginx/nginx.conf`<br>`/etc/passwd`,<br>, `/etc/zabbix/zabbix_server.conf` |
| **/var**                    | 可变数据（日志、缓存、数据库）      | `/var/log/`<br><br>, `/var/lib/mysql/`                                          |
| **/usr/share**              | 与架构无关的静态资源（html、文档等） | `/usr/share/zabbix/ui/`                                                         |
| **/opt**                    | 可选的第三方软件             | `/opt/mysql/`<br><br>（自编译或手动安装的）                                                |
| **/tmp**                    | 临时文件目录               | 临时缓存、安装包中转等                                                                     |
| /dev                        |                      |                                                                                 |
| /mut                        | 用来挂宅光盘等设备            |                                                                                 |
| /opt                        | 第三方软件                |                                                                                 |
| /run                        | 系统开机产生的各种信息          |                                                                                 |

---

### 以 Zabbix 为例

| 类型       | 文件路径                                            | 说明                |
| -------- | ----------------------------------------------- | ----------------- |
| 程序主文件    | `/usr/sbin/zabbix_server`                       | 服务可执行程序           |
| 服务定义文件   | `/usr/lib/systemd/system/zabbix-server.service` | systemd 服务启动配置    |
| 配置文件     | `/etc/zabbix/zabbix_server.conf`                | Zabbix Server 主配置 |
| Web 前端文件 | `/usr/share/zabbix/ui/`                         | 前端网页代码            |
| 日志文件     | `/var/log/zabbix/zabbix_server.log`             | 运行日志              |
| 数据文件     | `/var/lib/mysql/`                               | 存储监控数据            |

结构分区明确、逻辑清晰

---
### RPM 安装背后的逻辑

当你执行：

```
sudo rpm -ivh zabbix-server-mysql.rpm
```

RPM 包管理器做了三件事：
1. **把文件解压到标准路径**（遵守上面那套布局）  
2.  **注册 systemd 服务单元文件**  
3. **安装配置文件到 /etc 并打上“配置文件标记”**（以后升级不会覆盖）
---

### “记忆口诀”
你可以记成一句话：
**「配置在 etc，程序在 bin/lib，数据在 var，前端在 share」**

```
etc → 配置  
usr/bin → 可执行程序  
usr/lib → 程序依赖/服务文件  
var → 运行数据 & 日志  
usr/share → 前端 & 静态资源
```

---

### nginx的结构

拿 nginx 举例：

```
whereis nginx
```

输出类似：

```
nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx /usr/lib/systemd/system/nginx.service
```

这就对应：

- `/usr/sbin/nginx` → 程序
- `/etc/nginx` → 配置
- `/usr/share/nginx` → 静态网页
- `/usr/lib/systemd/system/nginx.service` → 服务文件
完全符合上面的逻辑。

### 重要的文件夹

1. /lost+found 使用标准的ext2/ext3/ext4/文件系统格式产生的目录，目的是在文件系统发生错误的时候将遗失碎片放置到这里
2. /proc 虚拟文件系统 存放的内存中的数据
3. /sys 同/proc类似记录内存核心与系统硬件的相关信息

# 第2章  Linux发展及系统安装

互联网飞速发展，用户对网站体验要求也越来越高，目前主流WEB网站后端承载系统均为Linux操作系统，Android手机也是基于Linux内核而研发，企业大数据、云存储、虚拟化等先进技术也均是基于Linux操作系统为载体，满足企业的高速发展。

本章向读者介绍Linux发展前景、Windows与Linux操作系统区别、硬盘分区介绍、CentOS 7 Linux操作系统安装图解及菜鸟学好Linux必备大绝招。

## Windows操作系统简介

为什么要学习Windows操作系统呢，了解Windows系统结构，可以让我们快速学习Linux操作系统，通过对比学习的方法，我们可以更快的学会Linux。

计算机硬件组成包括：CPU、内存、网卡、硬盘、DVD光驱、电源、主板、显示器、鼠标、键盘等设备，计算机硬件是不能直接被人使用的，需要在其上安装各种操作系统，安装完操作系统，并安装驱动程序，方可进行操作、办公、上网冲浪等。

计算机的硬件组成：

n  CPU，相当于人的大脑，中央处理器；

n  内存，存储设备，临时存储，CPU所需要数据，从内存中读取，内存读写速度很快；

n  硬盘，持久化设备，内存空间小，费用高，大量的数据存在硬盘，硬盘读写速度比内存慢；（SSD、SAS、SATA）；

驱动程序主要指的是设备驱动程序（Device Driver），是一种可以使[计算机](http://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA)系统和设备通信的特殊程序，相当于[硬件](http://baike.baidu.com/item/%E7%A1%AC%E4%BB%B6)的接口，[操作系统](http://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)只有通过这个接口，才能控制[硬件](http://baike.baidu.com/item/%E7%A1%AC%E4%BB%B6)设备，进行资源调度。

 Windows操作系统主要以窗口形式对用户展示，操作系统须安装在硬盘上，安装系统之前需对硬盘进行分区并格式化，默认Windows操作系统安装在C盘分区，D盘分区用于存放数据文件。

通俗的讲，安装操作系统时，需要对磁盘进行格式化，格式化需要指定格式化的类型，告诉操作系统如何去管理磁盘空间，文件如何存放，如何查找及调用。操作系统不知道怎么存放文件以及文件结构，文件系统概念就诞生了。

文件系统是[操作系统](http://baike.baidu.com/view/880.htm)用于明确[磁盘](http://baike.baidu.com/view/157418.htm)或分区上文件的方法和[数据存储结构](http://baike.baidu.com/view/9900.htm)，文件系统由三部分组成：与文件管理有关软件、被管理文件以及实施文件管理所需数据结构。

Windows操作系统，文件系统类型一般有FAT、FAT16、FAT32、NTFS等，不同的文件系统类型，有不同的特性，例如NTFS文件系统类型支持文件及文件夹安全设置，而FAT32文件系统类型不支持，NTFS支持单文件最多为单个磁盘分区的容量大小2T，而FAT32单个最大文件不能超过4GB。

Windows操作系统从设计层面来讲，主要用来管理[电脑硬件](https://www.baidu.com/s?wd=%E7%94%B5%E8%84%91%E7%A1%AC%E4%BB%B6&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d-mH04uAR1nWRdnyn3nvf0IAYqnWm3PW64rj0d0AP8IA3qPjfsn1bkrjKxmLKz0ZNzUjdCIZwsrBtEXh9GuA7EQhF9pywdQhPEUiqkIyN1IA-EUBtknjfdrjnznjfsPHDsPWb4nHT4)与软件资源的程序，大致包括五个方面的管理功能:进程与处理机管理、作业管理、存储管理、设备管理、[文件管理](https://www.baidu.com/s?wd=%E6%96%87%E4%BB%B6%E7%AE%A1%E7%90%86&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d-mH04uAR1nWRdnyn3nvf0IAYqnWm3PW64rj0d0AP8IA3qPjfsn1bkrjKxmLKz0ZNzUjdCIZwsrBtEXh9GuA7EQhF9pywdQhPEUiqkIyN1IA-EUBtknjfdrjnznjfsPHDsPWb4nHT4)。Windwos操作系统从个人使用角度来讲，主要用于个人电脑办公、软件安装、上网冲浪、游戏、数据分析、数据存储等功能。

## 硬盘分区简介

学习Windows、Linux操作系统，必然要了解硬盘设备，硬盘是电脑主要的存储[媒介](http://baike.baidu.com/item/%E5%AA%92%E4%BB%8B)之一，硬盘要能够安装系统或者存放数据，必须进行分区和格式化，Windows系统常见分区有三种：主磁盘分区、扩展磁盘分区、逻辑磁盘分区。

一块硬盘设备，主分区至少有1个，最多4个，扩展分区可以为0，最多1个，且主分区+扩展分区总数不能超过4个，逻辑分区可以有若干个。在Windows下激活的主分区是硬盘的启动分区，他是独立的，也是硬盘的第一个分区，通常就是我们所说的C盘系统分区。

扩展分区是不能直接用的，他是以逻辑分区的方式来使用的，扩展分区可分成若干逻辑分区。他们的关系是包含的关系，所有的逻辑分区都是扩展分区的一部分。

在Windows系统安装时，硬盘驱动器是通过磁盘0,磁盘1来显示,其中磁盘0表示第一块硬盘,磁盘1表示第二块硬盘,然后在第一块硬盘磁盘0上进行分区，最多不能超过4个主分区,分区为C、D、E、F。

硬盘接口是[硬盘](http://baike.baidu.com/item/%E7%A1%AC%E7%9B%98)与[主机系统](http://baike.baidu.com/item/%E4%B8%BB%E6%9C%BA%E7%B3%BB%E7%BB%9F)间的连接部件，作用是在硬盘[缓存](http://baike.baidu.com/item/%E7%BC%93%E5%AD%98)和主机内存之间传输数据。不同的硬盘接口决定着硬盘与计算机之间的连接速度，在整个系统中，硬盘接口的优劣直接影响着程序运行快慢和系统性能好坏，常见的硬盘接口类型为：[IDE](http://baike.baidu.com/item/IDE)（Integrated Drive Electronics）、[SATA](http://baike.baidu.com/item/SATA)（Serial Advanced Technology Attachment）、[SCSI](http://baike.baidu.com/item/SCSI)（Small Computer System Interface）、SAS（Serial Attached SCSI）和[光纤通道](http://baike.baidu.com/item/%E5%85%89%E7%BA%A4%E9%80%9A%E9%81%93)等。

[IDE接口](http://baike.baidu.com/item/IDE%E6%8E%A5%E5%8F%A3)硬盘多用于家用，部分也应用于传统[服务器](http://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E5%99%A8)，[SCSI、SAS接口](http://baike.baidu.com/item/SCSI%E6%8E%A5%E5%8F%A3)的硬盘则主要应用于服务器市场，而光纤通道用于高端服务器上，SATA主要用于个人家庭办公电脑及低端服务器。

在Linux操作系统中，读者可以看到硬盘驱动器的第一块IDE硬盘接口的硬盘设备为hda，或者SATA硬盘接口的硬盘设备为sda，主分区编号为hda1-4或者sda1-4，逻辑分区从5开始。如果有第二块硬盘，主分区编号为hdb1-4或者sdb1-4。

不管是Windows还是Linux操作系统，硬盘的总容量=主分区的容量+扩展分区的容量，而扩展分区的容量=各个逻辑分区的容量之和。主分区也可成为“引导分区”，会被操作系统和主板认定为这个硬盘的第一个分区，所以C盘永远都是排在所有磁盘分区的第一的位置上。

MBR（Master Boot Record）和GPT（GUID Partition Table）是在磁盘上存储分区信息的两种不同方式。这些分区信息包含了分区从哪里开始的信息，这样操作系统才知道哪个扇区是属于哪个分区的，以及哪个分区是可以启动操作系统的。

在磁盘上创建分区时，必须选择MBR或者GPT，默认是MBR，也可以通过其他方式修改为GPT方式。MBR分区的硬盘最多支持4个主分区，如果想支持更多主分区，可以考虑使用GPT格式分区。

## Linux安装环境准备


## 菜鸟学好Linux大绝招

Linux系统安装是初学者的门槛，系统安装完毕后，很多初学者不知道该如何学习，不知道如何快速进阶，下面作者总结了菜鸟学好Linux技能的大绝招：

1. 初学者完成Linux系统分区及安装之后，需熟练掌握Linux系统管理必备命令：cd、ls、pwd、clear、chmod、chown、chattr、useradd、userdel、groupadd、vi、vim、cat、more、less、mv、cp、rm、rmdir、touch、ifconfig、ip addr、ping、route、echo、wc、expr、bc、ln、head、tail、who、hostname、top、df、du、netstat、ss、kill、alias、man、tar、zip、unzip、jar、fdisk、free、uptime、lsof、lsmod、lsattr、dd、date、crontab、ps、find、awk、sed、grep、sort、uniq等，每个命令至少练习30遍，逐步掌握每个命令的用法及应用场景；
2. 初学者进阶之路，需熟练构建Linux下常见服务：NTP、VSFTPD、DHCP、SAMBA、DNS、Apache、MySQL、Nginx、Zabbix、Squid、Varnish、LVS、Keepalived、ELK、MQ、Zookeeper、Docker、Openstack、Hbase、Mongodb、Redis、CEPH、Prometheus、Jenkins、SVN、GIT等，遇到问题先思考，没有头绪可以借助百度、Google搜索引擎，问题解决后，将解决问题的步骤总结并形成文档；
3. 理解操作系统的每个命令，每个服务的用途，为什么要配置这个服务，为什么需要调整该参数，只有带着目标去学习才能更快的成长，才能让你去发掘更多新知识；
4. 熟练搭建Linux系统上各种服务之后，需要理解每个服务的完整配置和优化，可以拓展思维。例如LNMP所有服务放在一台机器上，能否分开放在多台服务器以平衡压力呢，该如何去构建和部署呢？一台物理机构建Docker虚拟化，如果是100台、1000台如何去实施呢，会遇到哪些问题呢；
5. Shell是Linux最经典的命令解释器，Shell脚本可以实现自动化运维，平时多练习Shell脚本编程，每个Shell脚本多练习几遍，从中吸取关键的参数、语法，不断的练习，不断的提高；
6. 建立个人学习博客，把平时工作、学习中的知识都记录到博客，一方面可以供别人参考，另一方面可以提高自己文档编写及总结的能力；
7. 学习Linux技术是一个长期的过程，一定要坚持，遇到各种错误、问题可以借助百度、Google搜索引擎，如果解决不了，可以请教同学、朋友及你的老师；
8. 通过以上步骤的学习方法，不断进步，如果想达到高级、资深大牛级别，还需要进一步深入学习WEB集群架构、网站负载均衡、网站架构优化、自动化运维、运维开发、虚拟化、云计算、分布式集群等知识；
9. 最后一句话，多练习才是硬道理，实践出真知。

# 第3章 CentOS系统管理

## 操作系统启动概念

### BIOS

基本输入输出系统（Basic Input Output System，BIOS）是一组固化到计算机主板上的只读内存[镜像](http://baike.baidu.com/item/%E9%95%9C%E5%83%8F/1574)（Read Only Memory image，ROM）芯片上的程序，它保存着计算机最重要的基本输入输出的程序、系统设置信息、开机后自检程序和系统自启动程序。主要功能是为计算机提供最底层的、最直接的硬件设置和控制。

### MBR

### GPT

### GRUB

### Linux操作系统启动流程

## NetworkManager概念剖析

NetworkManager是2004年Red Hat启动的项目，旨在能够让Linux用户更轻松地处理现代网络需求，尤其是无线网络，能自动发现网卡并配置ip地址。类似在手机上同时开启wifi和蜂窝网络，自动探测可用网络并连接，无需手动切换。虽然初衷是针对无线网络，但在服务器领域，NM已大获成功。

NM能管理各种网络：

n  有线网卡、无线网卡；

n  动态ip、静态ip；

n  以太网、非以太网；

n  物理网卡、虚拟网卡。

NM使用方法：

n  nmcli：命令行。这是最常用的工具，本文将详细讲解该工具使用；

n  nmtui：在shell终端开启文本图形界面；

n  Freedesktop applet：如GNOME上自带的网络管理工具；

n  cockpit：redhat自带的基于web图形界面的"驾驶舱"工具，具有dashborad和基础管理功能。

为什么要用NM？

n  工具齐全：命令行、文本界面、图形界面、web；

n  广纳天地：纳管各种网络，有线、无线、物理、虚拟；

n  参数丰富：多达200多项配置参数（包括ethtool参数）；

n  一统江湖：RedHat系、Suse系、Debian/Ubuntu系，均支持；

n  大势所趋：下一个大版本的rhel只能通过NM管理网络。

Nmcli使用方法：

nmcli使用方法非常类似linux ip命令、cisco交换机命令，并且支持tab补全（详见本文最后的Tips），也可在命令最后通过-h、--help、help查看帮助。在nmcli中有2个命令最为常用：

n  nmcli connection

译作连接，可理解为配置文件，相当于ifcfg-ethX。可以简写为nmcli c

n  nmcli device

译作设备，可理解为实际存在的网卡（包括物理网卡和虚拟网卡）。可以简写为nmcli d

在NM里，有2个维度：连接（connection）和设备（device），这是多对一的关系。想给某个网卡配ip，首先NM要能纳管这个网卡。设备里存在的网卡（即nmcli d可以看到的），就是NM纳管的。接着，可以为一个设备配置多个连接（即nmcli c可以看到的），每个连接可以理解为一个ifcfg配置文件。同一时刻，一个设备只能有一个连接活跃。可以通过nmcli c up切换连接。

Nm connection有2种状态：

n  活跃（带颜色字体）：表示当前该connection生效

n  非活跃（正常字体）：表示当前该connection不生效

Device有4种常见状态：

n  connected：已被NM纳管，并且当前有活跃的connection

n  disconnected：已被NM纳管，但是当前没有活跃的connection

n  unmanaged：未被NM纳管

n  unavailable：不可用，NM无法纳管，通常出现于网卡link为down的时候（比如ip link set ethX down）

## NMCLI常见命令实战

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| #查看ip（类似于ifconfig、ip addr）<br><br>nmcli<br><br>#创建connection，配置静态ip（等同于配置ifcfg，其中BOOTPROTO=none，并ifup启动）<br><br>nmcli c add type ethernet con-name ethX ifname ethX ipv4.addr 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual<br><br>#创建connection，配置动态ip（等同于配置ifcfg，其中BOOTPROTO=dhcp，并ifup启动）<br><br>nmcli c add type ethernet con-name ethX ifname ethX ipv4.method auto<br><br>#修改ip（非交互式）<br><br>nmcli c modify ethX ipv4.addr '192.168.1.100/24'<br><br>nmcli c up ethX<br><br>#修改ip（交互式）<br><br>nmcli c edit ethX<br><br>nmcli> goto ipv4.addresses<br><br>nmcli ipv4.addresses> change<br><br>Edit 'addresses' value: 192.168.1.100/24<br><br>Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes<br><br>nmcli ipv4> save<br><br>nmcli ipv4> activate<br><br>nmcli ipv4> quit<br><br>#启用connection（相当于ifup）<br><br>nmcli c up ethX<br><br>#停止connection（相当于ifdown）<br><br>nmcli c down<br><br>#删除connection（类似于ifdown并删除ifcfg）<br><br>nmcli c delete ethX<br><br>#查看connection列表<br><br>nmcli c show<br><br>#查看connection详细信息<br><br>nmcli c show ethX<br><br>#重载所有ifcfg或route到connection（不会立即生效）<br><br>nmcli c reload<br><br>#重载指定ifcfg或route到connection（不会立即生效）<br><br>nmcli c load /etc/sysconfig/network-scripts/ifcfg-ethX<br><br>nmcli c load /etc/sysconfig/network-scripts/route-ethX<br><br>#立即生效connection，有3种方法<br><br>nmcli c up ethX<br><br>nmcli d reapply ethX<br><br>nmcli d connect ethX<br><br>#查看device列表<br><br>nmcli d<br><br>#查看所有device详细信息<br><br>nmcli d show<br><br>#查看指定device的详细信息<br><br>nmcli d show ethX<br><br>#激活网卡<br><br>nmcli d connect ethX<br><br>#关闭无线网络（NM默认启用无线网络）<br><br>nmcli r all off<br><br>#查看NM纳管状态<br><br>nmcli n<br><br>#开启NM纳管<br><br>nmcli n on<br><br>#关闭NM纳管（谨慎执行）<br><br>nmcli n off<br><br>#监听事件<br><br>nmcli m<br><br>#查看NM本身状态<br><br>nmcli<br><br>#检测NM是否在线可用<br><br>nm-online |

## TCP/IP协议概述

要学好Linux，对网络协议也要有充分的了解和掌握，例如传输控制协议/[因特网](http://baike.baidu.com/view/1706.htm)互联协议（Transmission Control Protocol/Internet Protocol，TCP/IP），TCP/IP名为网络[通讯协议](http://baike.baidu.com/view/278358.htm)，是Internet最基本的协议、Internet国际[互联网](http://baike.baidu.com/view/6825.htm)络的基础，由[网络层](http://baike.baidu.com/view/239600.htm)的IP协议和[传输层](http://baike.baidu.com/view/239605.htm)的TCP协议组成。

TCP/IP 定义了电子设备如何连入[因特网](http://baike.baidu.com/view/1706.htm)，以及数据如何在它们之间传输的标准。协议采用了4层的层级结构，每一层都呼叫它的下一层所提供的协议来完成自己的需求。

TCP负责发现[传输](http://baike.baidu.com/view/389471.htm)的问题，一有问题就发出信号，要求重新传输，直到所有[数据安全](http://baike.baidu.com/view/2308446.htm)正确地传输到目的地，而IP是给[因特网](http://baike.baidu.com/view/1706.htm)的每台联网设备规定一个地址。

基于TCP/IP的参考模型将协议分成四个层次，它们分别是网络接口层、网际互连层（IP层）、[传输层](http://baike.baidu.com/view/239605.htm)（TCP层）和应用层。如图3-9为TCP/IP跟OSI参考模型层次的对比：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817661366-21a5ea5f-5aec-4689-adf2-05e75ba90c80.png)

图3-9 ISO7层模型与TCP/IP四层对比

     OSI模型与TCP/IP模型协议功能实现对照表，如图3-10所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817661533-64561174-9ec2-4624-97ee-057c06b0f9e6.png)

图3-10 ISO7层模型与TCP/IP层次功能对比

## IP地址及网络常识

互联网协议地址（Internet Protocol Address，IP），IP地址是[IP协议](http://baike.baidu.com/view/2802.htm)提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。IP地址被用来给[Internet](http://baike.baidu.com/view/11165.htm)上的每个通信设备的一个编号，每台联网的PC上都需要有IP地址，这样才能正常通信。

IP地址是一个32位的二进制数，通常被分割为4个“8位[二进制](http://baike.baidu.com/view/18536.htm)数”（即4个字节）。IP地址通常用“[点分十进制](http://baike.baidu.com/view/828066.htm)”表示成（a.b.c.d）的形式，其中，a,b,c,d都是0~255之间的十进制整数。

常见的IP地址，分为[IPv4](http://baike.baidu.com/view/21992.htm)与[IPv6](http://baike.baidu.com/view/5228.htm)两大类。IP地址编址方案将IP地址空间划分为A、B、C、D、E五类，其中A、B、C是基本类，D、E类作为多播和保留使用。

IPV4有4段数字，每一段最大不超过255。由于互联网的蓬勃发展，IP位址的需求量愈来愈大，使得IP位址的发放愈趋严格，各项资料显示全球IPv4位址在2011年已经全部分发完毕。

地址空间的不足必将妨碍互联网的进一步发展。为了扩大[地址空间](http://baike.baidu.com/view/1507129.htm)，拟通过IPv6重新定义地址空间。IPv6采用128位地址长度。在IPv6的设计过程中除了一劳永逸地解决了地址短缺问题以外，IPV6的诞生可以给全球每一粒沙子配置一个IP地址，还考虑了在IPv4中解决不好的其它问题，如图3-11所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817661747-81487434-c6cb-4535-9685-5759b597c38b.png)

图3-11 IPV4与IPV6地址

### **IP** **地址分类**

IPV4地址编址方案有A、B、C、D、E五类，其中A、B、C是基本类，D、E类作为多播和保留使用，如下为分类详解：

1.      A类IP地址

一个A类IP地址是指，在IP地址的四段号码中，第一段号码为网络号码，剩下的三段号码为本地计算机的号码。如果用二进制表示IP地址的话，A类IP地址就由1字节的网络地址和3字节主机地址组成，网络地址的最高位必须是“0”。A类IP地址中网络的标识长度为8位，主机标识的长度为24位，A类网络地址数量较少，有126个网络，每个网络可以容纳主机数达1600万台。

A类IP地址 地址范围1.0.0.0到127.255.255.255 （二进制表示为：00000001 00000000 00000000 00000000 - 01111110 11111111 11111111 11111111），最后一个为广播地址，A类IP地址的子网掩码为255.0.0.0，每个网络支持的最大主机数为256的3次方-2=16777214台。

2.      B类IP地址

一个B类IP地址是指在IP地址的四段号码中，前两段号码为网络号码。如果用二进制表示IP地址的话，B类IP地址就由2字节的网络地址和2字节主机地址组成，网络地址的最高位必须是“10”。

B类IP地址中网络的标识长度为16位，主机标识的长度为16位，B类网络地址适用于中等规模的网络，有16384个网络，每个网络所能容纳的计算机数为6万多台。

B类IP地址地址范围128.0.0.0-191.255.255.255（二进制表示为：10000000 00000000 00000000 00000000----10111111 11111111 11111111 11111111）。

 最后一个是广播地址，B类IP地址的子网掩码为255.255.0.0，每个网络支持的最大主机数为256的2次方-2=65534台。

3.      C类IP地址

一个C类IP地址是指在IP地址的四段号码中，前三段号码为网络号码，剩下的一段号码为本地计算机的号码。如果用二进制表示IP地址的话，C类IP地址就由3字节的网络地址和1字节主机地址组成，网络地址的最高位必须是“110”。C类IP地址中网络的标识长度为24位，主机标识的长度为8位，C类网络地址数量较多，有209万余个网络。适用于小规模的局域网络，每个网络最多只能包含254台计算机。

C类IP地址范围192.0.0.0-223.255.255.255[3]  （二进制表示为: 11000000 00000000 00000000 00000000 - 11011111 11111111 11111111 11111111）。C类IP地址的子网掩码为255.255.255.0，每个网络支持的最大主机数为256-2=254台。

4.      D类IP地址

D类IP地址又称之为多播地址(Multicast Address)，即组播地址。在以太网中，多播地址命名了一组应该在这个网络中应用接收到一个分组的站点。多播地址的最高位必须是“1110”，范围从224.0.0.0到239.255.255.255。

5.      特殊的地址

每一个字节都为0的地址（“0.0.0.0”）表示当前主机，IP地址中的每一个字节都为1的IP地址（“255．255．255．255”）是当前子网的广播地址，IP地址中凡是以“11110”开头的E类IP地址都保留用于将来和实验使用。

IP地址中不能以十进制“127”作为开头，而以数字127．0．0．1到127．255．255．255段的IP地址称为回环地址，用于回路测试，如：127.0.0.1可以代表本机IP地址，网络ID的第一个8位组也不能全置为“0”，全“0”表示本地网络。

### **子网掩码**

子网掩码(Subnet Mask)又名[网络掩码](http://baike.baidu.com/view/1169777.htm)、[地址掩码](http://baike.baidu.com/view/4040643.htm)，它是一种用来指明一个[IP地址](http://baike.baidu.com/view/3930.htm)的哪些位标识的是[主机](http://baike.baidu.com/view/23880.htm)所在的子网，以及哪些位标识的是主机的位掩码。

通常的讲，子网掩码不能单独存在，它必须结合IP地址一起使用。子网掩码只有一个作用，就是将某个IP地址划分成[网络地址](http://baike.baidu.com/view/547479.htm)和[主机地址](http://baike.baidu.com/view/547482.htm)两部分。

子网掩码是一个32位地址，用于屏蔽IP地址的一部分以区别网络标识和主机标识，并说明该IP地址是在局域网上，还是在远程网上。

对于A类地址，默认的子网掩码是255.0.0.0，而对于B类地址来说默认的子网掩码是255.255.0.0；对于C类地址来说默认的子网掩码是255.255.255.0。

[互联网](http://baike.baidu.com/view/6825.htm)是由各种小型[网络构成](http://baike.baidu.com/view/2039649.htm)的，每个网络上都有许多[主机](http://baike.baidu.com/view/23880.htm)，这样便构成了一个有层次的结构。IP地址在设计时就考虑到地址分配的层次特点，将每个IP地址都分割成[网络号](http://baike.baidu.com/view/2271538.htm)和[主机](http://baike.baidu.com/view/23880.htm)号两部分，以便于IP地址的[寻址](http://baike.baidu.com/view/1303626.htm)操作。

子网掩码的设定必须遵循一定的规则。与[二进制](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6)IP地址相同，子网掩码由1和0组成，且1和0分别连续。子网掩码的长度也是32位，左边是网络位，用[二进制](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6)数字“1”表示，1的数目等于网络位的长度；右边是主机位，用二进制数字“0”表示，0的数目等于主机位的长度。

### **网关地址**

[网关](http://baike.baidu.com/view/807.htm)（Gateway）是一个网络连接到另一个网络的“关口”， 网关实质上是一个网络通向其他网络的IP[地址](http://baike.baidu.com/view/494802.htm)。主要用于不同网络传输数据。

例如我们电脑设备上网，如果是接入到同一个交换机，在交换机内部传输数据是不需要经过网关的，但是如果两台设备不在一个交换机网络，则需要在本机配置网关，内网服务器的数据通过网关，网关把数据转发到其他的网络的网关，直至找到对方的主机网络，然后返回数据。

### **MAC** **地址**

媒体访问控制（Media Access Control或者Medium Access Control，MAC），也即是物理地址、硬件地址，用来定义[网络设备](http://baike.baidu.com/view/1158081.htm)的位置。

在[OSI模型](http://baike.baidu.com/view/571842.htm)中，第三层[网络层](http://baike.baidu.com/view/239600.htm)负责 [IP地址](http://baike.baidu.com/view/3930.htm)，第二层数据链路层则负责 MAC地址。因此一个主机会有一个MAC地址，而每个[网络位置](http://baike.baidu.com/view/1643855.htm)会有一个专属于它的IP地址。

IP地址工作在OSI参考模型的第三层网络层。两者之间分工明确，默契合作，完成通信过程。IP地址专注于网络层，将数据包从一个网络转发到另外一个网络；而MAC地址则专注于数据链路层，将一个数据帧从一个节点传送到相同链路的另一个节点。

IP地址和MAC地址一般是成对出现。如果一台计算机要和网络中另一外计算机通信，那么这两台设备必须配置IP地址和MAC地址，而MAC地址是网卡出厂时设定的，这样配置的IP地址就和MAC地址形成了一种对应关系。

在数据通信时，IP地址负责表示计算机的网络层地址，网络层设备（如路由器）根据IP地址来进行操作；MAC地址负责表示计算机的数据链路层地址，数据链路层设备，根据MAC地址来进行操作。IP和MAC地址这种映射关系是通过[地址解析协议](http://baike.baidu.com/view/149421.htm)（Address Resolution Protocol，ARP）来实现的。

## Linux系统配置IP

Linux操作系统安装完毕，那接下来如何让Linux操作系统能上外网呢？如下为Linux服务器配置IP的方法。

     Linux服务器网卡默认配置文件在/etc/sysconfig/network-scripts/下，命名的名称一般为:ifcfg-eth0或者ifcfg-ens32 ，例如DELL R720标配有4块千兆网卡，在系统显示的名称依次为：eth0、eth1、eth2、eth3。

修改服务器网卡IP地址命令为vi /etc/sysconfig/network-scripts/ifcfg-eth0 （注CentOS7网卡名ifcfg-ens32）。vi命令打开网卡配置文件，默认为DHCP方式，配置如下：

|   |
|---|
|DEVICE=eth0<br><br>BOOTPROTO=dhcp<br><br>HWADDR=00:0c:29:52:c7:4e<br><br>ONBOOT=yes<br><br>TYPE=Ethernet|

vi命令打开网卡配置文件，修改BOOTPROTO为DHCP方式，同时添加IPADDR、NETMASK、GATEWAY信息如下：

|   |
|---|
|DEVICE=eth0<br><br>BOOTPROTO=static<br><br>HWADDR=00:0c:29:52:c7:4e<br><br>ONBOOT=yes<br><br>TYPE=Ethernet<br><br>IPADDR=192.168.1.103<br><br>NETMASK=255.255.255.0<br><br>GATEWAY=192.168.1.1|

服务器网卡配置文件，详细参数如下：

|   |
|---|
|DEVICE=eth0   #物理设备名ONBOOT=yes   # [yes\|no]（重启网卡是否激活网卡设备）BOOTPROTO=static #[none\|static\|bootp\|dhcp]（不使用协议\|静态分配\|BOOTP协议\|DHCP协议）<br><br>TYPE=Ethernet           #网卡类型<br><br>IPADDR=192.168.1.103    #IP地址NETMASK=255.255.255.0  #子网掩码GATEWAY=192.168.1.1    #网关地址|

服务器网卡配置完毕后，重启网卡服务：/etc/init.d/network restart 即可。

然后查看ip地址，命令为：ifconfig或者ip addr show 查看当前服务器所有网卡的IP地址。

CentOS 7 Linux中，如果没有ifconfig命令，可以用ip addr list/show查看，也可以安装ifconfig命令，需安装软件包net-tools，命令如图3-12所示：

|   |
|---|
|yum install net-tools -y|

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817661902-2cf6b853-bed5-4d6e-80da-312797d7ddd1.png)

图3-12 YUM安装net-tools工具

## Linux系统配置DNS

如上网卡IP地址配置完毕，如果服务器需上外网，还需配置域名解析地址（Domain Name System，DNS），DNS主要用于将请求的域名转换为IP地址，DNS地址配置方法如下：

修改vi  /etc/resolv.conf 文件，在文件中加入如下两条:

|   |
|---|
|nameserver 202.106.0.20<br><br>nameserver 8.8.8.8|

如上分别表示主DNS于备DNS，DNS配置完毕后，无需重启网络服务,DNS是立即生效。

可以ping  -c  6  [www.baidu.com](http://www.baidu.com/) 查看返回结果，如果有IP返回，则表示服务器DNS配置正确，如图3-13所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817662115-c2b3b60c-bfa7-4f9d-96d4-78fe5ee95b74.png)

图3-13 ping命令返回值

## Linux网卡名称命名

CentOS7服务器，默认网卡名为ifcfg-ens32，如果我们想改成ifcfg-eth0，使用如下步骤即可：

（1）      编辑/etc/sysconfig/grub文件，命令为vi /etc/sysconfig/grub，在倒数第二行quiet后加入如下代码，并如图3-14所示：

|   |
|---|
|net.ifnames=0 biosdevname=0|

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817662323-aa4855b6-b0f4-4a24-93a2-2c390563fbb4.png)

图3-14 网卡配置ifnames设置

（2）      执行命令grub2-mkconfig -o /boot/grub2/grub.cfg，生成新的grub.cfg文件，如图3-15所示：

|   |
|---|
|grub2-mkconfig -o /boot/grub2/grub.cfg|

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817662514-aae8d23e-620b-450c-ba3b-cc189fd4ac07.png)

图3-15 生成新的grub.cnf文件

（3）      重命名网卡名称，执行命令mv ifcfg-ens32 ifcfg-eth0，修改ifcfg-eth0文件中DEVICE= ens32为DEVICE= eth0，如图3-16所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817662732-473a07d8-fd45-49df-aeb3-b021023d4e42.png)

图3-16 重命名网卡名称

（4）      重启服务器，并验证网卡名称是否为eth0，Reboot完后，如图3-17所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817662927-1734993a-d968-49b9-a970-ab822073200c.png)图3-17 验证网卡设备名称

## CentOS7和8密码重置

修改CentOS7 ROOT密码非常简单，只需登录系统，执行命令passwd回车即可，但是如果忘记ROOT，无法登录系统，该如何去重置ROOT用户的密码呢？如下为重置ROOT用户的密码的方法：

（1）      Reboot重启系统，系统启动进入欢迎界面，加载内核步骤时，按e，然后选中“CentOS Linux （3.10.0-327.e17.x86_64）7 （Core）”，如图3-18所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817663170-cac6d9e3-2d8f-499a-b471-74320bfe2c32.png)

图3-18 内核菜单选择界面

（2）      继续按e进入编辑模式，找到ro  crashkernel=auto xxx项，将ro改成rw init=/sysroot/bin/sh，如图3-19-1和2所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817663349-43f75516-55f8-4391-8cfc-6bb6dfd04851.png)

图3-19-1 （CentOS7）内核编辑界面

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817663580-00d130f1-1921-4ec0-8af5-162f49e57028.png)

图3-19-2（CentOS8） 内核编辑界面

（3）      修改为后如图3-20所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817663770-5695d746-7d58-4702-8f07-0697d05c300d.png)

图3-20 内核编辑界面

（4）      按ctrl+x按钮进入单用户模式，如图3-21所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817663957-2439da6d-aeae-40f4-a5b9-e0c2c366e76d.png)

图3-21 进入系统单用户模式

（5）      执行命令chroot  /sysroot访问系统，并使用passwd修改root密码，如图3-22所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817664173-4e7cbef3-c55d-4cc6-83c2-00d62f93133b.png)

图3-22 修改ROOT用户密码

（6）      更新系统信息，touch  /.autorelabel，执行命令touch /.autorelabel，在/目录下创建一个.autorelabel文件，如果该文件存在，系统在重启时就会对整个文件系统进行relabeling重新标记，可以理解为对文件进行底层权限的控制和标记，如果seLinux属于disabled关闭状态则不需要执行这条命令，如图3-23所示：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817664452-776667c9-2ba6-49df-853b-00b42cd36118.png)

图3-23 创建autorelabel文件

## 远程管理Linux服务器

系统安装完毕后，可以通过远程工具来连接到Linux服务器，远程连接服务器管理的好处在于可以跨地区管理服务器，例如读者在北京，想管理的服务器在上海某IDC机房，通过远程管理后，不需要到IDC机房现场去操作，直接通过远程工具即可管理，与在现场的管理是一模一样。

远程管理Linux服务器要满足如下三个步骤：

（1）      服务器配置IP地址，如果服务器在公网，需配置公网IP，如果服务器在内部局域网，可以直接配置内部私有IP即可；

（2）      服务器安装SSHD软件服务并启动该服务，几乎所有的Linux服务器系统安装完毕均会自动安装并启动SSHD服务，SSHD服务监听22端口，关于SSHD服务、OpenSSH及SSH协议后面章节会讲解；

（3）      在服务器中防火墙服务需要允许22端口对外开放，初学者可以临时关闭防火墙，CentOS6 Linux关闭防火墙的命令：service iptables stop，而CentOS7 Linux关闭防火墙的命令：systemctl  stop  firewalld.service。

常见的Linux远程管理工具包括：SecureCRT、Xshell、Putty、Xmanger等工具。目前主流的远程管理Linux服务器工具为SecureCRT，官网[https://www.vandyke.com](https://www.vandyke.com) 下载并安装SecureCRT，打开工具，点击左上角quick connect快速连接，弹出界面如图3-24所示，连接配置具体步骤如下：

q  协议（P）：选择SSH2

q  主机名（H）：输入Linux服务器IP地址

q  端口（o）: 22

q  防火墙（F）：None

q  用户名（U）：root

单击下方的“连接”，会提示输入密码，输入root用户对应密码即可。

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817664694-f323a099-474a-4cd6-aa5a-2b95e8024854.png)

图3-24 SecureCRT远程Linux服务器

通过SecureCRT远程连接Linux服务器之后，会发现如图3-25所示界面，与服务器本地操作界面一样，在命令行可以执行命令，操作结果与在服务器现场操作是一样。

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817664899-1f1b5a16-a97f-4a62-9643-25a0b3c815de.png)

图3-25  SecureCRT远程Linux服务器

## Linux系统目录功能

通过以上知识的学习,读者已经能够独立安装并配置Linux服务器IP并远程连接，为了进一步学习Linux，需熟练掌握Linux系统各个目录的功能。

Linux主要树结构目录包括：/、/root、/home、/usr、/bin、/tmp、/sbin、/proc、/boot等，如图3-26所示，为典型的Linux目录结构如下：

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1737817665126-22e62704-5f02-4124-9f85-0d3a9cd6e8c4.png)

图3-26 Linux目录树形结构

Linux系统中常见目录功能如下：

q  / 根目录；

q  /bin 存放必要的命令；

q  /boot 存放内核以及启动所需的文件；

q  /[dev](http://www.baike.com/wiki/dev) 存放硬件设备文件；

q  /etc 存放系统配置文件；

q  /home 普通用户的宿主目录，用户数据存放在其主目录中； 

q  /lib|lib64  存放必要的运行库；

q  /mnt 存放临时的映射文件系统，通常用来挂载使用；

q  /proc 存放存储进程和系统信息；

q  /root 超级用户的主目录；

q  /sbin 存放系统管理程序；

q  /tmp 存放临时文件；

q  /usr  存放应用程序，命令程序文件、程序库、手册和其它文档；

q  /var  系统默认日志存放目录。
# 第4章 Linux必备命令集

## 文件/目录操作

### 2.1.1. mkdir

```
# 默认不是递归创建文件
mkdir ./目录名 

mkdir ./test/test1 # mkdir: 无法创建目录"./test/test1": 没有那个文件或目录

# 创建不存在的父目录
mkdir -p 目录路径
```

### 2.1.2. ls

文件的权限 通过ls -l可以查看文件的权限  
权限一共10位字符第一位字符为文件类型  
后面九位三三一组，分别位文件所以者，群组，其他人员  
每三位又可以分为r,w,x分别对于读，写，执行的权限权值分别位4，2，1，如果没有权限则权值为0  
chmod 修改权限的方法  
修改test.txt权限为 ‘- rwx r-- r--’ chmod 744 test.txt  
也可以利用+ - = 来设置  
chmod u=rwx,g/o=r test.txt chmod u+/-/=rwx test.txt  
u 拥有者 a 所以人 o 其他人 g 群组

### 2.1.3. mv

### 2.1.4. rm

```
rm -rf ./xxx/xx
```

### 2.1.5. find

#### 2.1.5.1. 常用参数

| 参数                                      | 说明                                                         |
| --------------------------------------- | ---------------------------------------------------------- |
| -print0                                 | 使得找到得文件名使用\0结尾，而不是换行，因为\0不可用于文件夹或者文件名所以可以用来有效避免文件名导致得的各种问题 |
| -maxdepth 1                             | 设置find遍历递归的最大深度，值为1说明只遍历当前目录，不遍历子目录中的文件                    |
| prinft ""                               | printf 可以格式化输出，在输出路径名的同时输出其他想要观察的内容，提高效率                   |
| -name                                   | 按名称匹配文件                                                    |
| -size +10M（M,k）                         | 根据文件的大小匹配文件，+10M表示大于10MB，-10M表示小于10MB,10M表示10MB的文件         |
| -mtime +10（天数）10恰好10天，+10大于10天，-10小于10天 | 根据修改时间的天数匹配文件，还有类似的根据分钟匹配的参数                               |
| -exec 命令 +（+可以把多条命令批量执行，提高速度）           | 在找到文件的同时，可以对这个文件执行相关命令的参数 使用占位符{},将路径传递给命令                 |
| -ok 命令 同exec在执行命令的时候需要用户确认              | 同exec参数，在其基础上询问用户是否执行该操作                                   |

#### 2.1.5.2. `find` 时间相关参数

| 参数         | 单位       | 含义                         | 数字前符号解释                                                             |
| ---------- | -------- | -------------------------- | ------------------------------------------------------------------- |
| `-mtime n` | 天（24 小时） | 文件 **内容最后修改时间**            | `n`<br><br>= 恰好 n 天前，`+n`<br><br>= 超过 n 天前，`-n`<br><br>= 少于 n 天前    |
| `-atime n` | 天（24 小时） | 文件 **最后访问时间**              | 同上                                                                  |
| `-ctime n` | 天（24 小时） | 文件 **状态变化时间**（权限、所有者、链接数等） | 同上                                                                  |
| `-mmin n`  | 分钟       | 文件 **内容最后修改时间**            | `n`<br><br>= 恰好 n 分钟前，`+n`<br><br>= 超过 n 分钟前，`-n`<br><br>= 少于 n 分钟前 |
| `-amin n`  | 分钟       | 文件 **最后访问时间**              | 同上                                                                  |
| `-cmin n`  | 分钟       | 文件 **状态变化时间**              | 同上                                                                  |

**示例**

假设当前时间是 2025-09-25 12:00：

1. 找出最近 3 天修改过的文件：

```
find . -mtime -3
```

1. 找出恰好 2 天前访问过的文件：

```
find . -atime 2
```

1. 找出最近 60 分钟修改过的文件：

```
find . -mmin -60
```

1. 找出超过 7 天未访问的文件：

```
find . -atime +7
```

#### 2.1.5.3. `printf` 参数（GNU find 支持）

`-printf` 可以自定义输出格式，类似 C 语言的 `printf`。

**基本语法**

```
find /path -type f -printf "format_string\n"
```

**常用占位符**

|占位符|含义|
|---|---|
|`%p`|文件路径（默认输出）|
|`%f`|文件名，不含路径|
|`%h`|文件所在目录（不包含文件名）|
|`%s`|文件大小（字节）|
|`%k`|文件大小（单位 1024 字节）|
|`%u`|文件所有者用户名|
|`%g`|文件所属组|
|`%m`|文件权限（八进制，如 755）|
|`%t`|文件最后修改时间（ctime）|
|`%T@`|文件最后修改时间（UNIX 时间戳）|
|`%Tc`|文件最后修改时间（完整可读格式）|
|`%y`|文件类型（f = 普通文件，d = 目录，l = 链接）|
|`\n`|换行|
|`\t`|制表符|

---

**示例**

1. 输出文件路径和大小（字节）：

```
find . -type f -printf "%p %s\n"
```

1. 输出文件权限、大小、所有者和文件名：

```
find . -type f -printf "%m %s %u %f\n"
```

1. 输出最近修改的时间和文件名：

```
find . -type f -printf "%Tc %f\n"
```

1. 输出文件路径，每个路径用 NUL 字符分隔（常配合 `xargs -0` 使用）：

```
find . -type f -print0
```

---

#### 2.1.5.4. `ls` 参数（简单格式化）

```
find /path -type f -ls
```

效果类似 `ls -l`，会显示：

- inode
- 权限
- 链接数
- 所有者
- 文件大小
- 最后修改时间
- 文件路径

---

⚡ 小技巧：

- `-printf` 可自定义输出，更灵活
- `-ls` 输出简洁，适合快速查看
- 和管道组合时，推荐使用 `-printf "%p\0"` + `xargs -0` 安全处理文件名有空格或特殊字符的情况

#### 2.1.5.5. 案例

- `find ... -print0`：输出结果时，用 **NUL 字符（\0）** 分隔文件名，而不是换行。（NUL 是二进制 0，合法文件名中不会出现它，所以能保证唯一安全的分隔符。）
- `tar --null -T -`：告诉 `tar` 从输入里读文件列表时，**用 NUL 来分隔**，而不是默认的换行。

这样就能正确区分出：

- `a b.txt` 仍然是一个完整文件名。
- `weird name.txt`（文件名里真的有换行）也能被正确识别。

**使用find批量删除文件**

```
# 使用find命令找到当前目录以及子目录的所有txt文件，然后执行rm, + 符号会最后批量执行命令提高效率
find . -name "*.txt" -exec rm -f {} +
# 错误示范 这样会打包多个文件，然后后一个文件覆盖前一个文件
find . -name "*.txt" -exec tar zcvf backup.tar.gz {} + 
```

**使用find找到当前目录下所txt然后压缩**

```
# find 命令先找到符合条件的文件路径，然后通过-print0参数设置分割符为\0，
# 使用\0做分割符可以避免文件名使用空格等符号带来的问题，因为文件名不能包含\0
# --null 是告诉tar使用\0做分割符 -T 是从文件获取被压缩的文件名
find . -name "*.txt" -print0 | tar -zcvf backup.tar.gz --null  -T -
```

### 2.1.6. ln

为文件创建链接

```
# 将时区文件下的shanghai软连接到/etc/localtime，这样系统时区就变为shanghai时区
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 2.2. 文件查看

### 2.2.1. cat

```bash
# 创建MySQL配置文件
cat > /etc/mysql/my.cnf <<EOF
[mysqld]
server-id = 2
log_bin = /var/log/mysql/mysql-bin.log
relay_log = /var/log/mysql/mysql-relay-bin.log
read_only = 1
binlog_format = ROW

[client]
default-character-set = utf8mb4
EOF

```


### 2.2.2. tail（看文件末尾）

```
# 默认读取最后10行
tail 文件
# 读取最后指定字节
tail -c 10kb 文件
# 监视指定的文件内容变化
tail -f mail.log # 等同于--follow=descriptor，根据文件描述符进行追踪，当文件改名或被删除，追踪停止
tail -F mail.log # 等同于--follow=name --retry，根据文件名进行追踪，并保持重试，即该文件被删除或改名后，如果再次创建相同的文件名，会继续追踪
 # 显示 mail.log 最后的 25 行
tail -25 mail.log
```

### 2.2.3. head（看文件开头）

默认显示前10行

### 2.2.4. less（分页查看大文件）

比 `more` 更强大，可以前后翻页，还能搜索。

- 基本用法：

```
less /var/log/messages
```

- 常用操作：

- **方向键 / PgUp / PgDn / 空格** → 翻页
- `g` → 跳到文件开头
- `G` → 跳到文件末尾
- `/关键字` → 向下搜索
- `?关键字` → 向上搜索
- `n` → 下一个匹配
- `N` → 上一个匹配
- `q` → 退出

参数：

- `-N` → 显示行号
- `-S` → 不自动换行（长行截断显示）
- `-X` → 退出后保留屏幕内容

### 2.2.5. more

### 2.2.6. hexdump

以多种进制查看文件内容

```
hexdump ./a.out | head -n 10
```

```
0000000 457f 464c 0102 0001 0000 0000 0000 0000
0000010 0002 003e 0001 0000 1040 0040 0000 0000
0000020 0040 0000 0000 0000 3ca8 0000 0000 0000
0000030 0000 0000 0040 0038 000d 0040 0020 001f
0000040 0006 0000 0004 0000 0040 0000 0000 0000
0000050 0040 0040 0000 0000 0040 0040 0000 0000
0000060 02d8 0000 0000 0000 02d8 0000 0000 0000
0000070 0008 0000 0000 0000 0003 0000 0004 0000
0000080 0318 0000 0000 0000 0318 0040 0000 0000
0000090 0318 0040 0000 0000 001c 0000 0000 0000
```

![](1758856874229-6dab4486-caf3-4422-b74f-6304eb52f3f1.png)

## 2.3. 文本编辑/处理

### 2.3.1. vim编辑器

#### 2.3.1.1. 命令模式

方向键

kjhl 分别对应上下左右

复制粘贴

v自由复制选取 可以具体的选择单词的每一个部分

V 行选区,直接选择当前行

选择之后使用y复制

yy 复制当前行

yw复制一个单词

p粘贴

跳转

f 往后搜索

F往前搜索

可以数字+e/w/b 表次数

e/E 跳到单词的尾部,一直往后

w/W往后跳跳到单词的首部

b往回跳到单词首部

^/0 行首

$ 行尾

*当前单词的首字母

% 在当前块的两个括号反复横跳

数字加方向键为在该方向走多少行

G移动到文件最后一行

gg移动到第一行

数字加gg 跳转到指定行号

H 屏幕当前显示的第一行

M 当前显示的中部

L 当前显示的尾部

zz将光标所在行变为屏幕中间位置

块级删除

```
 public void foo(){
     // 使用 ci",会删除引号内的块,而不会删除引号
     // 使用ca"会删除引号内的块以及引号	
     // " 可以换为其他的例如{ [ 
    new String ("helloworld");
}
```

插入模式

- i 在光标之前，进入插入模式
- I 在本行开头，进入插入模式
- a 在光标之后，进入插入模式
- A 在本行结尾，进入插入模式
- o 在本行之后新增一行，并进入插入模式
- O 在本行之前新增一行，并进入插入模式
- s 删除当前字符，并进入插入模式
- S 删除当前行中的所有文本，并进入插入模式

idea共享系统剪切板

```
"共享剪切板
set clipboard+=unnamed
"--将 jj 和 jk 映射为 <Esc>
"jj和jk为主流配置，可按喜好自行调整
imap jj <Esc>
```

#### 2.3.1.2. 显示行号

```
:set nu 或 :set number
使 Vim 在编辑窗口左侧显示每行的行号，方便定位和跳转。
-- 关闭行号
:set nonu 或 :set nonumber

-- 查看当前行号的状态
:set nu ?
```

#### 2.3.1.3. 指定区域中搜索替换字符串

```
-- 搜索55行到100行中的class 替换为CLASS
:55,100s/class/CLASS/gic
```

|部分|含义|
|---|---|
|`:55,100`|55行到100行|
|`s`|substitute，替换命令|
|`/class/`|要查找的字符串（不区分大小写，因为有 `i`）|
|`/CLASS/`|要替换成的新字符串|
|`g`|替换每行中所有匹配项（global）|
|`i`|忽略大小写（ignore case）|
|`c`|每次替换前都提示确认（confirm）|

```
-- 全文替换
:1,$s/class/CLASS/gic
:%s/class/CLASS/gic
```

#### 2.3.1.4. 长方形区域操作

```
--删除55行~100行开头10个字符
ctrl+v + 100gg + 10+l +d
```

#### 2.3.1.5. 保存文件

|命令|含义|
|---|---|
|`:w`|保存当前文件|
|`:w filename`|另存为其他文件|
|`:w !sudo tee filename`|以 root 权限保存到文件|

如果没有权限保存（如系统配置文件）：

你可以用 Vim 的特技命令保存为 root：

```
vim
:w !sudo tee man.test.config
```

这条命令的意思是：

- 把当前内容通过管道传给 `sudo tee man.test.config` 来写入
- 是绕过“没有写权限”的好办法

#### 2.3.1.6. 多文件/多窗口编辑

假设一个例子，你想要将刚刚我们的 hosts 内的 IP 复制到你的 /etc/hosts 这个档案去， 那么该如何编辑？ 我们知道在 vi 内可以使用 ：r filename 来读入某个档案的内容， 不过，这样毕竟是将整个档案读入啊！ 如果我只是想要部分内容呢？ 呵呵！ 这个时候多档案同时编辑就很有用了。 我们可以使用 vim 后面同时接好几个档案来同时开启喔！

多文件单窗口编辑

```
vim 文件1 文件2
```

|命令|备注|
|---|---|
|:n|编辑下一个文件|
|:N|编辑上一个文件|
|:files|列出可编辑文件|

多窗口单文件

```
:sp 文件名（不写默认打开当前文件在另一个水平窗口）
:vs 文件名（不写默认打开当前文件在另一个垂直窗口）
-- 关闭窗口
-- 切换到指定窗口 然后 :q 或者 ctrl+w c 或 ctrl+w q
-- 关闭其他窗口只保留当前窗口 :only 
```

#### 2.3.1.7. 窗口操作命令

|命令|备注|
|---|---|
|`ctrl+w+j`|切换到下面这个窗口|
|`ctrl+w+l`|切换到右面这个窗口|
|`ctrl+w+h`|切换到左面这个窗口|
|`crtl+w+k`|切换到上面这个窗口|
|`Ctrl+w w`|在窗口间切换|
|`Ctrl+w =`|让所有窗口宽度相等|
|`Ctrl+w <`|减小当前窗口宽度|
|`Ctrl+w >`|增加当前窗口宽度（ctrl+w 10>）|

多窗口多文件

```
:vs 或 :vsplit  另一个文件 
:sp 或 :split  另一个文件
```

#### 2.3.1.8. 挑字补全

如果你只想用“挑字补全”这个功能，用 `Ctrl+n` / `Ctrl+p` 就够了。

在插入模式输入字符`ima`然后按`ctrl+x`+`ctrl+n`

|插入模式组合键|功能说明|
|---|---|
|`Ctrl+n`  <br>  <br>/ `Ctrl+p`|补全当前 buffer 中的单词（最常用）|
|`Ctrl+x Ctrl+n`|补全当前 buffer 的关键字|
|`Ctrl+x Ctrl+p`|补全所有 buffer 的关键字|
|`Ctrl+x Ctrl+l`|补全整行（Line）|
|`Ctrl+x Ctrl+f`|补全文件名（File）|
|`Ctrl+x Ctrl+d`|补全宏定义（Define，依赖 Ctags）|
|`Ctrl+x Ctrl+]`|补全标签（Tags）|
|`Ctrl+x Ctrl-k`|用字典补全（dictionary）|
|`Ctrl+x Ctrl-t`|用 thesaurus 补全（词库）|
|`Ctrl+x Ctrl-i`|来自包含的头文件中的关键字补全|
|`Ctrl+x Ctrl-v`|Vim 脚本文件名补全|
|`Ctrl+x s`|拼写建议补全（需要启用拼写检查）|

#### 2.3.1.9. 打开指定文件

```
:e filename.txt
```

- 这会让 Vim 打开 `filename.txt` 文件，原来的 buffer 会被当前文件替代（如果你没保存会警告）。

#### 2.3.1.10. 重新加载当前文件内容（放弃所有修改）：

```
:e!
```

- 加了 `!`，强制重新载入当前文件的内容（**放弃你所有未保存的更改**）。
- 相当于“还原到磁盘上的版本”。

#### 2.3.1.11. 示例命令一览

|命令|作用|
|---|---|
|`:e`|重新载入当前文件（若无参数）|
|`:e foo.txt`|打开 `foo.txt`  <br>  <br>文件|
|`:e!`|强制重载当前文件，放弃本地更改|
|`:e %`|重新打开当前文件（`%`  <br>  <br>代表当前文件）|
|`:e #`|切换回上一个编辑的文件|

#### 2.3.1.12. 小技巧

- 想查看当前打开文件名？

```
 :echo @%
```

- 快速打开当前目录下文件（自动补全）：

```
 :e <Tab>
```

连按两次 `<Tab>` 会列出目录下所有文件供你选择。

```
:e . → 打开当前目录浏览器（可以上下选文件）
```

![](lake%20import/学习笔记/Attachments/1751534718862-39c7ca87-471d-4bf9-a611-d71a3a1dbe64.png "null")

### 2.3.2. tee（ 从标准输入读取数据，同时写到标准输出和文件里）

- 不会打断管道（照常输出到终端/下一个命令）。
- 同时把内容保存到文件。

#### 2.3.2.1. 参数

```
[wrw@vbox linux_study]$ tee --help
用法：tee [选项]... [文件]...
将标准输入复制到每个指定文件，并显示到标准输出。

  -a, --append          内容追加到给定的文件而非覆盖
  -i, --ignore-interrupts       忽略中断信号
  -p                        对写入非管道的行为排查错误
      --output-error[=模式]   设置写入出错时的行为。见下面“模式”部分
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

模式确定向输出写入出错时的行为：
  'warn'         对向任何文件输出出错的情况进行诊断
  'warn-nopipe'  对向除了管道以外的任何文件输出出错的情况进行诊断
  'exit'         一旦输出出错，则退出程序
  'exit-nopipe'  一旦输出出错且非管道，则退出程序
-p 选项的默认模式是“warn-nopipe”。
当 --output-error 没有给出时，默认的操作是在向管道写入出错时立刻退出，
且在向非管道写入出错时对问题进行诊断。

GNU coreutils 在线帮助：<https://www.gnu.org/software/coreutils/>
请向 <http://translationproject.org/team/zh_CN.html> 报告任何翻译错误
完整文档 <https://www.gnu.org/software/coreutils/tee>
或者在本地使用：info '(coreutils) tee invocation'
```

#### 2.3.2.2. 案例

#### 2.3.2.3. 读入标准输入，并重定向标准输出

通过`cat`将index.html的内容读到标准输入，通过|管道符将标准输入传给`tee`，`tee`在将标准输入写入文件的同时，输出到标准输出，使用重定向符`>`将内容输出到`/dev/null`。自此屏幕上不显示输出 。

```
cat index.html | tee index.html.bak > /dev/null
```

#### 2.3.2.4. 从vim中读入标准输入

直接使用下面的命令会在当前目录创建名称为`**tee ce**`的文件。

```
:w tee ce
```

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1751592697320-b8f5c237-3e62-4c49-a0a3-1a00952a80b7.png "null")

Vim 中 `:w` 的语法是：

```
vim
:w [filename]
```

它只接受**一个参数**作为文件名。

所以：

```
vim
:w tee cc
```

被 Vim 解释为：

```
bash
:w teecc
```

🟥 你以为是运行 shell 命令 `tee cc`，实际上 Vim 把 `tee` 和 `cc` 当成了一个文件名 `teecc`。

---

✅ 正确做法（使用 shell 命令写入文件）

你需要在 Vim 中加 `!` 来表明：**要把内容传给 shell 命令** `tee cc`，而不是保存成叫 `teecc` 的文件。

```
vim
:w !tee cc
```

这才会：

- 把当前 buffer 的内容作为标准输入发送给 `tee`
- `tee` 把它写到 `cc` 文件中

---

✅ 推荐加 `/dev/null` 防止干扰输出：

```
vim
:w !tee cc > /dev/null
```

否则 `tee` 会把内容再打印一遍到 Vim 的命令窗口里，很乱。

### 2.3.3. cut

从输入流中截取字符串是非常常见且实用的需求，特别是在你用命令行或脚本处理日志、配置、文本输出、命令结果等场景中。

#### 2.3.3.1. 参数

```
[wrw@vbox linux_study]$ cut --help
用法：cut [选项]... [文件]...
从每个输入<文件>中输出指定部分到标准输出。

如果没有指定文件，或者文件为"-"，则从标准输入读取。

必选参数对长短选项同时适用。
  -b, --bytes=列表              只选中指定的这些字节
  -c, --characters=列表         只选中指定的这些字符
  -d, --delimiter=分界符        使用指定分界符代替制表符作为区域分界
  -f, --fields=LIST       select only these fields;  also print any line
                            that contains no delimiter character, unless
                            the -s option is specified
  -n                      with -b: don't split multibyte characters
      --complement              补全选中的字节、字符或域
  -s, --only-delimited          不打印没有包含分界符的行
      --output-delimiter=字符串 使用指定的字符串作为输出分界符，默认采用输入
                                的分界符
  -z, --zero-terminated    以 NUL 字符而非换行符作为行尾分隔符
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

仅使用f -b, -c 或-f 中的一个。每一个列表都是专门为一个类别作出的，或者您可以用逗号隔
开要同时显示的不同类别。您的输入顺序将作为读取顺序，每个仅能输入一次。
每种参数格式表示范围如下：
    N     从第1个开始数的第N个字节、字符或域
    N-    从第N个开始到所在行结束的所有字符、字节或域
    N-M   从第N个开始到第M个之间(包括第M个)的所有字符、字节或域
    -M    从第1个开始到第M个之间(包括第M个)的所有字符、字节或域

GNU coreutils 在线帮助：<https://www.gnu.org/software/coreutils/>
请向 <http://translationproject.org/team/zh_CN.html> 报告任何翻译错误
完整文档 <https://www.gnu.org/software/coreutils/cut>
或者在本地使用：info '(coreutils) cut invocation'
```

#### 案例

#### 2.3.3.2. 从winget的升级信息中挑出要升级应用的id

```
PS C:\Users\wrwww> winget update
名称                                           ID                                    版本          可用          源
-----------------------------------------------------------------------------------------------------------------------
Microsoft Visual C++ 2005 Redistributable (x6… Microsoft.VCRedist.2005.x64           8.0.56336     8.0.61000     winget
Apple Mobile Device Support                    Apple.AppleMobileDeviceSupport        18.0.0.45     18.5.0.13     winget
TortoiseSVN 1.13.1.28686 (64 bit)              TortoiseSVN.TortoiseSVN               1.13.28686    1.14.9.29743  winget
IntelliJ IDEA 2025.1.1.1                       JetBrains.IntelliJIDEA.Ultimate       2025.1.1.1    2025.1.3      winget
QQ                                             Tencent.QQ.NT                         9.9.18.33800  9.9.20.36580  winget
QQ音乐                                         Tencent.QQMusic                       21.11         21.60         winget
Wireshark 4.4.3 x64                            WiresharkFoundation.Wireshark         4.4.3         4.4.7         winget
Visual Studio Community 2022                   Microsoft.VisualStudio.2022.Community < 17.14.6     17.14.6       winget
Microsoft Visual C++ 2015-2022 Redistributabl… Microsoft.VCRedist.2015+.x86          14.44.35112.1 14.44.35211.0 winget
Microsoft Visual C++ 2015-2022 Redistributabl… Microsoft.VCRedist.2015+.x64          14.44.35112.1 14.44.35211.0 winget
PowerShell 7.5.1.0-x64                         Microsoft.PowerShell                  7.5.1.0       7.5.2.0       winget
企业微信                                       Tencent.WeCom                         4.1.36.6011   4.1.38.6021   winget
Cursor (User)                                  Anysphere.Cursor                      1.1.6         1.2.1         winget
UC浏览器                                       Alibaba.UC                            1.1.0.6       1.1.0.8       winget
14 升级可用。
```

```
[wrw@vbox linux_study]$ cat ./update_info | grep "微信" | tr -s ' '  |cut -d ' ' -f 2  -
Tencent.WeCom
```

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1751614927644-788c079c-a76d-4415-ba54-56abffd9bd99.png "null")

使用cat读取文本到输入流通过grep 提取有含有微信的那行字符串，应用名称和id之间含有太多空格了不能直接通过cut去划分然后截取，使用tr去去掉重复字符， tr -s [字符集] 它会把输入中连续出现的相同字符压缩为单个字符。这样就成为`企业微信 Tencent.WeCom 4.1.36.6011 4.1.38.6021 winget`这样之后我们使用cut去截取，拿到第二个字符片段。

#### 2.3.3.3. 从日志中提取时间戳

```
[ERROR] 2025-07-04 17:40:01 - Database connection failed
```

```
echo "[ERROR] 2025-07-04 17:40:01 - Database connection failed" | cut -d ' ' -f 2-3  -
```

#### 2.3.3.4. 从命令输出中提取版本号字段

**命令输出：**

```
node -v
v18.17.1
```

**要求：**  
提取出纯版本号 `18.17.1`（去掉前面的 `v`）

```
-- 使用这个可能不是最优，这里只是用来练习cut
node -v | cut -d 'v' -f 2 -
```

#### 2.3.3.5. 压缩空格 + 取第 3 列（表格题）

**模拟表格行：**

```
QQ音乐             Tencent.QQMusic          21.11       21.60      winget
```

**要求：**  
提取出当前版本号字段 `21.11`，你需要先压缩空格使字段对齐。

```
[wrw@vbox linux_study]$ echo "QQ音乐             Tencent.QQMusic          21.11       21.60      winget" | tr -s ' ' | cut -d ' ' -f 3 -
21.11
```

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1751615809068-ac7057fa-7f97-476e-9fd1-56f43009514c.png "null")

#### 2.3.3.6. 提取文件扩展名

**输入字符串：**

```
 filename="report.final.version3.pdf"
```

**要求：**  
用命令提取出扩展名（`pdf`）。

```
[wrw@vbox linux_study]$ echo 'filename="report.final.version3.pdf"' | cut -d '.' -f 4 - | cut -d '"' -f 1 - 
pdf
```

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1751615941922-e91c2fa4-ef64-44e4-a5bf-b98f6455f929.png "null")

### 2.3.4. tr

### 2.3.5. awk

- **核心思想**：对输入的每一行做「模式–动作」处理。
- 默认以空白（空格或制表符）分隔字段，可通过 `-F` 修改分隔符。

#### 2.3.5.1. 基本语法

```
awk -F '<分隔符>' 'pattern { action }' 文件
```

- `pattern`：正则或条件，匹配到的行才执行 `{ action }`。
- `{ action }`：对每行做的操作（打印、计算、修改…）。

#### 2.3.5.2. 常用内置变量

|变量|含义|
|---|---|
|`$0`|整行文本|
|`$1,$2…`|第1、第2…个字段|
|`NF`|当前行字段数|
|`NR`|已处理行数（行号）|

#### 2.3.5.3. 示例

1. **打印第 2 字段**（等价于 `cut -d ' ' -f2`）

```
awk '{print $2}' logfile.txt
```

2. **用冒号分隔**

```
awk -F: '{print $1, $7}' /etc/passwd
# 显示用户名和 shell
```

3. **条件过滤**

```
awk '$3 > 1000 { print $1, $3 }' data.txt
# 只有第3列大于1000的行才打印用户名和第3列
```

4. **求和/统计**

```
awk '{sum += $2} END {print "Total =", sum}' sales.log
```

5. **复杂脚本**

```
awk -F',' '
  NR==1 {print $0; next}      # 打印表头
  $3 > 100 {printf "%-10s %s\n",$1,$3}
' report.csv
```

#### 2.3.5.4. 案例

#### 2.3.5.5. 提取winget需要升级软件的id

```
PS C:\Users\wrwww> winget update
名称                                           ID                                    版本          可用          源
-----------------------------------------------------------------------------------------------------------------------
Microsoft Visual C++ 2005 Redistributable (x6… Microsoft.VCRedist.2005.x64           8.0.56336     8.0.61000     winget
Apple Mobile Device Support                    Apple.AppleMobileDeviceSupport        18.0.0.45     18.5.0.13     winget
TortoiseSVN 1.13.1.28686 (64 bit)              TortoiseSVN.TortoiseSVN               1.13.28686    1.14.9.29743  winget
IntelliJ IDEA 2025.1.1.1                       JetBrains.IntelliJIDEA.Ultimate       2025.1.1.1    2025.1.3      winget
QQ                                             Tencent.QQ.NT                         9.9.18.33800  9.9.20.36580  winget
QQ音乐                                         Tencent.QQMusic                       21.11         21.60         winget
Wireshark 4.4.3 x64                            WiresharkFoundation.Wireshark         4.4.3         4.4.7         winget
Visual Studio Community 2022                   Microsoft.VisualStudio.2022.Community < 17.14.6     17.14.6       winget
Microsoft Visual C++ 2015-2022 Redistributabl… Microsoft.VCRedist.2015+.x86          14.44.35112.1 14.44.35211.0 winget
Microsoft Visual C++ 2015-2022 Redistributabl… Microsoft.VCRedist.2015+.x64          14.44.35112.1 14.44.35211.0 winget
PowerShell 7.5.1.0-x64                         Microsoft.PowerShell                  7.5.1.0       7.5.2.0       winget
企业微信                                       Tencent.WeCom                         4.1.36.6011   4.1.38.6021   winget
Cursor (User)                                  Anysphere.Cursor                      1.1.6         1.2.1         winget
UC浏览器                                       Alibaba.UC                            1.1.0.6       1.1.0.8       winget
14 升级可用。
```

从单一行字符串中提取

```
[wrw@vbox linux_study]$ echo "QQ音乐             Tencent.QQMusic          21.11       21.60      winget" | awk '{print $2}' -
Tencent.QQMusic
```

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1751616848231-e98a03f9-860a-4d3d-b4a2-88ae7ae5ad41.png "null")

从多行字符串中提取

```
[wrw@vbox linux_study]$ cat ./update_info | awk '/QQ/ {print $2}'
Tencent.QQ.NT
Tencent.QQMusic
[wrw@vbox linux_study]$ cat ./update_info | awk '/音乐/ {print $2}'
Tencent.QQMusic
```

![](lake%20import/学习笔记/Attachments/1751617188711-7f6db532-c26c-4935-a379-6c75f742e327.png "null")

好的！下面是为你量身定制的一篇 **AWK 学习复盘文档**，全面回顾了你在练习中掌握的技能点、出现的问题、解决方法，并补充了一些进阶建议。你可以将此文作为知识总结或未来深入使用 awk 的学习笔记。

---

#### 2.3.5.6. AWK 学习复盘文档

#### 2.3.5.7. 学习背景与目标

通过命令行文本处理（尤其是 `awk`），高效完成如日志分析、数据提取、文本格式转换等工作任务。目标是熟练掌握 awk 的核心语法、使用模式和实际问题解决能力。

#### 2.3.5.8. 学习内容总览

#### 2.3.5.9. AWK 基本结构

```
awk 'pattern { action }' filename
```

- `pattern`：可以是行号、正则、字段比较、逻辑判断等
- `{ action }`：当 pattern 匹配时执行的动作，常见有 `print`、`printf`、变量操作等
- `BEGIN`, `END`：分别在读入前和文件读完后执行

#### 2.3.5.10. 常用内建变量

|变量|含义|
|---|---|
|`$0`|当前行全文|
|`$1`  <br>  <br>~`$n`|第 n 个字段|
|`FS`|输入字段分隔符|
|`OFS`|输出字段分隔符|
|`NR`|当前行号（全局）|
|`FNR`|当前文件行号（多文件时）|
|`NF`|当前行字段数量|

---

#### 2.3.5.11. 常见操作练习总结

#### 2.3.5.12. 提取字段：

```
awk -F',' 'NR > 1 { print $1, $3 }' file.csv
```

#### 2.3.5.13. 条件筛选：

```
awk -F',' 'NR > 1 && $3 > 80 { print $1, $3 }'
```

#### 2.3.5.14. 保留表头 + 条件输出：

```
awk -F',' 'NR==1 || $3 > 80'
```

#### 2.3.5.15. 格式化输出（对齐）：

```
awk -F',' 'NR==1 { printf "%-10s %-4s %-4s\n", $1, $2, $3; next }
           { printf "%-10s %-4s %-4s\n", $1, $2, $3 }'
```

#### 2.3.5.16. 计算总和和平均：

```
awk -F',' 'NR > 1 && $3 ~ /^[0-9]+$/ {
  sum += $3; count++
}
END {
  if (count > 0)
    printf "total:%d\navg:%.2f\n", sum, sum/count
  else
    print "没有有效数据"
}'
```

#### 2.3.5.17. 找出最大值：

```
awk -F',' 'NR==1 { next }
$3 > max { max=$3; name=$1 }
END { print name, max }'
```

#### 2.3.5.18. 分组统计（如每个班级平均分）：

```
awk -F',' '
NR > 1 {
  class_sum[$2] += $3
  class_count[$2]++
}
END {
  for (c in class_sum)
    printf "Class %s avg: %.2f\n", c, class_sum[c] / class_count[c]
}'
```

#### 2.3.5.19. 遇到的问题与解决方式

#### 2.3.5.20. 正则错误：写成 `[0~9]` 而不是 `[0-9]`

- **问题：**`[0~9]` 会匹配非法字符（如 ASCII 范围）
- **解决：** 使用 `[0-9]` 来匹配纯数字

#### 2.3.5.21. 除以 0 错误

- **表现：** awk 报错：`致命错误：试图除以 0`
- **原因：** 没有任何行满足过滤条件，`count == 0`
- **解决：**

```
if (count > 0)
  sum / count
else
  print "没有有效数据"
```

#### 2.3.5.22. 输出格式乱：字符串带逗号、换行混乱

- 原因是 `printf` 格式没有对齐或误用了逗号 `,`
- 正确方式：

```
printf "avg: %.2f\n", sum / count
```

#### 2.3.5.23. 空行干扰处理

- 解决方式：

```
NF > 0 && $3 ~ /^[0-9]+$/
```

或加正则判断 `$0 ~ /^[ \t]*$/ { next }` 跳过全空行。

#### 2.3.5.24. END 写成 end

- awk 的 `END` 是关键字，必须大写
- `end` 会被当成普通变量名 → 无任何效果

#### 2.3.5.25. 正则表达式回顾

|正则|含义|
|---|---|
|`/^[0-9]+$/`|纯数字（整数）|
|`/^-?[0-9]+$/`|可为负整数|
|`/^ *$/`|纯空格或空行|
|`/a/`|包含 `a`的字符串|
|`tolower($1) ~ /a/`|名字中含大小写 a|

#### 2.3.5.26. 实战建议

- 使用 `awk -F','` 指定 CSV 分隔符更稳定
- 处理数据前最好 `NR > 1` 跳过表头
- 用 `NF > 0` 防止空行影响
- `printf` 比 `print` 更适合格式化对齐
- `END {}` 中一定加除零判断
- 结合 `sort`, `uniq`, `cut` 等工具提升效率

#### 2.3.5.27. 目前完成练习回顾

|题号|内容说明|状态|
|---|---|---|
|1|找出最高分的学生|✅ 已完成|
|2|班级分组统计平均|🔜 待挑战|
|3~7|多条件分段统计、字符串匹配等|🔜 可继续练|

#### 2.3.5.28. 总结

目前你已熟练掌握：

- 基本语法结构（包括 `NR`、`$0`、条件语句）
- 字段提取与条件判断
- 字符串正则匹配
- 数值计算（总和、平均、最大）
- 空行处理、防御性编程技巧
- `printf` 格式化输出

下一阶段建议挑战：

- 多文件处理（`awk 'FNR==1 && NR!=1{next}'` 合并去重）
- 嵌套 if/else 结构
- 多字段分组、排序处理（配合 `sort`）
- awk 脚本化执行（保存为 `.awk` 脚本）

### 2.3.6. sed

- **核心思想**：对文本流做「行—操作」编辑，不会把整个文件读入内存。
- 常用于**替换**、**删除/插入**、**行范围处理**。

#### 2.3.6.1. 基本语法

```
sed [选项] '地址 操作' 文件
```

- **地址**：可以是行号、正则 `/pattern/`、范围 `m,n`。
- **操作**：最常见 `s`（substitute），还包括 `d`（delete）、`i`（insert）、`a`（append）、`p`（print）等。

#### 2.3.6.2. 示例

1. **简单替换**

```
sed 's/foo/bar/g' file.txt
# 将每行中的 foo 全部替换为 bar
```

2. **仅替换第 N 处**

```
sed 's/foo/bar/2' file.txt
# 每行只替换第二个 foo
```

3. **按行号删除/打印**

```
sed '3d' file.txt      # 删除第3行
sed -n '5p' file.txt   # 只打印第5行
```

4. **按模式删除**

```
sed '/^#/d' config.txt # 删除以 # 开头的注释行
```

5. **行范围替换**

```
sed '2,4s/foo/bar/g' file.txt
# 只在第2到第4行替换
```

6. **插入和追加**

```
sed '3i\This is inserted before line 3' file.txt
sed '5a\This is appended after line 5' file.txt
```

好的，下面是为你精心整理的一篇 `**sed**` **学习复盘文档**，完整总结了你目前掌握的命令、应用场景、正则技巧、易错点和实战示例。这份笔记可作为你今后使用 `sed` 的操作指南和工具手册。

#### 2.3.6.3. 学习目标

掌握 `sed` 的基本用法，能够在 Linux/WSL/Powershell 中灵活使用它来进行：

- 文本查找替换
- 删除或插入行
- 匹配正则表达式
- 处理配置文件、日志、批量数据

#### 2.3.6.4. 什么是 `sed`

`sed` 是 **stream editor（流编辑器）**，专门用于处理文本流中内容的查找、替换、删除、插入等操作。

它擅长在**非交互式、自动化脚本**中完成对大量文本的快速处理。

#### 2.3.6.5. 基本语法

```
sed [选项] '指令' 文件
```

常用结构：

```
sed 's/旧文本/新文本/标志' file.txt
```

#### 2.3.6.6. 常用选项：

|选项|含义|
|---|---|
|`-n`|安静模式，不自动输出|
|`-i`|就地修改文件（危险）|
|`-e`|使用多条命令|
|`-r`|启用扩展正则（某些版本需要）|

---

#### 2.3.6.7. 常用命令一览

|命令|功能|
|---|---|
|`s/old/new/`|替换|
|`d`|删除匹配行|
|`i\ text`|在前插入新行|
|`a\ text`|在后追加新行|
|`c\ text`|替换整行|
|`p`|打印当前行（配合 `-n`使用）|
|`q`|处理完后立即退出|

#### 2.3.6.8. 替换类操作（`s///`）

#### 2.3.6.9. 当前行替换第一个匹配项

```
sed 's/foo/bar/' file.txt
```

#### 2.3.6.10. 替换每行中所有匹配项

```
sed 's/foo/bar/g'
```

#### 2.3.6.11. 替换整词匹配（避免部分替换）

```
sed 's/\<foo\>/bar/g'
```

#### 2.3.6.12. 多条替换规则

```
sed -e 's/foo/bar/g' -e 's/cat/dog/g' file.txt
```

#### 2.3.6.13. 删除类操作（`d`）

#### 2.3.6.14. 删除空行

```
sed '/^$/d' file.txt
```

#### 2.3.6.15. 删除只有空格或 tab 的行

```
sed '/^[[:space:]]*$/d' file.txt
```

#### 2.3.6.16. 删除第 3 行

```
sed '3d' file.txt
```

#### 2.3.6.17. 删除包含关键词的行

```
sed '/error/d' file.txt
```

#### 2.3.6.18. 删除特定范围（例如 BEGIN 到 END）

```
sed '/BEGIN/,/END/d' file.txt
```

#### 2.3.6.19. 插入 / 替换 / 添加行

#### 2.3.6.20. 在第 3 行上面插入一行

```
sed '3i\--- INSERTED ---' file.txt
```

#### 2.3.6.21. 在第 3 行下面添加一行

```
sed '3a\--- ADDED ---' file.txt
```

#### 2.3.6.22. 替换整行内容

```
sed '3c\This is the new content' file.txt
```

#### 2.3.6.23. 定位和条件控制

#### 2.3.6.24. 只处理特定行

```
sed '2s/foo/bar/' file.txt
```

#### 2.3.6.25. 按条件选择行 + 替换内容

```
sed '/^User:/s/root/admin/' file.txt
```

#### 2.3.6.26. 正则表达式技巧

|表达式|含义|
|---|---|
|`^`|行首|
|`$`|行尾|
|`.`|任意字符|
|`*`|重复 0 次或多次|
|`\+`|重复 1 次或多次（GNU 扩展）|
|`[a-z]`|匹配 a 到 z|
|`\w`  <br>, `\d`  <br>, `\s`|字符类，需开启 `-r`|

#### 2.3.6.27. 实战案例汇总

#### 2.3.6.28. 替换 API key 为新值

```
sed 's/API_KEY=.*/API_KEY=new_key_123/' config.env
```

#### 2.3.6.29. 删除所有注释和空行

```
sed '/^#/d; /^$/d' settings.conf
```

#### 2.3.6.30. 给每行加分号（结尾）

```
sed 's/$/;/' file.txt
```

#### 2.3.6.31. 将 `username:password` 替换为 `username******`

```
sed 's/:[^ ]\+$/******/' users.txt
```

#### 2.3.6.32. 替换并高亮某关键词（用于 debug）

```
sed 's/error/\x1b[31m&\x1b[0m/g' log.txt
```

#### 2.3.6.33. 遇到的问题总结

|问题|解决方法|
|---|---|
|无法追加或插入内容|`i`  <br>  <br>/`a`  <br>  <br>必须新起一行，不能修改行内|
|正则语法不通|检查是否需要加 `-r`  <br>  <br>或转义|
|直接修改文件误删|尽量别用 `-i`  <br>  <br>，先输出到新文件测试|
|行内替换失效|检查是否忘记加 `g`  <br>  <br>或匹配不准|

#### 2.3.6.34. sed 与其他工具对比（与 awk / vim）

|能力|sed|awk|vim|
|---|---|---|---|
|查找替换|✅|✅|✅|
|字段处理|❌|✅|❌|
|多行插入|✅|❌|✅|
|格式输出|❌|✅|✅|
|自动化批处理|✅|✅|❌|
|编辑可视化|❌|❌|✅|

### 2.3.7. 下一步学习建议

1. 熟悉 `sed` 多行处理技巧（使用 `N`, `P` 等）
2. 编写 `sed` 脚本文件，提升可维护性
3. 与 `awk`、`xargs`、`find` 配合批量修改项目文件
4. 用 `-i` 慎重执行（建议备份）
5. 熟练掌握 `s///g` 与 `d` 是基础核心

### 2.3.8. 与 `cut` 对比

|特性|`cut`|`awk`|`sed`|
|---|---|---|---|
|字段提取|简单、速度快|更灵活（算术运算、条件判断、多字段）|只能按字符/字节切，难做字段提取|
|模式匹配|无|强（正则、逻辑组合）|强（正则、大量编辑命令）|
|行修改|不支持|支持（打印修改后文本）|支持（可原地编辑）|
|脚本能力|无|嵌入式脚本、函数|脚本有限，适合流式替换|

### 2.3.9. grep

#### 2.3.9.1. 参数

- `-i`：忽略大小写。
- `-v`：反向选择，即只显示不匹配的行。
- `-c`：仅显示匹配的行数，而不是内容。
- `-l`：仅列出包含匹配项的文件名，而不显示匹配的行。
- `-n`：在输出的每一行前显示行号。
- `-E`：使用扩展正则表达式。
- `-F`：将模式作为固定字符串处理，而不是正则表达式。
- `-r` 或 `-R`：递归地搜索指定目录下的所有文件。

#### 2.3.9.2. 示例

1. **搜索文件中的文本**：

```
bash复制代码
grep "example" file.txt
```

这将搜索`file.txt`中包含"example"文本的所有行，并打印出来。

2. **忽略大小写**：

```
bash复制代码
grep -i "example" file.txt
```

这将忽略大小写地搜索"example"。

3. **反向选择**：

```
bash复制代码
grep -v "example" file.txt
```

这将打印出`file.txt`中不包含"example"的所有行。

4. **显示匹配的行号**：

```
bash复制代码
grep -n "example" file.txt
```

这将打印出包含"example"的每一行，并在行前显示其行号。

5. **递归搜索目录**：

```
bash复制代码
grep -r "example" /path/to/directory
```

这将在`/path/to/directory`目录及其所有子目录中递归地搜索包含"example"的文本。

#### 2.3.9.3. 注意事项

- `grep` 默认使用基本正则表达式（BRE），但你可以通过 `-E` 选项启用扩展正则表达式（ERE），以使用更复杂的匹配模式。
- 对于包含特殊字符的搜索模式，你可能需要对其进行转义或使用引号（单引号或双引号）将其括起来，以避免被 shell 解释。
- `grep` 的性能通常非常好，但如果你在处理非常大的文件或目录时遇到问题，考虑使用更高效的工具或方法，如 `ag`（The Silver Searcher）、`ack` 或 `ripgrep`（`rg`）。

### 2.3.10. sort

默认情况下，`sort` 按 **字典序** 排序：

1. **先比较每行的第一个字符**（左到右）
2. 如果第一个字符相同，就比较第二个字符，依次类推
3. 比较的是 **字符的 ASCII 值**（或者 locale 设置的顺序）

- `app` 和 `apple` 都以 `a` 开头 → 比较第二个字符 → `p` vs `p` → 比较第三个 → `p` vs `p` → 第四个字符 `app` 没了，所以排在前
- `apple` 和 `apricot` → 第四个字符 `l` vs `r` → `l` < `r`，所以 `apple` 在前

|-b|忽略每行前面出现的空格|
|---|---|
|-n|依据数值大小排序，默认是按照ascii排序，1，10，2默认排序后是，1，10，2，加上参数n是1，2，10|
|-z|使用0字节结尾而不是换行，类似find 的-print0 tar的--null|
|-r|倒序|
|-t|设置分隔符，可以用来配合-k按照指定列排序|
|-k|按照第k列排序|

### 2.3.11. uniq

### 2.3.12. wc

## 2.4. 压缩打包

### 2.4.1. 压缩文件的用途与技术

你是否有过文件档案太大，导致无法以正常的 email 方式发送出去 （很多 email 都有容量大约 25MB 每封信的限制啊！ )？ 又或者学校、厂商要求使用 CD 或 DVD 来传递归档用的数据，但是你的单一档案却都比这些传统的一次性储存媒体还要大！ 那怎么分成多片来烧录呢？ 还有，你是否有过要备份某些重要数据，偏偏这些数据量太大了，耗掉了你很多的磁盘空间呢？ 这个时候，那个好用的'档案压缩'技术可就派的上用场了！

因为这些比较大型的档案通过所谓的档案压缩技术之后，可以将他的磁盘使用量降低，可以达到减低档案容量的效果。 此外，有的压缩程序还可以进行容量限制， 使一个大型文件可以分割成为数个小型档案，以方便软碟片携带呢！由于我们记录数字是 1 ，考虑电脑所谓的二进制喔，如此一来， 1 会在最右边占据 1 个 bit ，而其他的 7 个 bits 将会自动的被填上 0 啰！ 你看看，其实在这样的例子中，那 7 个 bits 应该是'空的'才对！ 不过，为了要满足目前我们的作系统数据的访问，所以就会将该数据转为 byte 的型态来记录了！ 而一些聪明的计算机工程师就利用一些复杂的计算方式， 将这些没有使用到的空间'丢'出来，以让档案占用的空间变小！ 这就是压缩的技术啦！

另外一种压缩技术也很有趣，他是将重复的数据进行统计记录的。 举例来说，如果你的数据为'111....'共有100个1时， 那么压缩技术会记录为'100个1'而不是真的有100个1的位存在！ 这样也能够精简档案记录的容量呢！ 非常有趣吧！

简单的说，你可以将他想成，其实档案里面有相当多的'空间'存在，并不是完全填满的， 而'压缩'的技术就是将这些'空间'填满，以让整个档案占用的容量下降！ 不过，这些'压缩过的文件'并无法直接被我们的作系统所使用的，因此， 若要使用这些被压缩过的档案资料，则必须将他'还原'回来未压缩前的模样， 那就是所谓的'解压缩'啰！ 而至于压缩后与压缩的档案所占用的磁盘空间大小， 就可以被称为是'压缩比'啰！

### 2.4.2. 常见的压缩指令

在Linux的环境中，压缩档案的扩展名大多是：'*.tar， *.tar.gz, *.tgz, *.gz, *. Z， *.bz2， *.xz'，为什么会有这样的拓展名呢？ 不是说 Linux 的拓展名没有什么作用吗？

这是因为 Linux 支持的压缩指令非常多，且不同的指令所用的压缩技术并不相同，当然彼此之间可能就无法互通压缩/解压缩档案啰。 所以，当你下载到某个压缩文件时，自然就需要知道该档案是由哪种压缩指令所制作出来的，好用来对照着解压缩啊！ 也就是说，虽然 Linux 文件的属性基本上是与文件名没有绝对关系的， 但是为了帮助我们人类小小的脑袋瓜子，所以适当的副档名还是必要的！ 底下我们就列出几个常见的压缩文件扩展名吧：

```
*.Z         compress 程式壓縮的檔案；
*.zip       zip 程式壓縮的檔案；
*.gz        gzip 程式壓縮的檔案；
*.bz2       bzip2 程式壓縮的檔案；
*.xz        xz 程式壓縮的檔案；
*.tar       tar 程式打包的資料，並沒有壓縮過；
*.tar.gz    tar 程式打包的檔案，其中並且經過 gzip 的壓縮
*.tar.bz2   tar 程式打包的檔案，其中並且經過 bzip2 的壓縮
*.tar.xz    tar 程式打包的檔案，其中並且經過 xz 的壓縮
```

上面就是linux上常见的压缩指令，不过由于这些指令通常只能针对一个文档进行解压缩，很是不方便，于是就出现了打包指令tar，用于将多个文档打包为一个文档然后使用压缩指令进行压缩。后来，[GNU计划](http://www.gnu.org/)中，将整个 tar 与压缩的功能结合在一起，如此一来提供用户更方便并且更强大的压缩与打包功能！

### 2.4.3. gzip命令

gzip命令来自英文单词gunzip的缩写，其功能是压缩和解压文件。gzip是一个使用广泛的压缩命令，文件经过压缩后一般会以.gz后缀结尾，与tar命令合用后即为.tar.gz后缀。目前 gzip 可以解开 compress， zip 与 gzip 等软件所压缩的档案。

#### 2.4.3.1. 参数

```
[wrw@vbox linux_study]$ gzip --help
Usage: gzip [OPTION]... [FILE]...
Compress or uncompress FILEs (by default, compress FILES in-place).

Mandatory arguments to long options are mandatory for short options too.

  -c, --stdout      write on standard output, keep original files unchanged 将压缩后的内容输出到标准输出不删除源文件
  -d, --decompress  decompress 解压
  -f, --force       force overwrite of output file and compress links
  -h, --help        give this help
  -k, --keep        keep (don't delete) input files
  -l, --list        list compressed file contents
  -L, --license     display software license
  -n, --no-name     do not save or restore the original name and timestamp
  -N, --name        save or restore the original name and timestamp
  -q, --quiet       suppress all warnings
  -r, --recursive   operate recursively on directories
      --rsyncable   make rsync-friendly archive
  -S, --suffix=SUF  use suffix SUF on compressed files
      --synchronous synchronous output (safer if system crashes, but slower)
  -t, --test        test compressed file integrity
  -v, --verbose     verbose mode
  -V, --version     display version number
  -1, --fast        compress faster
  -9, --best        compress better

With no FILE, or when FILE is -, read standard input.

Report bugs to <bug-gzip@gnu.org>.
```

#### 2.4.3.2. 示例

```
gzip -c example.txt
```

```
gzip -c example.txt > example.txt.gz
```

```
cat example.txt | gzip -c > example.txt.gz
gzip -c < example.txt > example.txt.gz
```

```
gzip -dc example.txt.gz > example.txt
```

#### 2.4.3.3. 关联命令

zcat/zmore/zless/zgrep分别对应cat/more/less/grep,用于查看压缩后的文本文件内容

### 2.4.4. bzip2命令

若说 gzip 是为了取代 compress 并提供更好的压缩比而成立的，那么 bzip2 则是为了取代 gzip 并提供更佳的压缩比而来的。 bzip2 真是很不错用的东西这玩意的压缩比竟然比 gzip 还要好至于 bzip2 的用法几乎与 gzip 相同！ 看看底下的用法吧！ bzip2 连选项与参数都跟 gzip 一模一样！ 只是扩展名由.gz 变成 .bz2 而已！ 其他的用法都大同小异，

#### 2.4.4.1. 参数

```
[wrw@vbox linux_study]$ bzip2 --help
bzip2, a block-sorting file compressor.  Version 1.0.8, 13-Jul-2019.

   usage: bzip2 [flags and input files in any order]

   -h --help           print this message
   -d --decompress     force decompression
   -z --compress       force compression
   -k --keep           keep (don't delete) input files
   -f --force          overwrite existing output files
   -t --test           test compressed file integrity
   -c --stdout         output to standard out
   -q --quiet          suppress noncritical error messages
   -v --verbose        be verbose (a 2nd -v gives more)
   -L --license        display software version & license
   -V --version        display software version & license
   -s --small          use less memory (at most 2500k)
   -1 .. -9            set block size to 100k .. 900k
   --fast              alias for -1
   --best              alias for -9

   If invoked as `bzip2', default action is to compress.
              as `bunzip2',  default action is to decompress.
              as `bzcat', default action is to decompress to stdout.

   If no file names are given, bzip2 compresses or decompresses
   from standard input to standard output.  You can combine
   short flags, so `-v -4' means the same as -v4 or -4v, &c.
```

#### 2.4.4.2. 相关命令

bzcat/bzmore/bzless/bzgrep

### 2.4.5. xz命令

虽然 bzip2 已经具有很棒的压缩比，不过显然某些自由软件开发者还不满足，因此后来还推出了 xz 这个压缩比更高的软件！ 这个软件的用法也跟 gzip/bzip2 几乎一模一样！ 那我们就来瞧一瞧！

#### 2.4.5.1. 参数

```
[wrw@vbox linux_study]$ xz --help
用法：xz [选项]... [文件]...
使用 .xz 格式压缩或解压缩文件。

  -z, --compress      强制压缩
  -d, --decompress    强制解压缩
  -t, --test          测试压缩文件完整性
  -l, --list          列出 .xz 文件的信息
  -k, --keep          保留（不要删除）输入文件
  -f, --force         强制覆写输出文件和（解）压缩链接
  -c, --stdout        向标准输出写入，同时不要删除输入文件
  -0 ... -9           压缩预设等级；默认为 6；使用 7-9 的等级之前，请先考虑
                      压缩和解压缩所需的内存用量！（会占用大量内存空间）
  -e, --extreme       尝试使用更多 CPU 时间来改进压缩比率；
                      不会影响解压缩的内存需求量
  -T, --threads=数量  使用最多指定数量的线程；默认值为 1；设置为 0
                      可以使用与处理器内核数量相同的线程数
  -q, --quiet         不显示警告信息；指定两次可不显示错误信息
  -v, --verbose       输出详细信息；指定两次可以输出更详细的信息
  -h, --help          显示本短帮助信息并退出
  -H, --long-help     显示长帮助信息（同时列出高级选项）
  -V, --version       显示软件版本号并退出

如果没有指定文件，或者文件为"-"，则从标准输入读取。

请使用英文或芬兰语向 <lasse.collin@tukaani.org> 报告软件错误。
请使用中文向 TP 简体中文翻译团队 <i18n-zh@googlegroups.com>
报告软件的简体中文翻译错误。
XZ Utils 主页：<https://tukaani.org/xz/>
```

#### 2.4.5.2. 案例

```
xz -c text.txt > text.xz
```

```
xz -dc text.xz > text.txt
```

#### 2.4.5.3. 相关命令

xzcat/xzmore/xzless/xzgrep

### 2.4.6. tar命令

前一小节谈到的指令大多仅能针对单一档案来进行压缩，虽然 gzip， bzip2， xz 也能够针对目录来进行压缩，不过， 这两个命令对目录的压缩指的是'将目录内的所有文件“分别” 进行压缩'的动作！ 而不像在 Windows 的系统，可以使用类似 [WinRAR](http://www.rar.com.tw/) 这一类的压缩软件来将好多数据'包成一个档案'的样式。

这种将多个文件或目录包成一个大档案的指令功能，我们可以称呼他是一种'打包指令'啦！ 那 Linux 有没有这种打包指令呢？ 是有的！ 那就是鼎鼎大名的 tar 这个玩意儿了！ tar 可以将多个目录或文件打包成一个大档案，同时还可以通过 gzip/bzip2/xz 的支持，将该文件同时进行压缩！ 更有趣的是，由于 tar 的使用太广泛了，目前 Windows 的 WinRAR 也支持 .tar.gz 档名的解压缩呢！ 很不错吧！ 所以底下我们就来玩一玩这个咚咚！

#### 2.4.6.1. 参数

```
[wrw@vbox linux_study]$ tar --help
用法: tar [选项...] [FILE]...
GNU 'tar' saves many files together into a single tape or disk archive, and can
restore individual files from the archive.

Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
  tar -xf archive.tar          # Extract all files from archive.tar.

 主操作模式:
  -A, --catenate, --concatenate   追加 tar 文件至归档
  -c, --create               创建一个新归档
      --delete               从归档(非磁带！)中删除
  -d, --diff, --compare      找出归档和文件系统的差异
  -r, --append               追加文件至归档结尾
      --test-label           测试归档卷标并退出
  -t, --list                 列出归档内容
  -u, --update               仅追加比归档中副本更新的文件
  -x, --extract, --get       从归档中解出文件
        
 操作修饰符:

      --check-device         当创建增量归档时检查设备号(默认)
  -g, --listed-incremental=FILE   处理新式的 GNU 格式的增量备份
  -G, --incremental          处理老式的 GNU 格式的增量备份
      --hole-detection=TYPE  用于探测holes 的技术
      --ignore-failed-read
                             当遇上不可读文件时不要以非零值退出
      --level=NUMBER         所创建的增量列表归档的输出级别
      --no-check-device      当创建增量归档时不要检查设备号
      --no-seek              归档不可检索
  -n, --seek                 归档可检索
      --occurrence[=NUMBER]  仅处理归档中每个文件的第 NUMBER
                             个事件；仅当与以下子命令 --delete,
                             --diff, --extract 或是 --list
                             中的一个联合使用时，此选项才有效。而且不管文件列表是以命令行形式给出或是通过
                             -T 选项指定的；NUMBER 值默认为 1
      --sparse-version=MAJOR[.MINOR]
                             设置所用的离散格式版本(隐含
                             --sparse)
  -S, --sparse               高效处理离散文件

 本地文件名选择:
      --add-file=FILE        添加指定的 FILE 至归档(如果名字以 -
                             开始会很有用的)
  -C, --directory=DIR        改变至目录 DIR
      --exclude=PATTERN      排除以 PATTERN 指定的文件
      --exclude-backups      排除备份和锁文件
      --exclude-caches       除标识文件本身外，排除包含
                             CACHEDIR.TAG 的目录中的内容
      --exclude-caches-all   排除包含 CACHEDIR.TAG 的目录
      --exclude-caches-under 排除包含 CACHEDIR.TAG
                             的目录中所有内容
      --exclude-ignore=FILE  若存在FILE,
                             则从其中读取每个目录的例外匹配项
      --exclude-ignore-recursive=FILE
                             若存在FILE,
                             则从其中为每个目录及其子目录读取需要排除的例外匹配项
      --exclude-tag=FILE     除 FILE 自身外，排除包含 FILE
                             的目录中的内容
      --exclude-tag-all=FILE 排除包含 FILE 的目录
      --exclude-tag-under=FILE   排除包含 FILE 的目录中的所有内容
      --exclude-vcs          排除版本控制系统目录
      --exclude-vcs-ignores  从VCS 忽略文件中读取排除匹配项
      --no-null              禁用上一次的效果 --null 选项
      --no-recursion         避免目录中的自动降级
      --no-unquote           不要unquote 输入文件或成员名称
      --no-verbatim-files-from   -T
                             把以‘-’开始的文件作为选项(默认)
      --null                 -T 读取以空终止的名字; 隐含
                             --verbatim-files-from
      --recursion            目录递归（默认）
  -T, --files-from=FILE      从 FILE
                             中获取文件名来解压或创建文件
      --unquote              unquote 输入文件或成员名称(默认)
      --verbatim-files-from  -T
                             逐字读取文件名（不处理选项或进行转义）
  -X, --exclude-from=FILE    排除 FILE 中列出的模式串

 文件名匹配选项(同时影响排除和包括模式串):

      --anchored             模式串匹配文件名头部
      --ignore-case          忽略大小写
      --no-anchored          模式串匹配任意‘/’后字符（对
                             exclusion 为默认值）
      --no-ignore-case       匹配大小写(默认)
      --no-wildcards         逐字匹配字符串
      --no-wildcards-match-slash   通配符不匹配‘/’
      --wildcards            使用通配符（对 exclusion 为默认值）
      --wildcards-match-slash   wildcards match '/' (default)

 重写控制:

      --keep-directory-symlink   解压时保留已存在的目录符号链接
      --keep-newer-files
                             不要替换比归档中副本更新的已存在的文件
  -k, --keep-old-files       解压时不替换存在的文件,
                             而将其认为是错误
      --no-overwrite-dir     保留已存在目录的元数据
      --one-top-level[=DIR]  创建子目录以避免解压松散文件
      --overwrite            解压时重写存在的文件
      --overwrite-dir        解压时重写已存在目录的元数据(默认)
                            
      --recursive-unlink     解压目录之前先清除目录层次
      --remove-files         在添加文件至归档后删除它们
      --skip-old-files
                             解压时不替换存在的文件，而是自动忽略
  -U, --unlink-first         在解压要重写的文件之前先删除它们
  -W, --verify               在写入以后尝试校验归档

 选择输出流：

      --ignore-command-error 忽略子进程的退出代码
      --no-ignore-command-error
                             将子进程的非零退出代码认为发生错误
  -O, --to-stdout            解压文件至标准输出
      --to-command=COMMAND
                             将解压的文件通过管道传送至另一个程序

 操作文件属性:

      --atime-preserve[=METHOD]
                             在输出的文件上保留访问时间，要么通过在读取(默认
                             METHOD=‘replace’)后还原时间，要不就不要在第一次(METHOD=‘system’)设置时间
      --clamp-mtime          当文件比 --mtime
                             指定的文件更新时仅更新时间
      --delay-directory-restore
                             直到解压结束才设置修改时间和所解目录的权限
      --group=名称         强制将 NAME
                             作为所添加的文件的组所有者
      --group-map=FILE       用FILE 映射文件所有者GIDs 和名字
      --mode=CHANGES         强制将所添加的文件(符号)更改为权限
                             CHANGES
      --mtime=DATE-OR-FILE   从 DATE-OR-FILE 中为添加的文件设置
                             mtime
  -m, --touch                不要解压文件的修改时间
      --no-delay-directory-restore
                             取消 --delay-directory-restore 选项的效果
      --no-same-owner
                             将文件解压为您所有(普通用户默认此项)
      --no-same-permissions
                             从归档中解压权限时使用用户的掩码位(默认为普通用户服务)
      --numeric-owner        总是以数字代表用户/组的名称
      --owner=名称         强制将 NAME
                             作为所添加的文件的所有者
      --owner-map=FILE       用FILE 映射文件所有者UIDs 和名字
  -p, --preserve-permissions, --same-permissions
                              保留源文件权限
                             解压文件权限信息(默认只为超级用户服务)
      --same-owner
                             尝试解压时保持所有者关系一致(超级用户默认此项)
      --sort=ORDER           目录排序顺序: none(默认), name 或inode
  -s, --preserve-order, --same-order
                             成员参数按归档中的文件顺序列出

 操作extended 文件属性:

      --acls                 开启 POSIX ACLs 支持
      --no-acls              关闭 POSIX ACLs 支持
      --no-selinux           关闭 SELinux 上下文支持
      --no-xattrs            关闭extended 属性支持
      --selinux              开启 SELinux 上下文支持
      --xattrs               开启extended 属性支持
      --xattrs-exclude=MASK  为xattr 关键字指定排除匹配项
      --xattrs-include=MASK  为xattr 关键字指定包含匹配项

 设备选择和切换：

      --force-local
                             即使归档文件存在副本还是把它认为是本地归档
  -f, --file=ARCHIVE         使用归档文件或 ARCHIVE 设备
  -F, --info-script=名称, --new-volume-script=名称
                             在每卷磁带最后运行脚本(隐含 -M)
  -L, --tape-length=NUMBER   写入 NUMBER × 1024 字节后更换磁带
  -M, --multi-volume         创建/列出/解压多卷归档文件
      --rmt-command=COMMAND  使用指定的 rmt COMMAND 代替 rmt
      --rsh-command=COMMAND  使用远程 COMMAND 代替 rsh
      --volno-file=FILE      使用/更新 FILE 中的卷数

 设备分块:

  -b, --blocking-factor=BLOCKS   每个记录 BLOCKS x 512 字节
  -B, --read-full-records    读取时重新分块(只对 4.2BSD 管道有效)
  -i, --ignore-zeros         忽略归档中的零字节块(即文件结尾)
      --record-size=NUMBER   每个记录的字节数 NUMBER，乘以 512

 选择归档格式:

  -H, --format=FORMAT        创建指定格式的归档

 FORMAT 是以下格式中的一种:
    gnu                      GNU tar 1.13.x 格式
    oldgnu                   GNU 格式，其中 tar 版本 <= 1.12
    pax                      POSIX 1003.1-2001 (pax) 格式
    posix                    等同于 pax
    ustar                    POSIX 1003.1-1988 (ustar) 格式
    v7                       旧的 V7 tar 格式

      --old-archive, --portability
                             等同于 --format=v7
      --pax-option=关键字[[:]=值][,关键字[[:]=值]]...
                             控制 pax 关键字
      --posix                等同于 --format=posix
  -V, --label=TEXT           创建带有卷名 TEXT
                             的归档；在列出/解压时，使用 TEXT
                             作为卷名的模式串

 压缩选项:

  -a, --auto-compress        使用归档后缀名来决定压缩程序
  -I, --use-compress-program=PROG
                             通过 PROG 过滤(必须是能接受 -d
                             选项的程序)
  -j, --bzip2                通过 bzip2 过滤归档
  -J, --xz                   通过 xz 过滤归档
      --lzip                 通过 lzip 过滤归档
      --lzma                 通过 xz --format=lzma 过滤归档
      --lzop                 通过 lzop 过滤归档
      --no-auto-compress     不使用归档后缀名来决定压缩程序
      --zstd                 通过 zstd 过滤归档
  -z, --gzip, --gunzip, --ungzip   通过 gzip 过滤归档
  -Z, --compress, --uncompress   通过 compress 过滤归档

 本地文件选择:

      --backup[=CONTROL]     在删除前备份，选择 CONTROL 版本
      --hard-dereference
                             跟踪硬链接；将它们所指向的文件归档并输出
  -h, --dereference
                             跟踪符号链接；将它们所指向的文件归档并输出
  -K, --starting-file=MEMBER-NAME
                             从归档中的 MEMBER-NAME
                             成员处开始读取归档
      --newer-mtime=DATE     当只有数据改变时比较数据和时间
  -N, --newer=DATE-OR-FILE, --after-date=DATE-OR-FILE
                             只保存比 DATE-OR-FILE 更新的文件
      --one-file-system      创建归档时保存在本地文件系统中
  -P, --absolute-names       不要从文件名中清除引导符‘/’
      --suffix=STRING        在删除前备份，除非被环境变量
                             SIMPLE_BACKUP_SUFFIX
                             覆盖，否则覆盖常用后缀(‘’)

 文件名变换:

      --strip-components=NUMBER   解压时从文件名中清除 NUMBER
                             个引导部分
      --transform=EXPRESSION, --xform=EXPRESSION
                             使用 sed 代替 EXPRESSION
                             来进行文件名变换

 提示性输出:

      --checkpoint[=NUMBER]  每隔 NUMBER
                             个记录显示进度信息(默认为 10 个)
      --checkpoint-action=ACTION   在每个检查点上执行 ACTION
      --full-time            按文件原本时间格式打印
      --index-file=FILE      将详细输出发送至 FILE
  -l, --check-links
                             只要不是所有链接都被输出就打印信息
      --no-quote-chars=STRING   禁用来自 STRING 的字符引用
      --quote-chars=STRING   来自 STRING 的额外的引用字符
      --quoting-style=STYLE  设置名称引用风格；有效的 STYLE
                             值请参阅以下说明
  -R, --block-number         每个信息都显示归档内的块数
      --show-defaults        显示 tar 默认选项
      --show-omitted-dirs
                             列表或解压时，列出每个不匹配查找标准的目录
      --show-snapshot-field-ranges
                             显示快照文件区的有效范围
      --show-transformed-names, --show-stored-names
                             显示变换后的文件名或归档名
      --totals[=SIGNAL]      处理归档后打印出总字节数；当此
                             SIGNAL 被触发时带参数 -
                             打印总字节数；允许的信号为:
                             SIGHUP，SIGQUIT，SIGINT，SIGUSR1 和
                             SIGUSR2；同时也接受不带 SIG
                             前缀的信号名称
      --utc                  以 UTC 格式打印文件修改时间
  -v, --verbose              详细地列出处理的文件
      --warning=KEYWORD      警告控制
  -w, --interactive, --confirmation
                             每次操作都要求确认

 兼容性选项:

  -o                         创建归档时，相当于
                             --old-archive；展开归档时，相当于
                             --no-same-owner

 其它选项:

  -?, --help                 显示此帮助列表
      --restrict             禁用某些潜在的有危险的选项
      --usage                显示简短的用法说明
      --version              打印程序版本

长选项和相应短选项具有相同的强制参数或可选参数。

The backup suffix is '~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.
The version control may be set with --backup or VERSION_CONTROL, values are:

  none, off       never make backups
  t, numbered     make numbered backups
  nil, existing   numbered if numbered backups exist, simple otherwise
  never, simple   always make simple backups

--quoting-style 选项的有效参数为:

  literal
  shell
  shell-always
  shell-escape
  shell-escape-always
  c
  c-maybe
  escape
  locale
  clocale

此 tar 默认为:
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/etc/rmt
--rsh-command=/usr/bin/ssh
[wrw@vbox linux_study]$ 
```

#### 2.4.6.2. 案例

```
tar zcvf 压缩后的名称 被压缩的文件或者文件夹
-- z 使用gzip 
-- c 创建新归档
-- v 打印详细的打包信息
-- f 归档后得文件名
```

```
tar zxvf ./需要解压的文件
-- z gzip 
-- x 解压
-- v 打印详细信息
-- f 被压缩的文件
```

```
-- 左边 curl 把下载的二进制数据（压缩包内容）输出到 标准输出（stdout）；
-- | 管道符 把这段 stdout 数据传给右边命令的标准输入（stdin）；
-- 右边 tar -xzv 没有指定 -f 文件名，它就自动从 stdin 读取归档数据；
-- 因为你加了 -z，tar 会把 stdin 当作 gzip 格式解压。
curl http://example.com/file.tar.gz | tar -xzv
```

```
tar -cvf - /etc | tar xvf  - 
```

```
tar -cv -f /dev/st0 /home /root /etc
```

### 2.4.7. zip

```
zip 被压缩的文件
```

### 2.4.8. unzip

```
unzip 被解压的文件
```

## 2.5. 文件权限与属性

### 2.5.1. chown

### 2.5.2. chmod

chmod命令的基本语法如下：

```
# chmod -R 777 file
# 8进制权限表示 r=4 , w=2, x=1
chmod [options] mode file
# [a][u][g][o]：指定要修改权限的用户类型。
# a表示所有用户（all），
# u表示文件所有者（user），g表示文件所属组（group），o表示其他用户（others）。
# 文件所有者添加执行权限
chmod u+x file
```

- **[options]**：可选参数，用于指定修改权限的方式。例如，-R表示递归地修改目录及其子目录中的文件权限。
- **[mode]**：要修改的权限模式，可以用八进制数字模式或符号模式表示。
- **[file]**：要修改权限的文件或目录的名称。

## 2.6. 系统信息

### 2.6.1. `du`（Disk Usage）

#### 2.6.1.1. 功能

`du` 用于统计 **文件或目录的磁盘使用情况**，即查看某个目录或文件占用了多少空间。

#### 2.6.1.2. 常用参数

|参数|含义|
|---|---|
|`-h`|以人类可读形式显示（KB/MB/GB）|
|`-s`|只显示总计，不显示子目录详细信息|
|`--max-depth=N`|控制显示目录层级深度|
|`-a`|显示所有文件和目录的占用情况|

#### 2.6.1.3. 常用示例

1. 查看当前目录及子目录占用空间：

  

```
du
```

2. 以人类可读形式查看：

  

```
du -h
```

3. 查看当前目录总占用空间：

  

```
du -sh
```

4. 查看当前目录下每个子目录的大小（限制一层）：

  

```
du -h --max-depth=1
```

示例输出：

```
$ du -h --max-depth=1
50M     docker
200M    rust_web
10M     logs
```

解释：

- `docker` 目录占 50MB
- `rust_web` 目录占 200MB
- `logs` 目录占 10MB

  

适用场景：分析哪些目录占用磁盘大，定位垃圾文件或临时缓存。

---

### 2.6.2. `df`（Disk Free）

#### 2.6.2.1. 功能

`df` 用于显示 **磁盘分区的剩余空间和使用情况**，关注的是文件系统级别的空间，而不是单个目录。

#### 2.6.2.2. 常用参数

|参数|含义|
|---|---|
|`-h`|以人类可读形式显示（KB/MB/GB）|
|`-T`|显示文件系统类型|
|`-i`|显示 inode 使用情况|

#### 2.6.2.3. 常用示例

1. 查看所有分区使用情况：

  

```
df
```

2. 以人类可读形式显示：

  

```
df -h
```

示例输出：

```
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        19G   17G  2G   90% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
```

解释：

- `/dev/sda2` 总容量 19G，已用 17G，剩余 2G
- `Use%` 表示使用率 90%

  

适用场景：查看整个分区是否快满，判断是否需要扩容或清理空间。

---

### 2.6.3. `du` 与 `df` 区别

|命令|查看范围|使用目的|
|---|---|---|
|`du`|单个目录/文件|分析具体目录或文件占用空间|
|`df`|整个磁盘分区|查看磁盘剩余空间和使用率|

**总结**：

- 开发调试 Docker / 容器时：

- `du -sh /var/lib/docker` → 查看 Docker 占用空间
- `df -h` → 查看整个磁盘分区是否快满

  

---

### 2.6.4. uname

### 2.6.5. free

### 2.6.6. uptime

  

## 2.7. 进程与任务管理

### 2.7.1. top

`top` 是 Linux 下最常用的实时进程监控工具，类似 Windows 的任务管理器。它会动态刷新，显示系统整体和各个进程的资源使用情况。

#### 2.7.1.1. 常用启动方式

```
top             # 默认模式，按 CPU 占用排序
top -d 2        # 每 2 秒刷新一次
top -n 1        # 只输出一次（常用于脚本）
top -u username # 只看某个用户的进程
top -p 1234     # 只看某个 PID
```

#### 2.7.1.2. 界面结构

运行 `top` 后，输出大致如下：

```
top - 15:22:13 up 10 days,  3:21,  2 users,  load average: 0.12, 0.34, 0.56
Tasks: 192 total,   1 running, 191 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.3 us,  1.0 sy,  0.0 ni, 92.9 id,  0.5 wa,  0.0 hi,  0.3 si,  0.0 st
MiB Mem :   7956.4 total,   1024.0 free,   5120.0 used,   1812.4 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   2200.0 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 2134 root      20   0  905432  75632  12320 S   8.3  0.9   0:01.45 java
 1421 mysql     20   0 1623448 341252  14840 S   6.7  4.3  12:31.22 mysqld
```

---

#### 2.7.1.3. 系统信息

- `15:22:13` → 当前时间
- `up 10 days, 3:21` → 系统运行时长（开机多久了）
- `2 users` → 当前登录用户数
- `load average: 0.12, 0.34, 0.56` → 系统 1 分钟、5 分钟、15 分钟平均负载

👉 **经验值**：负载接近 CPU 核心数时表示比较忙。例如 4 核 CPU，load average 长期大于 4，说明系统压力大。

---

#### 2.7.1.4. 任务（进程）状态

- `Tasks: 192 total` → 总进程数
- `1 running` → 正在运行的进程
- `191 sleeping` → 睡眠状态（大部分进程都在等待）
- `0 stopped` → 停止的进程
- `0 zombie` → 僵尸进程数

---

#### 2.7.1.5. CPU 使用情况

- `us` → 用户空间使用率（应用程序）
- `sy` → 内核空间使用率（系统调用）
- `ni` → 调整过优先级的进程占用率
- `id` → 空闲率（越高越空闲）
- `wa` → I/O 等待
- `hi` → 硬件中断
- `si` → 软件中断
- `st` → 被虚拟机偷走的时间

---

#### 2.7.1.6. 内存

- `total` → 总内存
- `free` → 空闲内存
- `used` → 已用内存
- `buff/cache` → 缓存/缓冲区
- `avail Mem` → 可用内存（真正能用的，考虑了缓存回收）

Swap 部分同理。

---

#### 2.7.1.7. 进程列表字段

- **PID** → 进程号
- **USER** → 进程所属用户
- **PR/NI** → 优先级 / nice 值（数值越小优先级越高）
- **VIRT** → 虚拟内存占用
- **RES** → 常驻内存（实际占用的物理内存）
- **SHR** → 共享内存
- **S** → 进程状态（R=运行，S=睡眠，Z=僵尸，T=停止）
- **%CPU** → CPU 使用率
- **%MEM** → 内存使用率
- **TIME+** → 进程占用 CPU 的总时长
- **COMMAND** → 启动进程的命令

---

#### 2.7.1.8. 交互操作（运行时快捷键）

- `q` → 退出
- `h` → 帮助
- `P` → 按 **CPU 使用率** 排序（默认）
- `M` → 按 **内存使用率** 排序
- `T` → 按 **运行时间** 排序
- `k` → 杀进程（输入 PID）
- `r` → 修改进程优先级（renice）
- `u` → 只显示某个用户的进程
- `1` → 显示每个 CPU 核心的使用率

---

#### 2.7.1.9. 高级用法

1. 显示指定用户的进程：

```
top -u root
```

2. 导出快照（非动态，类似 ps）：

```
top -n 1 -b > top.txt
```

3. 只显示某个 PID：

```
top -p 1234
```

---

✅ 总结：

- `top` = **实时动态监控**，更像“任务管理器”。
- `ps` = **静态快照**，只显示当下。
- 一般场景下：`top` 用来看“谁最占资源”，`ps` 用来做“结果统计 / 脚本处理”。

---

明白，你问的 `-b` 参数就是在 `top -n 1 -b > top.txt` 里用的。

#### 2.7.1.10. `top -b` 参数说明

- `-b` = **batch mode（批处理模式）**
- 含义：让 `top` **以非交互方式输出**，每次刷新就是打印一份文本到标准输出，而不是启动动态界面。
- 主要用途：**配合脚本或重定向到文件**，因为默认 `top` 是 ncurses 界面，不能直接重定向。

---

#### 2.7.1.11. 例子

1. 输出一次到文件：

```
top -b -n 1 > top.txt
```

- `-n 1` → 执行 1 次刷新（默认 `top` 会不停刷新）
- 文件 `top.txt` 会包含静态的进程快照

2. 每 5 秒刷新 3 次并保存：

```
top -b -d 5 -n 3 > top.log
```

- `-d 5` → 每 5 秒刷新一次
- `-n 3` → 总共刷新 3 次

---

💡 小结：

- **默认 top** → 交互式动态界面
- **top -b** → 批处理模式，可用于脚本和日志输出
- **-n + -b** → 可以抓取一次或多次快照到文件

---

### 2.7.2. ps

#### 2.7.2.1. 参数

|--no-headers|去除表头打印||
|---|---|---|
||||
||||

ps aux --sort=-%mem

`--sort=-%mem` → 直接按内存占用降序排序

#### 2.7.2.2. `ps -e`

- `-e` 是 **标准 POSIX 选项**，意思是列出 **系统中所有进程**。
- 输出格式比较统一，便于和 `wc -l` 结合。
- 不会带额外的表头信息（或者可以用 `--no-headers` 去掉表头），直接统计行数就是进程数。

```
ps -e
```

---

#### 2.7.2.3. `ps aux`

- `aux` 是 BSD 风格的选项，不是 POSIX 标准。
- 含义：

- `a` → 显示所有终端的进程
- `u` → 以用户为列显示
- `x` → 显示没有终端的进程

- 输出包含更多列（USER, PID, %CPU, %MEM, VSZ, RSS, TTY, STAT, START, TIME, COMMAND），而且表头默认存在。

```
ps aux
```

**问题**：

- 用 `ps aux | wc -l` 统计时，表头也算一行，需要减 1。
- 由于 BSD 风格，可能和 POSIX 系统兼容性不完全一致。

---

#### 2.7.2.4. 总结

- `**ps -e**` → POSIX 标准，直接列出所有进程，方便统计数量。
- `**ps aux**` → BSD 风格，显示信息更详细，统计时需要处理表头。
- 如果只是想**统计进程数量**，用 `ps -e` 更简洁、稳妥。

### 2.7.3. kill

如果你发现某个端口被占用，但不知道是哪个程序在使用，可以结合 `lsof` 或 `fuser` 找到进程，然后用 `kill` 结束它：

```
kill -9 进程ID
```

### 2.7.4. jobs

### 2.7.5. fg/bg

## 2.8. 网络管理

### 2.8.1. ping

### 2.8.2. netstat

`netstat`（网络统计）是一个在多种操作系统中用来显示网络连接、路由表、接口统计信息等的命令行工具。它主要用于诊断和解决网络相关问题。通过`netstat`命令，你可以查看哪些端口正在被监听，哪些端口正在被使用，以及网络连接的详细信息等。

#### 2.8.2.1. 常见的`netstat`选项包括：

- `-a` 或 `--all`：显示所有选项，默认不显示监听端口。
- `-n`：以数字形式显示地址和端口号，而不是尝试解析域名或服务名。
- `-t`：仅显示TCP连接。
- `-u`：仅显示UDP连接。
- `-l`：仅显示监听状态的套接字（sockets），即等待进入连接的套接字。
- `-p`：显示监听端口的进程标识符和进程名称，每个进程标识符旁边都有一个进程名，进程名默认不显示，需要指定用户权限或使用sudo。
- `-r`：显示路由表。
- `-s`：显示每个协议的统计信息。

#### 2.8.2.2. 示例用法：

1. **查看所有监听端口**：

```
bash复制代码



netstat -tuln
```

这个命令会显示所有TCP（`-t`）和UDP（`-u`）的监听（`-l`）端口，并以数字形式（`-n`）显示地址和端口号。

2. **查看特定端口的监听情况**（例如，查看80端口）：

```
bash复制代码



netstat -tuln | grep :80
```

使用`grep`命令来过滤显示结果，只查看包含":80"的行。

3. **查看路由表**：

```
bash复制代码



netstat -r
```

这将显示系统的路由表，包括目标网络、网关等信息。

4. **查看所有连接的进程**（需要管理员权限）：

```
sudo netstat -tulnp
```

使用`-p`选项时，可能需要管理员权限来查看进程的详细信息。

#### 2.8.2.3. 注意：

- 在某些系统上（特别是Linux和macOS），`netstat`可能不是预装的。你可能需要安装`net-tools`包或使用其他工具（如`ss`命令，它是`netstat`的现代替代品）来获取类似的信息。
- `netstat`输出的解释可能因操作系统的不同而略有差异。
- 随着时间的推移，一些系统可能更推荐使用`ss`命令而不是`netstat`，因为`ss`更快且能提供更详细的信息。

### 2.8.3. telnet

### 2.8.4. curl

[链接](https://mp.weixin.qq.com/s/foR0USPhO4n861YSDXRXPw)

  

### 2.8.5. wget

### 2.8.6. ss

`ss` 是 Linux 系统中的一个用于查看**网络连接状态**的命令，它是 `netstat` 的现代替代工具，速度更快、信息更丰富。

#### 2.8.6.1. 常用用途

- 查看所有 TCP/UDP 连接
- 监听哪些端口开放
- 查看某个程序的连接情况
- 过滤某个 IP、端口、协议、状态等

#### 2.8.6.2. 常用语法和示例

##### 2.8.6.2.1. 查看当前所有连接

`ss`默认显示所有套接字（sockets）。

##### 2.8.6.2.2. 查看监听的端口（如服务程序）

`ss -ltn`

解释：

- `-l`：仅列出监听中的端口
- `-t`：仅 TCP
- `-n`：不解析域名（显示数字 IP 和端口）

结果类似：

`State Recv-Q Send-Q Local Address:Port Peer Address:Port LISTEN 0 128 0.0.0.0:22 0.0.0.0:*`

##### 2.8.6.2.3. 查看所有 TCP 连接

`ss -t`

##### 2.8.6.2.4. 查看所有 UDP 连接

`ss -u`

##### 2.8.6.2.5. 查看所有连接及其进程 PID

`ss -tunp`

解释：

- `-p`：显示哪个程序占用了端口（需要 root 权限）
- `-u`：UDP
- `-t`：TCP
- `-n`：不要解析域名

##### 2.8.6.2.6. 查看某个端口是否被监听

`ss -ltn 'sport = :8080'`

或者更简单写法：

`ss -ltn | grep 8080`

##### 2.8.6.2.7. 查看与某个远程地址的连接

`ss -tn dst 192.168.1.100`

#### 2.8.6.3. `ss` 和 `netstat` 的区别

|功能|`ss`|`netstat`|
|---|---|---|
|性能|✅ 更快|较慢|
|支持的套接字类型|✅ 更丰富|较少|
|可用性|大多数新系统默认安装|可能需要安装 `net-tools` 包|

#### 2.8.6.4. 总结表

|功能|命令|
|---|---|
|所有连接|`ss`|
|所有 TCP 连接|`ss -t`|
|所有 UDP 连接|`ss -u`|
|所有监听中的端口（TCP）|`ss -ltn`|
|显示进程 PID/程序名|`ss -tunp`|
|查看 8080 端口是否被监听|`ss -ltn|
|查看连接远程地址 1.2.3.4|`ss -tn dst 1.2.3.4`|

### 2.8.7. fuser

`fuser` 可以查看哪个进程占用了某个端口。

#### **查看占用端口的进程**

```
fuser 端口号/tcp
# 查看 8080 端口的占用情况
fuser 8080/tcp
# 8080/tcp:  1234
# 1234 是占用该端口的进程ID。
```

**强制关闭占用端口的进程**

```
fuser -k 端口号/tcp
# 强制关闭 8080 端口的进程：
fuser -k 8080/tcp
```

### 2.8.8. nmap

`nmap` 是一个强大的网络扫描工具，也可以用于检查本地端口占用情况。

扫描本地所有开放端口

```
nmap -sT -O 127.0.0.1
```

- `-sT`：TCP 扫描
- `-O`：尝试识别操作系统

### 2.8.9. isof

`lsof`（List Open Files）可以查看哪些进程打开了文件、套接字等。

查看所有端口占用

```
lsof -i
```

查看特定端口的占用

```
lsof -i :端口号
```

例如，查看 `22`（SSH）端口：

```
lsof -i :22
```

输出示例：

```
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1234 root    3u  IPv4  12345      0t0  TCP *:22 (LISTEN)
```

- `COMMAND`：进程名
- `PID`：进程ID
- `USER`：运行该进程的用户
- `NAME`：端口状态（LISTEN 表示正在监听）

### 2.8.10. 端口查看命令对比

|命令|用途|示例|
|---|---|---|
|`netstat -tuln`|查看所有监听端口|`netstat -tuln \| grep :80`|
|`ss -tuln`|更快的端口查看方式|`ss -tuln \| grep :3306`|
|`lsof -i :端口号`|查看具体端口的进程|`lsof -i :22`|
|`fuser 端口号/tcp`|查看并关闭占用端口的进程|`fuser -k 8080/tcp`|
|`nmap 127.0.0.1`|扫描本地开放端口|`nmap -sT 127.0.0.1`|

### 2.8.11. firewall

以下是一份详细的 **Linux防火墙（firewalld/iptables）使用教程**，涵盖基本概念、常用命令、配置示例及注意事项。教程分为 **firewalld（推荐）** 和 **iptables（传统）** 两部分，可根据系统选择。

#### 2.8.11.1. firewalld 详细教程

**firewalld** 是 Red Hat 系列（CentOS/RHEL/Fedora）的默认动态防火墙工具，支持“zone”概念和运行时修改规则。

#### 2.8.11.2. 安装与启停

```
# 安装（若未预装）
sudo yum install firewalld     # CentOS/RHEL
sudo apt install firewalld     # Debian/Ubuntu（需手动启用）

# 启动/停止/状态
sudo systemctl start firewalld
sudo systemctl enable firewalld  # 开机自启
sudo systemctl status firewalld
```

#### 2.8.11.3. Zone（区域）管理

firewalld 使用 **zone** 定义不同信任级别的网络环境：

- **默认zone**：`public`（默认值，适用于公共网络）
- 其他常用zone：`trusted`（完全信任）、`home`（家庭）、`dmz`（隔离区）

```
# 查看所有zone
firewall-cmd --get-zones

# 查看默认zone
firewall-cmd --get-default-zone

# 修改默认zone（如改为home）
sudo firewall-cmd --set-default-zone=home --permanent
sudo firewall-cmd --reload
```

#### 2.8.11.4. 开放/关闭端口

```
# 临时开放80/TCP（重启失效）
sudo firewall-cmd --add-port=80/tcp

# 永久开放443/TCP（--permanent参数）
sudo firewall-cmd --add-port=443/tcp --permanent
sudo firewall-cmd --reload  # 重载配置

# 关闭端口
sudo firewall-cmd --remove-port=80/tcp --permanent
sudo firewall-cmd --reload

# 查看已开放端口
firewall-cmd --list-ports
```

#### 2.8.11.5. 允许/拒绝服务

firewalld 内置服务模板（如`http`、`ssh`）：

```
# 允许SSH服务（默认22端口）
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload

# 拒绝FTP服务
sudo firewall-cmd --remove-service=ftp --permanent
sudo firewall-cmd --reload

# 查看所有允许的服务
firewall-cmd --list-services
```

#### 2.8.11.6. IP地址控制

```
# 允许特定IP访问（如192.168.1.100）
sudo firewall-cmd --add-source=192.168.1.100 --permanent
sudo firewall-cmd --reload

# 拒绝IP段（如192.168.2.0/24）
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.2.0/24" reject' --permanent
sudo firewall-cmd --reload
```

#### 2.8.11.7. 端口转发（NAT）

```
# 将80端口转发到内部192.168.1.10:8080
sudo firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=192.168.1.10 --permanent
sudo firewall-cmd --reload

# 查看转发规则
firewall-cmd --list-forward-ports
```

#### 2.8.11.8. 高级：自定义规则（Rich Rules）

```
# 允许来自192.168.1.50的ICMP（ping）
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.1.50" protocol value="icmp" accept' --permanent

# 记录并拒绝某IP的HTTP访问
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="203.0.113.10" service name="http" log prefix="HTTP-BLOCK" level="notice" reject' --permanent
sudo firewall-cmd --reload
```

### 2.8.12. iptables

**iptables** 是传统的Linux防火墙工具，适用于精细控制或老旧系统。

#### 2.8.12.1. 基本命令

```
# 查看规则（-v显示详细信息，-n禁用DNS解析）
sudo iptables -L -n -v

# 清空所有规则（谨慎！）
sudo iptables -F

# 保存规则（依赖系统）
sudo iptables-save > /etc/iptables.rules  # 手动保存
sudo apt install iptables-persistent      # Debian/Ubuntu自动化保存
```

#### 2.8.12.2. 常用规则示例

```
# 允许本地回环
sudo iptables -A INPUT -i lo -j ACCEPT

# 允许已建立的连接
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# 开放SSH（22端口）
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# 允许PING
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT

# 拒绝所有其他入站流量
sudo iptables -A INPUT -j DROP
```

#### 2.8.12.3. NAT与端口转发

```
# 启用IP转发（编辑/etc/sysctl.conf）
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# 将外部80端口转发到192.168.1.10:8080
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.10:8080
sudo iptables -t nat -A POSTROUTING -j MASQUERADE
```

---

#### 2.8.12.4. 注意事项

1. **规则顺序**：iptables规则从上到下匹配，第一条匹配的规则生效。
2. **持久化**：

- `firewalld`规则默认自动保存。
- `iptables`需手动保存（如`iptables-save`）。

3. **日志监控**：

```
# 查看firewalld日志
journalctl -u firewalld -f

# 查看iptables日志（需先配置LOG规则）
sudo tail -f /var/log/syslog | grep kernel
```

### 2.8.13. 总结

- **推荐新手**：用`firewalld`（简单、动态）。
- **高级用户**：用`iptables/nftables`（精细控制）。
- **生产环境**：建议结合`firewalld`的zone管理和`rich rules`。

如需具体场景配置（如Docker、K8s网络），可进一步扩展！

是的，`firewalld` 中的 `service` 实际上就是一组**预定义的端口 + 协议 + 有时还包含一些额外的配置（如模块、协议类型等）**。你可以把它理解为**端口规则的“别名”或集合**，目的是简化配置。

---

### 2.8.14. 查看一个 service 定义

我们用 `firewall-cmd --info-service=<服务名>` 来查看，比如：

#### 2.8.14.1. 示例：`ssh` 服务

```
$ firewall-cmd --info-service=ssh
```

输出示例：

```
ssh
  ports: 22/tcp
  protocols: 
  source-ports: 
  modules: 
  destination: 
  includes: 
  helpers: 
```

说明：

- `ssh` 这个 service 本质上就是允许 **TCP 端口 22**。
- 和你直接添加端口一样，只是用一个友好的名字来管理。

#### 2.8.14.2. 再看一个复杂一点的：`samba`

```
$ firewall-cmd --info-service=samba
```

输出类似：

```
samba
  ports: 137/udp 138/udp 139/tcp 445/tcp
```

你可以看到，它包含多个端口，涵盖 Samba 文件共享所需的全部端口。

#### 2.8.14.3. 所以总结：

|名称|是什么|
|---|---|
|`service`|一组已命名的端口 + 协议集合，用于简化规则设置|
|定义位置|`/usr/lib/firewalld/services/*.xml`|
|使用方式|可以添加到 zone 中，如：`--add-service=ssh`|
|作用|本质上等于允许了一些端口（可以用 `--add-port` 手动做）|

### 2.8.15. 想查看所有定义好的 services？

```
firewall-cmd --get-services
```

输出的全是可以被 `--add-service=xxx` 使用的服务名。

### 2.8.16. 自定义一个 service（进阶）

你可以自己写 XML 文件放到：

```
/etc/firewalld/services/myapp.xml
```

比如你想定义一个叫 `myapp` 的服务：

```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>MyApp</short>
  <description>My custom application port</description>
  <port protocol="tcp" port="12345"/>
</service>
```

然后重载配置：

```
sudo firewall-cmd --reload
```

之后就能用：

```
sudo firewall-cmd --add-service=myapp
```

## 2.9. 用户与权限

### 2.9.1. su

### 2.9.2. id

- `**id username**` 显示UID,GID,以及所属的组

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1759198803073-18e836fd-c6aa-465a-b10f-e3bf7eb9147b.png)

### 2.9.3. whoami

- 打印当前登录用户

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1759198827966-63c46ed4-f7e4-4fec-8d2f-7907d58ef1ad.png)

### 2.9.4. sudo

### 2.9.5. passwd

`passwd`用于设置用户的认证信息，包括用户密码、密码过期时间等。系统管理者则能用它管理系统用户的密码。只有管理者可以指定用户名称，一般用户只能变更自己的密码。

```
-d：删除密码，仅有系统管理者才能使用；
-f：强制执行；
-k：设置只有在密码过期失效后，方能更新；
-l：锁住密码；
-s：列出密码的相关信息，仅有系统管理者才能使用；
-u：解开已上锁的帐号。
```

```
passwd [option] 用户名
# 方法1：
$ passwd xiaoming
Changing password for user xiaoming.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
# 1.在生产工作环境中，密码必须要复杂点
# 2.使用 passwd 指定密码时，界面是看不到的。
# 3.直接使用 passwd 是交互式的操作

# 方法2：
$ echo "Admin@h3c" | passwd --stdin xiaoming
```

root用户忘记密码，使用单用户模式修改密码，以及在单用户模式修改密码后，密码文件可能安全上下文标记变了，需要重新打标签，否则可能重启进入不了，在touch /.autorelabel,exec /sbin/init 执行这个命令是在单用户模式切换到正常模式，正常在启动系统的时候，会执行sbin/init这个文件创建1号进程来对系统进行初始化，我们在进入单用户模式没有进行这一步进入的也不是完整系统，手动执行这个命令，系统会继续完成初始化，最终进入bash

### 2.9.6. userdel

`userdel`

```
-f：强制删除用户，即使用户当前已登录；
-r：删除用户的同时，删除与用户相关的所有文件。
```

  

```
# 该删除不会删除属于用户的home下的文件
$ userdel xiaoming
$ ls -ld /home/xiaoming/
drwx------ 2 1001 1001 62 Oct  7 15:17 /home/xiaoming/

# 该删除会删除用户下的目录
$ userdel -rf xiaohong
$ ls -l /opt/
```

### 2.9.7. groups

- 显示用户所属的组

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1759198902804-ddc24b40-9497-4c9c-a321-768d8f7dea3b.png)

可以通过访问`**/etc/group**`文件查看系统所有组信息

![](1759199002708-ca8dc70b-8a30-4e61-865f-b7c6a1406b41.png)

### 2.9.8. groupdel

- 删除组`**groupdel groupname**`

### 2.9.9. usermod

- 把用户加入附加组`**usermod -aG groupname username**`
- 设置用户的主组`**usermod -g groupname username**`

### 2.9.10. groupadd

- 创建新组`**groupadd groupname**`

  

## 2.10. 时间与计划任务

### 2.10.1. data

在 Linux 里，`date` 命令支持 **格式化输出**，常用来获取或显示时间。格式化规则和 `strftime` 函数类似。

---

#### 2.10.1.1. 基本用法

```
date +"FORMAT"
```

⚠️ 注意：需要加上 `+` 号。

---

#### 2.10.1.2. 常用格式化参数

|格式符|含义|示例输出|
|---|---|---|
|`%Y`|年（4 位数）|`2025`|
|`%y`|年（2 位数）|`25`|
|`%m`|月（01-12）|`09`|
|`%d`|日（01-31）|`26`|
|`%H`|小时（00-23）|`17`|
|`%I`|小时（01-12，12 小时制）|`05`|
|`%M`|分钟（00-59）|`48`|
|`%S`|秒（00-59）|`09`|
|`%N`|纳秒（000000000-999999999）|`123456789`|
|`%a`|星期（缩写）|`Thu`|
|`%A`|星期（全称）|`Thursday`|
|`%u`|星期几（1-7，1=周一）|`4`|
|`%w`|星期几（0-6，0=周日）|`3`|
|`%j`|一年中的第几天（001-366）|`269`|
|`%Z`|时区缩写|`CST`|
|`%z`|时区偏移|`+0800`|
|`%s`|Unix 时间戳（秒）|`1758870489`|

---

#### 2.10.1.3. 示例

1. **输出年月日**

```
date +"%Y-%m-%d"
# 2025-09-26
```

2. **输出完整时间**

```
date +"%Y-%m-%d %H:%M:%S"
# 2025-09-26 17:48:09
```

3. **输出时间戳**

```
date +%s
# 1758870489
```

4. **输出星期**

```
date +"Today is %A, %Y-%m-%d"
# Today is Friday, 2025-09-26
```

5. **输出纳秒级时间**

```
date +"%Y-%m-%d %H:%M:%S.%N"
# 2025-09-26 17:48:09.123456789
```

---

要不要我帮你整理一个 **date 常用格式化速查表（高清对照图）**，你可以直接保存下来，练习时随时看？

好的 👍 我帮你整理一份 **Linux** `date` **格式化输出速查表**，你可以直接保存下来当参考。

---

#### 2.10.1.4. 🕒 Linux `date` 格式化速查表

#### 2.10.1.5. 📌 日期部分

|格式|含义|示例|
|---|---|---|
|`%Y`|4 位年份|`2025`|
|`%y`|2 位年份|`25`|
|`%m`|月（01-12）|`09`|
|`%B`|月全称|`September`|
|`%b`|月缩写|`Sep`|
|`%d`|日（01-31）|`26`|
|`%e`|日（空格填充，1–31）|`26`|
|`%j`|一年中的第几天（001-366）|`269`|

---

#### 2.10.1.6. 时间部分

|格式|含义|示例|
|---|---|---|
|`%H`|小时（00-23）|`17`|
|`%I`|小时（01-12，12h 制）|`05`|
|`%M`|分钟（00-59）|`48`|
|`%S`|秒（00-59）|`09`|
|`%N`|纳秒（000000000-999999999）|`123456789`|
|`%p`|AM/PM|`PM`|

---

#### 2.10.1.7. 星期部分

|格式|含义|示例|
|---|---|---|
|`%A`|星期全称|`Friday`|
|`%a`|星期缩写|`Fri`|
|`%u`|星期几（1-7，1=周一）|`5`|
|`%w`|星期几（0-6，0=周日）|`5`|

---

#### 2.10.1.8. 时区 & 时间戳

|格式|含义|示例|
|---|---|---|
|`%Z`|时区缩写|`CST`|
|`%z`|时区偏移|`+0800`|
|`%s`|Unix 时间戳（秒）|`1758870489`|

---

#### 2.10.1.9. 常用组合

|命令|输出|
|---|---|
|`date +"%Y-%m-%d"`|`2025-09-26`|
|`date +"%Y-%m-%d %H:%M:%S"`|`2025-09-26 17:48:09`|
|`date +"%F %T"`|`2025-09-26 17:48:09`（简写）|
|`date +"%Y-%m-%d %H:%M:%S.%N"`|`2025-09-26 17:48:09.123456789`|
|`date +"Today is %A"`|`Today is Friday`|

---

### 2.10.2. cal

## 2.11. 软件安装与包管理

## 2.12. 其他常用工具

### xargs

```
xargs -0 → 安全处理文件名里有空格或特殊字符
```

### systemctl

很好，你提到的路径 `/usr/lib/systemd/system/nginx.service` 是 **systemd 服务配置文件（unit 文件）** 的一部分，用于定义 **nginx 服务在 systemd 下的启动方式、依赖关系、行为等信息**。

---
**支持的单元类型**

```bash

# 查看systemd支持的所有单元类型
systemctl --type=help
# 输出包括：
# service    - 服务进程
# socket     - 套接字（网络/本地）
# target     - 目标组（类似运行级别）
# device     - 设备
# mount      - 挂载点
# automount  - 自动挂载
# swap       - 交换分区
# timer      - 定时器（类似cron）
# path       - 路径监控
# slice      - 资源切片（cgroup）
# scope      - 临时进程组

```

#### 2.12.2.1. 路径说明

在不同的 Linux 发行版中，系统服务的 unit 文件默认可能放在以下路径之一：

- `/usr/lib/systemd/system/`：**系统安装的软件包自动创建的服务配置文件**
- `/lib/systemd/system/`：某些系统的软链接路径，也可能是主路径（如 Debian 系）
- `/etc/systemd/system/`：**用户自定义服务**，或对系统服务的**覆盖配置**
- `systemctl enable nginx` 会在`/etc/systemd/system/multi-user.target.wants/`  
    目录下创建一个指向  
    `/usr/lib/systemd/system/nginx.service` 的符号链接。
- **所以 下次系统启动时，Nginx 会自动启动。**

  

一般来说：

- **不要直接修改** `/usr/lib/systemd/system/*.service`，因为包升级会覆盖它
- 如果需要修改服务配置，应该创建覆盖文件：`/etc/systemd/system/nginx.service.d/override.conf`

#### 2.12.2.2. 查看 `nginx.service` 文件内容

你可以查看该文件的具体内容：

```
cat /usr/lib/systemd/system/nginx.service
# 或用带语法高亮的命令
systemctl cat nginx.service
```

示例内容可能如下：

```
[Unit]
Description=A high performance web server and a reverse proxy server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

---

#### 2.12.2.3. 每个字段的含义

|字段|作用|
|---|---|
|`Description`|服务描述|
|`After`|表示该服务在某些服务之后启动|
|`Type=forking`|表示主进程会 fork 子进程并退出（如 nginx）|
|`ExecStartPre`|启动前执行（如配置检查）|
|`ExecStart`|正式启动命令|
|`ExecReload`|重载配置用的命令|
|`ExecStop`|停止服务时执行|
|`PIDFile`|指定主进程的 PID 文件位置|
|`WantedBy=multi-user.target`|表示在正常多用户运行级别下自动启动|

---

#### 2.12.2.4. 🛠️ 四、如何修改服务配置（推荐方式）

#### 2.12.2.5. 步骤一：创建覆盖配置目录

```
sudo mkdir -p /etc/systemd/system/nginx.service.d/
```

#### 2.12.2.6. 步骤二：创建 override 文件

```
sudo nano /etc/systemd/system/nginx.service.d/override.conf
```

内容示例（比如加一个环境变量）：

```
[Service]
Environment="ENV_NAME=value"
```

#### 2.12.2.7. 步骤三：重载 systemd 并重启服务

```
sudo systemctl daemon-reexec       # 或 daemon-reload
sudo systemctl restart nginx
```

---

#### 2.12.2.8. 📌 五、优先级加载顺序（从低到高）

1. `/usr/lib/systemd/system/nginx.service`
2. `/etc/systemd/system/nginx.service`
3. `/etc/systemd/system/nginx.service.d/*.conf`

  

优先级高的配置会覆盖前者。例如你可以只覆盖某一行而不修改整个服务文件。

---

如果你愿意，我可以带你写一个完整的 nginx.service 覆盖配置，或帮你修改它的启动命令、环境变量、日志输出路径等功能。

**查看所有服务状态**

```bash

# 查看所有服务的状态（包括运行、停止、失败等）
systemctl list-units --type=service

# 只查看正在运行的服务
systemctl list-units --type=service --state=running

# 简洁格式显示运行中服务
systemctl --type=service --state=running

# 只显示失败的服务
systemctl --failed

# 查看所有已加载的服务单元（包括未运行）
systemctl list-unit-files --type=service

```
  
**查看特定的服务状态**

  ```bash

# 正在运行的服务
systemctl list-units --type=service --state=active

# 已停止的服务
systemctl list-units --type=service --state=inactive

# 失败的服务
systemctl list-units --type=service --state=failed

# 正在启动/停止中的服务
systemctl list-units --type=service --state=activating
systemctl list-units --type=service --state=deactivating
  
  ```

**格式化输出**

```bash

# 默认表格格式
systemctl list-units --type=service

# 更紧凑的表格
systemctl list-units --type=service --no-pager

# 显示完整信息（包括描述）
systemctl list-units --type=service --full

# 只显示服务名和状态
systemctl list-units --type=service --plain

# 以JSON格式输出（便于脚本处理）
systemctl list-units --type=service --output=json

# 只输出服务名
systemctl list-units --type=service --no-legend | awk '{print $1}'

# 使用自定义字段
systemctl list-units --type=service --output=json-pretty

```


**属性筛选**

```bash

# 查看Docker相关服务
systemctl list-units --type=service --state=running | grep -i docker

# 查看网络相关服务
systemctl list-units --type=service --state=running | grep -E '(network|net)'

# 查看Nginx/Apache服务
systemctl list-units --type=service --state=running | grep -E '(nginx|apache|httpd)'

```

**服务的详细信息**

```bash

# 查看服务的完整状态
systemctl status servicename.service

# 查看服务的依赖关系
systemctl list-dependencies servicename.service

# 查看服务的属性
systemctl show servicename.service

# 查看服务的日志
journalctl -u servicename.service

```

**服务运行时间**

```bash

# 查看服务的运行时长
systemctl status servicename.service | grep -A 1 "Active:"

# 使用systemctl show查看详细时间
systemctl show servicename.service -p ActiveEnterTimestamp
systemctl show servicename.service -p ActiveExitTimestamp

# 所有服务的运行时长统计
systemctl list-units --type=service --state=running \
  --output=json | jq '.[] | {unit: .unit, active_since: .active_enter_timestamp}'

```

### journalctl（日志管理）

**是什么**

`journalctl` 是 Linux 系统上的日志管理工具，用于查询和显示 `systemd` 的日志信息（journal）。

**基本使用**

```bash

# 查看完整系统日志
journalctl

# 跟踪实时日志（类似 tail -f）
journalctl -f

# 查看指定服务日志
journalctl -u nginx.service

```

**筛选**

```bash

# 指定开始时间
journalctl --since "2024-05-01 09:00:00"

# 结束时间
journalctl --until "1 hour ago"

# 昨天全天日志
journalctl -S yesterday -U today

# 最近2小时最近2小时
journalctl --since -2h

# 按优先级筛选
sudo journalctl -p err      # 只看错误
sudo journalctl -p warning  # 警告及以上
sudo journalctl -p info     # 信息及以上

# 按服务单元过滤
journalctl -u apache2.service

# 按进程PID过滤
journalctl _PID=1234

# 按可执行文件过滤
journalctl /usr/sbin/sshd

```

# 第5章  Linux用户及权限管理

Linux是一个多用户的操作系统，引入用户，可以更加方便管理Linux服务器，系统默认需要以一个用户的身份登入，而且在系统上启动进程也需要以一个用户身份去运行，用户可以限制某些进程对特定资源的权限控制。

## Linux用户及组

Linux操作系统对多用户的管理，是非常繁琐的，所以用组的概念来管理用户就变得简单，每个用户可以在一个独立的组，每个组也可以有零个用户或者多个用户。

Linux系统用户是根据用户ID来识别的，从默认ID编号从0开始，但是为了和老式系统兼容，用户ID限制在60000以下，Linux用户分总共分为四种，分别如下：
1. root用户  （ID 0）
2. 预分配用户 （ID 1-200）
3. 系统用户  （ID 201-999）
4. 普通用户  （ID 1000以上）

Linux系统中的每个文件或者文件夹，都有一个所属用户及所属组，使用id命令可以显示当前用户的信息，使用passwd命令可以修改当前用户密码。Linux操作系统用户的特点如下：

1. 每个用户拥有一个UserID，操作系统实际读取的是UID，而非用户名；
2. 每个用户属于一个主组，属于一个或多个附属组，一个用户最多有31个附属组；
3. 每个组拥有一个GroupID；
4. 每个进程以一个用户身份运行，该用户可对进程拥有资源控制权限；
5. 每个可登陆用户拥有一个指定的Shell环境。

## Linux用户管理

Linux用户在操作系统可以进行日常管理和维护，涉及到的相关配置文件如下：

/etc/passwd     保存用户信息

/etc/shdaow     保存用户密码（以加密形式保存）

/etc/group      保存组信息

/etc/login.defs   用户属性限制,密码过期时间,密码最大长度等限制

/etc/default/useradd 显示或更改默认的useradd配置文件

如需创建新用户，可以使用命令useradd，执行命令useradd  jfedu1即可创建jfedu1用户，同时会创建一个同名的组jfedu1，默认该用户属于jfedu1主组。

Useradd jfedu1命令默认创建用户jfedu1，会根据如下步骤进行操作：

读取/etc/default/useradd，根据配置文件执行创建操作；

在/etc/passwd文件中添加用户信息；

如使用passwd命令创建密码，密码会被加密保存在/etc/shdaow中；

为jfedu1创建家目录：/home/jfedu1；

将/etc/skel中的.bash开头的文件复制至/home/jfedu1家目录；

创建与用户名相同的jfedu1组，jfedu1用户默认属于jfeud1同名组；

Jfedu1组信息保存在/etc/group配置文件中。

## Linux组管理

所有的Linux或者Windows系统都有组的概念，通过组可以更加方便的管理用户，组的概念应用于各行行业，例如企业会使用部门、职能或地理区域的分类方式来管理成员，映射在Linux系统，同样可以创建用户，并用组的概念对其管理。

Linux组有如下特点：
1. 每个组有一个组ID；
2. 组信息保存在/etc/group中；
3. 每个用户至少拥有一个主组，同时还可以拥有31个附属组。

```
id       # 显示 UID, GID, 所属组
groups   # 显示当前用户属于哪些组
cat /etc/passwd   # 查看所有用户
cat /etc/group    # 查看所有组
```


## useradd

`useradd`用于Linux中创建的新的系统用户。useradd可用来建立用户帐号。帐号建好之后，再用passwd设定帐号的密码．而可用userdel删除帐号。使用useradd指令所建立的帐号，实际上是保存在/etc/passwd文本文件中。

```
useradd [option] 用户名
```

```
-c<备注>		：加上备注文字。备注文字会保存在passwd的备注栏位中；
-d<登入目录>	：指定用户登入时的启始目录；
-D					：变更预设值；
-e<有效期限>	：指定帐号的有效期限；
-f<缓冲天数>	：指定在密码过期后多少天即关闭该帐号；
-g<群组>		：指定用户所属的群组；
-G<群组>		：指定用户所属的附加群组；
-m					：自动建立用户的登入目录；
-M					：不要自动建立用户的登入目录；
-n					：取消建立以用户名称为名的群组；
-r					：建立系统帐号；
-s<shell>		：指定用户登入后所使用的shell；
-u<uid>			：指定用户id。
```

## passwd

`**passwd**`用于设置用户的认证信息，包括用户密码、密码过期时间等。系统管理者则能用它管理系统用户的密码。只有管理者可以指定用户名称，一般用户只能变更自己的密码。

```
-d：删除密码，仅有系统管理者才能使用；
-f：强制执行；
-k：设置只有在密码过期失效后，方能更新；
-l：锁住密码；
-s：列出密码的相关信息，仅有系统管理者才能使用；
-u：解开已上锁的帐号。
```

```
passwd [option] 用户名
# 方法1：
$ passwd xiaoming
Changing password for user xiaoming.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
# 1.在生产工作环境中，密码必须要复杂点
# 2.使用 passwd 指定密码时，界面是看不到的。
# 3.直接使用 passwd 是交互式的操作

# 方法2：
$ echo "Admin@h3c" | passwd --stdin xiaoming
```

## usermod

```shell
# 将jfedu用户属组修改为jfedu1，jfedu2附属组；
usermod -G jfedu1,jfedu2 jfedu
# 将jfedu用户加入到jfedu3，jfedu4附属组，-a为添加新组，原组保留
usermod  –a  -G  jfedu3,jfedu4  jfedu
# 修改jfedu用户，并指定新的家目录，同时指定其登陆的SHELL；
usermod -d  /tmp/  -s  /bin/sh jfedu
# 将jfedu用户名修改为jfedu1
usermod -l jfedu1 jfedu
# 锁定jfedu1用户及解锁jfedu1用户方法
usermod –L  jfedu1
usermod   -U  jfedu1

```

## userdel


```
-f：强制删除用户，即使用户当前已登录；
-r：删除用户的同时，删除与用户相关的所有文件。
```

  

```
# 该删除不会删除属于用户的home下的文件
$ userdel xiaoming
$ ls -ld /home/xiaoming/
drwx------ 2 1001 1001 62 Oct  7 15:17 /home/xiaoming/

# 该删除会删除用户下的目录
$ userdel -rf xiaohong
$ ls -l /opt/
```

## groupadd 

```
groupadd用法

-f, --force                       如果组已经存在则成功退出；

                                   并且如果 GID 已经存在则取消 –g；

-g, --gid GID                  为新组使用 GID；

-h, --help                    显示此帮助信息并推出；

-K, --key KEY=VALUE         不使用 /etc/login.defs 中的默认值；

-o, --non-unique                  允许创建有重复 GID 的组；

-p, --password PASSWORD  为新组使用此加密过的密码；

-r, --system                  创建一个系统账户；

groupmod用法        

-g, --gid GID                 将组 ID 改为 GID；

-h, --help                    显示此帮助信息并推出；

-n, --new-name NEW_GROUP    改名为 NEW_GROUP；

-o, --non-unique                  允许使用重复的 GID；

-p, --password PASSWORD  将密码更改为(加密过的) PASSWORD；

groupdel用法

groupdel jfedu                 删除jfedu组；
```
## groupdel
## groupmod

---

## Linux权限管理

Linux 用三类权限控制文件访问：

- **r (read)**：读
- **w (write)**：写
- **x (execute)**：执行（对文件是“能不能运行”，对目录是“能不能进入”）

权限分配给三类对象：

1. **User (文件属主)**
2. **Group (文件属组)**
3. **Others (其他用户)**

---

#### 3.1.5.1. 文件夹权限

只有r权限，可以看到文件夹中的文件，但是读取不到相关的信息，有x使用ll命令，提示没有权限，使用rx可以查看目录中文件以及信息，

所以理解了，读只能读出列表，读不出内容，需要读内容需要x

写读不到列表，但可以删目录，有x权限，在知道文件路径也可以操作

|操作|所需权限|
|---|---|
|读文件内容|文件 r + 所在目录 x|
|写文件内容|文件 w + 所在目录 x|
|执行文件|文件 x+目录x|
|删除/创建文件|目录 w + 目录 x（文件权限无关）|
|列出目录内容|目录 r|
|进入目录|目录 x|

|条件|能否执行文件|
|---|---|
|目录有 x + 文件有 x|✅ 可以执行|
|目录无 x + 文件有 x|❌ 不行，无法访问文件|
|目录有 x + 文件无 x|❌ 不行，文件不可执行|
|目录无 x + 文件无 x|❌ 完全不行|

#### 3.1.5.2. 示例：`ls -l`

```
-rwxr-xr--  1 alice devs  1234 Sep 29  hello.sh
```

拆解：

- `-rwxr-xr--`

- `-`：普通文件（d 表示目录）
- `rwx` → 属主 alice 权限：读、写、执行
- `r-x` → 属组 devs 权限：读、执行
- `r--` → 其他用户权限：只读

- `alice`：属主
- `devs`：属组

---

### 3.1.6. 文件系统级别的扩展属性

啊，你说的 `chattr` 是 **Linux 文件属性命令**，和普通的 `chmod` 不同，它操作的是 **文件系统级别的扩展属性**，有些权限 `chmod` 改不了的也能用 `chattr` 控制。

---

#### 3.1.6.1. 基本作用

- `chattr` → **Change Attribute（改变文件属性）**
- 可以对文件/目录设置一些特殊属性，比如：

- **i** → immutable（不可变）

- 文件无法修改、删除、重命名
- 对 root 也生效

- **a** → append-only（只追加）

- 文件只能追加内容，不能修改或删除已有内容

- **d** → no dump（备份时忽略）
- **A** → atime 不更新（访问时间不更新）

---

#### 3.1.6.2. 常用命令示例

```
# 查看文件属性
lsattr filename

# 设置不可变
sudo chattr +i filename

# 移除不可变
sudo chattr -i filename

# 设置只追加
sudo chattr +a filename
```

#### 3.1.6.3. 示例效果

```
echo "hello" > test.txt   # 正常写入
sudo chattr +i test.txt
echo "world" >> test.txt  # ❌ 报错，文件不可修改
rm test.txt               # ❌ 报错，文件不可删除
```

---

#### 3.1.6.4. 与权限的关系

- `**普通权限（r/w/x）**`控制用户访问
- **chattr 属性**是**更底层的保护**，能阻止即使是 root 或目录有 w 的删除/修改
- 典型应用：

- 系统关键配置文件 `/etc/passwd`、`/etc/shadow` 可以设置 `+i` 防止误删或恶意修改
- 日志文件设置 `+a` 防止被篡改

---

总结一句话：

`chattr` 是 Linux 的“文件系统保护开关”，比 chmod 更底层，可以实现文件不可改、不可删、只追加等特性。

---

### 3.1.7. 修改权限

#### 3.1.7.1. `chmod` 改权限

有两种写法：

(1) 符号法

```
chmod u+x file   # 给属主增加执行权限
chmod g-w file   # 去掉属组写权限
chmod o=r file   # 设置其他人只读
chmod a+r file   # 给所有人加读权限
chmod  –R  u+rwx,g+rwx,o+rwx  jfedu.net

```

(2) 数字法

权限用 **3 位八进制数**表示：

- r=4, w=2, x=1，组合相加
- 例子：

- `chmod 755 file`

- 属主 7 = rwx
- 属组 5 = r-x
- 其他 5 = r-x

- `chmod 644 file`

- 属主 6 = rw-
- 属组 4 = r--
- 其他 4 = r--
```
chmod  –R  777  jfedu.net
chmod  –R  555  jfedu.net
```
---

#### 3.1.7.2. `chown` 改所有者

```
chown bob file        # 把属主改为 bob
chown bob:devs file   # 把属主改为 bob，属组改为 devs
chown :devs file      # 只改属组


```

递归修改目录：

```
chown -R bob:devs dir/
```

---

#### 3.1.7.3. `chgrp` 改属组

```
chgrp devs file
```

---

## Linux特殊权限及掩码

- **suid (Set User ID)**：可执行文件执行时，使用文件属主的权限，前提这个用户或者组有执行权限
- **sgid (Set Group ID)**：目录内新建文件自动继承目录的组（ 程序运行时临时切换为文件属组 ）
- **sticky bit (t)**：目录内文件只能被属主或 root 删除（用于目录）

| 数字       | 权限位                 | 名称               | 文件上的作用                                                                      | 目录上的作用                                        |
| -------- | ------------------- | ---------------- | --------------------------------------------------------------------------- | --------------------------------------------- |
| **4xxx** | SUID (Set User ID)  | 以文件属主身份运行        | 程序运行时临时切换为文件属主（典型例子 `/usr/bin/passwd`，普通用户运行时实际以 `root` 身份修改 `/etc/shadow`） | 对目录基本无意义                                      |
| **2xxx** | SGID (Set Group ID) | 以文件属组身份运行 / 继承属组 | 程序运行时临时切换为文件属组                                                              | 在该目录中新建文件时，**继承目录的属组**（而不是当前用户的默认组），常用于团队协作目录 |
| **1xxx** | Sticky Bit          | 常驻内存（已废弃）        | 现代 Linux 基本无效                                                               | 目录下的文件只能由 **属主或 root** 删除（典型 `/tmp`）          |

示例：

```
chmod u+s file   # 设置 suid
chmod g+s dir    # 设置 sgid
chmod +t dir     # 设置 sticky bit（常见于 /tmp）
```

`ls -l` 中显示：

- suid → `-rwsr-xr-x`
- sgid → `drwxr-sr-x`
- sticky → `drwxrwxrwt`

---

### 实际应用场景

- 给脚本加执行权限：

```
chmod +x script.sh
```

- 共享目录（开发小组）：

```
chown -R alice:devs /project
chmod -R 770 /project   # 只有 alice 和 devs 组能访问
```

- Docker/Jenkins 数据卷权限问题：

```
chown -R 1000:1000 ./jenkins-data
```

---

### 学习资料推荐

📖 官方文档 & 教程：

- [GNU Coreutils: chmod](https://www.gnu.org/software/coreutils/manual/html_node/chmod-invocation.html)
- [Linux 权限详解 - 菜鸟教程](https://www.runoob.com/linux/linux-file-attr-permission.html)
- [Linux 文件权限与属性 - 阮一峰](https://www.ruanyifeng.com/blog/2010/02/linux_file_permissions.html)

---

# 4. 进程与任务管理


## **Linux 进程的五种基本状态**

| 状态         | 代号                       | 说明              | 含义                               |
| ---------- | ------------------------ | --------------- | -------------------------------- |
| **运行**     | **R** (Running/Runnable) | 正在运行 或 在运行队列中等待 | 进程要么正在 CPU 上执行，要么随时可以执行          |
| **可中断睡眠**  | **S** (Sleeping)         | 等待事件/资源完成       | 最常见的状态，等待用户输入、等待磁盘IO等，可以接收信号     |
| **不可中断睡眠** | **D** (Disk Sleep)       | 等待IO操作完成（通常是磁盘） | 等待硬件响应的状态，**不能接收信号**，通常出现在磁盘高负载时 |
| **停止**     | **T** (Stopped)          | 进程被暂停           | 收到 SIGSTOP 或 SIGTSTP 信号，或被调试器暂停  |
| **僵尸**     | **Z** (Zombie)           | 进程已终止，但未释放资源    | 子进程结束，但父进程没有回收它的进程描述符            |

## **主状态字符（第一个字符）**

| 字符    | 状态                    | 含义            | 示例场景                |
| ----- | --------------------- | ------------- | ------------------- |
| **R** | Running/Runnable      | 正在运行 或 在运行队列中 | `top` 命令本身，CPU密集型任务 |
| **S** | Sleeping              | 可中断睡眠（等待事件）   | 大多数服务进程（httpd、sshd） |
| **D** | Uninterruptible Sleep | 不可中断睡眠（等待IO）  | 磁盘读写、NFS等待          |
| **T** | Stopped               | 暂停/停止         | `Ctrl+Z` 挂起的进程      |
| **t** | Traced                | 被调试器跟踪        | `gdb` 调试中的进程        |
| **Z** | Zombie                | 僵尸进程          | 子进程结束，父进程未回收        |
| **X** | Dead                  | 死亡（几乎看不到）     | 瞬间状态                |

| 字符    | 含义             | 说明                    |
| ----- | -------------- | --------------------- |
| **s** | Session Leader | 进程是会话领导（通常是该终端的第一个进程） |
| **l** | Multi-threaded | 多线程进程（使用CLONE_THREAD） |
| **+** | Foreground     | 在前台进程组中（占用终端）         |
| **<** | High Priority  | 高优先级（可以占用更多CPU时间）     |
| **N** | Low Priority   | 低优先级（nice值>0）         |
| **L** | Locked Pages   | 有页面锁定在内存中（如实时进程）      |
| **W** | Swapped Out    | 被换出到交换分区（内核2.6+后很少见）  |

#### 内存的淘汰策略
fifo（先进先出）
lru(最近最少使用)
适用于访问比较均匀
lfu(最近最少被使用)
适用于访问模式稳定的，热点数据

#### 进程优先级
进程优先级决定了 **CPU 分配时间的先后顺序和多少**。高优先级的进程获得更多 CPU 时间，低优先级的进程获得较少 CPU 时间。

在 Linux 中，优先级通过 **Nice 值** 和 **实时优先级** 两个维度来控制。
实时进程优先级0-99（系统控制）
普通进程：优先级（100-139）由nice值映射
nice( **-20 到 19**) -20优先级最高，0是默认优先级


#### 查看 PID、NICE值、PRI优先级
ps -eo pid,ni,pri,cmd | head -10

# 以低优先级运行（nice=10）
nice -n 10 ./my_script.sh
# 以高优先级运行（需要root）
nice -n -5 ./important_task.sh

# 调整已运行进程的nice值（增加5）
renice -n 5 -p 1234
# 设置为高优先级（需要root）
renice -n -10 -p 1234
# 根据进程名调整
renice -n 5 -u username

### 进程管理工具

查看，找到进程的pid
管理，杀死进程

#### pstree 

进程状态信息/proc/123/

cpu与进程的绑定
 taskset

# 5. 系统管理

## 5.1. 网络管理

正常使用计算机ip地址子网掩码等信息是从网关获取，当我们重启设备的时候，ip地址可能会发生变化，远程ssh连接可能就会失效，为了应对这种情况，我们可以手动设置ip地址，设置为静态ip，当然这个ip应该在网关ip，可使用设备范围，ip+子网掩码，可以将ip分为网络部分和主机部分，静态ip的设置只能是这个主机范围，这样设备才能正常通过网关范围互联网

### 5.1.1. 静态ip的设置

[[配置静态ip]]

## 5.2. 本地化与时区

### 5.2.1. locale （本地化）

- **什么是 locale**

- 本地化环境，影响语言、时间、数字、小数点等格式

- **常见变量**

- `LANG`：默认语言环境
- `LC_ALL`：最高优先级，覆盖所有 `LC_*`
- `LC_TIME`：日期/时间格式
- `LC_NUMERIC`：数字小数点
- `LC_COLLATE`：排序规则

- **查看命令**

```
locale          # 查看当前所有 locale
localectl list-locales  # 列出可用 locale
```

- `LANG=zh_CN.UTF-8` → 默认中文
- `LANG=en_US.UTF-8` → 默认英文

- **示例**

```
date            # 在中文 locale 下会显示中文
LC_TIME=C date  # 临时使用 C locale（英文）
LC_ALL=C ls     # 临时使用英文环境，避免中文排序问题
```

### 5.2.2. `LC_TIME=C` 的作用

- `C` 是最基础的 **POSIX 默认环境**，相当于英文、纯 ASCII 模式。
- `LC_TIME=C` 就是告诉系统：**日期和时间一律按英文格式输出**，不受中文 locale 影响。

```
$ date '+%b %e'
9月 25   # 中文 locale 下输出

$ LC_TIME=C date '+%b %e'
Sep 25   # 强制英文环境
```

### 5.2.3. 时区

决定系统时间相对于 UTC 的偏移量（东八区、日本、美国等）。

### 5.2.4. Linux 时区管理

1. **查看当前时区**

```
timedatectl
```

输出会显示：

```
Local time: Fri 2025-09-26 17:20:15 JST
Universal time: Fri 2025-09-26 08:20:15 UTC
Time zone: Asia/Tokyo (JST, +0900)
```
    
1. **常见时区文件位置**

- `/usr/share/zoneinfo/` ：存放所有时区的数据库，比如 `Asia/Shanghai`，`America/New_York`。
- `/etc/localtime` ：实际生效的时区文件（通常是从 `zoneinfo` 里拷贝或软链接过来的）。

1. **修改时区**  
    方式一：使用 `timedatectl`

```
sudo timedatectl set-timezone Asia/Shanghai
```

方式二：手动替换 `/etc/localtime`

```
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

1. **时区与系统时间**

- 系统通常使用 **UTC** 存储时间（BIOS/RTC 也常建议设置为 UTC），
- Linux 根据时区偏移量将 UTC 转换为本地时间。
- 时区不影响时间戳（时间戳永远是从 1970-01-01 00:00:00 UTC 开始的秒数）。

  

  

# 6. 软件包管理

## 6.1. 软件包概念

包管理工具（Package Manager）是一组用于：

**安装、更新、卸载、查询和管理系统软件包（package）** 的命令与后台服务。

它是 Linux 的 “应用商店”：

- 负责从 **仓库（repository）** 下载软件；
- 自动解析 **依赖关系**；
- 安装、配置并维护软件版本。

在 Linux 中，一个软件不是以 `.exe` 或 `.msi` 分发，而是以 **软件包（package）** 形式。

## 6.2. 软件包内容

- 程序的二进制文件（如 `/usr/bin/nginx`）
- 配置文件（如 `/etc/nginx/nginx.conf`）
- 服务文件（如 `/usr/lib/systemd/system/nginx.service`）
- 元数据（软件名、版本、依赖等）

## 6.3. 软件包格式

|系统系列|包格式|典型后缀|
|---|---|---|
|**Debian / Ubuntu / Kali**|**DEB 包**|`.deb`|
|**CentOS / RHEL / Fedora / Rocky**|**RPM 包**|`.rpm`|

📦 例子：

- `nginx_1.22.0_amd64.deb`
- `nginx-1.22.0-1.el9.x86_64.rpm`

## 6.4. 仓库（Repository）

仓库就是软件存放的“服务器”或“镜像源”。

📌 它是一个包含大量 `.rpm` 或 `.deb` 包的目录，包管理器会：

1. 从仓库下载软件；
2. 根据索引（metadata）解析依赖；
3. 自动安装相关包。
4. Linux 仓库中的包都有 **GPG 签名**（GNU Privacy Guard）：

- 确保软件来源可靠；
- 防止篡改。

每个仓库都提供一个公钥文件（`gpgkey`）。  
安装时包管理器会验证签名。

---

### 6.4.1. 仓库源配置文件位置

|系统|配置文件目录|说明|
|---|---|---|
|**CentOS / RHEL / Rocky**|`/etc/yum.repos.d/*.repo`|YUM / DNF 的仓库定义文件|
|**Debian / Ubuntu / Kali**|`/etc/apt/sources.list`<br><br>和 `/etc/apt/sources.list.d/*.list`|APT 的仓库定义文件|

---

### 6.4.2. 仓库文件示例

#### 6.4.2.1. CentOS（YUM/DNF）

`/etc/yum.repos.d/base.repo`

```
[baseos]
name=BaseOS
baseurl=http://mirror.centos.org/centos/$releasever/BaseOS/$basearch/os/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-9
```

#### 6.4.2.2. Debian（APT）

`/etc/apt/sources.list`

```
deb http://deb.debian.org/debian bookworm main contrib non-free
deb http://security.debian.org/debian-security bookworm-security main
```

---

#### 6.4.2.3. 仓库的关键参数

|参数|含义|
|---|---|
|`name`|仓库名称|
|`baseurl`<br><br>/ `deb`|软件包所在的网络地址|
|`enabled`|是否启用此仓库（1 启用 / 0 禁用）|
|`gpgcheck`|是否检查签名|
|`gpgkey`|签名密钥路径|
|`mirrorlist`|镜像站列表（用于选择最近的源）|

#### 6.4.2.4. 缓存和索引

|系统|缓存命令|缓存路径|
|---|---|---|
|YUM / DNF|`yum makecache`<br><br>/ `dnf makecache`|`/var/cache/dnf`|
|APT|`apt update`|`/var/lib/apt/lists/`|

缓存索引保存了仓库中所有包的元信息（包名、版本、依赖等）。

### 本地仓库搭建

仓库信息同步
```
dnf install httpd
mkdir /etc/httd/bak
mv /etc/httpd/conf.d/* /etc/httpd/bak
sudo firewall-cmd add-port=80/tcp --permanent
getenforce 
setenforce 0 临时关闭selinux
cat /etc/selinux/config
# 正确的写法
sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
yum reposync --repoid-nju-extras -- download-metadata -p /var/www/html

mkdir /cdrom 
mount /dev/cdrom /cdrom 
cp -r /cdrom/baseos /var/www/html


```

## 6.5. 常见的包管理工具

centos系列

### 6.5.1. RPM：底层工具

最原始的包安装命令：

```
rpm -ivh nginx.rpm       # 安装
rpm -e nginx             # 卸载
rpm -qa | grep nginx     # 查询
```

⚠️ 缺点：**不处理依赖关系**。

Linux 软件往往依赖其他包，比如：

```
nginx → openssl, pcre, zlib
```

手动安装 `.rpm` 或 `.deb` 时可能失败，因为依赖没装。YUM / DNF / APT 会自动解决这些依赖。

---

### 6.5.2. YUM（Yellowdog Updater, Modified）

基于 RPM 的 **高级包管理器**，自动处理依赖。  
仓库配置文件在 `/etc/yum.repos.d/`。

常用命令：

```
yum install nginx        # 安装
yum remove nginx         # 卸载
yum list installed        # 查看所有已安装
yum info nginx           # 查看软件信息
yum makecache            # 生成本地缓存
yum update               # 更新系统
```

---

### 6.5.3. DNF（Dandified YUM）

YUM 的下一代版本，性能更高。  
CentOS 8 / Rocky / Fedora 默认使用 DNF。

命令兼容：

```
dnf install nginx
dnf remove nginx
dnf list installed
dnf makecache
```

debain系列

### 6.5.4. dpkg：底层工具

```
sudo dpkg -i nginx.deb       # 安装
sudo dpkg -r nginx           # 卸载
dpkg -l | grep nginx         # 查询
```

⚠️ 缺点：同样不处理依赖。

---

### 6.5.5. APT（Advanced Package Tool）

基于 dpkg 的高层管理工具。  
仓库在 `/etc/apt/sources.list`。

常用命令：

```
sudo apt update             # 更新软件源列表
sudo apt install nginx      # 安装
sudo apt remove nginx       # 卸载
sudo apt list --installed   # 查看已安装
sudo apt upgrade            # 升级所有包
```

---

### 6.5.6. apt-cache / apt-get（旧命令）

在新系统中已逐步合并进 `apt`。

例如：

```
sudo apt-cache search nginx
sudo apt-get install nginx
```

## 6.6. 在线软件仓库与本地包安装

## 6.7. 手动编译安装

  

  

# 7. 脚本与自动化

## 7.1. 变量与环境变量

好的，我们来系统梳理一下 **Shell 中的变量与环境变量**，让你对这部分内容有清晰认识，为后面讲展开机制和命令使用打基础。

---

### 7.1.1. 一、Shell 变量（普通变量）

#### 7.1.1.1. 1️⃣ 定义与赋值

```
VAR="hello world"
NUM=123
```

- 变量名 **区分大小写**
- **不要在等号两边加空格**（`VAR = 1` 会报错）

#### 7.1.1.2. 2️⃣ 使用变量

```
echo $VAR      # 输出 hello world
echo ${NUM}    # 输出 123
```

- `$VAR` → 获取变量值
- `${VAR}` → 推荐在复杂字符串中使用，防止歧义

#### 7.1.1.3. 示例

```
FILE="log"
echo "${FILE}_2025.txt"   # 输出 log_2025.txt
```

#### 7.1.1.4. 3️⃣ 只在当前 Shell 有效

- 普通变量只在 **当前 Shell 会话** 中存在
- 子 Shell（例如用 `bash script.sh` 启动的脚本）不能直接访问

---

### 7.1.2. 二、环境变量（Exported Variables）

- 环境变量 **就是一种被** `**export**` **标记的 Shell 变量**
- 它本身并没有神奇的机制，只是在 **Shell 启动子进程时，会被传递给子进程**
- 也就是说，普通变量默认 **只存在当前 Shell**，而环境变量 **可以被子进程继承**

#### 7.1.2.1. 1️⃣ 定义与导出

- `PATH` 也是普通变量 + export 的组合
- 它被系统程序（子进程）读取，用于查找可执行文件

```
export PATH="/usr/local/bin:$PATH"
export USER_NAME="Alice"
```

- `export` 会把变量加入 **环境变量**，子进程可以继承
- 例如，启动一个脚本或程序，都会看到这些环境变量

#### 7.1.2.2. 2️⃣ 查看环境变量

```
echo $PATH
printenv
env
```

- `env` 或 `printenv` 会列出当前所有环境变量

#### 7.1.2.3. 3️⃣ 子进程继承

- 当你执行一个程序或脚本时，Shell 会把环境变量的键值对 **传递给子进程**
- 子进程收到的是一份 **拷贝**，修改子进程内的变量不会影响父进程

```
export MYVAR="hello"
bash -c 'echo $MYVAR'   # 输出 hello
```

- 因为 `MYVAR` 是环境变量，子 Shell 可以继承
- 如果只是普通变量：

```
MYVAR="hello"
bash -c 'echo $MYVAR'   # 输出空
```

---

### 7.1.3. 三、变量和环境变量的区别

|特性|普通变量|环境变量|
|---|---|---|
|生命周期|当前 Shell|当前 Shell + 子 Shell|
|可见性|当前 Shell|子进程也可见|
|定义方式|`VAR=value`|`export VAR=value`|
|继承|不继承|继承|

---

### 7.1.4. 四、常用示例

```
# 定义普通变量
NAME="Bob"

# 导出环境变量
export NAME

# 子 Shell 能访问
bash -c 'echo $NAME'

# PATH 追加
export PATH="$PATH:/opt/mybin"

# 使用变量构建文件名
FILE="${NAME}_log.txt"
echo $FILE
```

---

💡 小技巧：

1. **区分普通变量和环境变量**，决定子进程是否能访问
2. **尽量用大写字母命名环境变量**（例如 PATH, HOME）
3. **使用** `${VAR}` 可以避免和字符串粘连导致的解析错误

---

### 7.1.5. shell展开机制

对，你理解得基本正确，我给你系统梳理一下 **Shell 展开机制和 echo 行为**，让你彻底弄明白。

---

#### 7.1.5.1. 1️⃣ Shell 展开大致顺序

当你在 shell 输入命令，例如：

```
echo file{1,2}.txt file?.txt
```

Shell 会按**一定顺序**对命令行进行展开：

1. **Brace Expansion（花括号展开）**

- `{a,b}` → 生成所有组合
- 只影响花括号内的内容，外面的前缀/后缀保留
- 例：`file{1,2}.txt` → `file1.txt file2.txt`

2. **Tilde Expansion（波浪号展开）**

- `~` → 当前用户家目录 `/home/user`

3. **Parameter Expansion（变量展开）**

- `$VAR` → 变量值

4. **Command Substitution（命令替换）**

- `$(command)` → 命令输出

5. **Arithmetic Expansion（算术展开）**

- `$((1+2))` → `3`

6. **Word Splitting（按 IFS 切分成单词）**

- 默认 IFS = 空格、Tab、换行
- 作用在变量展开后的字符串，按空格分成独立参数

7. **Filename Expansion / Globbing（通配符展开）**

- `*`、`?`、`[]` → 匹配文件名

|通配符|功能|示例|匹配结果|
|---|---|---|---|
|`*`|匹配任意数量的字符（包括零个）|`*.txt`|匹配所有以 `.txt`<br><br>结尾的文件|
|`?`|匹配任意单个字符|`file?.txt`|`file1.txt`<br><br>、`fileA.txt`<br><br>，但不匹配 `file10.txt`|
|`[]`|匹配方括号内的任意单个字符|`file[12].txt`|`file1.txt`<br><br>或 `file2.txt`<br><br>，不匹配 `file3.txt`|

---

#### 7.1.5.2. 2️⃣ echo 的行为

- `echo` 只是一个命令，它**接收 shell 展开的参数**
- shell 展开完成后，`echo` 得到一个参数列表，然后把它们用空格连接输出
- 例子：

```
echo file{1,2}.txt
```

- shell 展开 → `echo file1.txt file2.txt`
- echo 输出 → `file1.txt file2.txt`

echo 本身不会影响展开，也不会拆分参数，它只按参数顺序打印

---

#### 7.1.5.3. 3️⃣ 总结关键点

1. **所有 Shell 展开都是在命令执行前完成的**
2. **空格是参数分隔符**

- 展开后，shell 会按空格切分成参数列表

3. **echo 不会改变展开结果**

- 它只是把参数打印出来

4. **禁止 shell 展开**

- 单引号 `'…'` → 完全禁止展开
- 双引号 `"…"` → 禁止大部分展开，但允许变量替换 `$VAR` 和命令替换 `$(…)`
- 反斜杠 `\` → 转义特殊字符

---

#### 7.1.5.4. 4️⃣ 举例对比

```
VAR="foo bar"

echo file{1,2}.txt
# 输出: file1.txt file2.txt
# shell 展开 brace，生成两个参数

echo "$VAR"
# 输出: foo bar
# 双引号禁止 word splitting，所以作为一个参数

echo $VAR
# 输出: foo bar
# 未加引号，word splitting 按空格拆成两个参数
```

---

💡 所以你可以理解为：

- **展开是在命令执行前发生的**
- **空格决定了参数边界**
- **echo 只负责打印参数，不影响展开**

---

## 7.2. 流与重定向

### steam

Linux 中的 流 是数据的一种传输方式，是一种字节流，可以从一个地方“流”向另一个地方。常见的三种标准流（标准 IO 流）是：  
名称 文件描述符 默认连接的对象 作用  
标准输入（stdin） 0 键盘 程序的输入来源  
标准输出（stdout） 1 终端（屏幕） 程序的正常输出内容  
标准错误（stderr） 2 终端（屏幕） 程序的错误输出内容  
这些流都可以被重定向，从而实现灵活的数据传输。

### 重定向流

和 < 是 Linux shell（尤其是 Bash）中用于流的重定向（redirection）的关键符号，它们的作用是将标准流从默认的设备重定向到别的地方（如文件、设备、命令等）。  
符号 含义 示例  
重定向标准输出 到文件（覆盖） echo hello > out.txt

重定向标准输出 到文件（追加） echo world >> out.txt  
< 将文件作为标准输入 sort < input.txt  
2> 重定向标准错误 到文件 ls not_exist 2> error.log  
2>> 重定向标准错误到文件（追加） ls not_exist 2>> error.log  
& 合并输出（复制文件描述符） command > all.log 2>&1  
<& 将文件描述符作为输入（较少用） command <&0

_将标准输出重定向到null，终端将什么也不显示_

cat main.c > /dev/nul

：将标准输出重定向到文件（追加模式）  
echo "world" >> output.txt  
会把 "world" 追加在 output.txt 的末尾，不会覆盖原内容。

<：将文件作为标准输入  
sort < unsorted.txt  
等价于 cat unsorted.txt | sort，也是把 unsorted.txt 的内容当作输入传给 sort。

2>：将**标准错误（stderr）**重定向到文件  
ls no_file 2> error.log  
把错误信息写入 error.log，而不是打印到终端。

2>&1：将标准错误重定向到标准输出

  
ls no_file > all.log 2>&1  
意思是：标准输出写入 all.log，标准错误也跟着一起写进去。  
2>&1 是说：“把文件描述符 2（标准错误）重定向到文件描述符 1（标准输出）”  
1.2 | & || && 符号

  

  

## 7.3. 管道与组合命令

### 7.3.1. 管道 （|）

管道符号 | 用于将一个命令的输出作为另一个命令的输入。这种方式允许你通过链式命令处理数据，而不需要将中间结果保存到文件中。

查看当前目录下所有文件和文件夹，并只显示文件（不包括目录）的名称：

ls -l | grep '^-'

### 7.3.2. 逻辑与（&&）

如果前一个命令执行成功（即退出状态为0），则执行后一个命令。这通常用于确保只有当前一个命令成功完成时，才继续执行下一个命令。  
示例：  
cd /some/directory && ls  
在这个例子中，如果cd /some/directory命令成功地将当前目录更改为/some/directory，则执行ls命令列出该目录下的内容。如果cd命令失败（例如，目录不存在），则不会执行ls命令。

### 7.3.3. 逻辑或（||）

如果前一个命令执行失败（即退出状态非0），则执行后一个命令。这通常用于为前一个命令的失败情况提供一个备用选项或执行一些清理工作。  
示例：  
grep 'pattern' file.txt || echo 'Pattern not found'  
在这个例子中，grep 'pattern' file.txt命令在file.txt中搜索指定的模式。如果找到了匹配项，grep命令会成功执行并退出，不会执行echo 'Pattern not found'。但是，如果grep没有找到匹配项并因此失败，则会执行echo命令来报告未找到模式。

## 7.4. 前后台任务

& 符号用于在后台执行命令。当你在命令行中运行一个需要较长时间完成的命令时，你可能不想等待它完成就继续执行其他命令。通过在命令的末尾添加 &，你可以让该命令在后台运行，而你可以立即在同一终端中执行其他命令。

### 7.4.1. 在后台执行一个长时间运行的命令，如 sleep 1000（模拟等待1000秒）：

sleep 1000 &  
#这条命令会立即返回，不会阻塞终端，  
#而 sleep 1000 命令将在后台运行。

### 7.4.2. 你可以通过 jobs 命令查看后台任务的列表，

#并使用 fg 命令将某个任务带回到前台，

或者使用 bg 命令继续让它在后台运行。

# 8. 网络与安全

## 8.1. selinux

### 8.1.1. 关闭SELINUX

```
vi /etc/sysconfig/selinux

// 添加 SELINUX=disable
```

  

# 9. 高级运维工具

# 10. 内核与原理

# 11.