# 第1章  Linux快速入门

Linux是一套免费使用和自由传播的[类UNIX](http://baike.baidu.com/item/%E7%B1%BBUnix)[操作系统](http://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/192)，是一个基于[POSIX](http://baike.baidu.com/item/POSIX)移植操作系统接口（Portable Operating System Interface of UNIX，POSIX）和[UNIX](http://baike.baidu.com/item/UNIX)的多用户、[多任务](http://baike.baidu.com/item/%E5%A4%9A%E4%BB%BB%E5%8A%A1)、支持[多线程](http://baike.baidu.com/item/%E5%A4%9A%E7%BA%BF%E7%A8%8B)和多[CPU](http://baike.baidu.com/item/CPU)的操作系统。

目前被广泛使用于企业服务器、WEB网站平台、大数据、虚拟化、Android、超级计算机等领域，未来Linux将应用各行各业，例如云计算、物联网、人工智能等。

本章向读者介绍Linux发展简介、Linux发行版特点、32位及64位CPU特性及Linux内核命名规则。

## 1.1 为什么要学习Linux

我们为什么要学习Linux？我们目前的处境是什么？我们想达到什么样的目标？在谈到这三个问题时，相信每个人都有自己的答案，我们来自不同的家庭，各种经历也都不一样，但最终的目标都是希望通过学习技术，提升自己的专业技术。真正做一个对社会有贡献的人。

想想我们刚步入学堂的那一刻起，心里就狠狠下决心，以后不管做什么，都要有一番出息，可是20年、30年过去了，我们收获了什么，得到了什么，到底是在追求什么？方向又在哪里呢？

在生活中各种挫折、感情、生活以及很多零碎的事情，让我们很难静下心来学习，当我们某天突然惊醒，年少已不在。所以今天就下定决心，现在就要学习，去行动，去改变。

人生最可怕的是在自以为舒适的地方待得太久，等到外界改变来的时候，已经晚了，我们不能像温水煮青蛙一样，待在温水里，没有觉察到周围事物的变化，最终被社会所淘汰,如图1-1所示。

![](学习笔记/Attachments/1737817648005-3aa00dd2-0866-40d4-b90e-4f110a1314a7.jpeg)

图1-1 温水煮青蛙

## 1.2     Linux操作系统简介

Linux操作系统是基于UNIX以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统，Linux能运行各种工具软件、应用程序及网络协议，它支持安装在32位和64位CPU硬件上。

通常的讲，Linux这个词本身只表示Linux内核，但是人们已经习惯用Linux来形容整个基于Linux内核的操作系统，并且是一种使用GNU通用公共许可证（GNU General Public License，GPL）工程各种工具和数据库的操作系统。

GNU是“GNU is Not Unix”，UNIX是一种广泛使用的商业操作系统，由于GNU将要实现以UNIX系统的接口标准，因此GNU计划可以分别开发不同的操作系统部件，并且采用了部分当时已经可自由使用的软件。

为了保证GNU软件可以自由地“使用、复制、修改和发布”，所有的GNU软件都在一份禁止其他人添加任何限制的情况下授权所有权利给任何人的协议条款里，我们把这个条款称之为GNU通用公共许可证（GNU General Public License，GPL）。

1991年的10月5日，Linux创始人Linus Torvalds在comp.os.minix[新闻组](http://baike.baidu.com/view/834.htm)上发布消息，正式向外宣布Linux内核的诞生，1994年3月Linux 1.0发布，代码量17万行，当时是完全按照自由免费的协议发布，随后正式采用GPL协议，目前GPL协议版本包括：GPLv1、GPLv2、GPLv3以及未来的GPLv4、GPLv5等。

## 1.3     Linux操作系统优点

随着IT产业的不断发展，Linux操作系统应用领域越来越广泛，尤其是近年来Linux在服务器领域飞速的发展，主要得益于Linux操作系统具备的如下优点：

q  开源、免费；

q  系统迭代更新；

q  系统性能稳定；

q  安全性高；

q  多任务，多用户；

q  耗资源少；

q  内核小；

q  应用领域广泛；

q  使用及入门容易。

## 1.4     Linux操作系统发行版

学习Linux操作系统，需要选择不同的发行版本，Linux操作系统是一个大类别，Linux操作系统主流发行版本包括：Red Hat Linux、CentOS、Ubuntu、SUSE Linux、Fedora Linux等，具体发行版本区别如下：

1.      Red Hat Linux

Red Hat Linux是最早的Linux发行版本之一，同时也是最著名的Linux版本，Red Hat Linux已经创造了自己的品牌，也是读者经常听到的“红帽操作系统”。Red Hat 1994年创立，目前公司全世界有3000多人，一直致力于开放的源代码体系，向用户提供一套完整的服务，这使得它特别适合在公共网络中使用。这个版本的Linux也使用最新的内核，还拥有大多数人都需要使用的主体软件包。

Red Hat Linux发行版操作系统的安装过程非常简单，图形安装过程提供简易设置服务器的全部信息，磁盘分区过程可以自动完成，还可以通过图形界面（Graphical User Interface，GUI）完成安装，即使对于Linux新手来说这些都非常简单。后期如果想批量安装Red Hat Linux系统，可以通过批量的工具来实现快速安装。

2.      CentOS

社区企业版操作系统（Community Enterprise Operating System，CentOS）是Linux发行版之一，它是来自于Red Hat Enterprise Linux依照开放源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。

CentOS于Red Hat Linux不同之处在于CentOS并不包含封闭的源代码软件，可以开源免费使用，得到运维人员、企业、程序员的青睐，CentOS发行版操作系统是目前企业使用最多的系统之一，2016年12月12日，CentOS7基于 Red Hat Enterprise Linux 的 CentOS Linux 7 (1611) 系统正式对外发布。

3.      Ubuntu 

Ubuntu是一个以桌面应用为主的Linux操作系统，其名称来自非洲南部祖鲁语或豪萨语的“ubuntu”一词（译为吾帮托或乌班图），意思是“人性”、“我的存在是因为大家的存在”，是非洲传统的一种价值观。

Ubuntu基于Debian发行版和GNOME桌面环境， Ubuntu发行版操作系统的目标在于为一般用户提供一个最新的、同时稳定的以开放自由软件构建而成的操作系统，目前Ubuntu具有庞大的社区力量，用户可以方便地从社区获得帮助。

4.      SUSE Linux

SUSE(发音 /ˈsuːsə/)，SUSE Linux 出自德国，SuSE Linux AG公司发行维护的Linux发行版，是属于此公司的注册商标2003年11月4日，Novell表示将会对SUSE提出收购。收购的工作于2004年1月完成。

Novell也向大家保证SUSE的开发工作仍会继续下去，Novell更把公司内全线电脑的系统换成SUSE LINUX，并同时表示将会把SUSE特有而优秀的系统管理程序 - YaST2以GPL授权释出。

5.      Fedora Linux

Fedora是一个知名的Linux发行版，是一款由全球社区爱好者构建的面向日常应用的快速、稳定、强大的操作系统。它允许任何人自由地使用、修改和重发布，无论现在还是将来。它由一个强大的社群开发，这个社群的成员以自己的不懈努力，提供并维护自由、开放源码的软件和开放的标准。

Fedora 约每六个月会发布新版本，美国当地时间2021年11月3日，北京时间2021年11月4日，Fedora Project 宣布 Fedora 23 正式对外发布，2021年6月发布Fedora 26版本。

6.      Rocky linux

Rocky Linux 是一个社区化的企业级操作系统。其设计为的是与美国顶级企业 Linux 发行版实现 100％ Bug 级兼容，而原因是后者的下游合作伙伴转移了发展方向。目前社区正在集中力量发展有关设施。Rocky Linux 由 CentOS 项目的创始人 Gregory Kurtzer 领导。

Red Hat决定使用一个滚动发布模型CentOS Stream来替代稳定的CentOS Linux。CentOS社区之前明确表示CentOS 8支持到2029年，而现在却说要在2021年底结束支持，这么一出缺乏信用的做法让许多用户感到愤怒。

虽然有一种简单的方法可以从CentOS 8迁移到CentOS Stream，但并不是每个人都希望在生产服务器上采用滚动发行版本。尽管有许多可用的服务器发行版，但CentOS是首选，因为它是RHEL的免费社区版本。

人们想要RHEL的社区分支，这就是为什么CentOS的原始创建者Gregory M. Kurtzer为全新的Rocky Linux创建了一个存储库，它与RHEL完全兼容。

但是Rocky Linux并不是唯一一个试图填补CentOS留下空白的系统。面向企业的服务器发行版，CloudLinux已经宣布他们也在致力于RHEL的社区驱动分支。

CloudLinux的新RHEL分支，该公司提供定制的RHEL和CentOS解决方案已有11年之久。CloudLinux刚刚宣布，他们将在2021年第一季度发布一个开源的、由社区驱动的RHEL分支。

CloudLinux Inc.是一家总部位于美国的公司，开发、销售并支持基于RHEL的定制操作系统，例如CloudLinux OS、CloudLinux OS+，并为CentOS 6提供扩展的生命周期支持。

该公司成立于2009年，拥有大量的Linux专家。由于他们已经这样做了大约11年，可以肯定地说，他们的开源CloudLinux将会是CentOS的绝佳替代品之一。在他们的博客文章中，他们声明他们将使所有的构建和测试软件免费、开源、易于安装。

如果您使用的是CentOS 8，他们将发布与其非常相似的操作系统。他们还将提供稳定且经过测试的更新，直到2029年完全免费。最重要的是，您将能够通过执行一个命令来从CentOS 8迁移到CloudLinux，该命令将切换仓库和密钥。

最后想说的时，IBM虽然消灭了CentOS，但现在社区已经带来了两个CentOS。这对大公司来说是一个教训，开源社区不是企业垄断的地方。

## 1.5     32位与64位操作系统的区别

学习Linux操作系统之前，需要理解计算机基本的常识，计算机内部对数据的传输和储存都是使用二进制，二进制是计算技术中广泛采用的一种[数制](http://baike.baidu.com/item/%E6%95%B0%E5%88%B6)，而Bit（比特）则表示二进制位，[二进制数](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%95%B0)是用0和1两个[数码](http://baike.baidu.com/item/%E6%95%B0%E7%A0%81)来表示的数。基数为2，进位规则是“逢二进一”，0或者1分别表示一个Bit二进制位。

Bit位是计算机最小单位，而字节是计算机中数据处理的基本单位，转换单位为：1Byte=8Bit，4Byte=32Bit。

随着计算机技术的发展，尤其是中央处理器（Central Processing Unit，CPU）技术的变革，CPU的位数指的是通用寄存器（General-Purpose Registers， GPRs）的数据宽度，也就是处理器一次可以处理的数据量多少。

目前主流CPU处理器分为32位CPU处理器和64位CPU处理器，32位CPU处理器可以一次性处理4个字节的数据量。而64位处理器一次性处理8个字节的数据量（1Byte=8bit），64位CPU处理器对计算机处理器在RAM里（随机存取储存器）处理信息的效率比32位CPU做了很多优化，效率更高。

X86_32位操作系统和X86_64操作系统也是基于CPU位数的支持，具体区别如下：

q  32位操作系统表示32位CPU对内存寻址的能力；

q  64位操作系统表示64位CPU对内存寻址的能力；

q  32位的操作系统安装在32位CPU处理器和64位CPU处理器上；

q  64位操作系统只能安装64位CPU处理器上；

q  32位操作系统对内存寻址不能超过4GB；

q  64位操作系统对内存寻址可以超过4GB，企业服务器更多安装64位操作系统，支持更多内存资源的利用；

q  64位操作系统是为高性能处理需求设计，数据处理、图片处理、实时计算等领域需求；

q  32位操作系统是为普通用户设计，普通办公、上网冲浪等需求。

## 1.6     Linux内核命名规则

Linux内核是Linux操作系统的核心，一个完整的Linux发行版包括进程管理、内存管理、文件系统、系统管理、网络操作等部分。

Linux内核官网可以下载Linux内核版本、现行版本，历史版本，可以了解版本与版本之间的特性。

     Linux内核版本命名在不同的时期有其不同的命名规范，其中在2.X版本中，X如果为奇数表示开发版、X如果为偶数表示稳定版，从2.6.X以及3.X，内核版本命名就没有严格的约定规范。

从Linux内核1994年发布1.0发布到目前主流3.X、4.x版本，最新稳定版本是5.14，查看Linux操作系统内核如图1-2所示：

![](学习笔记/Attachments/1737817648284-2abf3beb-b8e4-49b7-8def-da1adcff4119.png)

图1-2操作系统内核

Linux内核命名格式为 “R.X.Y-Z”，其中R、X、Y、Z命名意义如下：

q  数字R表示内核版本号，版本号只有在代码和内核有重大改变的时候才会改变，到目前为止有4个大版本更新。

q  数字X表示内核主版本号，主版本号根据传统的奇偶系统版本编号来分配，奇数为开发版，偶数为稳定版。

q  数字Y表示内核次版本号，次版本号是无论在内核增加安全补丁、修复Bug、实现新的特性或者驱动时都会改变。

q  数字Z表示内核小版本号，小版本号会随着内核功能的修改、Bug修复而发生变化。

官网内核版本如图1-3所示，Mainline表示主线开发版本，Stable表示稳定版本，稳定版本主要由mainline测试通过而发布。Longterm表示长期支持版，会持续更新及Bug修复，如果长期版本被标记为EOL（End of Life），则表示不再提供更新。

![](学习笔记/Attachments/1737817648501-1fbed42d-46cb-4573-be98-7aaeb3786722.png)

图1-3官网内核版本

# 第2章  Linux发展及系统安装

互联网飞速发展，用户对网站体验要求也越来越高，目前主流WEB网站后端承载系统均为Linux操作系统，Android手机也是基于Linux内核而研发，企业大数据、云存储、虚拟化等先进技术也均是基于Linux操作系统为载体，满足企业的高速发展。

本章向读者介绍Linux发展前景、Windows与Linux操作系统区别、硬盘分区介绍、CentOS 7 Linux操作系统安装图解及菜鸟学好Linux必备大绝招。

## 2.1     Linux发展前景及就业形势

权威部门统计，未来几年内我国软件行业的从业机会十分庞大，中国每年对IT软件人才的需求将达到200万人左右。而对于Linux专业人才的就业前景，更是广阔；据悉在未来5-10年内Linux专业人才的需求将达到150万，尤其是有Linux行业经验的，资深的Linux工程师非常缺乏，薪资也非常诱人，平均月薪15000-25000，甚至更高，Linux行业薪资如图2-1所示：

![](学习笔记/Attachments/1737817648783-8adec420-c391-45bf-9fa2-6dc1529743e5.png)

图2-1 Linux行业薪资

## 2.2     Windows操作系统简介

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

## 2.3     硬盘分区简介

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

## 2.4     Linux安装环境准备

要学好Linux这门技术，首先需安装Linux操作系统，Linux操作系统安装是每个初学者的门槛。而安装Linux操作系统，最大的困惑莫过于给操作系统进行磁盘分区。

虽然目前各种发行版本的 Linux 已经提供了友好的图形交互界面，但很多初学者还是感觉无从下手,这其中原因主要是不清楚Linux 的分区规定。

Linux 系统安装中规定，同样每块硬盘设备最多只能分 4个主分区（其中包含扩展分区）构成，任何一个扩展分区都要占用一个主分区号码，也就是在一个硬盘中，主分区和扩展分区一共最多是 4 个。

为了让读者能将本书所有Linux技术应用于企业，本书案例以企业里主流Linux操作系统CentOS为蓝本，目前主流CentOS发行版本为CentOS8.0。

读者在安装CentOS操作系统时，如果没有多余的计算机裸机设备，可以基于Windows主机上安装Vmware workstation工具，该工具的用途可以在Windows主机上创建多个计算机裸机设备资源，包括：CPU、内存、硬盘、网卡、DVD光驱、USB接口、声卡，创建的多个计算机裸机设备共享Windows主机的所有资源。

读者在安装CentOS操作系统时，如果有多余的计算机裸机设备或者企业服务器，可以将CentOS系统直接安装在多余的设备上，安装之前需要下载CentOS8.0操作系统镜像文件(International Organization for Standardization,ISO 9660标准)，通过刻录工具，将ISO镜像文件刻录至DVD光盘或者U盘里，通过DVD或者U盘启动然后安装系统。

如下为在Windwos主机上安装VMware workstation虚拟机软件，虚拟机软件的用途是可以在真实机上模拟一个新的计算机完整的资源设备，进而可以在计算机裸设备上安装CentOS8.0操作系统步骤：

（1）      安装环境准备

q  [VMware workstation 10.0](http://www.xp510.com/xiazai/ossoft/desktools/22610.html)

q  CentOS 8.0 x86_64

（2）      Vmware Workstation 10.0下载（其他版本也可以）

|[http://download3.vmware.com/software/wkst/file/VMware-workstation-full-10.0.1-1379776.exe](http://download3.vmware.com/software/wkst/file/VMware-workstation-full-10.0.1-1379776.exe)|
|---|

（3）      CentOS8.0操作系统ISO镜像下载

|[http://ftp.sjtu.edu.cn/centos/8.0.1905/isos/x86_64/CentOS-8-x86_64-1905-dvd1.iso](http://ftp.sjtu.edu.cn/centos/8.0.1905/isos/x86_64/CentOS-8-x86_64-1905-dvd1.iso)|
|---|

（4）      将VMware workstation 10.0和CentOS8 ISO镜像文件下载至Windows系统，双击VMware-workstation-full-10.0.1-1379776.exe，根据提示完成安装，会在Windows桌面显示VMware Workstation图标，如图2-2所示：

![](学习笔记/Attachments/1737817649005-7295ab08-b065-402c-880e-b7bab290ef2c.png)

图2-2 VMware Workstation图标

（5）      双击桌面VMware Workstation图标打开虚拟机软件，单击“创建新的虚拟机”，如图2-3所示：

![](学习笔记/Attachments/1737817649182-83ada21d-1da6-42ce-a329-2a280c85f2ea.png)

图2-3 VMware Workstation创建虚拟机

（6）      新建虚拟机向导，选择自定义（高级）（C）选项，如图2-4所示：

![](学习笔记/Attachments/1737817649394-3e3bb3fb-b059-4e98-b56c-b1efdc2f47b1.png)

图2-4 创建虚拟机向导

（7）      安装客户机操作系统，选择“稍后安装操作系统（S）”，如图2-5所示:

![](学习笔记/Attachments/1737817649687-5bc2ad4d-5af3-4cef-ac05-85adc8bc86de.png)

图2-5 安装客户机操作系统

（8）      选择客户机操作系统，由于我们即将安装CentOS8.0操作系统，所以需要勾选“Linux（L）”，同时版本（V）选择“CentOS64位”，如图2-6所示：

![](学习笔记/Attachments/1737817649974-15755622-e4b6-4ee1-8e55-293344782c3e.png)

图2-6 操作系统版本

（9）      虚拟机内存设置，默认为1024MB，如图2-7所示：

![](学习笔记/Attachments/1737817650162-daca2c0a-a193-433c-9cc8-d426144e08f0.png)

图2-7 虚拟机内存分配

（10）  选择虚拟机网络类型，此处选择-网络连接为-“使用桥接模式”，如图2-8所示：

![](学习笔记/Attachments/1737817650354-a5b30ad9-63b6-40e1-964e-ddea92e239c0.png)

图2-8 虚拟机网络类型

（11）  指定磁盘容量，设置虚拟机硬盘大小为40GB，将虚拟磁盘拆分成多个文件，如下图2-9所示：

![](学习笔记/Attachments/1737817650533-048ec02b-ae6d-491e-8832-cbcf45c21473.png)

图2-9设置虚拟机磁盘容量

（12）  虚拟机硬件资源创建完成，设备详情里面包括计算机常用设备，例如内存、处理器、硬盘、CD/DVD、网络适配器等，如图2-10所示：

![](学习笔记/Attachments/1737817650735-2a3fcc85-2466-4830-b125-fb203c3aa3e3.png)

图2-10虚拟机裸机设备

（13）  将CentOS8.0 ISO系统镜像文件添加至虚拟机CD/DVD中，双击虚拟机“CD/DVD（IDE）自动检测”选项，选择CentOS-8.0-x86_64-1905-DVD1.iso镜像文件，如图2-11所示：

![](学习笔记/Attachments/1737817650910-da0587c9-174e-4697-b1d3-294598ed450e.png)

图2-11选择系统安装镜像

## 2.5     CentOS8.x系统安装图解

要学好Linux技术，首先就是学会安装好Linux系统，Linux操作系统安装在计算机硬件设备上，常见的安装方式有两种：

n  计算机硬件设备-ISO光盘|U盘安装;

n  VMware虚拟化软件-创建一台计算机硬件设备-IOS光盘|U盘安装;

VMware Workstation是VMware公司旗下的一款能够“虚拟计算机”的软件，被广泛应用于IT人员去模拟多个计算机设备，并且支持在虚拟的计算机上去安装各种操作系统，从而不需要另外花钱购买计算机硬件设备来安装系统。

VMware Workstation产生的计算机也被称为“虚拟机”，可以让我们在一台PC、笔记本电脑上同时可以运行多个Windows、Mac OS、Linux等操作系统。

![](学习笔记/Attachments/1737817651105-01ee1d35-3327-4d55-b56d-10c377b44f80.png)

（1）      从VMware官网：[https://www.vmware.com/cn/products.html](https://www.vmware.com/cn/products.html) 下载VMware workstation软件（VMware Workstation 64位_16.2.3.21887.exe）至Windows 电脑上，然后选中文件-右键以管理员方式运行，选择下一步，如图所示：

![](学习笔记/Attachments/1737817651326-1ab648ad-edde-4a0a-80fd-4064d0710440.png)

（2）      选择自定义安装，根据提示选择下一步，一直循环，产品使用秘钥暂时无需输入（选择试用30天），后期可以输入，直到安装成功，根据提示选择重启系统，如图所示：

![](学习笔记/Attachments/1737817651517-830b8e30-8814-4dc4-a582-0fdadd374230.png)

（3）      安装完成之后，桌面上会有VMware Workstation Pro图标，点击右键管理员方式运行，如图所示：

![](学习笔记/Attachments/1737817651709-c00be180-aacf-4537-8b74-11281d065c55.png)

（4）      选择创建新的虚拟机（计算机硬件设备）-选择典型-稍后安装操作系统-客户端操作系统（CentOS 8 64位）-虚拟机名称默认-磁盘容量20G默认-点击完成。如图所示：

（5）      从CentOS官方（合作站点：清华大学）下载CentOS8.x镜像文件，下载至Windows 10电脑D盘对应目录下，下载链接如下：

[https://mirrors.tuna.tsinghua.edu.cn/](https://mirrors.tuna.tsinghua.edu.cn/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-DVD-2009.iso)

（6）      镜像下载完成之后，打开VMware Workstation-CentOS 8 64位虚拟机-界面，双击打开CD/DVD（IDE）-找到CentOS ISO镜像文件-确定，如图所示：

![](学习笔记/Attachments/1737817651889-90e09c7c-6d21-4f7f-aae0-926d3d9280ae.png)

（7）      最后将CentOS 8 64位虚拟机（计算机）加电启动，点击开机按钮-默认会查找光盘启动项-加载光盘CentOS8镜像文件，如图所示：

![](学习笔记/Attachments/1737817652181-9977278c-e003-48c4-93ed-af4ead7c00e9.png)

（8）      鼠标单击进入VMware虚拟机里面，选择第一项Install CentOS Linux 7或者8，直接按Enter键进行系统安装。

![](学习笔记/Attachments/1737817652436-ec7ef0c3-d254-4c81-92fa-722512a07ad9.png)

图2-12选择安装菜单

（9）      继续按Enter键启动安装进程，进入光盘检测，按Esc键跳过检测，如图2-13所示：

![](学习笔记/Attachments/1737817652626-61e494f0-8350-4597-b3aa-b366089fcec6.png)

图2-13跳过ISO镜像检测

（10）  CentOS8欢迎界面，选择安装过程中界面显示的语言，初学者可以选择“简体中文”或者默认English，如图2-14所示：

![](学习笔记/Attachments/1737817652796-190fa7c5-d688-42a8-b22f-3ce997424a92.png)

图2-14选择安装过程语言

（11）  CentOS8 Installation Summary安装总览界面，如图2-15所示：

![](学习笔记/Attachments/1737817653048-a4063632-d88a-4beb-ab73-86d6779b0138.png)

图2-15 Installation Summary界面

（12）  选择“I will configure partioning或者Custom”，单击Done，如图2-16所示：

![](学习笔记/Attachments/1737817653293-007db854-6f2f-48a8-92bf-e591e36ff608.png)

图2-16 磁盘分区方式选择

（13）  下拉框选择“Standard Partition”，选择+号，创建分区，如图2-17所示：

![](学习笔记/Attachments/1737817653547-05aef9aa-8d3d-46d6-916f-c7f60c67797e.png)

图2-17 磁盘分区类型选择

（14）  Linux操作系统分区与Windows操作系统分区C盘、D盘有很大区别，Liunx操作系统是采用树形的文件系统管理方式，所有的文件存储以/（根）开始，如图2-18所示。

![](学习笔记/Attachments/1737817653747-244881db-aa68-4d86-b3fb-2b188055991c.png)

图2-18 Linux文件系统目录结构

Linux是以文件的方式存储，例如/dev/sda代表整块硬盘，/dev/sda1表示硬盘第一分区，/dev/sda2表示硬盘第二分区，为了能将目录和硬盘分区关联，所以Linux采用挂载点的方式来关联磁盘分区，/boot目录、/根目录、/data目录跟磁盘管理后，称之为分区，每个分区功能如下：

q  /boot分区用于存放Linux内核及系统启动过程所需文件;

q  Swap分区又称为交换分区，类似Windows系统的虚拟内存，物理内存不够用时，以供程序使用Swap内存;物理内存32G，虚拟内存512MB！阿里云交换分区设置0;

q  /分区用于系统安装核心分区及所有文件存放的根系统;

q  /data分区为自定义分区，企业服务器中用于作应用数据存放。

如图2-19所示，创建/boot分区并挂载，分区大小为200MB：

![](学习笔记/Attachments/1737817653915-9ec2ffd8-5173-4537-b931-4bbbe74fadbd.png)

图2-19 创建/boot分区

单击“Add mount point”即可，磁盘分区默认文件系统类型为XFS，根据如上方法，依次创建swap分区，大小为0MB，创建/分区，大小为剩余所有空间，最终如图2-20所示：

![](学习笔记/Attachments/1737817654155-ff4305bb-2dd3-4d4d-98bd-7a6a4634a437.png)

图2-20 磁盘完整分区

（15）  选择SOFTWARE SELECTION，设置为Minimal Install最小化安装，如果后期需要开发包、开发库等软件，可以在系统安装完后，根据需求安装即可，如图2-21所示:

![](学习笔记/Attachments/1737817654365-8599d411-6152-408e-9c90-b430326d191f.png)

图2-21 选择安装的软件

（16）  操作系统时区选择，选择Asia-Shanghai，关闭Network Time，如图2-22所示：

![](学习笔记/Attachments/1737817654581-211a535b-93e2-4324-a1dc-e78e8d76c169.png)

图2-22 操作系统时区选择

（17）  以上配置完毕后，单击“Begin Installation”，单击“Root PASSWORD”设置Root用户密码，如图2-23所示，如果需要新增普通用户，可以单击“USER CREATEION”进行创建即可。

![](学习笔记/Attachments/1737817654762-0b7c47d6-97ff-40bc-b49f-fc05acab23e8.png)

图2-23 设置ROOT用户密码

（18）  安装进程完毕，单击“Reboot”重启系统，如图2-24所示：

![](学习笔记/Attachments/1737817654941-4a9f4d36-cbe7-4123-a1bc-d01b6faeece2.png)

![](学习笔记/Attachments/1737817655112-f26511c7-d576-4e4a-bdf0-25e9a818e9f4.png)

图2-24 系统安装完毕

（19）  重启CentOS 8 Linux操作系统，进入Login登录界面，“localhost login:”处输入root，按Enter键，然后“Password：”处输入系统安装时设定的密码，输入密码时不会提示，密码输入完按Enter键，即可登录CentOS 8 Linux操作系统，默认登录的终端称为Shell终端，所有的后续操作指令均在Shell终端上执行，默认显示字符提示，其中#代表当前是root用户登录，如果是$表示当前为普通用户。如图2-25所示：

![](学习笔记/Attachments/1737817655326-6f2cb13e-a972-435b-9205-451c146f32e5.png)

（20）  安装了Centos8之后，发现了一系列的问题，当我使用yum命令安装插件时，系统提示Error: Failed to download metadata for repo ‘AppStream’

原因是CentOS 8将不再从官方CentOS项目获得开发资源。需要更改镜像资源，用下面这种方式解决了问题。

|#备份当前centos8系统所有的repo源文件；<br><br>cd /etc/yum.repos.d/<br><br>mkdir -p /data/backup/<br><br>mv * /data/backup/<br><br>#下载阿里网repo源文件；<br><br>[https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo](https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo)<br><br>#创建文件centos.repo。由于wget命令不可用，只能将该文件中的内容复制然后粘贴在文件中；<br><br>#刷新并重新设置缓存<br><br>yum clean all<br><br>#如果没有网络，需要重启Centos8、Rocky Linux网卡服务；<br><br>nmcli c reload<br><br>nmcli c up ens33<br><br>#安装network工具，支持network管理网络；<br><br>yum install network-scripts -y|
|---|

![](学习笔记/Attachments/1737817655595-2b924860-74ef-4f7d-a95b-b301653e5426.png)

![](学习笔记/Attachments/1737817655879-37ec627b-d12e-4fa6-84c8-d7c69af72298.png)

## 2.6     Rocky Linux 9.x系统安装图解

如果直接在硬件设备上安装Rocky Linux系统，不需要安装虚拟机等步骤，直接将U盘或者光盘插入DVD光驱即可，打开电源设备。

![](学习笔记/Attachments/1737817656093-023f185e-3bdc-48db-9af2-c54b0161ec24.png)

（1）      如图2-12所示，光标选择第一项Install Rocky Linux，直接按Enter键进行系统安装。

![](学习笔记/Attachments/1737817656320-86a1809d-14a6-44e8-ade2-5523b4c2a794.png)

图2-12选择安装菜单

（2）      继续按Enter键启动安装进程，进入光盘检测，按Esc键跳过检测，如图2-13所示：

![](学习笔记/Attachments/1737817656573-2d2f6990-480b-4dec-92d1-e9e98a89f921.png)

图2-13跳过ISO镜像检测

（3）      Rocky Linux欢迎界面，选择安装过程中界面显示的语言，初学者可以选择“简体中文”或者默认English，如图2-14所示：

![](学习笔记/Attachments/1737817656793-19c61a5a-ca60-4f76-be00-52497eeb507c.png)

图2-14选择安装过程语言

（4）      Rocky Linux Installation Summary安装总览界面，如图2-15所示：

![](学习笔记/Attachments/1737817657013-04a134fa-508a-4fe5-a2cf-3ae55bac7a31.png)

图2-15 Installation Summary界面

（5）      选择“I will configure partioning或者Custom”，单击Done，如图2-16所示：

![](学习笔记/Attachments/1737817657222-1cebf50b-347e-4923-b8c1-c8823da21200.png)

图2-16 磁盘分区方式选择

（6）      下拉框选择“Standard Partition”，选择+号，创建分区，如图2-17所示：

![](学习笔记/Attachments/1737817657989-6245991c-1835-443b-92eb-9165138b5a75.png)

图2-17 磁盘分区类型选择

（7）      Linux操作系统分区与Windows操作系统分区C盘、D盘有很大区别，Liunx操作系统是采用树形的文件系统管理方式，所有的文件存储以/（根）开始，如图2-18所示。

![](学习笔记/Attachments/1737817653747-244881db-aa68-4d86-b3fb-2b188055991c.png)

图2-18 Linux文件系统目录结构

Linux是以文件的方式存储，例如/dev/sda代表整块硬盘，/dev/sda1表示硬盘第一分区，/dev/sda2表示硬盘第二分区，为了能将目录和硬盘分区关联，所以Linux采用挂载点的方式来关联磁盘分区，/boot目录、/根目录、/data目录跟磁盘管理后，称之为分区，每个分区功能如下：

q  /boot分区用于存放Linux内核及系统启动过程所需文件;

q  Swap分区又称为交换分区，类似Windows系统的虚拟内存，物理内存不够用时，以供程序使用Swap内存;物理内存32G，虚拟内存512MB！阿里云交换分区设置0;

q  /分区用于系统安装核心分区及所有文件存放的根系统;

q  /data分区为自定义分区，企业服务器中用于作应用数据存放。

（8）      选择SOFTWARE SELECTION，设置为Minimal Install最小化安装，如果后期需要开发包、开发库等软件，可以在系统安装完后，根据需求安装即可，如图2-21所示:

![](学习笔记/Attachments/1737817658205-1b66f960-9c06-4f6c-bf8a-c423d35256e6.png)

图2-21 选择安装的软件

（9）      操作系统时区选择，选择Asia-Shanghai，关闭Network Time，如图2-22所示：

![](学习笔记/Attachments/1737817658334-b9ddb231-defb-4ca4-a43f-d52ea02739e1.png)

图2-22 操作系统时区选择

（10）  Rocky Linux9.x默认ROOT用户是禁用的，可以在此开启或者登录系统修改/etc/ssh/sshd_config配置文件开启，如图所示：

![](学习笔记/Attachments/1737817658647-a0fedec3-4c9e-43c9-b35f-158b2613aee5.png)

（11）  以上配置完毕后，单击“Begin Installation”，如图2-23所示。

![](学习笔记/Attachments/1737817659105-2023e64a-6c65-46f2-bae3-861b23e6ef6d.png)

图2-23 设置ROOT用户密码

（12）  安装进程完毕，单击“Reboot”重启系统，如图2-24所示：

![](学习笔记/Attachments/1737817659258-326e2b3e-b4b7-4877-ae1c-5ecd63b44f18.png)

图2-24 系统安装完毕

（13）  重启Rocky Linux Linux操作系统，进入Login登录界面，“localhost login:”处输入root，按Enter键，然后“Password：”处输入系统安装时设定的密码，输入密码时不会提示，密码输入完按Enter键，即可登录Rocky Linux Linux操作系统，默认登录的终端称为Shell终端，所有的后续操作指令均在Shell终端上执行，默认显示字符提示，其中#代表当前是root用户登录，如果是$表示当前为普通用户。如图2-25所示：

![](学习笔记/Attachments/1737817659423-537a88e3-4c46-40d7-af1f-094f93135143.png)

图2-25 Login登录系统界面

|#刷新并重新设置缓存<br><br>yum clean all<br><br>#如果没有网络，需要重启Rocky Linux网卡服务；<br><br>nmcli c reload<br><br>nmcli c up ens33<br><br>#修改网卡配置文件；<br><br>vi /etc/NetworkManager/system-connections/ens33.nmconnection<br><br>[ipv4]<br><br>address1=192.168.101.135/24,192.168.101.2<br><br>dns=8.8.8.8<br><br>method=manual<br><br>#method=auto //为自动获取IP；<br><br>#重启网卡服务；systemctl restart NetworkManager  <br>nmcli c down ens33  <br>nmcli c up ens33|
|---|

## 2.7     菜鸟学好Linux大绝招

Linux系统安装是初学者的门槛，系统安装完毕后，很多初学者不知道该如何学习，不知道如何快速进阶，下面作者总结了菜鸟学好Linux技能的大绝招：

q  初学者完成Linux系统分区及安装之后，需熟练掌握Linux系统管理必备命令：cd、ls、pwd、clear、chmod、chown、chattr、useradd、userdel、groupadd、vi、vim、cat、more、less、mv、cp、rm、rmdir、touch、ifconfig、ip addr、ping、route、echo、wc、expr、bc、ln、head、tail、who、hostname、top、df、du、netstat、ss、kill、alias、man、tar、zip、unzip、jar、fdisk、free、uptime、lsof、lsmod、lsattr、dd、date、crontab、ps、find、awk、sed、grep、sort、uniq等，每个命令至少练习30遍，逐步掌握每个命令的用法及应用场景；

q  初学者进阶之路，需熟练构建Linux下常见服务：NTP、VSFTPD、DHCP、SAMBA、DNS、Apache、MySQL、Nginx、Zabbix、Squid、Varnish、LVS、Keepalived、ELK、MQ、Zookeeper、Docker、Openstack、Hbase、Mongodb、Redis、CEPH、Prometheus、Jenkins、SVN、GIT等，遇到问题先思考，没有头绪可以借助百度、Google搜索引擎，问题解决后，将解决问题的步骤总结并形成文档；

q  理解操作系统的每个命令，每个服务的用途，为什么要配置这个服务，为什么需要调整该参数，只有带着目标去学习才能更快的成长，才能让你去发掘更多新知识；

q  熟练搭建Linux系统上各种服务之后，需要理解每个服务的完整配置和优化，可以拓展思维。例如LNMP所有服务放在一台机器上，能否分开放在多台服务器以平衡压力呢，该如何去构建和部署呢？一台物理机构建Docker虚拟化，如果是100台、1000台如何去实施呢，会遇到哪些问题呢；

q  Shell是Linux最经典的命令解释器，Shell脚本可以实现自动化运维，平时多练习Shell脚本编程，每个Shell脚本多练习几遍，从中吸取关键的参数、语法，不断的练习，不断的提高；

q  建立个人学习博客，把平时工作、学习中的知识都记录到博客，一方面可以供别人参考，另一方面可以提高自己文档编写及总结的能力；

q  学习Linux技术是一个长期的过程，一定要坚持，遇到各种错误、问题可以借助百度、Google搜索引擎，如果解决不了，可以请教同学、朋友及你的老师；

q  通过以上步骤的学习方法，不断进步，如果想达到高级、资深大牛级别，还需要进一步深入学习WEB集群架构、网站负载均衡、网站架构优化、自动化运维、运维开发、虚拟化、云计算、分布式集群等知识；

q  最后一句话，多练习才是硬道理，实践出真知。

## 2.8     本章小结

通过对本章内容的学习，读者对Linux系统有了一个初步的理解，了解Linux行业的发展前景，学会了如何在企业中或者虚拟机中安装Linux系统。

对32位、64位CPU处理器以及对Linux内核版本命名也有进一步的认识，同时掌握了学习Linux的大绝招。

## 2.9     同步作业

1.      企业中服务器品牌DELL R730，其硬盘总量为300G，现需安装CentOS 7 Linux操作系统，请问如何进行分区？

2.      GNU与GPL的区别是什么？

3.      企业一台Linux服务器，查看该Linux内核显示：3.10.0-327.36.3.el7.x86_64，请分别说出点号分割的每个数字及字母的含义？

4.      CentOS Linux至今发布了多少个系统版本？

5.      如果Linux系统采用光盘安装，如何将ISO镜像文件刻录成光盘，请写出具体实现流程？

# 第3章   CentOS系统管理

Linux系统安装完毕，需要对Linux系统进行管理和维护，让Linux服务器能真正应用于企业中。

本章向读者介绍Linux系统引导原理、启动流程、系统目录、权限、命令及CentOS7和CentOS6在系统管理、命令方面有什么区别，让我们一起来遨游在Linux的海洋里。

## 3.1     操作系统启动概念

不管是Windows还是Linux操作系统，底层设备一般均为物理硬件，操作系统启动之前会对硬件进行检测，然后硬盘引导启动操作系统，如下为操作系统启动相关的各个概念：

### 3.1.1  BIOS

基本输入输出系统（Basic Input Output System，BIOS）是一组固化到计算机主板上的只读内存[镜像](http://baike.baidu.com/item/%E9%95%9C%E5%83%8F/1574)（Read Only Memory image，ROM）芯片上的程序，它保存着计算机最重要的基本输入输出的程序、系统设置信息、开机后自检程序和系统自启动程序。主要功能是为计算机提供最底层的、最直接的硬件设置和控制。

### 3.1.2  MBR

全新硬盘在使用之前必须进行分区格式化，硬盘分区初始化的格式主要由两种，分别是：MBR格式和GPT格式。

如果使用MBR格式，操作系统将创建主引导记录扇区（Main Boot Record，MBR），MBR位于整块硬盘的0[磁道](http://baike.baidu.com/view/201106.htm)0柱面1[扇区](http://baike.baidu.com/view/201129.htm)，主要功能是操作系统对磁盘进行读写时，判断分区的合法性以及分区引导信息的定位。

[主引导扇区](http://baike.baidu.com/view/418400.htm)总共为512字节，MBR只占用了其中的446个字节，另外的64个字节为[硬盘分区表](http://baike.baidu.com/view/1385.htm) (Disk Partition Table，DPT)，最后两个字节“55，AA”是分区的结束标志。

在MBR硬盘中，硬盘分区信息直接存储于[主引导记录](http://baike.baidu.com/item/%E4%B8%BB%E5%BC%95%E5%AF%BC%E8%AE%B0%E5%BD%95)（MBR）中，同时主引导记录还存储着系统的[引导程序](http://baike.baidu.com/item/%E5%BC%95%E5%AF%BC%E7%A8%8B%E5%BA%8F)，如图3-1所示：

![](学习笔记/Attachments/1737817659614-b9564443-481e-436e-9437-ec3cba4be13b.png)图3-1 MBR分区表内容

MBR是计算机启动最先执行的硬盘上的程序，只有512字节大小，所以不能载入[操作系统](https://www.baidu.com/s?wd=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YznWf3m1nzP1fYm16vnhwh0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3En1RYP1cvn10L)的核心，只能先载入一个可以载入计算机核心的程序，我们称之为引导程序。

因为MBR分区标准决定了MBR只支持在2TB以下的硬盘，对于后面的多余空间只能浪费。为了支持能使用大于2T硬盘空间，微软和英特尔公司在可扩展固件接口(Extensible Firmware Interface，EFI)方案中开发了全局唯一的标识符（Globally unique identifier，GUID），进而全面支持大于2T硬盘空间在企业中使用。

### 3.1.3  GPT

全局唯一的标识符（Globally unique identifier，GUID），正逐渐取代MBR成为新标准。它和统一的可扩展固件接口 (Unified Extensible Firmware Interface,UEFI)相辅相成。

UEFI用于取代老旧的BIOS，而GPT则取代老旧的MBR。之所以称为“GUID分区表”，是因为驱动器上的每个分区都有一个全局唯一的标识符。

在GPT硬盘中，分区表的位置信息储存在GPT头中。出于兼容性考虑，第一个扇区同样有一个与MBR类似的标记，叫做受保护的主引导记录（Protected Main Boot Record，PMBR）。

PMBR的作用是当使用不支持GPT的分区工具时，整个硬盘将显示为一个受保护的分区，以防止分区表及硬盘数据遭到破坏，而其中存储的内容和MBR一样，之后才是GPT头。

GPT优点支持2T以上磁盘，如果使用Fdisk分区，最大只能建立2TB大小的分区，创建大于2TB的分区，需使用parted，同时必须使用64位操作系统，Mac、Linux系统都能支持GPT分区格式，Windows 7/8 64bit、Windows Server 2008 64bit支持GPT。图3-2所示，为GPT硬盘分区表内容：

![](学习笔记/Attachments/1737817659814-beb0d247-d393-481e-b24c-0ff9293c379e.png)

图3-2 GPT分区表内容

### 3.1.4  GRUB

GNU项目的多操作系统启动程序（GRand Unified Bootloader，GRUB），可以支持多操作系统的引导，它允许用户可以在[计算机](http://baike.baidu.com/view/3314.htm)内同时拥有多个[操作系统](http://baike.baidu.com/view/880.htm)，并在计算机启动时选择希望运行的操作系统。

GRUB可用于选择[操作系统](http://baike.baidu.com/view/880.htm)分区上的不同[内核](http://baike.baidu.com/view/1366.htm)，也可用于向这些[内核](http://baike.baidu.com/view/1366.htm)传递启动参数。它是一个多重[操作系统](http://baike.baidu.com/view/880.htm)启动管理器。用来引导不同系统，如Windows，Linux。Linux常见的引导程序包括：LILO、GRUB、GRUB2，CentOS 7 Linux默认使用GRUB2引导程序，引导系统启动。如图3-3所示为GRUB加载引导流程：

![](学习笔记/Attachments/1737817660020-b6fd0ddd-97f0-4ffd-ad32-8f5f519a7e08.png)

图3-3 GRUB引导流程

GRUB2是基于GRUB开发成更加安全强大的多系统引导程序，最新[Linux](http://lib.csdn.net/base/linux)发行版都是使用GRUB2作为引导程序。同时GRUB2采用了模块化设计，使得GRUB2核心更加精炼，使用更加灵活，同时也就不需要像GRUB分为stage1,stage1.5,stage2三个阶段。

## 3.2     Linux操作系统启动流程

初学者对Linux操作系统启动流程的理解，能有助于后期在企业中更好的维护Linux服务器，能快速定位系统问题，进而解决问题。Linux操作系统启动流程如图3-4所示：

![](学习笔记/Attachments/1737817660209-9aed7a07-4c98-45af-8992-a91f55d2de29.png)

图3-4 系统启动流程

（1）      加载BIOS

计算机电源加电质检，首先加载基本输入输出系统（Basic Input Output System，BIOS），BIOS中包含硬件CPU、内存、硬盘等相关信息，包含设备启动顺序信息、硬盘信息、内存信息、时钟信息、即插即用（Plug-and-Play，PNP）特性等。加载完BIOS信息，计算机将根据顺序进行启动。

（2）      读取MBR

读取完BIOS信息，计算机将会查找BIOS所指定的硬盘MBR引导扇区，将其内容复制到0x7c00地址所在的物理内存中。被复制到物理内存的内容是Boot Loader，然后进行引导。

（3）      GRUB引导

GRUB启动引导器是计算机启动过程中运行的第一个软件程序，当计算机读取内存中的GRUB配置信息后，会根据其配置信息来启动硬盘中不同的操作系统。

（4）      加载Kernel

计算机读取内存映像，并进行解压缩操作，屏幕一般会输出“Uncompressing Linux”的提示，当解压缩内核完成后，屏幕输出“OK, booting the kernel”。系统将解压后的内核放置在内存之中，并调用start_kernel()函数来启动一系列的初始化函数并初始化各种设备，完成Linux核心环境的建立。

（5）      设定Inittab运行等级

内核加载完毕，会启动Linux操作系统第一个守护进程init，然后通过该进程读取/etc/inittab文件，/etc/inittab文件的作用是设定Linux的运行等级，Linux常见运行级别如下：

q  0：关机模式；

q  1：单用户模式；

q  2：无网络支持的多用户模式；

q  3：字符界面多用户模式；

q  4：保留，未使用模式；

q  5：图像界面多用户模式；

q  6：重新引导系统，重启模式。

（6）      加载rc.sysinit

读取完运行级别，Linux系统执行的第一个用户层文件/etc/rc.d/rc.sysinit，该文件功能包括：设定PATH运行变量、设定网络配置、启动swap分区、设定/proc、系统函数、配置Selinux等。

（7）      加载内核模块

读取/etc/modules.conf文件及/etc/modules.d目录下的文件来加载系统内核模块。该模块文件，可以后期添加或者修改及删除。

（8）      启动运行级别程序

根据之前读取的运行级别，操作系统会运行rc0.d到rc6.d中的相应的脚本程序，来完成相应的初始化工作和启动相应的服务。其中以S开头表示系统即将启动的程序，如果以K开头，则代表停止该服务。S和K后紧跟的数字为启动顺序编号。如图3-5所示：![](学习笔记/Attachments/1737817660451-847f0e3b-dabf-4166-a6cd-9def64a316fa.png)图3-5 运行级别服务

（9）      读取rc.local文件

操作系统启动完相应服务之后，会读取执行/etc/rc.d/rc.local文件，可以将需要开机启动的任务加入到该文件末尾，系统会逐行去执行并启动相应命令，如图3-6所示：

![](学习笔记/Attachments/1737817660611-a2444127-6e72-446c-9de2-48c4be123490.png)

图3-6 开机运行加载文件

（10）  执行/bin/login程序

执行/bin/login程序，启动到系统登录界面，操作系统等待用户输入用户名和密码，即可登录到Shell终端，如图3-7所示，输入用户名、密码即可登录Linux操作系统，至此Linux操作系统完整流程启动完毕。

![](学习笔记/Attachments/1737817660831-3ce59bf6-e7b3-4418-80fd-57678bd80414.png)

图3-7 系统登录界面

## 3.3     CentOS6与CentOS7区别

CentOS6默认采用Sysvinit风格，Sysvinit就是system V风格的init系统，Sysvinit用术语runlevel 来定义"预订的运行模式"。Sysvinit 检查 '/etc/inittab' 文件中是否含有'initdefault' 项，该选项指定init的默认运行模式。Sysvinit 使用脚本，文件命名规则和软链接来实现不同的Runlevel,串行启动各个进程及服务。

CentOS7默认采用Systemd风格，Systemd是Linux系统中最新的初始化系统（init），它主要的设计目标是克服 Sysvinit 固有的缺点，提高系统的启动速度。

Systemd 和 Ubuntu 的 Upstart 是竞争对手，预计会取代 UpStart。Systemd的目标是尽可能启动更少的进程，尽可能将更多进程并行启动。如图3-8所示为CentOS6与CentOS7操作系统的区别：

![](学习笔记/Attachments/1737817660997-5d958d28-8f2d-4f12-8efd-bbf05dc966ac.png)图3-8 CentOS6与CentOS7操作系统区别

Linux操作系统文件系统类型主要由EXT3、EXT4、XFS等,其中CentOS6普遍采用EXT3和EXT4文件系统格式，而CentOS7默认采用XFS格式。如下为EXT3、EXT4、XFS区别：

q   EXT4是第四代扩展文件系统（Fourth EXtended filesystem，EXT4）是Linux系统下的日志文件系统，是EXT3文件系统的后继版本；

q   EXT3类型文件系统支持最大16TB文件系统和最大2TB文件；

q   EXT4分别支持1EB（1EB=1024PB，1PB=1024TB）的文件系统，以及16TB的单个文件；

q   EXT3只支持32,000个子目录，而EXT4支持无限数量的子目录；

q   EXT4磁盘结构的inode个数支持40亿，而且EXT4的单个文件大小支持到16T(4K block size)  ；

q   XFS是一个64位文件系统，最大支持8EB减1字节的单个文件系统，实际部署时取决于宿主操作系统的最大块限制，常用语64位操作系统，发挥更好的性能；

q   XFS一种高性能的日志文件系统，最早于1993年，由[Silicon Graphics](http://baike.baidu.com/item/Silicon%20Graphics)为他们的[IRIX](http://baike.baidu.com/item/IRIX)[操作系统](http://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)而开发，是IRIX 5.3版的默认文件系统；

q   XFS于2000年5月，[Silicon Graphics](http://baike.baidu.com/item/Silicon%20Graphics)以[GPL](http://baike.baidu.com/item/GNU%E9%80%9A%E7%94%A8%E5%85%AC%E5%85%B1%E8%AE%B8%E5%8F%AF%E8%AF%81)发布这套系统的源代码，之后被移植到Linux内核上，XFS特别擅长处理大文件，同时提供平滑的[数据传输](http://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93)。

## 3.4     CentOS7与CentOS8区别

CentOS8已于2021年9月对外发布，CentOS 完全遵守 Red Hat 的再发行政策，并且致力与上游产品在功能上完全兼容。CentOS 对组件的修改主要是去除 Red Hat 的商标及美工图。

该版本还包含全新的 CentOS Streams ，Centos Stream 是一个滚动发布的 Linux 发行版，它介于 Fedora Linux的上游开发和 RHEL 的下游开发之间而存在。你可以把 CentOS Streams 当成是用来体验最新红帽系 Linux 特性的一个版本，而无需等太久。

CentOS 8 主要改动和 RedHat Enterprise Linux 8 是一致的，基于 Fedora 28 和内核版本 4.18, 为用户提供一个稳定的、安全的、一致的基础，跨越混合云部署，支持传统和新兴的工作负载所需的工具。下表为CentOS7和CentOS8区别对比。

![](学习笔记/Attachments/1737817661202-7f58a6ca-8e45-4dcd-b95d-44dda42e51a5.png)

在CentOS7上，同时支持network.service和NetworkManager.service（简称NM）。默认情况下，这2个服务都有开启，但许多人都会将NM禁用掉。在CentOS8上，已废弃network.service，因此只能通过NM进行网络配置，包括动态ip和静态ip。换言之，在CentOS8上，必须开启NM，否则无法使用网络。

CentOS8依然支持network.service，只是默认没安装，后期可以通过dnf或者yum安装Network.service来管理网卡服务。

## 3.5     NetworkManager概念剖析

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

## 3.6     NMCLI常见命令实战

|#查看ip（类似于ifconfig、ip addr）<br><br>nmcli<br><br>#创建connection，配置静态ip（等同于配置ifcfg，其中BOOTPROTO=none，并ifup启动）<br><br>nmcli c add type ethernet con-name ethX ifname ethX ipv4.addr 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual<br><br>#创建connection，配置动态ip（等同于配置ifcfg，其中BOOTPROTO=dhcp，并ifup启动）<br><br>nmcli c add type ethernet con-name ethX ifname ethX ipv4.method auto<br><br>#修改ip（非交互式）<br><br>nmcli c modify ethX ipv4.addr '192.168.1.100/24'<br><br>nmcli c up ethX<br><br>#修改ip（交互式）<br><br>nmcli c edit ethX<br><br>nmcli> goto ipv4.addresses<br><br>nmcli ipv4.addresses> change<br><br>Edit 'addresses' value: 192.168.1.100/24<br><br>Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes<br><br>nmcli ipv4> save<br><br>nmcli ipv4> activate<br><br>nmcli ipv4> quit<br><br>#启用connection（相当于ifup）<br><br>nmcli c up ethX<br><br>#停止connection（相当于ifdown）<br><br>nmcli c down<br><br>#删除connection（类似于ifdown并删除ifcfg）<br><br>nmcli c delete ethX<br><br>#查看connection列表<br><br>nmcli c show<br><br>#查看connection详细信息<br><br>nmcli c show ethX<br><br>#重载所有ifcfg或route到connection（不会立即生效）<br><br>nmcli c reload<br><br>#重载指定ifcfg或route到connection（不会立即生效）<br><br>nmcli c load /etc/sysconfig/network-scripts/ifcfg-ethX<br><br>nmcli c load /etc/sysconfig/network-scripts/route-ethX<br><br>#立即生效connection，有3种方法<br><br>nmcli c up ethX<br><br>nmcli d reapply ethX<br><br>nmcli d connect ethX<br><br>#查看device列表<br><br>nmcli d<br><br>#查看所有device详细信息<br><br>nmcli d show<br><br>#查看指定device的详细信息<br><br>nmcli d show ethX<br><br>#激活网卡<br><br>nmcli d connect ethX<br><br>#关闭无线网络（NM默认启用无线网络）<br><br>nmcli r all off<br><br>#查看NM纳管状态<br><br>nmcli n<br><br>#开启NM纳管<br><br>nmcli n on<br><br>#关闭NM纳管（谨慎执行）<br><br>nmcli n off<br><br>#监听事件<br><br>nmcli m<br><br>#查看NM本身状态<br><br>nmcli<br><br>#检测NM是否在线可用<br><br>nm-online|
|---|

## 3.7     TCP/IP协议概述

要学好Linux，对网络协议也要有充分的了解和掌握，例如传输控制协议/[因特网](http://baike.baidu.com/view/1706.htm)互联协议（Transmission Control Protocol/Internet Protocol，TCP/IP），TCP/IP名为网络[通讯协议](http://baike.baidu.com/view/278358.htm)，是Internet最基本的协议、Internet国际[互联网](http://baike.baidu.com/view/6825.htm)络的基础，由[网络层](http://baike.baidu.com/view/239600.htm)的IP协议和[传输层](http://baike.baidu.com/view/239605.htm)的TCP协议组成。

TCP/IP 定义了电子设备如何连入[因特网](http://baike.baidu.com/view/1706.htm)，以及数据如何在它们之间传输的标准。协议采用了4层的层级结构，每一层都呼叫它的下一层所提供的协议来完成自己的需求。

TCP负责发现[传输](http://baike.baidu.com/view/389471.htm)的问题，一有问题就发出信号，要求重新传输，直到所有[数据安全](http://baike.baidu.com/view/2308446.htm)正确地传输到目的地，而IP是给[因特网](http://baike.baidu.com/view/1706.htm)的每台联网设备规定一个地址。

基于TCP/IP的参考模型将协议分成四个层次，它们分别是网络接口层、网际互连层（IP层）、[传输层](http://baike.baidu.com/view/239605.htm)（TCP层）和应用层。如图3-9为TCP/IP跟OSI参考模型层次的对比：

![](学习笔记/Attachments/1737817661366-21a5ea5f-5aec-4689-adf2-05e75ba90c80.png)

图3-9 ISO7层模型与TCP/IP四层对比

     OSI模型与TCP/IP模型协议功能实现对照表，如图3-10所示：

![](学习笔记/Attachments/1737817661533-64561174-9ec2-4624-97ee-057c06b0f9e6.png)

图3-10 ISO7层模型与TCP/IP层次功能对比

## 3.8     IP地址及网络常识

互联网协议地址（Internet Protocol Address，IP），IP地址是[IP协议](http://baike.baidu.com/view/2802.htm)提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。IP地址被用来给[Internet](http://baike.baidu.com/view/11165.htm)上的每个通信设备的一个编号，每台联网的PC上都需要有IP地址，这样才能正常通信。

IP地址是一个32位的二进制数，通常被分割为4个“8位[二进制](http://baike.baidu.com/view/18536.htm)数”（即4个字节）。IP地址通常用“[点分十进制](http://baike.baidu.com/view/828066.htm)”表示成（a.b.c.d）的形式，其中，a,b,c,d都是0~255之间的十进制整数。

常见的IP地址，分为[IPv4](http://baike.baidu.com/view/21992.htm)与[IPv6](http://baike.baidu.com/view/5228.htm)两大类。IP地址编址方案将IP地址空间划分为A、B、C、D、E五类，其中A、B、C是基本类，D、E类作为多播和保留使用。

IPV4有4段数字，每一段最大不超过255。由于互联网的蓬勃发展，IP位址的需求量愈来愈大，使得IP位址的发放愈趋严格，各项资料显示全球IPv4位址在2011年已经全部分发完毕。

地址空间的不足必将妨碍互联网的进一步发展。为了扩大[地址空间](http://baike.baidu.com/view/1507129.htm)，拟通过IPv6重新定义地址空间。IPv6采用128位地址长度。在IPv6的设计过程中除了一劳永逸地解决了地址短缺问题以外，IPV6的诞生可以给全球每一粒沙子配置一个IP地址，还考虑了在IPv4中解决不好的其它问题，如图3-11所示：

![](学习笔记/Attachments/1737817661747-81487434-c6cb-4535-9685-5759b597c38b.png)

图3-11 IPV4与IPV6地址

### **3.5.1**                  **IP****地址分类**

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

### **3.5.2**                  **子网掩码**

子网掩码(Subnet Mask)又名[网络掩码](http://baike.baidu.com/view/1169777.htm)、[地址掩码](http://baike.baidu.com/view/4040643.htm)，它是一种用来指明一个[IP地址](http://baike.baidu.com/view/3930.htm)的哪些位标识的是[主机](http://baike.baidu.com/view/23880.htm)所在的子网，以及哪些位标识的是主机的位掩码。

通常的讲，子网掩码不能单独存在，它必须结合IP地址一起使用。子网掩码只有一个作用，就是将某个IP地址划分成[网络地址](http://baike.baidu.com/view/547479.htm)和[主机地址](http://baike.baidu.com/view/547482.htm)两部分。

子网掩码是一个32位地址，用于屏蔽IP地址的一部分以区别网络标识和主机标识，并说明该IP地址是在局域网上，还是在远程网上。

对于A类地址，默认的子网掩码是255.0.0.0，而对于B类地址来说默认的子网掩码是255.255.0.0；对于C类地址来说默认的子网掩码是255.255.255.0。

[互联网](http://baike.baidu.com/view/6825.htm)是由各种小型[网络构成](http://baike.baidu.com/view/2039649.htm)的，每个网络上都有许多[主机](http://baike.baidu.com/view/23880.htm)，这样便构成了一个有层次的结构。IP地址在设计时就考虑到地址分配的层次特点，将每个IP地址都分割成[网络号](http://baike.baidu.com/view/2271538.htm)和[主机](http://baike.baidu.com/view/23880.htm)号两部分，以便于IP地址的[寻址](http://baike.baidu.com/view/1303626.htm)操作。

子网掩码的设定必须遵循一定的规则。与[二进制](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6)IP地址相同，子网掩码由1和0组成，且1和0分别连续。子网掩码的长度也是32位，左边是网络位，用[二进制](http://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6)数字“1”表示，1的数目等于网络位的长度；右边是主机位，用二进制数字“0”表示，0的数目等于主机位的长度。

### **3.5.3**                  **网关地址**

[网关](http://baike.baidu.com/view/807.htm)（Gateway）是一个网络连接到另一个网络的“关口”， 网关实质上是一个网络通向其他网络的IP[地址](http://baike.baidu.com/view/494802.htm)。主要用于不同网络传输数据。

例如我们电脑设备上网，如果是接入到同一个交换机，在交换机内部传输数据是不需要经过网关的，但是如果两台设备不在一个交换机网络，则需要在本机配置网关，内网服务器的数据通过网关，网关把数据转发到其他的网络的网关，直至找到对方的主机网络，然后返回数据。

### **3.5.4**                  **MAC****地址**

媒体访问控制（Media Access Control或者Medium Access Control，MAC），也即是物理地址、硬件地址，用来定义[网络设备](http://baike.baidu.com/view/1158081.htm)的位置。

在[OSI模型](http://baike.baidu.com/view/571842.htm)中，第三层[网络层](http://baike.baidu.com/view/239600.htm)负责 [IP地址](http://baike.baidu.com/view/3930.htm)，第二层数据链路层则负责 MAC地址。因此一个主机会有一个MAC地址，而每个[网络位置](http://baike.baidu.com/view/1643855.htm)会有一个专属于它的IP地址。

IP地址工作在OSI参考模型的第三层网络层。两者之间分工明确，默契合作，完成通信过程。IP地址专注于网络层，将数据包从一个网络转发到另外一个网络；而MAC地址则专注于数据链路层，将一个数据帧从一个节点传送到相同链路的另一个节点。

IP地址和MAC地址一般是成对出现。如果一台计算机要和网络中另一外计算机通信，那么这两台设备必须配置IP地址和MAC地址，而MAC地址是网卡出厂时设定的，这样配置的IP地址就和MAC地址形成了一种对应关系。

在数据通信时，IP地址负责表示计算机的网络层地址，网络层设备（如路由器）根据IP地址来进行操作；MAC地址负责表示计算机的数据链路层地址，数据链路层设备，根据MAC地址来进行操作。IP和MAC地址这种映射关系是通过[地址解析协议](http://baike.baidu.com/view/149421.htm)（Address Resolution Protocol，ARP）来实现的。

## 3.9     Linux系统配置IP

Linux操作系统安装完毕，那接下来如何让Linux操作系统能上外网呢？如下为Linux服务器配置IP的方法。

     Linux服务器网卡默认配置文件在/etc/sysconfig/network-scripts/下，命名的名称一般为:ifcfg-eth0或者ifcfg-ens32 ，例如DELL R720标配有4块千兆网卡，在系统显示的名称依次为：eth0、eth1、eth2、eth3。

修改服务器网卡IP地址命令为vi /etc/sysconfig/network-scripts/ifcfg-eth0 （注CentOS7网卡名ifcfg-ens32）。vi命令打开网卡配置文件，默认为DHCP方式，配置如下：

|DEVICE=eth0<br><br>BOOTPROTO=dhcp<br><br>HWADDR=00:0c:29:52:c7:4e<br><br>ONBOOT=yes<br><br>TYPE=Ethernet|
|---|

vi命令打开网卡配置文件，修改BOOTPROTO为DHCP方式，同时添加IPADDR、NETMASK、GATEWAY信息如下：

|DEVICE=eth0<br><br>BOOTPROTO=static<br><br>HWADDR=00:0c:29:52:c7:4e<br><br>ONBOOT=yes<br><br>TYPE=Ethernet<br><br>IPADDR=192.168.1.103<br><br>NETMASK=255.255.255.0<br><br>GATEWAY=192.168.1.1|
|---|

服务器网卡配置文件，详细参数如下：

|DEVICE=eth0   #物理设备名ONBOOT=yes   # [yes\|no]（重启网卡是否激活网卡设备）BOOTPROTO=static #[none\|static\|bootp\|dhcp]（不使用协议\|静态分配\|BOOTP协议\|DHCP协议）<br><br>TYPE=Ethernet           #网卡类型<br><br>IPADDR=192.168.1.103    #IP地址NETMASK=255.255.255.0  #子网掩码GATEWAY=192.168.1.1    #网关地址|
|---|

服务器网卡配置完毕后，重启网卡服务：/etc/init.d/network restart 即可。

然后查看ip地址，命令为：ifconfig或者ip addr show 查看当前服务器所有网卡的IP地址。

CentOS 7 Linux中，如果没有ifconfig命令，可以用ip addr list/show查看，也可以安装ifconfig命令，需安装软件包net-tools，命令如图3-12所示：

|yum install net-tools -y|
|---|

![](学习笔记/Attachments/1737817661902-2cf6b853-bed5-4d6e-80da-312797d7ddd1.png)

图3-12 YUM安装net-tools工具

## 3.10  Linux系统配置DNS

如上网卡IP地址配置完毕，如果服务器需上外网，还需配置域名解析地址（Domain Name System，DNS），DNS主要用于将请求的域名转换为IP地址，DNS地址配置方法如下：

修改vi  /etc/resolv.conf 文件，在文件中加入如下两条:

|nameserver 202.106.0.20<br><br>nameserver 8.8.8.8|
|---|

如上分别表示主DNS于备DNS，DNS配置完毕后，无需重启网络服务,DNS是立即生效。

可以ping  -c  6  [www.baidu.com](http://www.baidu.com/) 查看返回结果，如果有IP返回，则表示服务器DNS配置正确，如图3-13所示：

![](学习笔记/Attachments/1737817662115-c2b3b60c-bfa7-4f9d-96d4-78fe5ee95b74.png)

图3-13 ping命令返回值

## 3.11  Linux网卡名称命名

CentOS7服务器，默认网卡名为ifcfg-ens32，如果我们想改成ifcfg-eth0，使用如下步骤即可：

（1）      编辑/etc/sysconfig/grub文件，命令为vi /etc/sysconfig/grub，在倒数第二行quiet后加入如下代码，并如图3-14所示：

|net.ifnames=0 biosdevname=0|
|---|

![](学习笔记/Attachments/1737817662323-aa4855b6-b0f4-4a24-93a2-2c390563fbb4.png)

图3-14 网卡配置ifnames设置

（2）      执行命令grub2-mkconfig -o /boot/grub2/grub.cfg，生成新的grub.cfg文件，如图3-15所示：

|grub2-mkconfig -o /boot/grub2/grub.cfg|
|---|

![](学习笔记/Attachments/1737817662514-aae8d23e-620b-450c-ba3b-cc189fd4ac07.png)

图3-15 生成新的grub.cnf文件

（3）      重命名网卡名称，执行命令mv ifcfg-ens32 ifcfg-eth0，修改ifcfg-eth0文件中DEVICE= ens32为DEVICE= eth0，如图3-16所示：

![](学习笔记/Attachments/1737817662732-473a07d8-fd45-49df-aeb3-b021023d4e42.png)

图3-16 重命名网卡名称

（4）      重启服务器，并验证网卡名称是否为eth0，Reboot完后，如图3-17所示：

![](学习笔记/Attachments/1737817662927-1734993a-d968-49b9-a970-ab822073200c.png)图3-17 验证网卡设备名称

## 3.12  CentOS7和8密码重置

修改CentOS7 ROOT密码非常简单，只需登录系统，执行命令passwd回车即可，但是如果忘记ROOT，无法登录系统，该如何去重置ROOT用户的密码呢？如下为重置ROOT用户的密码的方法：

（1）      Reboot重启系统，系统启动进入欢迎界面，加载内核步骤时，按e，然后选中“CentOS Linux （3.10.0-327.e17.x86_64）7 （Core）”，如图3-18所示：

![](学习笔记/Attachments/1737817663170-cac6d9e3-2d8f-499a-b471-74320bfe2c32.png)

图3-18 内核菜单选择界面

（2）      继续按e进入编辑模式，找到ro  crashkernel=auto xxx项，将ro改成rw init=/sysroot/bin/sh，如图3-19-1和2所示：

![](学习笔记/Attachments/1737817663349-43f75516-55f8-4391-8cfc-6bb6dfd04851.png)

图3-19-1 （CentOS7）内核编辑界面

![](学习笔记/Attachments/1737817663580-00d130f1-1921-4ec0-8af5-162f49e57028.png)

图3-19-2（CentOS8） 内核编辑界面

（3）      修改为后如图3-20所示：

![](学习笔记/Attachments/1737817663770-5695d746-7d58-4702-8f07-0697d05c300d.png)

图3-20 内核编辑界面

（4）      按ctrl+x按钮进入单用户模式，如图3-21所示：

![](学习笔记/Attachments/1737817663957-2439da6d-aeae-40f4-a5b9-e0c2c366e76d.png)

图3-21 进入系统单用户模式

（5）      执行命令chroot  /sysroot访问系统，并使用passwd修改root密码，如图3-22所示：

![](学习笔记/Attachments/1737817664173-4e7cbef3-c55d-4cc6-83c2-00d62f93133b.png)

图3-22 修改ROOT用户密码

（6）      更新系统信息，touch  /.autorelabel，执行命令touch /.autorelabel，在/目录下创建一个.autorelabel文件，如果该文件存在，系统在重启时就会对整个文件系统进行relabeling重新标记，可以理解为对文件进行底层权限的控制和标记，如果seLinux属于disabled关闭状态则不需要执行这条命令，如图3-23所示：

![](学习笔记/Attachments/1737817664452-776667c9-2ba6-49df-853b-00b42cd36118.png)

图3-23 创建autorelabel文件

## 3.13  远程管理Linux服务器

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

![](学习笔记/Attachments/1737817664694-f323a099-474a-4cd6-aa5a-2b95e8024854.png)

图3-24 SecureCRT远程Linux服务器

通过SecureCRT远程连接Linux服务器之后，会发现如图3-25所示界面，与服务器本地操作界面一样，在命令行可以执行命令，操作结果与在服务器现场操作是一样。

![](学习笔记/Attachments/1737817664899-1f1b5a16-a97f-4a62-9643-25a0b3c815de.png)

图3-25  SecureCRT远程Linux服务器

## 3.14  Linux系统目录功能

通过以上知识的学习,读者已经能够独立安装并配置Linux服务器IP并远程连接，为了进一步学习Linux，需熟练掌握Linux系统各个目录的功能。

Linux主要树结构目录包括：/、/root、/home、/usr、/bin、/tmp、/sbin、/proc、/boot等，如图3-26所示，为典型的Linux目录结构如下：

![](学习笔记/Attachments/1737817665126-22e62704-5f02-4124-9f85-0d3a9cd6e8c4.png)

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

# 第4章  Linux必备命令集

Linux系统启动默认为字符界面，一般不会启动图形界面，所以对命令行的熟练程度能更加方便、高效的管理Linux系统。

本章向读者介绍Linux系统必备命令各项参数及功能场景，Linux常见命令包括：cd、ls、pwd、mkdir、rm、cp、mv、touch、cat、head、tail、chmod、vim等。

## 4.1     Linux命令集

初学者完成Linux系统安装以后，学习Linux操作系统必备的指令，基于Linux指令管理Linux操作系统，必备Linux指令有哪些？

n  基础命令相关一：

Cd、ls、pwd、help、man、if、for、while、case、select、read、test、ansible、iptables、firewall-cmd、salt、mv、cut、uniq、sort、wc、source、sestatus、setenforce；

n  基础命令相关二：

Date、ntpdate、crontab、rsync、ssh、scp、nohup、sh、bash、hostname、hostnamectl、source、ulimit、export、env、set、at、dir、db_load、diff、dmsetup、declare；

n  用户权限相关：

Useradd、userdel、usermod、groupadd、groupmod、groupdel、Chmod、chown、chgrp、umask、chattr、lsattr、id、who、whoami、last、su、sudo、w、chpasswd、chroot；

n  文件管理相关：

Touch、mkdir、rm、rmdi、vi、vim、cat、head、tail、less、more、find、sed、grep、awk、echo、ln、stat、file；

n  软件资源管理：

Rpm、yum、tar、unzip、zip、gzip、wget、curl、rz、sz、jar、apt-get、bzip2、service、systemctl、make、cmake、chkconfig；

n  系统资源管理：

Fdisk、mount、umount、mkfs.ext4、fsck.ext4、parted、lvm、dd、du、df、top、iftop、free、w、uptime、iostat、vmstat、iotop、ps、netstat、lsof、ss、sar；

n  网络管理相关：

Ping、ifconfig、ip addr、ifup、ifdown、nmcli、route、nslookup、traceroute、dig、tcpdump、nmap、brctl、ethtool、setup、arp、ab、iperf；

n  Linux系统开关机：

Init、reboot、shutdown、halt、poweroff、runlevel、login、logout、exit；

Linux命令可以分为内置命令和外部命令，内置命令是直接内置在shell程序之中，随系统启动而自动加载至内存，不受此盘文件影响。外部命令由相应的系统软件提供，用户需要时才从硬盘中读入内存。

# 查看内置命令：[root@node1 ~]# enable  
enable .

enable :

enable [

enable alias

enable bg

enable bind

...省略号# 禁用某个内置命令：[root@www.jfedu.net ~]# enable -n alias  
  
# 启用内置命令（默认是启动的）：[root@www.jfedu.net ~]# enable alias

## 4.2     cd命令详解

cd命令主要用于目录切换，例如：cd  /home切换至/home目录，cd /root表示切换至/root目录 ；cd ../切换至上一级目录；cd  ./切换至当前目录。

其中.和..可以理解为相对路径，例如cd  ./test表示以当前目录为参考，表示相对于当前，而cd /home/test表示完整的路径，理解为绝对路径），如图4-1所示：

![](学习笔记/Attachments/1737817665331-d11a78a0-7692-475f-a510-8b0e41fcce3c.png)

图4-1 Linux cd命令操作

## 4.3     ls命令详解

ls命令主要用于浏览目录下的文件或者文件夹，使用方法参考：ls  ./ 查看当前目录所有的文件和目录，ls  -a 查看所有的文件，包括隐藏文件,以.开头的文件，常用参数详解如下:

|-a, --all                    不隐藏任何以. 开始的项目；<br><br>-A, --almost-all              列出除. 及.. 以外的任何项目；<br><br>    --author                 与-l 同时使用时列出每个文件的作者；<br><br>-b, --escape                  以八进制溢出序列表示不可打印的字符；<br><br>    --block-size=大小     块以指定大小的字节为单位；<br><br>-B, --ignore-backups      不列出任何以"~"字符结束的项目；<br><br>-d, --directory               当遇到目录时列出目录本身而非目录内的文件；<br><br>-D, --dired                产生适合Emacs 的dired 模式使用的结果；<br><br>-f                            不进行排序，-aU 选项生效，-lst 选项失效；<br><br>-i, --inode                    显示每个文件的inode 号；<br><br>-I, --ignore=PATTERN     不显示任何符合指定shell PATTERN 的项目；<br><br>-k                           即--block-size=1K；<br><br>-l                            使用较长格式列出信息；<br><br>-n, --numeric-uid-gid     类似 -l，但列出UID 及GID 号；<br><br>-N, --literal                   输出未经处理的项目名称 (如不特别处理控制字符) ；<br><br>-r, --reverse                  排序时保留顺序；<br><br>-R, --recursive               递归显示子目录；<br><br>-s, --size                       以块数形式显示每个文件分配的尺寸；<br><br>-S                           根据文件大小排序；<br><br>-t                            根据修改时间排序；<br><br>-u                           同-lt 一起使用：按照访问时间排序并显示；<br><br>                              同-l一起使用：显示访问时间并按文件名排序；<br><br>                              其他：按照访问时间排序；<br><br>-U                           不进行排序；按照目录顺序列出项目；<br><br>-v                           在文本中进行数字(版本)的自然排序。|
|---|
|### 范例：<br><br>【-l】参数主要是可以看到文件的更详细的信息。<br><br>[root@www.jfedu.net ~]# ls -l /etc/fstab<br><br>-rw-r--r--  1  root  root  501  4月   1  2021 /etc/fstab<br><br>长格式分别显示了文件类型及权限，文件链接次数，文件所有者，文件属组，文件大小，文件修改时间，文件名|

## 4.4     pwd命令详解

pwd命令主要用于显示或者查看当前所在的目录路径，如图4-2所示：

![](学习笔记/Attachments/1737817665542-df31e27c-4edb-43fc-8543-448c728e02d7.png)

图4-2 pwd命令查看当前目录

## 4.5     mkdir命令详解

mkdir命令主要用于创建目录，用法mkdir  dirname，命令后接目录的名称，常用参数详解如下:

|用法：mkdir [选项]... 目录；若指定目录不存在则创建目录；<br><br>长选项必须使用的参数对于短选项时也是必需使用的；<br><br>-m, --mode=模式              设置权限模式(类似chmod)，而不是rwxrwxrwx 减umask；<br><br>-p, --parents               需要时创建目标目录的上层目录，但即使这些目录已存在也不当作错误处理；<br><br>-v, --verbose               每次创建新目录都显示信息；<br><br>-Z, --context=CTX         将每个创建的目录的SELinux 安全环境设置为CTX；<br><br>--help                       显示此帮助信息并退出；<br><br>--version                    显示版本信息并退出。|
|---|
|### 范例：<br><br># 【-p】递归创建目录，如果上级目录不存在，自动创建上级目录；如果目录已经，则不创建，不会提示报错：<br><br>mkdir -p /data/nginx/html<br><br># 【-d】指定创建目录的权限：<br><br>[root@www.jfedu.net ~]# mkdir  -m  700 /data/jfedu<br><br>[root@www.jfedu.net ~]# ll  /data/jfedu-d<br><br>drwx------ 2 root root 6 11月 27 11:34 /data/jfedu|

## 4.6     **rm**命令**详解**

rm 命令主要用于删除文件或者目录，用法 rm –rf  test.txt (-r表示递归，-f表示强制)，常用参数详解如下:

|用法：rm [选项]... 文件...删除 (unlink) 文件。<br><br>-f, --force                     强制删除。忽略不存在的文件，不提示确认；<br><br>-i                         在删除前需要确认；<br><br>-I                         在删除超过三个文件或者递归删除前要求确认。此选项比-i 提示内容更少，但同样可以阻止大多数错误发生；<br><br>-r, -R, --recursive          递归删除目录及其内容；<br><br>-v, --verbose              详细显示进行的步骤；<br><br>--help                    显示此帮助信息并退出；<br><br>--version                 显示版本信息并退出；<br><br>默认时，rm 不会删除目录，使用--recursive(-r 或-R)选项可删除每个给定的目录，以及其下所有的内容；<br><br>要删除第一个字符为"-"的文件 (例如"-foo")，请使用以下方法之一：<br><br>rm -- -foo<br><br>rm ./-foo|
|---|

## 4.7     **cp****命令详解**

cp 命令主要用于拷贝文件，用法,cp  old.txt  /tmp/new.txt ，常用来备份，如果拷贝目录需要加-r参数，常用参数详解如下:

|用法：cp [选项]... [-T] 源文件 目标文件<br><br>　或：cp [选项]... 源文件... 目录<br><br>　或：cp [选项]... -t 目录 源文件...<br><br>将源文件复制至目标文件，或将多个源文件复制至目标目录。<br><br>长选项必须使用的参数对于短选项时也是必需使用的。<br><br>-a, --archive                    等于-dR --preserve=all；<br><br>    --backup[=CONTROL            为每个已存在的目标文件创建备份；<br><br>-b                               类似--backup 但不接受参数；<br><br>    --copy-contents                 在递归处理是复制特殊文件内容；<br><br>-d                               等于--no-dereference --preserve=links；<br><br>-f, --force                        如果目标文件无法打开则将其移除并重试(当 -n 选项；<br><br>                                        存在时则不需再选此项)；<br><br>-i, --interactive                       覆盖前询问(使前面的 -n 选项失效)；<br><br>-H                              跟随源文件中的命令行符号链接；<br><br>-l, --link                         链接文件而不复制；<br><br>-L, --dereference                     总是跟随符号链接；<br><br>-n, --no-clobber                      不要覆盖已存在的文件(使前面的 -i 选项失效)；<br><br>-P, --no-dereference                  不跟随源文件中的符号链接；<br><br>-p                               等于--preserve=模式,所有权,时间戳；<br><br>    --preserve[=属性列表             保持指定的属性(默认：模式,所有权,时间戳)，如果；<br><br>                                        可能保持附加属性：环境、链接、xattr 等；<br><br>-c                               same as --preserve=context；<br><br>    --sno-preserve=属性列表         不保留指定的文件属性；<br><br>    --parents                        复制前在目标目录创建来源文件路径中的所有目录；<br><br>-R, -r, --recursive                     递归复制目录及其子目录内的所有内容。|
|---|
|### 范例：<br><br># 【-u】只复制源文件有更新的，否则不执行。<br><br>[root@www.jfedu.net ~]# cp fstab /tmp/<br><br>cp：是否覆盖"/tmp/fstab"？ y<br><br>[root@www.jfedu.net ~]# cp -u fstab /tmp/ #因为文件没变，所以没有执行。<br><br>[root@www.jfedu.net ~]# echo "this is update"  >> fstab<br><br>[root@www.jfedu.net ~]# cp -u fstab /tmp/ #因为源文件更新，所以执行复制动作<br><br>cp：是否覆盖"/tmp/fstab"？ y|
|# 【-d】复制链接文件，如果直接复制，不带参数，会导致软连接失效，直接创建普通文件。<br><br>[root@www.jfedu.net ~]# ln -s /etc/fstab fs<br><br>[root@www.jfedu.net ~]# cp -d fs /tmp/<br><br>[root@www.jfedu.net ~]# ll /tmp/fs<br><br>lrwxrwxrwx 1 root root 10 9月   2 14:34 /tmp/fs -> /etc/fstab|
|# 【-S】复制同名文件到目的目录时，对已存在的文件进行备份，且自定义备份文件后缀名。<br><br>[root@www.jfedu.net ~]# cp /etc/passwd /tmp/<br><br>[root@www.jfedu.net ~]# \cp -S ".`date +%F`" /etc/passwd /tmp/<br><br>[root@www.jfedu.net ~]# ll /tmp/passwd*<br><br>-rw-r--r-- 1 root root 1119 9月   2 14:37 /tmp/passwd<br><br>-rw-r--r-- 1 root root 1119 9月   2 14:36 /tmp/passwd.2021-09-02|
|【-a 】如果参数都记不住，就记住它吧，可以实现递归，复制软连接，保留文件属性。|

## 4.8     **mv****命令详解**

mv 命令主要用于重命名或者移动文件或者目录，用法, mv old.txt new.txt，常用参数详解如下:

|用法：mv [选项]... [-T] 源文件 目标文件；<br><br>或：mv [选项]... 源文件... 目录；<br><br>或：mv [选项]... -t 目录 源文件；<br><br>将源文件重命名为目标文件，或将源文件移动至指定目录。长选项必须使用的参数对于短选项时也是必需使用的。<br><br>      --backup[=CONTROL]       为每个已存在的目标文件创建备份；<br><br>-b                                  类似--backup 但不接受参数；<br><br>-f, --force                            覆盖前不询问；<br><br>-i, --interactive                            覆盖前询问；<br><br>-n, --no-clobber                          不覆盖已存在文件，如果您指定了-i、-f、-n 中的多个，仅最后一个生效；<br><br>    --strip-trailing-slashes                 去掉每个源文件参数尾部的斜线；<br><br>-S, --suffix=SUFFIX                  替换常用的备份文件后缀；<br><br>-t, --target-directory=DIRECTORY          指定目的目录；<br><br>-T, --no-target-directory                  将目标文件视作普通文件处理；<br><br>-u, --update                              只在源文件文件比目标文件新，或目标文件；<br><br>                                             不存在时才进行移动；<br><br>-v, --verbose                                详细显示进行的步骤；<br><br>--help                              显示此帮助信息并退出；<br><br>--version                                 显示版本信息并退出。|
|---|
|### 范例：<br><br>【-t】将文件或者目录移动到指定目录：<br><br>[root@www.jfedu.net ~]# cp /etc/passwd .<br><br>[root@www.jfedu.net ~]# mv -t /tmp/ passwd<br><br>[root@www.jfedu.net ~]# ll /tmp/passwd<br><br>-rw-r--r-- 1 root root 1119 9月   2 14:47 /tmp/passwd|
|【-b】移动文件前，对已存在的文件进行备份：<br><br>[root@www.jfedu.net ~]# mv -b passwd  /tmp/<br><br>mv：是否覆盖"/tmp/passwd"？ y<br><br>[root@www.jfedu.net ~]# ll /tmp/passwd*<br><br>-rw-r--r-- 1 root root 1119 9月   2 14:47 /tmp/passwd<br><br>-rw-r--r-- 1 root root 1119 9月   2 14:37 /tmp/passwd~|
|【-f】 强制覆盖，不提示：<br><br>[root@www.jfedu.net ~]# cp /etc/passwd .<br><br>[root@www.jfedu.net ~]# mv -f passwd  /tmp/|

## 4.9     **touch****命令详解**

touch 命令主要用于创建普通文件，用法为touch test.txt，如果文件存在，则表示修改当前文件时间，常用参数详解如下:

|用法：touch [选项]... 文件...<br><br>将每个文件的访问时间和修改时间改为当前时间；<br><br>不存在的文件将会被创建为空文件，除非使用-c 或-h 选项；<br><br>如果文件名为"-"则特殊处理，更改与标准输出相关的文件的访问时间；<br><br>长选项必须使用的参数对于短选项时也是必需使用的；<br><br>-a                        只更改访问时间；<br><br>-c, --no-create                 不创建任何文件；<br><br>-d, --date=字符串             使用指定字符串表示时间而非当前时间；<br><br>-f                        (忽略)；<br><br>-h, --no-dereference          会影响符号链接本身，而非符号链接所指示的目的地；<br><br>                                   (当系统支持更改符号链接的所有者时，此选项才有用)；<br><br>-m                            只更改修改时间；<br><br>-r, --reference=文件            使用指定文件的时间属性而非当前时间；<br><br>-t STAMP                      使用[[CC]YY]MMDDhhmm[.ss] 格式的时间而非当前时间；<br><br>--time=WORD               使用WORD 指定的时间：access、atime、use 都等于-a；<br><br>                                   选项的效果，而modify、mtime 等于-m 选项的效果；<br><br>--help                           显示此帮助信息并退出；<br><br>--version                     显示版本信息并退出。|
|---|
|### 范例：<br><br>【-t】指定修改文件时间：<br><br>[root@www.jfedu.net ~]# touch -t 202109021412 file<br><br>[root@www.jfedu.net ~]# ll file<br><br>-rw-r--r-- 1 root root 0 9月   2 14:12 file|

## 4.10  **cat****命令详解**

cat 命令主要用于查看文件内容，用法 cat test.txt 可以查看test.txt内容，常用参数详解如下:

|用法：cat [选项]... [文件]...<br><br>将[文件]或标准输入组合输出到标准输出。<br><br>-A, --show-all                       等于-vET；<br><br>-b, --number-nonblank             对非空输出行编号；<br><br>-e                             等于-vE；<br><br>-E, --show-ends                     在每行结束处显示"$"；<br><br>-n, --number                  对输出的所有行编号；<br><br>-s, --squeeze-blank                  不输出多行空行；<br><br>-t                             与-vT 等价；<br><br>-T, --show-tabs                     将跳格字符显示为^I；<br><br>-u                             (被忽略)；<br><br>-v, --show-nonprinting              使用^ 和M- 引用，除了LFD和 TAB 之外；<br><br>--help                           显示此帮助信息并退出；<br><br>--version                      显示版本信息并退出。|
|---|

cat还有一种用法，cat …EOF…EOF，表示追加内容至/tmp/test.txt文件中，如下：

|# 在文件最后行，追加内容：<br><br>cat >>/tmp/test.txt<<EOF<br><br>My Name is JFEDU.NET<br><br>I am From Bei jing.<br><br>EOF|
|---|
|# 从文件开头用新内容覆盖原内容：<br><br>cat >/tmp/test.txt<<EOF<br><br>This is new line<br><br>EOF|

## 4.11  **zip****命令详解**

Zip主要用来解压缩文件，或者对文件进行打包操作。文件经它压缩后会另外产生具有“.zip”扩展名的压缩文件，执行完不会删除源文件。

|# 语法：<br><br>zip  选项  参数<br><br># 常用选项：<br><br>-F：尝试修复已损坏的压缩文件；<br><br>-g：将文件压缩后附加在已有的压缩文件之后，而非另行建立新的压缩文件； -h：在线帮助；<br><br>-k：使用MS-DOS兼容格式的文件名称；<br><br>-l：压缩文件时，把LF字符置换成LF+CR字符；<br><br>-m：将文件压缩并加入压缩文件后，删除原始文件，即把文件移到压缩文件中； -o：以压缩文件内拥有最新更改时间的文件为准，将压缩文件的更改时间设成和该文件相同；<br><br>-q：不显示指令执行过程；<br><br>-r：递归处理，将指定目录下的所有文件和子目录一并处理；<br><br>-S：包含系统和隐藏文件；<br><br>-t<日期时间>：把压缩文件的日期设成指定的日期；<br><br>-n：压缩效率n是一个介于1~9的数值。|
|---|
|### 范例：<br><br># 压缩单个文件：zip fstab.zip /etc/fstab|
|# 把/tmp/整个目录压缩到指定文件：<br><br>[root@www.jfedu.net ~]# zip tmp.zip /tmp/*|
|【-n】指定压缩级别：<br><br>zip -9  fstab2.zip  /etc/fstab|
|【-r】递归压缩目录中的子目录：<br><br>【-m】压缩完移除源文件，只保留压缩后的文件：<br><br>[root@www.jfedu.net ~]# zip -rm  tmp.zip /tmp/*|
|# 解压缩文件：<br><br>[root@www.jfedu.net ~]# unzip tmp.zip|

## 4.12  **gzip****命令详解**

gzip是在Linux系统中经常使用的一个对文件进行压缩和解压缩的命令。gzip不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和tar命令一起构成Linux操作系统中比较流行的压缩文件格式。

|# 语法：<br><br>gzip 选项  参数<br><br># 常用选项：<br><br>-d：解开压缩文件；<br><br>-f：强行压缩文件。<br><br>-h：在线帮助；<br><br>-l：列出压缩文件的相关信息；<br><br>-N：压缩文件时，保存原来的文件名称及时间戳记；<br><br>-q：不显示警告信息；<br><br>-r：递归处理，将指定目录下的所有文件及子目录一并处理；<br><br>-t：测试压缩文件是否正确无误；<br><br>-v：显示指令执行过程；<br><br>-V：显示版本信息；<br><br>-n： 指定压缩级别，-1或--fast表示最快压缩方法（低压缩比），-9或--best表示最慢压缩方法（高压缩比）。默认级别为6。<br><br>-c：保留原始文件，生成标准输出流（结合重定向使用）。|
|---|
|### 范例：<br><br># 压缩普通文件：<br><br>[root@www.jfedu.net ~]# dd if=/dev/zero of=jfedu bs=1M count=100 #创建一个一百兆的文件<br><br>记录了100+0 的读入<br><br>记录了100+0 的写出<br><br>104857600字节(105 MB)已复制，1.42292 秒，73.7 MB/秒<br><br>[root@www.jfedu.net ~]# gzip jfedu<br><br>[root@www.jfedu.net ~]# ll -h<br><br>总用量 100K<br><br>-rw-r--r-- 1 root root 100K 9月   2 15:38 jfedu.gz #经过压缩，100M的文件压缩为100K|
|【-r】递归压缩目录中的文件，不会压缩目录本身：<br><br>[root@www.jfedu.net ~]# gzip -r /tmp/<br><br>[root@www.jfedu.net ~]# ll /tmp/<br><br>总用量 20<br><br>-rw-r--r-- 1 root root 312 4月   1 2020 fs.gz<br><br>-rw-r--r-- 1 root root 326 9月   2 14:31 fstab.gz<br><br>...省略部分文件|
|# 【-c】压缩完文件后，保留原文件：<br><br>[root@www.jfedu.net ~]# cp /etc/fstab  .<br><br>[root@www.jfedu.net ~]# gzip -c fstab > fstab.gz<br><br>[root@www.jfedu.net ~]# ll<br><br>总用量 8<br><br>-rw-r--r-- 1 root root 501 9月   2 15:45 fstab<br><br>-rw-r--r-- 1 root root 315 9月   2 15:45 fstab.gz|
|【-d】解压文件：<br><br>[root@www.jfedu.net ~]# gzip -d fstab.gz<br><br>或者：<br><br>[root@www.jfedu.net ~]# gunzip fstab.gz<br><br>[root@www.jfedu.net ~]# ls<br><br>fstab|
|# 不解压，直接查看压缩文件内容：<br><br>[root@www.jfedu.net ~]# zcat  fstab.gz<br><br>#<br><br># /etc/fstab<br><br># Created by anaconda on Wed Apr  1 20:41:42 2020<br><br>...省略部分内容|

## 4.13  **bzip2****命令详解**

Linux常用解压缩命令之一，压缩比非常高。经过bzip2压缩的文件后缀格式为bz2。

|# 语法：<br><br>bzip2 [option]  参数<br><br># 常用选项：<br><br>-k：保留源文件<br><br>-d：解压缩<br><br>-1~9：定义压缩级别|
|---|
|### 范例：<br><br># 压缩普通文件：<br><br>[root@www.jfedu.net ~]# ll -h<br><br>总用量 100M<br><br>-rw-r--r-- 1 root root 100M 9月   2 15:55 jfedu<br><br>[root@www.jfedu.net ~]# bzip2 jfedu<br><br>[root@www.jfedu.net ~]# ll -h<br><br>总用量 4.0K<br><br>-rw-r--r-- 1 root root 113 9月   2 15:55 jfedu.bz2|
|【-d】解压缩文件：<br><br>[root@www.jfedu.net ~]# ll -h<br><br>总用量 4.0K<br><br>-rw-r--r-- 1 root root 113 9月   2 15:55 jfedu.bz2<br><br>[root@www.jfedu.net ~]# bzip2 -d jfedu.bz2<br><br>[root@www.jfedu.net ~]# ll<br><br>总用量 102400<br><br>-rw-r--r-- 1 root root 104857600 9月   2 15:55 jfedu|
|【-k】压缩完，保留原文件：<br><br>[root@www.jfedu.net ~]# bzip2 -k jfedu<br><br>[root@www.jfedu.net ~]# ll -h<br><br>总用量 101M<br><br>-rw-r--r-- 1 root root 100M 9月   2 15:55 jfedu<br><br>-rw-r--r-- 1 root root  113 9月   2 15:55 jfedu.bz2|

## 4.14  **tar****命令详解**

tar命令主要是用于创建归档文件。

|# 语法：<br><br>tar  选项  参数<br><br># 常用选项：<br><br>-c：创建新的tar包<br><br>-f：指定tar包名<br><br>-r：添加文件到归档文件，须与f结合使用，指定归档文件<br><br>-z：指定gzip压缩tar包，后缀为.tar.gz<br><br>-j：指定bzip2解压缩文件，后缀为.tar.bz2<br><br>-p：保留文件的权限和属性<br><br>--remove-files：归档后删除源文件|
|---|
|### 范例：<br><br># 创建一个归档文件：<br><br>[root@www.jfedu.net ~]# tar -cf jfedu.tar jfedu*|
|# 【-r】在归档文件中添加新文件：<br><br>[root@www.jfedu.net ~]# tar -rf jfedu.tar  fstab|
|# 【-t】列出归档文件中的文件：<br><br>[root@www.jfedu.net ~]# tar -tf jfedu.tar<br><br>jfedu<br><br>jfedu.bz2<br><br>fstab|
|# 【x】从归档文件中提取文件：<br><br>[root@www.jfedu.net ~]# tar -xf jfedu.tar|
|# 【z】创建归档文件时，直接压缩文件：<br><br>[root@www.jfedu.net ~]# tar -czf jfedu.tar.gz fstab  jfedu<br><br>[root@www.jfedu.net ~]# ll jfedu.tar.gz<br><br>-rw-r--r-- 1 root root 102216 9月   2 16:21 jfedu.tar.gz|

## 4.15  **head****命令详解**

head命令主要用于查看文件内容，通常查看文件前10行，head -10 /var/log/messages可以查看该文件前10行的内容，常用参数详解如下:

|用法：head [选项]... [文件]...<br><br>将每个指定文件的头10 行显示到标准输出。<br><br>如果指定了多于一个文件，在每一段输出前会给出文件名作为文件头。<br><br>如果不指定文件，或者文件为"-"，则从标准输入读取数据，长选项必须使用的参数对于短选项时也是必需使用的；<br><br>-q, --quiet, --silent                        不显示包含给定文件名的文件头；<br><br>-v, --verbose                       总是显示包含给定文件名的文件头；<br><br>--help                              显示此帮助信息并退出；<br><br>--version                           显示版本信息并退出；<br><br>-c,  --bytes=[-]K                   显示每个文件的前K 字节内容，如果附加"-"参数，则除了每个文件的最后K字节数据外显示剩余全部内容；<br><br>-n, --lines=[-]K                       显示每个文件的前K 行内容，如果附加"-"参数，则除了每个文件的最后K 行外显示剩余全部内容。|
|---|

## 4.16  **tail****命令详解**

tail命令主要用于查看文件内容，通常查看末尾10行，tail –fn 100 /var/log/messages可以实时查看该文件末尾100行的内容，常用参数详解如下:

|用法：tail [选项]... [文件]...<br><br>显示每个指定文件的最后10 行到标准输出。<br><br>若指定了多于一个文件，程序会在每段输出的开始添加相应文件名作为头。<br><br>如果不指定文件或文件为"-" ，则从标准输入读取数据。<br><br>长选项必须使用的参数对于短选项时也是必需使用的。<br><br>-n, --lines=K                           输出的总行数，默认为10行；<br><br>-q, --quiet, --silent                     不输出给出文件名的头；<br><br>--help                                    显示此帮助信息并退出；<br><br>--version                                 显示版本信息并退出；<br><br>-f, --follow[={name\|descriptor}]             即时输出文件变化后追加的数据；<br><br>              -f, --follow 等于--follow=descriptor<br><br>-F            即--follow=name –retry<br><br>-c, --bytes=K                                 输出最后K字节；另外，使用-c +K 从每个文件的第K字节输出。|
|---|

## 4.17  less命令详解

less命令通常用于查看大文件，可以分屏显示文件内容。

|# 语法：<br><br>less 选项  参数<br><br># 打开文件后，常用快捷键<br><br>空格    显示下一屏内容<br><br>b       显示上一屏内容<br><br>f       显示下一屏内容|
|---|

## 4.18  more命令详解

|more [-dlfpcsu] [-num] [+/pattern] [+linenum] [file ...]<br><br>more [-dlfpcsu ] [-num ] [+/ pattern] [+ linenum] [file ... ]<br><br>命令参数：<br><br>+n 从笫n行开始显示<br><br>-n 屏幕显示n行<br><br>+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示<br><br>-c 从顶部清屏，然后显示<br><br>-d 提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能<br><br>-l 忽略Ctrl+l（换页）字符<br><br>-p 通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似<br><br>-s 把连续的多个空行显示为一行<br><br>-u 把文件内容中的下画线去掉<br><br>常用操作命令：<br><br>Enter 向下n行，需要定义。默认为1行<br><br>Ctrl+F 向下滚动一屏<br><br>空格键 向下滚动一屏<br><br>Ctrl+B 返回上一屏<br><br>= 输出当前行的行号<br><br>：f 输出文件名和当前行的行号<br><br>V 调用vi编辑器<br><br>!命令 调用Shell，并执行命令<br><br>q 退出more.|
|---|

## 4.19  **chmod****命令详解**

chmod命令主要用于修改文件或者目录的权限，例如chmod o+w  test.txt，赋予test.txt其他人w写权限，常用参数详解如下:

|用法：chmod [选项]... 模式[,模式]... 文件...<br><br>　或：chmod [选项]... 八进制模式 文件...<br><br>　或：chmod [选项]... --reference=参考文件 文件，将每个文件的模式更改为指定值。<br><br>-c, --changes                            类似 --verbose，但只在有更改时才显示结果<br><br>    --no-preserve-root            不特殊对待根目录(默认)；<br><br>    --preserve-root               禁止对根目录进行递归操作；<br><br>-f, --silent, --quiet                         去除大部份的错误信息；<br><br>-R, --recursive                      以递归方式更改所有的文件及子目录；<br><br>--help                                显示此帮助信息并退出；<br><br>--version                             显示版本信息并退出；<br><br>-v, --verbose                             为处理的所有文件显示诊断信息；<br><br>--reference=参考文件               使用指定参考文件的模式，而非自行指定权限模式。|
|---|

## 4.20  **chown****命令详解**

chown命令主要用于文件或者文件夹宿主及属组的修改，命令格式例如chown –R root.root /tmp/test.txt，表示修改test.txt文件的用户和组均为root，常用参数详解如下:

|用法：chown [选项]... [所有者][:[组]] 文件...<br><br>　或：chown [选项]... --reference=参考文件 文件...<br><br>更改每个文件的所有者和/或所属组。<br><br>当使用 --referebce 参数时，将文件的所有者和所属组更改为与指定参考文件相同。<br><br>-f, --silent, --quiet 去除大部份的错误信息<br><br>--reference=参考文件               使用参考文件的所属组，而非指定值；<br><br>-R, --recursive                      递归处理所有的文件及子目录；<br><br>-v, --verbose                       为处理的所有文件显示诊断信息；<br><br>-H                                      命令行参数是一个通到目录的符号链接，则遍历符号链接；<br><br>-L                                 历每一个遇到的通到目录的符号链接；<br><br>-P                                      历任何符号链接(默认)；<br><br>--help                                  显示帮助信息并退出；<br><br>--version                             显示版本信息并退出。|
|---|

## 4.21  **echo****命令详解**

echo命令主要用于打印字符或者回显，例如输入echo ok，会显示ok， echo  ok  > test.txt 则会把ok字符覆盖test.txt内容。>表示覆盖，原内容被覆盖，>>表示追加，原内容不变。

例如echo ok >> test.txt,表示向test.txt文件追加OK字符，不覆盖原文件里的内容，常用参数详解如下:

|使用-e扩展参数选项时，与如下参数一起使用，有不同含义，例如：<br><br>\a 发出警告声<br><br>\b 删除前一个字符<br><br>\c 最后不加上换行符号；<br><br>\f 换行但光标仍旧停留在原来的位置；<br><br>\n 换行且光标移至行首；<br><br>\r 光标移至行首，但不换行；<br><br>\t 插入tab； \v 与\f相同；<br><br>\\ 插入\字符；<br><br>\033[30m 黑色字 \033[0m<br><br>\033[31m 红色字 \033[0m<br><br>\033[32m 绿色字 \033[0m<br><br>\033[33m 黄色字 \033[0m<br><br>\033[34m 蓝色字 \033[0m<br><br>\033[35m 紫色字 \033[0m<br><br>\033[36m 天蓝字 \033[0m<br><br>\033[37m 白色字 \033[0m<br><br>\033[40;37m 黑底白字 \033[0m<br><br>\033[41;37m 红底白字 \033[0m<br><br>\033[42;37m 绿底白字 \033[0m<br><br>\033[43;37m 黄底白字 \033[0m<br><br>\033[44;37m 蓝底白字 \033[0m<br><br>\033[45;37m 紫底白字 \033[0m<br><br>\033[46;37m 天蓝底白字 \033[0m<br><br>\033[47;30m 白底黑字 \033[0m|
|---|

echo颜色打印扩展，auto_lamp_v2.sh内容如下：

|echo -e "\033[36mPlease Select Install Menu follow:\033[0m"<br><br>echo -e "\033[32m1)Install Apache Server\033[1m"<br><br>echo "2)Install MySQL Server"<br><br>echo "3)Install PHP Server"<br><br>echo "4)Configuration index.php and start LAMP server"<br><br>echo -e "\033[31mUsage: { /bin/sh $0 1\|2\|3\|4\|help}\033[0m"|
|---|

执行结果如图4-3所示：

![](学习笔记/Attachments/1737817665793-616d252c-d3cc-4b32-95d3-ad87ad13b015.png)图4-3 echo –e颜色打印

## 4.22  **df****命令详解**

df命令常用于磁盘分区查询，常用命令df –h，查看磁盘分区信息，常用参数详解如下:

|用法：df [选项]... [文件]...<br><br>显示每个文件所在的文件系统的信息，默认是显示所有文件系统。<br><br>长选项必须使用的参数对于短选项时也是必需使用的。<br><br>-a, --all                  显示所有文件系统的使用情况，包括虚拟文件系统；<br><br>-B, --block-size=SIZE      使用字节大小块；<br><br>-h, --human-readable       以人们可读的形式显示大小；<br><br>-H, --si                  同-h，但是强制使用1000而不是1024；<br><br>-i, --inodes                   显示inode 信息而非块使用量；<br><br>-k                      即--block-size=1K；<br><br>-l, --local               只显示本机的文件系统；<br><br>    --no-sync                取得使用量数据前不进行同步动作(默认)；<br><br>-P, --portability            使用POSIX 兼容的输出格式；<br><br>    --sync              取得使用量数据前先进行同步动作；<br><br>-t, --type=类型          只显示指定文件系统为指定类型的信息；<br><br>-T, --print-type          显示文件系统类型；<br><br>-x, --exclude-type=类型       只显示文件系统不是指定类型信息；<br><br>--help                     显示帮助信息并退出；<br><br>--version                  显示版本信息并退出。|
|---|

## 4.23  **du****命令详解**

du命令常用于查看文件在磁盘中的使用量，常用命令du -sh，查看当前目录所有文件及文件及的大小，常用参数详解如下:

|用法：du [选项]... [文件]...<br><br>　或：du [选项]... --files0-from=F<br><br>计算每个文件的磁盘用量，目录则取总用量。<br><br>长选项必须使用的参数对于短选项时也是必需使用的。<br><br>-a, --all                        输出所有文件的磁盘用量，不仅仅是目录； --apparent-size                      显示表面用量，而并非是磁盘用量；虽然表面用量通常会小一些，但有时它会因为稀疏文件间的"洞"、内部碎片、非直接引用的块等原因而变大；<br><br>-B, --block-size=大小           使用指定字节数的块；<br><br>-b, --bytes                    等于--apparent-size --block-size=1；<br><br>-c, --total                      显示总计信息；<br><br>-H                            等于--dereference-args (-D)；<br><br>-h, --human-readable              以可读性较好的方式显示尺寸(例如：1K 234M 2G)；<br><br>    --si                       类似-h，但在计算时使用1000 为基底而非1024；<br><br>-k                            等于--block-size=1K；<br><br>-l, --count-links                    如果是硬连接，就多次计算其尺寸；<br><br>-m                          等于--block-size=1M；<br><br>-L, --dereference              找出任何符号链接指示的真正目的地；<br><br>-P, --no-dereference               不跟随任何符号链接(默认)；<br><br>-0, --null                      将每个空行视作0 字节而非换行符；<br><br>-S, --separate-dirs                 不包括子目录的占用量；<br><br>-s, --summarize              只分别计算命令列中每个参数所占的总用量；<br><br>-x, --one-file-system               跳过处于不同文件系统之上的目录；<br><br>-X, --exclude-from=文件      排除与指定文件中描述的模式相符的文件；<br><br>-D, --dereference-args             解除命令行中列出的符号连接；<br><br>    --files0-from=F          计算文件F中以NUL结尾的文件名对应占用的磁盘空间，如果F 的值是"-"，则从标准输入读入文件名。|
|---|

如上为Linux初学者必备命令，当然Linux命令还有很多，后面章节会随时学习新的命令。

## 4.24  fdisk命令详解

|fdisk: Usage:<br><br> fdisk [options] <disk>    change partition table<br><br> fdisk [options] -l <disk> list partition table(s)<br><br> fdisk -s <partition>      give partition size(s) in blocks<br><br>Options:<br><br> -b <size>                 sector size (512, 1024, 2048 or 4096)<br><br> -c                        switch off DOS-compatible mode<br><br> -h                        print help<br><br> -u <size>                 give sizes in sectors instead of cylinders<br><br> -v                        print version<br><br>d delete a partition 删除分区；<br><br>l list known partition types列出分区类型；<br><br>m print this menu 帮助信息；<br><br>n add a new partition 添加一个分区；<br><br>o create a new empty DOS partition table；<br><br>p print the partition table 列出分区表；<br><br>q quit without saving changes 不保存退出；<br><br>t change a partition's system id 改变分区类型；<br><br>w write table to disk and exit 把分区表写入硬盘并退出。|
|---|

## 4.25  mount命令详解

|mount [-Vh]<br><br>mount -a [-fFnrsvw] [-t vfstype]<br><br>mount [-fnrsvw] [-o options [,...]] device \| dir<br><br>mount [-fnrsvw] [-t vfstype] [-o options] device dir<br><br>-V：                             显示mount工具版本号；<br><br>-l：                               显示已加载的文件系统列表；<br><br>-h：                                  显示帮助信息并退出；<br><br>-v：                                   输出指令执行的详细信息；<br><br>-n：                                  加载没有写入文件/etc/mtab中的文件系统；<br><br>-r：                                   将文件系统加载为只读模式；<br><br>-a：                                   加载文件/etc/fstab中配置的所有文件系统；<br><br>-o:                                指定mount挂载扩展参数，常见扩展指令：rw、remount、loop等，其中-o相关指令如下：<br><br>-o atime：                          系统会在每次读取文档时更新文档时间；<br><br>-o noatime：                           系统会在每次读取文档时不更新文档时间；<br><br>-o defaults:                              使用预设的选项 rw,suid,dev,exec,auto,nouser等；<br><br>-o exec                         允许执行档被执行；<br><br>-o user、-o nouser：                使用者可以执行 mount/umount的动作；<br><br>-o remount：                           将已挂载的系统分区重新以其他再次模式挂载；<br><br>-o ro：                               只读模式挂载；<br><br>-o rw：                                   可读可写模式挂载；<br><br>-o loop                          使用loop模式，把文件当成设备挂载至系统目录。<br><br>-t：                                   指定mount挂载设备类型，常见类型nfs、ntfs-3g、vfat、iso9660等，其中-t相关指令如下：<br><br>iso9660                         光盘或光盘镜像；<br><br>msdos                              Fat16文件系统；<br><br>vfat                            Fat32文件系统；<br><br>ntfs                                 NTFS文件系统；<br><br>ntfs-3g                          识别移动硬盘格式；<br><br>smbfs                         挂载Windows文件网络共享；<br><br>nfs                              Unix/Linux文件网络共享。|
|---|

## 4.26  parted命令详解

|帮助选项：<br><br>-h, --help                    显示此求助信息；<br><br>-l, --list                    列出所有设别的[分区](http://www.linuxprobe.com/chapter-06/)信息；<br><br>-i, --interactive             在必要时，提示用户；<br><br>-s, --script                  从不提示用户；<br><br>-v, --version                 显示版本；<br><br>操作命令：<br><br>cp [FROM-DEVICE] FROM-MINOR TO-MINOR           #将文件系统复制到另一个分区<br><br>help [COMMAND]                                 #打印通用求助信息，或关于 COMMAND 的信息<br><br>mklabel 标签类型                               #创建新的磁盘标签 (分区表)<br><br>mkfs MINOR 文件系统类型                        #在 MINOR 创建类型为“文件系统类型”的文件系统<br><br>mkpart 分区类型 [文件系统类型] 起始点 终止点   #创建一个分区<br><br>mkpartfs 分区类型 文件系统类型 起始点 终止点   #创建一个带有文件系统的分区<br><br>move MINOR 起始点 终止点                       #移动编号为 MINOR 的分区<br><br>name MINOR 名称                                #将编号为 MINOR 的分区命名为“名称”<br><br>print [MINOR]                                  #打印分区表，或者分区<br><br>quit                                           #退出程序<br><br>rescue 起始点 终止点                           #挽救临近“起始点”、“终止点”的遗失的分区<br><br>resize MINOR 起始点 终止点                     #改变位于编号为 MINOR 的分区中文件系统的大小<br><br>rm MINOR                                       #删除编号为 MINOR 的分区<br><br>select 设备                                    #选择要编辑的设备<br><br>set MINOR 标志 状态                            #改变编号为 MINOR 的分区的标志。|
|---|

## 4.27  free命令详解

|free [参数]<br><br>-b 　以Byte为单位显示内存使用情况；<br><br>-k 　以KB为单位显示内存使用情况；<br><br>-m 　以MB为单位显示内存使用情况；<br><br>-g   以GB为单位显示内存使用情况；<br><br>-o 　不显示缓冲区调节列；<br><br>-s<间隔秒数> 　持续观察内存使用状况；<br><br>-t 　显示内存总和列；<br><br>-V 　显示版本信息。|
|---|

## 4.28  diff命令详解

|diff[参数][文件1或目录1][文件2或目录2]<br><br># diff命令能比较单个文件或者目录内容。如果指定比较的是文件，则只有当输入为文本文件时才有效。以逐行的方式，比较文本文件的异同处。如果指定比较的是目录的的时候，diff 命令会比较两个目录下名字相同的<br><br># 文本文件。列出不同的二进制文件、公共子目录和只在一个目录出现的文件。<br><br>-a or --text 　#diff预设只会逐行比较文本文件。<br><br>-b or --ignore-space-change 　#不检查空格字符的不同。<br><br>-B or --ignore-blank-lines 　#不检查空白行。<br><br>-c 　#显示全部内文，并标出不同之处。<br><br>-C or --context 　#与执行"-c-"指令相同。<br><br>-d or --minimal 　#使用不同的演算法，以较小的单位来做比较。<br><br>-D or ifdef 　#此参数的输出格式可用于前置处理器巨集。<br><br>-e or --ed 　#此参数的输出格式可用于ed的script文件。<br><br>-f or -forward-ed 　#输出的格式类似ed的script文件，但按照原来文件的顺序来显示不同处。<br><br>-H or --speed-large-files 　#比较大文件时，可加快速度。<br><br>-l or --ignore-matching-lines 　#若两个文件在某几行有所不同，而这几行同时都包含了选项中指定的字符 or 字符串，则不显示这两个文件的差异。<br><br>-i or --ignore-case 　#不检查大小写的不同。<br><br>-l or --paginate 　#将结果交由pr程序来分页。<br><br>-n or --rcs 　#将比较结果以RCS的格式来显示。<br><br>-N or --new-file 　#在比较目录时，若文件A仅出现在某个目录中，预设会显示：Only in目录：文件A若使用-N参数，则diff会将文件A与一个空白的文件比较。<br><br>-p 　#若比较的文件为C语言的程序码文件时，显示差异所在的函数名称。<br><br>-P or --unidirectional-new-file 　#与-N类似，但只有当第二个目录包含了一个第一个目录所没有的文件时，才会将这个文件与空白的文件做比较。<br><br>-q or --brief 　#仅显示有无差异，不显示详细的信息。<br><br>-r or --recursive 　#比较子目录中的文件。<br><br>-s or --report-identical-files 　#若没有发现任何差异，仍然显示信息。<br><br>-S or --starting-file 　#在比较目录时，从指定的文件开始比较。<br><br>-t or --expand-tabs 　#在输出时，将tab字符展开。<br><br>-T or --initial-tab 　#在每行前面加上tab字符以便对齐。<br><br>-u,-U or --unified= 　#以合并的方式来显示文件内容的不同。<br><br>-v or --version 　#显示版本信息。<br><br>-w or --ignore-all-space 　#忽略全部的空格字符。<br><br>-W or --width 　#在使用-y参数时，指定栏宽。<br><br>-x or --exclude 　#不比较选项中所指定的文件 or 目录。<br><br>-X or --exclude-from 　#您可以将文件 or 目录类型存成文本文件，然后在=中指定此文本文件。<br><br>-y or --side-by-side 　#以并列的方式显示文件的异同之处。|
|---|

## 4.29  ping命令详解

|ping [参数] [主机名或IP地址]<br><br>-d 使用Socket的SO_DEBUG功能。<br><br>-f  极限检测。大量且快速地送网络封包给一台机器，看它的回应。<br><br>-n 只输出数值。<br><br>-q 不显示任何传送封包的信息，只显示最后的结果。<br><br>-r 忽略普通的Routing Table，直接将数据包送到远端主机上。通常是查看本机的网络接口是否有问题。<br><br>-R 记录路由过程。<br><br>-v 详细显示指令的执行过程。<br><br><p>-c 数目：在发送指定数目的包后停止。<br><br>-i 秒数：设定间隔几秒送一个网络封包给一台机器，预设值是一秒送一次。<br><br>-I 网络界面：使用指定的网络界面送出数据包。<br><br>-l 前置载入：设置在送出要求信息之前，先行发出的数据包。<br><br>-p 范本样式：设置填满数据包的范本样式。<br><br>-s 字节数：指定发送的数据字节数，预设值是56，加上8字节的ICMP头，一共是64ICMP数据字节。<br><br>-t 存活数值：设置存活数值TTL的大小。|
|---|

## 4.30  ifconfig命令详解

|ifconfig [网络设备] [参数]<br><br>up 启动指定网络设备/网卡;<br><br>down 关闭指定网络设备/网卡;<br><br>arp 设置指定网卡是否支持ARP协议;<br><br>-promisc 设置是否支持网卡的promiscuous模式，如果选择此参数，网卡将接收网络中发给它所有的数据包;<br><br>-allmulti 设置是否支持多播模式，如果选择此参数，网卡将接收网络中所有的多播数据包;<br><br>-a 显示全部接口信息;<br><br>-s 显示摘要信息（类似于 netstat -i）;<br><br>add 给指定网卡配置IPv6地址;<br><br>del 删除指定网卡的IPv6地址;<br><br><硬件地址> 配置网卡最大的传输单元;<br><br>mtu<字节数> 设置网卡的最大传输单元 (bytes);<br><br>netmask<子网掩码> 设置网卡的子网掩码。掩码可以是有前缀0x的32位十六进制数，也可以是用点分开的4个十进制数。如果不打算将网络分成子网，可以不管这一选项；如果要使用子网，那么请记住，网络中每一个系统必须有相同子网掩码。<br><br>tunel 建立隧道;<br><br>dstaddr 设定一个远端地址，建立点对点通信;<br><br>-broadcast<地址> 为指定网卡设置广播协议;<br><br>-pointtopoint<地址> 为网卡设置点对点通讯协议;<br><br>multicast 为网卡设置组播标志;<br><br>address 为网卡设置IPv4地址;<br><br>txqueuelen<长度> 为网卡设置传输列队的长度。|
|---|

## 4.31  wget命令详解

|wget [参数] [URL地址]<br><br>启动参数：<br><br>-V, –version 显示wget的版本后退出<br><br>-h, –help 打印语法帮助<br><br>-b, –background 启动后转入后台执行<br><br>-e, –execute=COMMAND 执行`.wgetrc’格式的命令，wgetrc格式参见/etc/wgetrc或~/.wgetrc<br><br>记录和输入文件参数：<br><br>-o, –output-file=FILE 把记录写到FILE文件中<br><br>-a, –append-output=FILE 把记录追加到FILE文件中<br><br>-d, –debug 打印调试输出<br><br>-q, –quiet 安静模式(没有输出)<br><br>-v, –verbose 冗长模式(这是缺省设置)<br><br>-nv, –non-verbose 关掉冗长模式，但不是安静模式<br><br>-i, –input-file=FILE 下载在FILE文件中出现的URLs<br><br>-F, –force-html 把输入文件当作HTML格式文件对待<br><br>-B, –base=URL 将URL作为在-F -i参数指定的文件中出现的相对链接的前缀<br><br>–sslcertfile=FILE 可选客户端证书<br><br>–sslcertkey=KEYFILE 可选客户端证书的KEYFILE<br><br>–egd-file=FILE 指定EGD socket的文件名<br><br>下载参数：<br><br>–bind-address=ADDRESS 指定本地使用地址(主机名或IP，当本地有多个IP或名字时使用)<br><br>-t, –tries=NUMBER 设定最大尝试链接次数(0 表示无限制).<br><br>-O –output-document=FILE 把文档写到FILE文件中<br><br>-nc, –no-clobber 不要覆盖存在的文件或使用.#前缀<br><br>-c, –continue 接着下载没下载完的文件<br><br>–progress=TYPE 设定进程条标记<br><br>-N, –timestamping 不要重新下载文件除非比本地文件新<br><br>-S, –server-response 打印服务器的回应<br><br>–spider 不下载任何东西<br><br>-T, –timeout=SECONDS 设定响应超时的秒数<br><br>-w, –wait=SECONDS 两次尝试之间间隔SECONDS秒<br><br>–waitretry=SECONDS 在重新链接之间等待1…SECONDS秒<br><br>–random-wait 在下载之间等待0…2*WAIT秒<br><br>-Y, –proxy=on/off 打开或关闭代理<br><br>-Q, –quota=NUMBER 设置下载的容量限制<br><br>–limit-rate=RATE 限定下载输率<br><br>目录参数：<br><br>-nd –no-directories 不创建目录<br><br>-x, –force-directories 强制创建目录<br><br>-nH, –no-host-directories 不创建主机目录<br><br>-P, –directory-prefix=PREFIX 将文件保存到目录 PREFIX/…<br><br>–cut-dirs=NUMBER 忽略 NUMBER层远程目录<br><br>HTTP 选项参数：<br><br>–http-user=USER 设定HTTP用户名为 USER.<br><br>–http-passwd=PASS 设定http密码为 PASS<br><br>-C, –cache=on/off 允许/不允许服务器端的数据缓存 (一般情况下允许)<br><br>-E, –html-extension 将所有text/html文档以.html扩展名保存<br><br>–ignore-length 忽略 `Content-Length’头域<br><br>–header=STRING 在headers中插入字符串 STRING<br><br>–proxy-user=USER 设定代理的用户名为 USER<br><br>–proxy-passwd=PASS 设定代理的密码为 PASS<br><br>–referer=URL 在HTTP请求中包含 `Referer: URL’头<br><br>-s, –save-headers 保存HTTP头到文件<br><br>-U, –user-agent=AGENT 设定代理的名称为 AGENT而不是 Wget/VERSION<br><br>–no-http-keep-alive 关闭 HTTP活动链接 (永远链接)<br><br>–cookies=off 不使用 cookies<br><br>–load-cookies=FILE 在开始会话前从文件 FILE中加载cookie<br><br>–save-cookies=FILE 在会话结束后将 cookies保存到 FILE文件中<br><br>FTP 选项参数：<br><br>-nr, –dont-remove-listing 不移走 `.listing’文件<br><br>-g, –glob=on/off 打开或关闭文件名的 globbing机制<br><br>–passive-ftp 使用被动传输模式 (缺省值).<br><br>–active-ftp 使用主动传输模式<br><br>–retr-symlinks 在递归的时候，将链接指向文件(而不是目录)<br><br>递归下载参数：<br><br>-r, –recursive 递归下载－－慎用!<br><br>-l, –level=NUMBER 最大递归深度 (inf 或 0 代表无穷)<br><br>–delete-after 在现在完毕后局部删除文件<br><br>-k, –convert-links 转换非相对链接为相对链接<br><br>-K, –backup-converted 在转换文件X之前，将之备份为 X.orig<br><br>-m, –mirror 等价于 -r -N -l inf -nr<br><br>-p, –page-requisites 下载显示HTML文件的所有图片<br><br>递归下载中的包含和不包含(accept/reject)：<br><br>-A, –accept=LIST 分号分隔的被接受扩展名的列表<br><br>-R, –reject=LIST 分号分隔的不被接受的扩展名的列表<br><br>-D, –domains=LIST 分号分隔的被接受域的列表<br><br>–exclude-domains=LIST 分号分隔的不被接受的域的列表<br><br>–follow-ftp 跟踪HTML文档中的FTP链接<br><br>–follow-tags=LIST 分号分隔的被跟踪的HTML标签的列表<br><br>-G, –ignore-tags=LIST 分号分隔的被忽略的HTML标签的列表<br><br>-H, –span-hosts 当递归时转到外部主机<br><br>-L, –relative 仅仅跟踪相对链接<br><br>-I, –include-directories=LIST 允许目录的列表<br><br>-X, –exclude-directories=LIST 不被包含目录的列表<br><br>-np, –no-parent 不要追溯到父目录<br><br>wget -S –spider url 不下载只显示过程|
|---|
|### 范例：<br><br># 【-P】下载文件到指定目录：<br><br>wget  -P   /usr/src/  [https://nginx.org/download/nginx-1.20.1.tar.gz](https://nginx.org/download/nginx-1.20.1.tar.gz)<br><br># 【-O】下载文件到指定目录并改名字：<br><br>wget -O /usr/src/nginx.tar.gz [https://nginx.org/download/nginx-1.20.1.tar.gz](https://nginx.org/download/nginx-1.20.1.tar.gz)<br><br># 【-b】在后台下载某文件,可以看wget-log日志看进度：<br><br>[root@www-jfedu-net ~]# wget -b [https://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-2009.iso](https://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-2009.iso)<br><br>继续在后台运行，pid 为 1677。<br><br>将把输出写入至 “wget-log”。<br><br>[root@www-jfedu-net ~]# tail wget-log<br><br>837100K .......... .......... .......... .......... .......... 18%  179K 6m17s<br><br>837150K .......... .......... .......... .......... .......... 18%  223M 6m17s<br><br>837200K .......... .......... .......... .......... .......... 18%  196K 6m18s<br><br>837250K .......... .......... .......... .......... .......... 18%  127M 6m18s<br><br>837300K .......... .......... .......... .......... .......... 18%  282M 6m18s<br><br>837350K .......... .......... .......... .......... .......... 18%  334M 6m18s<br><br>【-i】下载多个文件,，将要下载的文件地址放在某文件中：<br><br>[root@www-jfedu-net ~]# cat urlllist<br><br>[https://nginx.org/download/nginx-1.20.1.tar.gz](https://nginx.org/download/nginx-1.20.1.tar.gz)<br><br>[https://nginx.org/download/nginx-1.18.0.tar.gz](https://nginx.org/download/nginx-1.18.0.tar.gz)<br><br>[root@www-jfedu-net ~]# wget -i urlllist|

## 4.32  scp命令详解

|scp [参数] [原路径] [目标路径]<br><br>-1 强制scp命令使用协议ssh1<br><br>-2 强制scp命令使用协议ssh2<br><br>-4 强制scp命令只使用IPv4寻址<br><br>-6 强制scp命令只使用IPv6寻址<br><br>-B 使用批处理模式（传输过程中不询问传输口令或短语）<br><br>-C 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）<br><br>-p 保留原文件的修改时间，访问时间和访问权限。<br><br>-q 不显示传输进度条。<br><br>-r 递归复制整个目录。<br><br>-v 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。<br><br>-c cipher 以cipher将数据传输进行加密，这个选项将直接传递给ssh。<br><br>-F ssh_config 指定一个替代的ssh配置文件，此参数直接传递给ssh。<br><br>-i identity_file 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。<br><br>-l limit 限定用户所能使用的带宽，以Kbit/s为单位。<br><br>-o ssh_option 如果习惯于使用ssh_config(5)中的参数传递方式，<br><br>-P port 注意是大写的P, port是指定数据传输用到的端口号<br><br>-S program 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。|
|---|
|### 范例：<br><br># 将本地的文件发送给本地的临时目录：<br><br>[root@www.jfedu.net ~]# scp /etc/fstab   /tmp/|
|# 将本地的文件发送给远程服务器的临时目录：<br><br>[root@www.jfedu.net ~]# scp /etc/fstab  192.168.75.122:/tmp/<br><br>fstab                                                      100%  501    21.2KB/s   00:00|
|# 在121服务器把122服务器的文件发送给123服务器：<br><br>[root@www.jfedu.net ~]# scp  192.168.75.122:/etc/fstab 192.168.75.123:/tmp/|

## 4.33  rsync命令详解

rsync命令 是一个远程数据同步工具，通过算法来使本地和远程两个主机之间的文件达到同步，这个算法只传送两个文件的不同部分，而不是每次都整份传送，因此速度相当快。

|# 语法：<br><br>Usage: rsync [OPTION]... SRC [SRC]... DEST<br><br>  or   rsync [OPTION]... SRC [SRC]... [USER@]HOST:DEST<br><br>  or   rsync [OPTION]... SRC [SRC]... [USER@]HOST::DEST<br><br>  or   rsync [OPTION]... SRC [SRC]... rsync://[USER@]HOST[:PORT]/DEST<br><br>  or   rsync [OPTION]... [USER@]HOST:SRC [DEST]<br><br>  or   rsync [OPTION]... [USER@]HOST::SRC [DEST]<br><br>  or   rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST]<br><br># 常用选项：<br><br>-v, --verbose 详细模式输出。<br><br>-q, --quiet 精简输出模式。<br><br>-c, --checksum 打开校验开关，强制对文件传输进行校验。<br><br>-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD。<br><br>-r, --recursive 对子目录以递归模式处理。<br><br>-b, --backup 创建备份，也就是对于目的已经存在的文件名进行备份。<br><br>-l, --links 保留软链结。<br><br>-p, --perms 保持文件权限。<br><br>-o, --owner 保持文件属主信息。<br><br>-g, --group 保持文件属组信息。<br><br>-D, --devices 保持设备文件信息。<br><br>-t, --times 保持文件时间信息。<br><br>-e, --rsh=command 指定使用rsh、ssh方式进行数据同步。<br><br>--delete 删除那些DST中SRC没有的文件。<br><br>-z, --compress 对备份的文件在传输时进行压缩处理。|
|---|
|### 范例：<br><br># 将本地文件同步到本地临时目录：<br><br>[root@www.jfedu.net ~]# rsync /etc/fstab  /tmp/|
|# 【-a】递归同步，并且保留文件时间，权限等属性：<br><br>[root@www.jfedu.net ~]# rsync -a  /etc/fstab  /tmp/|
|#【--delete】使目的目录文件与源目录文件保持一致：<br><br>[root@www.jfedu.net ~]# rsync -av  --delete  /usr/local/bin/  /tmp/|
|# 将本地文件同步到远程：<br><br>[root@www.jfedu.net ~]# rsync -av /etc/fstab  192.168.75.122:/tmp/|
|# 指定远程服务器端口同步，如果远程服务器修改了ssh的默认端口：<br><br>rsync -av -e 'ssh -p 2222' /etc/fstab  192.168.75.122:/tmp/|

## 4.34  vi|vim编辑器实战

VI是一个命令行界面下的文本编辑工具，最早在1976年由Bill Joy开发，当时名字叫做ex。vi支持绝大多数操作系统（最早在BSD上发布），并且功能已经十分强大

1991年Bram Moolenaar基于vi进行改进，发布了vim，加入了对GUI的支持。

随着VIM更新发展，VIM已经不是普通意义上的文本编辑器，而是被广泛的作为在文本编辑、文本处理、代码开发等用途，Linux中主流的文本编辑器包括：VI、Vim、Sublime、Emacs、Light Table、Eclipse、Gedit等。

Vim强大的编辑能力中很大部分是来自于其普通模式命令。vim的设计理念是命令的组合。

q  “5dd”5表示总共5行，删除光标所在后的5行，包含光标行；

q  “d$” $"代表行尾,删除到行尾的内容，包含光标；

q  “2yy”表示复制光标及后2行，包括光标行；

q  “%d” %代表全部或者全局,%d表示删除文本所有的内容，也即是清空文档所有的内容。

VIM是一个主流开源的编辑器，其默认执行vim命令，会显示帮助乌干达贫困的孩子，如图4-4为vim与键盘键位功能对应关系：

![](学习笔记/Attachments/1737817666042-daceb024-6e90-46c4-960c-823ca5998560.png)图4-4 vim与键盘位置对应关系

## 4.35  VIM编辑器模式

Vim编辑器模式常用有三种，分别是：

q  命令行模式；

q  文本输入模式；

q  末行模式。

vim是vi的升级版本，它是安装在Linux操作系统中的一个软件，官网为：[www.vim.org](http://www.vim.org)

在Linux Shell终端下默认执行vim命令，按Enter键后：

q  默认进入命令行模式；

q  在命令行模式按i进入文本输入模式；

q  按ESC进入命令行模式；

q  按:进入末行模式。

## 4.36  VIM编辑器必备

VIM编辑器最强大的功能，就在于内部命令及规则使用，如下为VIM编辑器最常用的语法及规则：

|q  命令行模式：可以删除、复制、粘贴、撤销，可以切换到输入模式，输入模式跳转至命令行模式：按ESC键。<br><br>yy                          复制光标所在行；<br><br>nyy                          复制n行；<br><br>3yy                          复制3行；<br><br>p,P                          粘贴；<br><br>yw                               复制光标所在的词组，不会复制标点符号；<br><br>3yw                              复制三个词组；<br><br>u                           撤消上一次； <br><br>U                           撤消当前所有；<br><br>dd                               删除整行；<br><br>ndd                     删除n行；<br><br>x                           删除一个字符；<br><br>u                           逐行撤销；<br><br>dw                               删除一个词组；     <br><br>a                             从光标所在字符后一个位置开始录入；<br><br>A                            从光标所在行的行尾开始录入；<br><br>i                              从光标所在字符前一个位置开始录入；<br><br>I                             从光标所在行的行首开始录入；<br><br>o                            跳至光标所在行的下一行行首开始录入；<br><br>O                            跳至光标所在行的上一行行首开始录入；<br><br>R                            从光标所在位置开始替换；<br><br>q  末行模式主要功能包括：查找、替换、末行保存、退出等；<br><br>:w                           保存；<br><br>:q                            退出；   <br><br>:s/x/y                        替换1行；<br><br>:wq                      保存退出；<br><br>1,5s/x/y                     替换1,5行；<br><br>:wq!                         强制保存退出；          <br><br>1,$sx/y                       从第一行到最后一行；<br><br>:q!                           强制退出；<br><br>:x                            保存；<br><br>/word                      从前往后找，正向搜索；<br><br>?word                      从后往前走，反向搜索；<br><br>:s/old/new/g                   将old替换为new，前提是光标一定要移到那一行；<br><br>:s/old/new                 将这一行中的第一次出现的old替换为new，只替换第一个；<br><br>:1,$s/old/new/g            第一行到最后一行中的old替换为new；<br><br>:1,2,3s/old/new/g              第一行第二行第三行中的old改为new；<br><br>vim +2 jfedu.txt           打开jfedu.txt文件，并将光标定位在第二行；<br><br>vim +/string jfedu.txt       打开jfedu.txt文件，并搜索关键词。|
|---|

## 4.37  本章小结

通过对本章内容的学习，读者对Linux操作系统引导有了进一步的理解，能够快速解决Linux启动过程中的故障，同时学习了CentOS6与CentOS7系统的区别，理解了TCP/IP协议及IP地址相关基础内容。

学会了Linux初学必备的16个Linux命令，能使用命令熟练的操作Linux系统，通过对VIM编辑器的深入学习，能够熟练编辑、修改系统中任意的文本及配置文件。对Linux系统的认识及操作有了更进一步的飞跃。

## 4.38  同步作业

1.      修改密码的命令默认为passwd，需要按Enter键两次，如何一条命令快速修改密码呢?

2.      企业服务器，某天发现系统访问很慢，需要查看系统内核日志，请写出查看系统内核日志的命令；

3.      如果在Linux系统/tmp/目录，快速创建1000个目录，目录名为：jfedu1、jfedu2、jfedu依次类推，不断增加；

4.      Httpd.conf配置文件中存在很多以#号开头的行，请使用vim相关指令删除#开头的行；

# 第5章  Linux用户及权限管理

Linux是一个多用户的操作系统，引入用户，可以更加方便管理Linux服务器，系统默认需要以一个用户的身份登入，而且在系统上启动进程也需要以一个用户身份去运行，用户可以限制某些进程对特定资源的权限控制。

本章向读者介绍Linux系统如何管理创建、删除、修改用户角色、用户权限配置、组权限配置及特殊权限深入剖析。

## 5.1     Linux用户及组

Linux操作系统对多用户的管理，是非常繁琐的，所以用组的概念来管理用户就变得简单，每个用户可以在一个独立的组，每个组也可以有零个用户或者多个用户。

Linux系统用户是根据用户ID来识别的，从默认ID编号从0开始，但是为了和老式系统兼容，用户ID限制在60000以下，Linux用户分总共分为四种，分别如下：

q  root用户  （ID 0）

q  预分配用户 （ID 1-200）

q  系统用户  （ID 201-999）

q  普通用户  （ID 1000以上）

Linux系统中的每个文件或者文件夹，都有一个所属用户及所属组，使用id命令可以显示当前用户的信息，使用passwd命令可以修改当前用户密码。Linux操作系统用户的特点如下：

q  每个用户拥有一个UserID，操作系统实际读取的是UID，而非用户名；

q  每个用户属于一个主组，属于一个或多个附属组，一个用户最多有31个附属组；

q  每个组拥有一个GroupID；

q  每个进程以一个用户身份运行，该用户可对进程拥有资源控制权限；

q  每个可登陆用户拥有一个指定的Shell环境。

## 5.2     Linux用户管理

Linux用户在操作系统可以进行日常管理和维护，涉及到的相关配置文件如下：

q  /etc/passwd     保存用户信息

q  /etc/shdaow     保存用户密码（以加密形式保存）

q  /etc/group      保存组信息

q  /etc/login.defs   用户属性限制,密码过期时间,密码最大长度等限制

q  /etc/default/useradd 显示或更改默认的useradd配置文件

如需创建新用户，可以使用命令useradd，执行命令useradd  jfedu1即可创建jfedu1用户，同时会创建一个同名的组jfedu1，默认该用户属于jfedu1主组。

Useradd jfedu1命令默认创建用户jfedu1，会根据如下步骤进行操作：

q  读取/etc/default/useradd，根据配置文件执行创建操作；

q  在/etc/passwd文件中添加用户信息；

q  如使用passwd命令创建密码，密码会被加密保存在/etc/shdaow中；

q  为jfedu1创建家目录：/home/jfedu1；

q  将/etc/skel中的.bash开头的文件复制至/home/jfedu1家目录；

q  创建与用户名相同的jfedu1组，jfedu1用户默认属于jfeud1同名组；

q  Jfedu1组信息保存在/etc/group配置文件中。

在使用useradd命令创建用户时，可以支持如下参数：

|用法：useradd [选项] 登录<br><br>useradd -D<br><br>useradd -D [选项]<br><br>选项：<br><br>-b, --base-dir BASE_DIR                    指定新账户的家目录；<br><br>-c, --comment COMMENT              新账户的 GECOS 字段；<br><br>-d, --home-dir HOME_DIR                新账户的主目录；<br><br>-D, --defaults                              显示或更改默认的 useradd 配置；<br><br>-e, --expiredate EXPIRE_DATE              新账户的过期日期；<br><br>-f, --inactive INACTIVE                      新账户的密码不活动期；<br><br>-g, --gid GROUP                     新账户主组的名称或ID；<br><br>-G, --groups GROUPS                 新账户的附加组列表；<br><br>-h, --help                                  显示此帮助信息并推出；<br><br>-k, --skel SKEL_DIR                    使用此目录作为骨架目录；<br><br>-K, --key KEY=VALUE                不使用 /etc/login.defs 中的默认值；<br><br>-l, --no-log-init                          不要将此用户添加到最近登录和登录失败数据库；<br><br>-m, --create-home                           创建用户的主目录；<br><br>-M, --no-create-home                       不创建用户的主目录；<br><br>-N, --no-user-group                         不创建同名的组；<br><br>-o, --non-unique                          允许使用重复的 UID 创建用户；<br><br>-p, --password  PASSWORD                加密后的新账户密码；<br><br>-r, --system                                创建一个系统账户；<br><br>-R, --root CHROOT_DIR              chroot 到的目录；<br><br>-s, --shell SHELL                            新账户的登录 shell；<br><br>-u, --uid UID                               新账户的用户 ID；<br><br>-U, --user-group                           创建与用户同名的组；<br><br>-Z, --selinux-user SEUSER                  为SELinux 用户映射使用指定 SEUSER。|
|---|

Useradd案例演示：

（1）      新建jfedu用户，并加入到jfedu1，jfedu2附属组；

|useradd -G jfedu1,jfedu2 jfedu|
|---|

（2）      新建jfedu3用户，并指定新的家目录，同时指定其登陆的SHELL；

|useradd jfedu3  -d   /tmp/  -s  /bin/bash|
|---|

（3）      新建系统用户，不允许登录系统：

|useradd  -r  -s /sbin/nologin  jfedu4|
|---|

（4）      新建jfedu5用户，并指定用户家目录的根目录：

|[root@www-jfedu-net ~]#  useradd -b /data  jfedu5<br><br>[root@www-jfedu-net ~]#  su - jfedu5<br><br>[jfedu5@www-jfedu-net ~]$ pwd<br><br>/data/jfedu5|
|---|

（5）      新建jfedu6用户，并且指定uid：

|[root@www-jfedu-net ~]#  useradd -u 2000  jfedu6|
|---|

## 5.3  Linux组管理

所有的Linux或者Windows系统都有组的概念，通过组可以更加方便的管理用户，组的概念应用于各行行业，例如企业会使用部门、职能或地理区域的分类方式来管理成员，映射在Linux系统，同样可以创建用户，并用组的概念对其管理。

Linux组有如下特点：

q  每个组有一个组ID；

q  组信息保存在/etc/group中；

q  每个用户至少拥有一个主组，同时还可以拥有31个附属组。

通过命令groupadd、groupdel、groupmod来对组进行管理，详细参数使用如下：

|groupadd用法<br><br>-f, --force                       如果组已经存在则成功退出；<br><br>                                   并且如果 GID 已经存在则取消 –g；<br><br>-g, --gid GID                  为新组使用 GID；<br><br>-h, --help                    显示此帮助信息并推出；<br><br>-K, --key KEY=VALUE         不使用 /etc/login.defs 中的默认值；<br><br>-o, --non-unique                  允许创建有重复 GID 的组；<br><br>-p, --password PASSWORD  为新组使用此加密过的密码；<br><br>-r, --system                  创建一个系统账户；<br><br>groupmod用法        <br><br>-g, --gid GID                 将组 ID 改为 GID；<br><br>-h, --help                    显示此帮助信息并推出；<br><br>-n, --new-name NEW_GROUP    改名为 NEW_GROUP；<br><br>-o, --non-unique                  允许使用重复的 GID；<br><br>-p, --password PASSWORD  将密码更改为(加密过的) PASSWORD；<br><br>groupdel用法<br><br>groupdel jfedu                 删除jfedu组；|
|---|

Groupadd案例演示：

（1）      groupadd创建jingfeng组

|groupadd jingfeng|
|---|

（2）      groupadd创建jingfeng组，并指定GID为1000；

|groupadd -g 1000 jingfeng|
|---|

（3）      groupadd创建一个system组，名为jingfeng组

|groupadd  -r  jingfeng|
|---|

Groupmod案例演示：

（4）      groupmod修改组名称，将jingfeng组名，改成jingfeng1；

|groupmod -n jingfeng1 jingfeng|
|---|

（5）      groupmod修改组GID号，将原jingfeng1组gid改成gid 1000；

|groupmod –g 1000 jingfeng1|
|---|

## 5.4     Linux用户及组案例

Useradd主要用于新建用户,而用户新建完毕，可以使用usermod来修改用户及组的属性，如下为usermod详细参数：

|用法：usermod [选项] 登录<br><br>选项：<br><br>-c, --comment 注释            GECOS 字段的新值；<br><br>-d, --home HOME_DIR               用户的新主目录；<br><br>-e, --expiredate EXPIRE_DATE        设定帐户过期的日期为 EXPIRE_DATE；<br><br>-f, --inactive INACTIVE                过期 INACTIVE 天数后，设定密码为失效状态；<br><br>-g, --gid GROUP               强制使用 GROUP 为新主组；<br><br>-G, --groups GROUPS                新的附加组列表 GROUPS；<br><br>-a, --append GROUP                 将用户追加至上边 -G 中提到的附加组中，<br><br>                                        并不从其它组中删除此用户；<br><br>-h, --help                            显示此帮助信息并推出；<br><br>-l, --login LOGIN                     新的登录名称；<br><br>-L, --lock                            锁定用户帐号；<br><br>-m, --move-home                   将家目录内容移至新位置 (仅于 -d 一起使用)；<br><br>-o, --non-unique                    允许使用重复的(非唯一的) UID；<br><br>-p, --password PASSWORD          将加密过的密码 (PASSWORD) 设为新密码；<br><br>-R, --root CHROOT_DIR         chroot 到的目录；<br><br>-s, --shell SHELL                      该用户帐号的新登录shell环境；<br><br>-u, --uid UID                         用户帐号的新UID；<br><br>-U, --unlock                         解锁用户帐号；<br><br>-Z, --selinux-user  SEUSER            用户账户的新SELinux 用户映射。|
|---|

Usermod案例演示：

（1）      将jfedu用户属组修改为jfedu1，jfedu2附属组；

|usermod -G jfedu1,jfedu2 jfedu|
|---|

（2）      将jfedu用户加入到jfedu3，jfedu4附属组，-a为添加新组，原组保留；

|usermod  –a  -G  jfedu3,jfedu4  jfedu|
|---|

（3）      修改jfedu用户，并指定新的家目录，同时指定其登陆的SHELL；

|usermod -d  /tmp/  -s  /bin/sh jfedu|
|---|

（4）      将jfedu用户名修改为jfedu1；

|usermod -l jfedu1 jfedu|
|---|

（5）      锁定jfedu1用户及解锁jfedu1用户方法；

|usermod –L  jfedu1<br><br>usermod   -U  jfedu1|
|---|

Userdel案例演示：

使用userdel可以删除指定用户及其用户的邮箱目录或者Selinux映射环境：

q  userdel   jfedu1     保留用户的家目录；

q  userdel -r jfedu1      删除用户及用户家目录，用户如果已登录系统无法删除；

q  userdel -rf jfedu1     强制删除用户及该用户家目录，不论是否已登录系统。

## 5.5     Linux权限管理

Linux权限是操作系统用来限制对资源访问的机制，权限一般分为读、写、执行。系统中每个文件都拥有特定的权限、所属用户及所属组，通过这样的机制来限制哪些用户或用户组可以对特定文件进行相应的操作。

Linux每个进程都是以某个用户身份运行，进程的权限与该用户的权限一样，用户的权限越大，则进程拥有的权限就越大。

Lnux中有的文件及文件夹都有至少权限三种权限，常见的权限如表5-1所示:

|**权限**|**对文件的影响**|**对目录的影响**|
|---|---|---|
|r（读取）|可读取文件内容|可列出目录内容|
|w（写入）|可修改文件内容|可在目录中创建删除内容|
|x（执行）|可作为命令执行|可访问目录内容|
|目录必须拥有x权限，否则无法查看其内容|   |   |

表5-1 Linux 文件及文件及权限

Linux权限授权，默认是授权给三种角色，分别是user、group、other，Linux权限与用户之间的关联如下：

q  U代表User，G代表Group，O代表Other；

q  每个文件的权限基于UGO进行设置；

q  权限三位一组（rwx），同时需授权给三种角色，UGO；

q  每个文件拥有一个所属用户和所属组，对应UGO，不属于该文件所属用户或所属组使用O来表示；

在Linux系统中，可以通过ls –l查看jfedu.net目录的详细属性，如图5-1所示：

|drwxrwxr-x    2   jfedu1 jfedu1 4096   Dec 10 01:36      jfedu.net|
|---|

![](学习笔记/Attachments/1737817666218-8007f063-89ad-41df-9215-240cac3e1b61.png)图5-1 Linux jfedu.net目录详细属性

jfedu.net目录属性参数详解如下：

q  d 表示目录，同一位置如果为-则表示普通文件；

q  rwxrwxr-x 表示三种角色的权限，每三位为一种角色，依次为u，g，o权限，如上则表示user的权限为rwx，group的权限为rwx，other的权限为r-x；

q  2表示文件夹的链接数量，可理解为该目录下子目录的数量；

q  从左到右，第一个jfedu1表示该用户名，第二个jfedu1则为组名，其他人角色默认不显示；

q  4096表示该文件夹占据的字节数；

q  Dec 10 01:36 表示文件创建或者修改的时间；

q  Jfedu.net 为目录的名，或者文件名。

## 5.6     Chown属主及属组

修改某个用户、组对文件夹的属主及属组，用命令chown实现，案例演示如下：

（1）      修改jfedu.net文件夹所属的用户为root，其中-R参数表示递归处理所有的文件及子目录。

|chown -R  root  jfedu.net|
|---|

（2）      修改jfedu.net文件夹所属的组为root。

|chown  -R  :root  jfedu.net或者chgrp  –R  root  jfedu.net|
|---|

（3）      修改jfedu.net文件夹所属的用户为root，组也为root。

|chown  -R  root:root  jfedu.net<br><br>或者：<br><br>chown  -R  root.  jfedu.net|
|---|

## 5.7     Chmod用户及组权限

修改某个用户、组对文件夹的权限，用命令chmod实现，其中以代指ugo，+、-、=代表加入、删除和等于对应权限，具体案例如下：

（1）      授予用户对jfedu.net目录拥有rwx权限

|chmod  –R  u+rwx  jfedu.net|
|---|

（2）      授予组对jfedu.net目录拥有rwx权限

|chmod  –R  g+rwx  jfedu.net|
|---|

（3）      授予用户、组、其他人对jfedu.net目录拥有rwx权限

|chmod  –R  u+rwx,g+rwx,o+rwx  jfedu.net|
|---|

（4）      撤销用户对jfedu.net目录拥有w权限

|chmod  –R  u-w  jfedu.net|
|---|

（5）      撤销用户、组、其他人对jfedu.net目录拥有x权限

|chmod  –R  u-x,g-x,o-x  jfedu.net|
|---|

（6）      授予用户、组、其他人对jfedu.net目录只有rx权限

|chmod  –R  u=rx,g=rx,o=rx  jfedu.net|
|---|

## 5.8     Chmod二进制权限

Linux权限默认使用rwx来表示，为了更简化在系统中对权限进行配置和修改，Linux权限引入二进制表示方法，如下代码：

|Linux权限可以将rwx用二进制来表示，其中有权限用1表示，没有权限用0表示；<br><br>Linux权限用二进制显示如下：<br><br>rwx=111<br><br>r-x=101<br><br>rw-=110<br><br>r--=100<br><br>依次类推，转化为十进制，对应十进制结果显示如下：<br><br>rwx=111=4+2+1=7<br><br>r-x=101=4+0+1=5<br><br>rw-=110=4+4+0=6<br><br>r--=100=4+0+0=4<br><br>得出结论，用r=4,w=2,x=1来表示权限。|
|---|

使用二进制方式来修改权限案例演示如下，其中默认jfedu.net目录权限为755：

（1）      授予用户对jfedu.net目录拥有rwx权限

|chmod  –R  755  jfedu.net|
|---|

（2）      授予组对jfedu.net目录拥有rwx权限

|chmod  –R  775  jfedu.net|
|---|

（3）      授予用户、组、其他人对jfedu.net目录拥有rwx权限

|chmod  –R  777  jfedu.net|
|---|

（4）      撤销用户对jfedu.net目录拥有w权限

|chmod  –R  555  jfedu.net|
|---|

（5）      撤销用户、组、其他人对jfedu.net目录拥有x权限

|chmod  –R  644  jfedu.net|
|---|

（6）      授予用户、组、其他人对jfedu.net目录只有rx权限

|chmod  –R  555  jfedu.net|
|---|

## 5.9     Linux特殊权限及掩码

Linux权限除了常见的rwx权限之外，还有很多特殊的权限，细心的读者会发现，为什么Linux目录默认权限755，而文件默认权限为644呢，这是因为Linux权限掩码umask导致。

     每个Linux终端都拥有一个umask属性，umask熟悉可以用来确定新建文件、目录的默认权限，默认系统权限掩码为022。在系统中每创建一个文件或者目录，文件默认权限是666，而目录权限则为777，权限对外开放比较大，所以设置了权限掩码之后，默认的文件和目录权限减去umask值才是真实的文件和目录的权限。

q  对应目录权限为：777-022=755；

q  对应文件权限为：666-022=644；

q  执行umask命令可以查看当前默认的掩码，umask -S 023可以设置默认的权限掩码。

在Linux权限中，除了普通权限外，还有如下表5-2所示，三个特殊权限：

|权限|对文件的影响|对目录的影响|
|---|---|---|
|Suid|以文件的所属用户身份执行，而非执行文件的用户|无|
|sgid|以文件所属组身份去执行|在该目录中创建任意新文件的所属组与该目录的所属组相同|
|sticky|无|对目录拥有写入权限的用户仅可以删除其拥有的文件，无法删除其他用户所拥有的文件|

表5-2 Linux三种特殊权限

Linux中设置特殊权限方法如下：

q  设置suid：          chmod u+s jfedu.net

q  设置sgid：          chmod g+s jfedu.net

q  设置sticky：   chmod o+t jfedu.net

特殊权限与设置普通权限一样，可以使用数字方式表示：

q  SUID       =   4

q  SGID       =   2

q  Sticky =   1

可以通过chmod 4755 jfedu.net对该目录授予特殊权限为s的权限，Linux系统中s权限的应用常见包括：su、passwd、sudo，如图5-2所示：

![](学习笔记/Attachments/1737817666440-8f97deae-a2cb-449a-8c66-2041959631ec.png)

图5-2 Linux特殊权限s应用

|### 范例：<br><br>【u+s】设置使文件在执行阶段具有文件所有者的权限:<br><br># 创建普通用户<br><br>[root@www-jfedu-net ~]# useradd  jfedu<br><br># 设置登录密码<br><br>[root@www-jfedu-net ~]# echo "1" \| passwd --stdin jfedu<br><br>更改用户 jfedu 的密码 。<br><br>passwd：所有的身份验证令牌已经成功更新。<br><br># 切换到普通用户：<br><br>[root@www-jfedu-net ~]# su - jfedu<br><br># 用普通用户身份执行netstat命令，可以发现，普通用户读不到进程的pid信息：<br><br>[jfedu@www-jfedu-net ~]$ netstat -nltp<br><br>(No info could be read for "-p": geteuid()=1000 but you should be root.)<br><br>Active Internet connections (only servers)<br><br>Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name   <br><br>tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      -                  <br><br>tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                  <br><br>tcp6       0      0 ::1:25                  :::*                    LISTEN      -                  <br><br>tcp6       0      0 :::22                   :::*                    LISTEN      - <br><br># 切换到管理身份，对netstat命令添加suid权限：<br><br>[root@www-jfedu-net ~]# chmod u+s /usr/bin/netstat<br><br># 再次切换到普通用户，执行netstat命令，可以发现正常读取到进程pid信息<br><br>[root@www-jfedu-net ~]# su - jfedu<br><br>上一次登录：二 9月  7 10:00:12 CST 2021pts/0 上<br><br>[jfedu@www-jfedu-net ~]$ netstat -nltp<br><br>Active Internet connections (only servers)<br><br>Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name   <br><br>tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      919/master         <br><br>tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      835/sshd           <br><br>tcp6       0      0 ::1:25                  :::*                    LISTEN      919/master         <br><br>tcp6       0      0 :::22                   :::*                    LISTEN      835/sshd|
|---|
|【g+s】任何用户在此目录下创建的文件都具有和该目录所属的组相同的组<br><br>[root@www-jfedu-net ~]# chmod g+s /data<br><br>[root@www-jfedu-net ~]# chmod o+w /data<br><br>[jfedu@www-jfedu-net ~]$ touch /data/jfedu<br><br># jfedu文件的属组是data目录的属组一致。<br><br>[jfedu@www-jfedu-net ~]$ ll /data/jfedu<br><br>-rw-rw-r-- 1 jfedu root 0 9月   7 10:14 /data/jfedu|

## 5.10  本章小结

通过对本章内容的学习，读者可以了解Linux用户和组的系统知识，同时账号Linux用户和组在系统中各种案例操作。读者可以熟练新建用户、删除用户、修改用户属性、添加组、修改组以及删除组。

## 5.11  同步作业

1.      某互联网公司职能及员工信息表，如表5-3所示，请在Linux系统中创建相关员工，并把员工加入到部门。

|**部门**|**职能**|
|---|---|
|讲师部(teacher)|jfwu,jfcai|
|市场部(market)|jfxin,jfqi|
|管理部(manage)|jfedu,jfteach|
|运维部(operater)|jfhao,jfyang|

表5-3 Linux用户和组管理

2.      批量创建1-100个用户，用户名以jfedu开头，后面紧跟1,2,3，例如jfedu1,jfedu2,jfedu3。

3.      使用useradd创建用户并通过-p参数指定密码，设定完密码需通过系统能正常验证并登陆。

4.      小王公司服务器，使用Root用户通过SecureCRT远程登陆后，如图5-3所示，发现登录终端变成bash-4.1#，是什么原因导致？以及如何修复为正常的登录SHELL环境，请写出答案。

![](学习笔记/Attachments/1737817666636-357d33aa-276d-4cea-b5aa-e7b05e9028ff.png)图5-3 SecureCRT登录Linux系统界面

# 第6章  Linux软件包企业实战

通过前几章的学习，读者掌握了Linux系统基本命令、用户及权限等知识，Linux整个体系的关键不在于系统本身，而是在于可以基于Linux系统去安装和配置企业中相关的软件、数据及应用程序，所以对软件的维护是运维工程师的重中之重。

本章向读者介绍Linux系统软件的安装、卸载、配置、维护以及如何构建企业本地YUM光盘源及HTTP本地源。

## 6.1     RPM软件包管理

Linux软件包管理大致可分为二进制包、源码包，使用的工具也各不相同。Linux常见软件包分为两种，分别是源代码包（Source Code）、二进制包（Binary Code），源代码包是没有经过编译的包，需要经过GCC、C++编译器环境编译才能运行，二进制包无需编译，可以直接安装使用。

通常而言，可以通过后缀简单区别源码包和二进制包，例如.tar.gz、.zip、.rar结尾的包通常称之为源码包，以.rpm结尾的软件包称之为二进制包。真正区分是否为源码还是二进制还得基于代码里面的文件来判断，例如包含.h、.c、.cpp、.cc等结尾的源码文件，称之为源码包，而代码代码里面存在bin可执行文件，称之为二进制包。

CentOS操作系统中有一款默认软件管理的工具，红帽包管理工具（Red Hat Package Manager，RPM）。

使用RPM工具可以对软件包实现快速安装、管理及维护。RPM管理工具适用的操作系统包括：CentOS，RedHat，Fedora，SUSE等，RPM工具常用于管理.rpm后缀结尾的软件包。

l  **RPM****的优点：**

1.      软件已经编译打包，所以传输和安装方便，让用户免除编译。

2.      在安装之前，会先检查系统的磁盘、操作系统版本等，避免错误安装。

3.      在安装好之后，软件的信息都已经记录在linux主机的数据库上，方便查询、升级和卸载。

l  **RPM****的缺点：**

1.      软件包安装的环境必须与打包时的环境一致。

2.      必须安装了软件的依赖软件。

RPM软件包命令规则详解如下：

|RPM包命名格式为：<br><br>name-version.rpm<br><br>name-version-noarch.rpm<br><br>name-version-arch.src.rpm<br><br>如下软件包格式：<br><br>epel-release-6-8.noarch.rpm<br><br>perl-Pod-Plainer-1.03-1.el6.noarch.rpm<br><br>yasm-1.2.0-4.el7.x86_64.rpm<br><br>RPM包格式解析如下：<br><br>q  name                 软件名称，例如yasm、perl-pod-Plainer；<br><br>q  version          版本号，1.2.0通用格式：“主版本号.次版本号.修正号”；<br><br>4表示是发布版本号，该RPM包是第几次编译生成的；<br><br>q  arch              适用的硬件平台，RPM支持的平台有：i386、i586、i686、x86_64、sparc、alpha等。<br><br>q  .rpm                  后缀包表示编译好的二进制包，可用rpm命令直接安装；<br><br>q  .src.rpm              源代码包，源码编译生成.rpm格式的RPM包方可使用；<br><br>q  el*                软件包发行版本，el6表示该软件包适用于RHEL 6.x/CentOS 6.x；<br><br>q  devel：              开发包；<br><br>q  noarch：            软件包可以在任何平台上安装。|
|---|

RPM工具命令详解如下：

|RPM 选项 PACKAGE_NAME<br><br>-a, --all                             查询所有已安装软件包；<br><br>-q，--query                      表示询问用户，输出信息；<br><br>-l, --list                       打印软件包的列表；<br><br>-f, --file                              FILE 查询包含 FILE 的软件包；<br><br>-i, --info                            显示软件包信息，包括名称，版本，描述；<br><br>-v, --verbose                      打印输出详细信息；<br><br>-U, --upgrade                    升级RPM软件包；<br><br>-h,--hash                         软件安装，可以打印安装进度条；<br><br>--last                            列出软件包时，以安装时间排序，最新的在上面；<br><br>-e, --erase                        卸载rpm软件包<br><br>--force                           表示强制，强制安装或者卸载；<br><br>--nodeps                       RPM包不依赖<br><br>-l, --list                              列出软件包中的文件；<br><br>--provides                          列出软件包提供的特性；<br><br>-R, --requires                     列出软件包依赖的其他软件包；<br><br>--scripts                          列出软件包自定义的小程序。|
|---|

RPM企业案例演示：

|rpm  -q       httpd nginx             检查httpd、nginx包是否安装；<br><br>rpm  -qa\|grep      httpd         检查httpd相关的软件包是否安装。<br><br>rpm  -ql       httpd                     查看httpd安装的路径；<br><br>rpm  -qi       httpd                     查看httpd安装的版本信息；<br><br>rpm  -qc       httpd                     查看httpd安装的配置文件路径；<br><br>rpm  -qd      httpd                     查看httpd安装的文档路径（帮助文档）；<br><br>rpm -qf  /usr/bin/netstat                    根据现有的文件方向查找对应的包；<br><br>rpm  -e            httpd                 卸载httpd软件，如果有依赖，可能会失败；<br><br>rpm  -e  --nodeps    httpd         忽略依赖，强制卸载httpd；<br><br>rpm  -ivh                httpd-2.4.10-el7.x86_64.rpm       安装httpd软件包；<br><br>rpm  -Uvh               httpd-2.4.10-el7.x86_64.rpm       升级httpd软件；<br><br>rpm  -ivh  --nodeps    httpd-2.4.10-el7.x86_64.rpm  不依赖其他软件包；|
|---|

## 6.2     Tar软件包管理

Linux操作系统除了使用RPM管理工具对软件包管理之外，还可以通过tar、zip、jar等工具进行源码包的管理。

### 6.2.1  Tar命令参数详解

|-A, --catenate, --concatenate         将存档与已有的存档合并<br><br>-c, --create                       建立新的存档<br><br>-d, --diff, --compare             比较存档与当前文件的不同之处<br><br>--delete                     从存档中删除<br><br>-r, --append                 附加到存档结尾<br><br>-t, --list                            列出存档中文件的目录<br><br>-u, --update                 仅将较新的文件附加到存档中<br><br>-x, --extract, --get                  解压文件<br><br>-j, --bzip2, --bunzip2            有bz2属性的软件包；<br><br>-z, --gzip, --ungzip                  有gz属性的软件包；<br><br>-b, --block-size N              指定块大小为 Nx512 字节（缺省时 N=20)；<br><br>-B, --read-full-blocks               读取时重组块；<br><br>-C, --directory DIR             指定新的目录；<br><br>--checkpoint                 读取存档时显示目录名；<br><br>-f, --file [HOSTNAME:]F        指定存档或设备，后接文件名称；<br><br>--force-local                   强制使用本地存档，即使存在克隆；<br><br>-G, --incremental              建立老 GNU 格式的备份；<br><br>-g, --listed-incremental             建立新 GNU 格式的备份；<br><br>-h, --dereference              不转储动态链接，转储动态链接指向的文件；<br><br>-i, --ignore-zeros                  忽略存档中的 0 字节块（通常意味着文件结束）；<br><br>--ignore-failed-read               在不可读文件中作 0 标记后再退出；<br><br>-k, --keep-old-files                 保存现有文件；从存档中展开时不进行覆盖；<br><br>-K, --starting-file F            从存档文件 F 开始；<br><br>-l, --one-file-system                在本地文件系统中创建存档；<br><br>-L, --tape-length N            在写入 N*1024 个字节后暂停，等待更换磁盘；<br><br>-m, --modification-time          当从一个档案中恢复文件时，不使用新的时间标签；<br><br>-M, --multi-volume                建立多卷存档,以便在几个磁盘中存放；<br><br>-O, --to-stdout                将文件展开到标准输出；<br><br>-P, --absolute-paths                不要从文件名中去除 '/'；<br><br>-v, --verbose                       详细显示处理的文件；<br><br>--version                            显示tar 程序的版本号；<br><br>--exclude                    FILE不把指定文件包含在内；<br><br>-X, --exclude-from FILE             从指定文件中读入不想包含的文件的列表。|
|---|

### 6.2.2  TAR企业案例演示

|tar      -cvf     jfedu.tar.gz          jfedu        打包jfedu文件或者目录，打包后名称jfedu.tar.gz；<br><br>tar      -tf      jfedu.tar.gz                             查看jfedu.tar.gz包中内容；<br><br>tar      -rf      jfedu.tar.gz          jfedu.txt              将jfedu.txt文件追加到jfedu.tar.gz中<br><br>tar      -xvf     jfedu.tar.gz                        解压jfedu.tar.gz程序包；<br><br>tar      -czvf   jfedu.tar.gz          jfedu        使用gzip格式打包并压缩jfedu目录；<br><br>tar      -cjvf    jfedu.tar.bz2         jfedu        使用bzip2格式打包并压缩jfedu目录；<br><br>tar      -czf    jfedu.tar.gz * -X      list.txt          使用gzip格式打包并压当前目录所有文件，排除list.txt中记录的文件；<br><br>tar      -czf    jfedu.tar.gz    *  --exclude=zabbix-3.2.4.tar.gz --exclude=nginx-1.16.0.tar.gz                  使用gzip格式打包并压当前目录所有文件及目录，排除zabbix-3.2.4.tar.gz和nginx-1.16.0.tar.gz软件包。|
|---|

### 6.2.3  TAR实现Linux系统备份

Tar命令工具除了用于日常打包、解压源码包或者压缩包之外，最大的亮点是还可以用于Linux操作系统文件及目录的备份，使用tar -g可以基于GNU 格式的增量备份，备份原理是基于检查目录或者文件的atime、mtime、ctime属性是否被修改。文件及目录时间属性详解如下：

q  文件被访问的时间(Access time,atime)；

q  文件内容被改变的时间（Modified time，mtime)；

q  文件写入、权限更改的时间（Change time，ctime)。

总结，更改文件内容mtime和ctime都会改变，但ctime可以在mtime未发生变化时被更改，例如修改文件权限，文件mtime时间不变，而ctime时间改变。TAR增量备份案例演示步骤如下：

（1）      /root目录创建jingfeng文件夹，同时在jingfeng文件夹中，新建jf1.txt，jf2.txt文件，如图6-1所示：

![](学习笔记/Attachments/1737817666819-2aa5c92d-666f-4ad8-ad64-7f342bc0b3c2.png)图6-1 创建jingfeng目录及文件

（2）      使用tar命令第一次完整备份jingfeng文件夹中的内容，-g指定快照snapshot文件，第一次没有该文件则会自动创建，如图6-2所示：

|cd /root/jingfeng/<br><br>tar  -g  /data/backup/snapshot  -czvf  /data/backup/2021jingfeng.tar.gz|
|---|

![](学习笔记/Attachments/1737817667063-6cd6271d-e0d3-4317-98bc-5c66128d6342.png)

图6-2 tar备份jingfeng目录中文件

（3）      使用tar命令第一次完整备份jingfeng文件夹中之后，会生成快照文件：/data/backup/snapshot，后期增量备份会以snapshot文件为参考，在jingfeng文件夹中再创建jf3.txt jf4.txt文件，然后通过tar命令增量备份jingfeng目录所有内容，如图6-3所示：

|cd  /root/jingfeng/<br><br>touch jf3.txt jf4.txt<br><br>tar -g /data/backup/snapshot -czvf /data/backup/2021jingfeng_add1.tar.gz *|
|---|

![](学习笔记/Attachments/1737817667428-71343c62-a7f9-46ae-b6a0-7d74f32bbf2d.png)

图6-3 tar增量备份jingfeng目录中文件

如上图6-3所示，增量备份时，需-g指定第一次完整备份的快照snapshot文件，同时增量打包的文件名不能跟第一次备份后的文件名重复，通过tar –tf可以查看打包后的文件内容。

### 6.2.4  Shell+TAR实现增量备份

企业中日常备份的数据包括/boot、/etc、/root、/data目录等，备份的策略参考：每周1-6执行增量备份，每周日执行全备份。同时在企业中备份操作系统数据均使用Shell脚本完成，此处auto_backup_system.sh脚本供参考，后面章节会系统讲解Shell脚本，脚本内容如下：

|#!/bin/bash<br><br>#Automatic Backup Linux System Files<br><br>#By Author www.jfedu.net<br><br>#Define Variables<br><br>SOURCE_DIR=(<br><br>    $*<br><br>)<br><br>TARGET_DIR=/data/backup/<br><br>YEAR=`date +%Y`<br><br>MONTH=`date +%m`<br><br>DAY=`date +%d`<br><br>WEEK=`date +%u`<br><br>FILES=system_backup.tgz<br><br>CODE=$?<br><br>if<br><br>    [ -z $SOURCE_DIR ]；then<br><br>    echo -e "Please Enter a File or Directory You Need to Backup:\n--------------------------------------------\nExample $0 /boot /etc ......"<br><br>    exit<br><br>fi<br><br>#Determine Whether the Target Directory Exists<br><br>if<br><br>    [ ! -d $TARGET_DIR/$YEAR/$MONTH/$DAY ]；then<br><br>    mkdir -p $TARGET_DIR/$YEAR/$MONTH/$DAY<br><br>    echo "This $TARGET_DIR Created Successfully !"<br><br>fi<br><br>#EXEC Full_Backup Function Command<br><br>Full_Backup()<br><br>{<br><br>if<br><br>    [ "$WEEK" -eq "7" ]；then<br><br>    rm -rf $TARGET_DIR/snapshot<br><br>    cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；tar -g $TARGET_DIR/snapshot -czvf $FILES `echo ${SOURCE_DIR[@]}`<br><br>    [ "$CODE" == "0" ]&&echo -e  "--------------------------------------------\nFull_Backup System Files Backup Successfully !"<br><br>fi<br><br>}<br><br>#Perform incremental BACKUP Function Command<br><br>Add_Backup()<br><br>{<br><br>   cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；<br><br>if<br><br>    [ -f $TARGET_DIR/$YEAR/$MONTH/$DAY/$FILES ]；then<br><br>    read -p "$FILES Already Exists, overwrite confirmation yes or no ? : " SURE<br><br>    if [ $SURE == "no" -o $SURE == "n" ]；then<br><br>    sleep 1 ；exit 0<br><br>    fi<br><br>#Add_Backup Files System<br><br>    if<br><br>        [ $WEEK -ne "7" ]；then<br><br>        cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；tar -g $TARGET_DIR/snapshot -czvf $$_$FILES `echo ${SOURCE_DIR[@]}`<br><br>        [ "$CODE" == "0" ]&&echo -e  "-----------------------------------------\nAdd_Backup System Files Backup Successfully !"<br><br>   fi<br><br>else<br><br>   if<br><br>      [ $WEEK -ne "7" ]；then<br><br>      cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；tar -g $TARGET_DIR/snapshot -czvf $FILES `echo ${SOURCE_DIR[@]}`<br><br>      [ "$CODE" == "0" ]&&echo -e  "-------------------------------------------\nAdd_Backup System Files Backup Successfully !"<br><br>   fi<br><br>fi<br><br>}<br><br>Full_Backup；Add_Backup|
|---|

## 6.3     ZIP软件包管理

ZIP也是计算机文件的压缩的算法，原名Deflate（真空），发明者为菲利普·卡兹（Phil Katz)），他于1989年1月公布了该格式的资料。ZIP通常使用后缀名“.zip”。

主流的压缩格式包括tar、rar、zip、war、gzip、bz2、iso等。从性能上比较，TAR、WAR、RAR格式较ZIP格式压缩率较高，但压缩时间远远高于ZIP，Zip命令行工具可以实现对zip属性的包进行管理，也可以将文件及文件及打包成zip格式。如下为ZIP工具打包常见参数详解：

|-f                       freshen：只更改文件；<br><br>-u                      update：只更改或新文件；<br><br>-d                      从压缩文件删除文件；<br><br>-m                     中的条目移动到zipfile（删除OS文件）；<br><br>-r                       递归到目录；<br><br>-j                       junk（不记录）目录名；<br><br>-l                       将LF转换为CR LF（-11 CR LF至LF）；<br><br>-1                      压缩更快1-9压缩更好；<br><br>-q                      安静操作，不输出执行的过程；<br><br>-v                      verbose操作/打印版本信息；<br><br>-c                       添加一行注释；<br><br>-z                       添加zipfile注释；<br><br>-o                      读取名称使zip文件与最新条目一样旧；<br><br>-x                       不包括以下名称；<br><br>-F                      修复zipfile（-FF尝试更难）；<br><br>-D                      不要添加目录条目；<br><br>-T                      测试zip文件完整性；<br><br>-X                      eXclude eXtra文件属性；<br><br>-e                      加密 - 不要压缩这些后缀；<br><br>-h2                        显示更多的帮助。|
|---|

ZIP企业案例演示：

（1）      通过zip工具打包jingfeng文件夹中所有内容，如图6-4所示：

|zip  -rv  jingfeng.zip /root/jingfeng/|
|---|

![](学习笔记/Attachments/1737817667627-f37105e2-c5fe-487d-a41c-a4387ed1cbd5.png)图6-4 zip对jingfeng目录打包备份

（2）      通过zip工具打包jingfeng文件夹中所有内容，排除部分文件，如图6-5所示：

|zip  -rv  jingfeng.zip  *  -x  jf1.txt<br><br>zip  -rv  jingfeng.zip  *  -x  jf2.txt -x jf3.txt|
|---|

![](学习笔记/Attachments/1737817667808-20ffe58e-076e-4d4f-9898-7f07979e4d53.png)图6-5 zip对jingfeng目录打包备份，排除部分文件

（3）      通过zip工具删除jingfeng.zip中jf3.txt文件，如图6-6所示

|zip jingfeng.zip -d jf3.txt|
|---|

（4）      通过unzip工具解压jingfeng.zip文件夹中所有内容，如图6-6所示：

|unzip  jingfeng.zip<br><br>unzip  jingfeng.zip  -d  /data/backup/ 可以-d指定解压后的目录|
|---|

![](学习笔记/Attachments/1737817667993-3792547b-a941-41bd-b0ac-956975fa23b7.png)

图6-6 unzip对jingfeng目录解压

## 6.4     源码包软件安装

通常使用RPM工具管理.rpm结尾的二进制包，而标准的.zip、tar结尾的源代码包则不能使用RPM工具去安装、卸载及升级，源码包安装有三个步骤，如下：

|q  ./configure       预编译，主要用于检测系统基准环境库是否满足，生成MakeFile文件；<br><br>q  make         编译，基于第一步生成的makefile文件，进行源代码的编译；<br><br>q  make install      安装，编译完毕之后，将相关的可运行文件安装至系统中；<br><br>使用make编译时，Linux操作系统必须有GCC编译器，用于编译源码。|
|---|

源码包安装通常需要./configure、make、make install三个步骤，某些特殊源码可以只有三步中的其中一个步骤，或者两个步骤。

以CentOS 7 Linux系统为基准，在其上安装Nginx源码包，企业中源码安装的详细步骤如下：

（1）      Nginx.org官网下载nginx-1.20.1.tar.gz包

|wget [http://nginx.org/download/nginx-1.20.1.tar.gz](http://nginx.org/download/nginx-1.20.1.tar.gz)|
|---|

（2）      Nginx源码包解压

|tar  -xvf nginx-1.20.1.tar.gz|
|---|

（3）      源码Configure预编译，需进入解压后的目录执行./configure指令：

|cd nginx-1.20.1<br><br># 预编译主要是用来检查系统环境是否满足安装软件包的条件，并生成Makefile文件，该文件为编译、安装、升级nginx指明了相应参数。<br><br>./configure  --help 可以查看预编译参数<br><br>--prefix       指定nginx编译安装的目录；<br><br>--user=***     指定nginx的属主<br><br>--group=***    指定nginx的属主与属组<br><br>--with-***     指定编译某模块<br><br>--without-**   指定不编译某模块<br><br>--add-module   编译第三方模块|
|---|
|# 安装依赖：<br><br>[root@www-jfedu-net nginx-1.20.1]# yum  install zlib-devel pcre-devel -y<br><br># 开始预编译：<br><br>[root@www-jfedu-net nginx-1.20.1]# ./configure  --prefix=/usr/local/nginx|

（4）      make编译：

|# 上一步预编译成功后，会产生两个文件，一个是Makefile文件，另一个是objs目录，查看Makefile内容如下：<br><br>[root@www-jfedu-net nginx-1.20.1]# cat Makefile<br><br>default:     build<br><br>clean:<br><br>     rm -rf Makefile objs<br><br>build:<br><br>     $(MAKE) -f objs/Makefile<br><br>install:<br><br>     $(MAKE) -f objs/Makefile install<br><br>modules:<br><br>     $(MAKE) -f objs/Makefile modules<br><br>upgrade:<br><br>        /usr/local/nginx/sbin/nginx -t<br><br>     kill -USR2 `cat /usr/local/nginx/logs/nginx.pid`<br><br>     sleep 1<br><br>     test -f /usr/local/nginx/logs/nginx.pid.oldbin<br><br>     kill -QUIT `cat /usr/local/nginx/logs/nginx.pid.oldbin`<br><br># 可以根据Makefile的参数，执行以下命令：<br><br>make clean : 重新预编译时，通常执行这条命令删除上次的编译文件<br><br>make build : 编译，默认参数，可省略build参数<br><br>make install : 安装<br><br>make modules : 编译模块<br><br>make  upgrade : 在线升级（不停服务升级）<br><br># 开始编译并安装：<br><br>make  && make install|
|---|

通过以上几个步骤，源码包软件安装成功，源码包在编译及安装时，可能会遇到各种错误，需要把错误解决之后，然后再进行下一步安装即可，后面章节会重点针对企业使用的软件进行案例演练。

## 6.5     YUM软件包管理

前端软件包管理器（Yellow Updater Modified，YUM）适用于CentOS、Fedora、RedHat及SUSE中的Shell命令行，主要用于管理RPM包，于RPM工具使用范围类似，YUM工具能够从指定的服务器自动下载RPM包并且安装，还可以自动处理依赖性关系。

使用RPM工具管理和安装软件时，会发现rpm包有依赖，需要逐个手动下载安装，而YUM工具的最大便利就是可以自动安装所有依赖的软件包，从而提升效率，节省时间。

### 6.5.1  YUM工作原理

学习YUM，一定要理解YUM工作原理，YUM正常运行，需要依赖两个部分，一是YUM源端，二是YUM客户端，也即用户使用端。

YUM客户端安装的所有RPM包都是来自YUM服务端，YUM源端通过HTTP或者FTP服务器发布。而YUM客户端能够从YUM源端下载依赖的RPM包是由于在YUM源端生成了RPM包的基准信息，包括RPM包版本号、配置文件、二进制信息、依赖关系等。

YUM客户端需要安装软件或者搜索软件，会查找/etc/yum.repos.d下以.repo结尾文件，CentOS Linux默认的.repo文件名为CentOS-Base.repo，该文件中配置了YUM源端的镜像地址，所以每次安装、升级RPM包，YUM客户端均会查找.repo文件。

YUM客户端如果配置了CentOS官方repo源，客户端操作系统必须能联外网，满足网络条件，才能下载软件并安装，如果没有网络，也可以构建光盘源或者内部YUM源。在只要YUM客户端时，YUM客户端安装软件，默认会把YUM源地址、Header信息、软件包、数据库信息、缓存文件存储在/var/cache/yum中，每次使用YUM工具，YUM优先通过Cache查找相关软件包，Cache中不存在，然后在访问外网YUM源。

### 6.5.2  配置YUM源（仓库）

|# 配置本地镜像仓库：<br><br>[root@www-jfedu-net ~]# mount /dev/cdrom /mnt<br><br>mount: /dev/sr0 写保护，将以只读方式挂载<br><br># 在仓库目录下创建本地仓库文件，内容如下：<br><br>[root@www-jfedu-net ~]# cat  /etc/yum.repos.d/local.repo<br><br>[local]<br><br>name=centos-$releasever-local<br><br>baseurl=file:///mnt<br><br>gpgcheck=1<br><br>gpgkey=file:///mnt/RPM-GPG-KEY-CentOS-$releasever<br><br>#  查看仓库情况：<br><br>[root@www-jfedu-net ~]# yum repolist \| grep local<br><br>local                        centos-7-local                              4,071|
|---|
|## 配置centos的base仓库，以下两个仓库任意配置一个即可，因为仓库ID不能冲突，配置多个也只有一个生效：<br><br># 安装163的yum源：<br><br>wget -O /etc/yum.repos.d/CentOS7-Base-163.repo [http://mirrors.163.com/.help/CentOS7-Base-163.repo](http://mirrors.163.com/.help/CentOS7-Base-163.repo)<br><br># 安装阿里云的yum源：<br><br>wget -O /etc/yum.repos.d/CentOS-Base.repo [http://mirrors.aliyun.com/repo/Centos-7.repo](http://mirrors.aliyun.com/repo/Centos-7.repo)|
|## 配置epel扩展仓库：<br><br># centos 7：<br><br>wget -O /etc/yum.repos.d/epel.repo [http://mirrors.aliyun.com/repo/epel-7.repo](http://mirrors.aliyun.com/repo/epel-7.repo)<br><br># centos 8：<br><br>yum install -y [https://mirrors.aliyun.com/epel/epel-release-latest-8.noarch.rpm](https://mirrors.aliyun.com/epel/epel-release-latest-8.noarch.rpm)<br><br>sed -i 's\|^#baseurl=https://download.fedoraproject.org/pub\|baseurl=https://mirrors.aliyun.com\|' /etc/yum.repos.d/epel*<br><br>sed -i 's\|^metalink\|#metalink\|' /etc/yum.repos.d/epel*|

### 6.5.3  YUM企业案例演练

由于YUM工具的使用简便、快捷、高效，在企业中得到广泛的使用，得到众多IT运维、程序人员的青睐，要能熟练使用YUM工具，需要先掌握YUM命令行参数的使用，如下为YUM命令工具的参数详解及实战步骤：

|YUM命令工具指南，YUM格式为：<br><br>YUM [command] [package] -y\|-q 其中的[options]是可选。-y安装或者卸载出现YES时，自动确认yes；-q不显示安装的过程。<br><br>yum install httpd                         安装httpd软件包；<br><br>yum search                                       YUM搜索软件包；<br><br>yum list    httpd                          显示指定程序包安装情况httpd；<br><br>yum list                                       显示所有已安装及可安装的软件包；<br><br>yum remove  httpd                           删除程序包httpd；<br><br>yum erase   httpd                            删除程序包httpd；<br><br>yum update                                      内核升级或者软件更新；<br><br>yum update  httpd                            更新httpd软件；<br><br>yum check-update                                  检查可更新的程序；<br><br>yum info    httpd                           显示安装包信息httpd；<br><br>yum provides                                   列出软件包提供哪些文件；<br><br>yum provides "*/rz"                           列出rz命令由哪个软件包提供；<br><br>yum grouplist                               查询可以用groupinstall安装的组名称；<br><br>yum groupinstall "Chinese Support"         安装中文支持；<br><br>yum groupremove "Chinese Support"           删除程序组Chinese Support；<br><br>yum deplist httpd                                   查看程序httpd依赖情况；<br><br>yum clean   packages                            清除缓存目录下的软件包；<br><br>yum clean   headers                              清除缓存目录下的headers；<br><br>yum clean   all                               清除缓存目录下的软件包及旧的headers。|
|---|

（1）      基于CentOS 7 Linux，执行命令yum install httpd -y，安装httpd服务，如图6-7所示：

![](学习笔记/Attachments/1737817668163-9fa24da6-eeda-49b1-80d0-3bd047d38908.png)图6-7 YUM 安装httpd软件

（2）      执行命令yum grouplist，检查groupinstall的软件组名，如图6-8所示：

![](学习笔记/Attachments/1737817668330-33440f25-139f-4564-915e-3b02931f6931.png)图6-8 YUM Grouplist显示组安装名称

（3）      执行命令yum groupinstall "GNOME Desktop" -y，安装Linux图像界面，如图6-9所示:

![](学习笔记/Attachments/1737817668514-358f2b7a-78d8-494a-b3af-47c05d88ec7d.png)

图6-9 GNOME Desktop图像界面安装

（4）      执行命令yum install httpd php php-devel php-mysql mariadb mariadb-server -y，安装中小企业LAMP架构环境，如图6-10所示:

![](学习笔记/Attachments/1737817668672-2eb7e330-ab1c-44ea-a8a1-6443f6a9d085.png)

图6-10 LAMP中小企业架构安装

（5）      执行命令yum  remove  ntpdate -y，卸载ntpdate软件包，如图6-11所示:

![](学习笔记/Attachments/1737817668868-c18096e1-2f24-4b8d-bcfa-da950e5f99d9.png)图6-11 卸载NTPDATE软件

（6）      执行命令yum provides rz或者yum provides "*/rz"，查找rz命令的提供者，如图6-12所示：

![](学习笔记/Attachments/1737817669047-4528eb04-2637-45b0-9b98-01bd2f1664b9.png)图6-12 查找RZ命令的提供者

（7）      执行命令yum update -y，升级Linux所有可更新的软件包或Linux内核升级，如图6-13所示：

![](学习笔记/Attachments/1737817669233-0fc657ef-84e6-4d92-a5b4-da309a41e59c.png)图6-13 软件包升级或内核升级

## 6.6     YUM优先级配置实战

基于YUM安装软件时,通常会配置多个Repo源，而Fastest mirror 插件是为拥有多个镜像的软件库配置文件而设计的。它会连接到每一个镜像，计算连接所需的时间，然后将镜像按快到慢排序供YUM应用。

默认CentOS Linux系统，Fastestmirror插件是开启的，所以安装软件会从最快的镜像源安装，但是由于Repo源很多，而在这些源中都存在某些软件包,但有些软件有重复,甚至冲突,能否可以优先从一些Repo源中去查找，如果找不到，再去其他源中找呢？

可以使用YUM优先级插件解决该问题，YUM提供的插件yum-plugin-priorities，直接YUM安装即可，命令如下：

yum install -y yum-plugin-priorities

![](学习笔记/Attachments/1737817669399-2ec5fd8c-afc5-4cd3-b1a8-373c651599aa.png)

修改YUM源优先级配置文件，设置为Enabled，开启优先级插件，1为开启，0为禁止；

vim /etc/yum/pluginconf.d/priorities.conf

enabled = 1

![](学习笔记/Attachments/1737817669575-5f8720e6-9f94-4c81-a8f4-c8528818092c.png)

vim 修改/etc/yum.repos./xx.repo文件，在base段中加入如下指令：（优先级为1表示优先被查找，越大其反而被后续查找）

priority=1

![](学习笔记/Attachments/1737817669868-9024f7eb-27f3-4c75-ab8a-771f611d005b.png)

基于YUM安装ntpdate软件，测试已经优先从163源中查找；

![](学习笔记/Attachments/1737817670078-bc5f2ef1-8dd1-4819-ac0e-a1e77b6f7fbc.png)

## 6.7     基于ISO镜像构建YUM本地源

通常而言，YUM客户端使用前提是必须联外网，YUM安装软件时，检查repo配置文件查找相应的YUM源仓库，企业IDC机房很多服务器为了安全起见，是禁止服务器上外网的，所以不能使用默认的官方YUM源仓库。

构建本地YUM光盘源，其原理是通过查找光盘中的软件包，实现YUM安装，配置步骤如下：

（1）      将CentOS-7-x86_64-DVD-1511.iso镜像加载至虚拟机CD/DVD或者放入服务器CD/DVD光驱中，并将镜像文件挂载至服务器/mnt目录，如图6-14所示，挂载命令：

|mount      /dev/cdrom    /mnt/|
|---|

![](学习笔记/Attachments/1737817670277-f18216a5-65ba-45fc-acd2-aae6be7c99db.png)

图6-14 CentOS ISO镜像文件挂载

（2）      备份/etc/yum.repos.d/CentOS-Base.repo文件为CentOS-Base.repo.bak，同时在/etc/yum.repos.d目录下创建media.repo文件，并写入如下内容：

|[yum]<br><br>name=CentOS7<br><br>baseurl=file:///mnt<br><br>enabled=1<br><br>gpgcheck=1<br><br>gpgkey=file:///mnt/RPM-GPG-KEY-CentOS-7|
|---|

Media.repo配置文件详解：

|name=CentOS7                               YUM源显示名称；<br><br>baseurl=file:///mnt                             ISO镜像挂载目录；<br><br>gpgcheck=1                                          是否检查GPG-KEY；<br><br>enabled=1                                            是否启用YUM源；<br><br>gpgkey=file:///mnt/RPM-GPG-KEY-CentOS-7  指定载目录下的GPG-KEY文件验证。|
|---|

（3）      运行命令yum clean all清空YUM Cache，执行yum install screen –y安装screen软件如图6-15所示：

![](学习笔记/Attachments/1737817670549-477bfe3c-1c03-4344-b389-1eb61872ec31.png)图6-15  YUM 安装Screen软件

（4）      至此YUM光盘源构建完毕，在使用YUM源时，会遇到部分软件无法安装，原因是因为光盘中软件包不完整导致，同时光盘源只能本机使用，其他局域网服务器无法使用。

## 6.8     基于HTTP构建YUM网络源

YUM光盘源默认只能本机使用，局域网其他服务器无法使用YUM光盘源，如果想使用的话，需要在每台服务器上构建YUM本地源，该方案在企业中不可取，所以需要构建HTTP局域网YUM源解决，可以通过CreateRepo创建本地YUM源端，repo即为Repository。

构建HTTP局域网YUM源方法及步骤如下：

（1）      挂载光盘镜像文件至/mnt

|mount    /dev/cdrom    /mnt/|
|---|

（2）      拷贝/mnt/Packages目录下所有软件包至/var/www/html/centos/

|mkdir  -p  /var/www/html/centos/<br><br>cp    -R   /mnt/Packages/*  /var/www/html/centos/|
|---|

（3）      使用Createrepo创建本地源，执行如下命令会在Centos目录生成repodata目录，目录内容如图6-16所示：

|yum  install  createrepo*  -y<br><br>cd /var/www/html<br><br>createrepo  centos/|
|---|

![](学习笔记/Attachments/1737817670776-4c8fdc7e-3514-4248-a16c-b47bb2ebb758.png)图6-16 Createrepo生成repodata目录

（4）      利用HTTP发布YUM本地源

本地YUM源通过CreateRepo搭建完毕，需要借助HTTP WEB服务器发布/var/www/html/centos/中所有软件，YUM或者RPM安装HTTP WEB服务器，并启动httpd服务。

|yum install  httpd  httpd-devel  -y 安装HTTP WEB服务；<br><br>useradd  apache  -g  apache        创建apache用户和组；<br><br>systemctl  restart  httpd.service          重启HTTPD服务；<br><br>setenforce 0                            临时关闭SeLinux应用级安全策略；<br><br>systemctl stop firewalld.service        停止防火墙；<br><br>ps -ef \|grep httpd                     查看HTTPD进程是否启动。|
|---|

（5）      在YUM客户端，创建/etc/yum.repos.d/http.repo文件，写入如下内容：

|[base]<br><br>name="CentOS7 HTTP YUM"<br><br>baseurl=http://192.168.1.115/centos/<br><br>gpgcheck=0<br><br>enabled=1<br><br>[updates]<br><br>name="CentOS7 HTTP YUM"<br><br>baseurl=http://192.168.1.115/centos<br><br>gpgcheck=0<br><br>enabled=1|
|---|

（6）      至此在YUM客户端上执行如下命令，如图6-17所示：

|yum clean all                清空YUM Cache；         <br><br>yum install ntpdate  -y       安装NTPDATE软件。|
|---|

![](学习笔记/Attachments/1737817670954-5ec2c27a-c77e-4195-a4b0-dc521f38a8c0.png)图6-17  HTTP YUM源客户端验证

## 6.9     YUM源端软件包扩展

默认使用ISO镜像文件中的软件包构建的HTTP YUM源，会发现缺少很多软件包，如果服务器需要挂载移动硬盘，Mount挂载移动硬盘需要ntfs-3g软件包支持，而本地光盘镜像中没有该软件包，此时需要往YUM源端添加ntfs-3g软件包，添加方法如下：

（1）      切换至/var/www/html/centos目录，官网下载NTFS-3G软件包。

|cd /var/www/html/centos/<br><br>wget [http://dl.fedoraproject.org/pub/epel/7/x86_64/n/ntfs-3g-2021.2.22-3.el7.x86_64.rpm](http://dl.fedoraproject.org/pub/epel/7/x86_64/n/ntfs-3g-2016.2.22-3.el7.x86_64.rpm)<br><br>[http://dl.fedoraproject.org/pub/epel/7/x86_64/n/ntfs-3g-devel-2021.2.22-3.el7.x86_64.rpm](http://dl.fedoraproject.org/pub/epel/7/x86_64/n/ntfs-3g-devel-2016.2.22-3.el7.x86_64.rpm)|
|---|

（2）      Createrepo命令更新软件包，同理，如需新增其他软件包，同样把软件下载至本地，然后通过createrepo更新即可，如图6-18所示：

|createrepo   --update  centos/|
|---|

![](学习笔记/Attachments/1737817671160-c929e900-6d07-4fbf-ab59-98cf474f7432.png)

图6-18  CreateRepo update更新软件包

（3）      客户端YUM验证，安装NTFS-3G软件包，如图6-19所示：

![](学习笔记/Attachments/1737817671374-4669a6ef-4a7b-422a-a1da-5b48b07a99ec.png)图6-19  YUM INSTALL NTFS-3G软件包

## 6.10  同步外网YUM源

在企业实际应用场景中，仅仅靠光盘里面的RPM软件包是不能满足需要，我们可以把外网的YUM源中的所有软件包同步至本地，可以完善本地YUM源的软件包数量及完整性。

获取外网YUM源软件常见方法包括Rsync、Wget、Reposync，三种同步方法的区别Rsync方式需要外网YUM源支持RSYNC协议，Wget可以直接获取，而Reposync可以同步几乎所有的YUM源，下面以Reporsync为案例，同步外网YUM源软件至本地，步骤如下：

（1）      下载CentOS7 REPO文件至/etc/yum.repos.d/，并安装reposync命令工具：

|wget  [http://mirrors.163.com/.help/CentOS7-Base-163.repo](http://mirrors.163.com/.help/CentOS7-Base-163.repo%20-O%20centos.repo)<br><br>[mv CentOS7-Base-163.repo /etc/yum.repos.d/centos.repo](http://mirrors.163.com/.help/CentOS7-Base-163.repo%20-O%20centos.repo)<br><br>yum  clean all<br><br>yum install yum-utils createrepo –y<br><br>yum repolist|
|---|

（2）      通过reposync命令工具获取外网YUM源所有软件包，-r指定repolist id，默认不加-r表示获取外网所有YUM软件包，-p参数表示指定下载软件的路径，如图6-20（a）、图6-20（b）所示：

|reposync  -r  base     -p     /var/www/html/centos/<br><br>reposync  -r  updates  -p     /var/www/html/centos/|
|---|

![](学习笔记/Attachments/1737817671531-021c644d-8b78-4089-93b9-7e1ea3272917.png)

图6-20（a） Reposync获取外网YUM源软件包

![](学习笔记/Attachments/1737817671767-74e8d527-a55b-45c2-810d-967fb58bb6a4.png)

图6-20（b） Reposync获取外网YUM源软件包

（3）      通过reposync工具下载完所有的软件包之后，需要执行createrepo更新本地YUM仓库：

|createrepo  /var/www/html/centos/|
|---|

## 6.11  本章小结

通过对本章内容的学习，读者掌握了Linux安装不同包的工具及命令，使用RPM及YUM管理.RPM结尾的二进制包，基于configure、make、make install实现源码包安装，并能够对软件进行安装、卸载及维护。

能够独立构建企业光盘源、HTTP网络YUM源，实现无外网网络使用YUM安装各种软件包及工具，同时能随时添加新的软件包至本地Yum源中。

## 6.12  同步作业

1.      RPM及YUM管理工具的区别是什么？

2.      企业中安装软件，何时选择YUM安装或者源码编译安装？

3.      将Linux系统中PHP5.3版本升级至PHP5.5版本，升级方法有几种，分别写出升级步骤？

4.      使用源码编译安装httpd-2.4.25.tar.bz2，写出安装的流程及注意事项。

5.      如何将CentOS 7 Linux字符界面升级为图形界面，并设置系统启动默认为图形界面？

# 第7章  Linux磁盘管理

Linux系统一切以文件的方式存储于硬盘，应用程序数据需要时刻读写硬盘，所以企业生产环境中对硬盘的操作变得尤为重要，对硬盘的维护和管理也是每个运维工程师必备工作之一。

本章向读者介绍硬盘简介、硬盘数据存储方式、如何在企业生产服务器添加硬盘、对硬盘进行分区、初始化以及对硬盘进行故障修复等。

## 7.1     计算机硬盘简介

硬盘是计算机主要存储[媒介](http://baike.baidu.com/item/%E5%AA%92%E4%BB%8B)之一，由一个或者多个铝制或者玻璃制的[碟片](http://baike.baidu.com/item/%E7%A2%9F%E7%89%87)组成，[碟片](http://baike.baidu.com/item/%E7%A2%9F%E7%89%87)外覆盖有[铁磁性](http://baike.baidu.com/item/%E9%93%81%E7%A3%81%E6%80%A7)材料，硬盘内部由磁道、柱面、扇区、磁头等部件组成，如图7-1所示：

![](学习笔记/Attachments/1737817671978-2b548bcd-8c4f-4ef8-8f21-814eae7368b6.png)

图7-1 硬盘内部结构组成

Linux系统中硬件设备相关配置文件存放在/dev/下，常见硬盘命名：/dev/hda、/dev/sda、/dev/sdb、/dev/sdc、/dev/vda。不同硬盘接口，在系统中识别的设备名称不一样。

IDE硬盘接口在Linux中设备名为/dev/hda，SAS、SCSI、SATA硬盘接口在Linux中设备名为sda，高效云盘硬盘接口会识别为/dev/vda等。

文件储存在硬盘上，硬盘的最小存储单位叫做Sector（扇区），每个Sector储存512字节。操作系统在读取硬盘的时候，不会逐个Sector的去读取，这样效率非常低，为了提升读取效率，操作系统会一次性连续读取多个Sector，即一次性读取多个Sector称为一个Block（块）。

由多个Sector组成的Block是文件存取的最小单位。Block的大小常见的有1KB、2KB、4KB，Block在Linux中常设置为4KB，即连续八个Sector组成一个Block。

/boot分区Block一般为1KB，而/data/分区或者/分区的Block为4K。可以通过如下三种方法查看Linux分区的Block大小：

|dumpe2fs /dev/sda1 \|grep "Block size"<br><br>tune2fs -l /dev/sda1 \|grep "Block size"<br><br>stat /boot/\|grep "IO Block"|
|---|

例如创建一个普通文件，文件大小为10Bytes，而默认设置Block为4K，如果有1万个小文件，由于每个Block只能存放一个文件，如果文件的大小比Block大，会申请更多的Block，相反如果文件的大小比默认Block小，仍会占用一个Block，这样剩余的空间会被浪费掉。

q  1万个文件理论只占用空间大小：10000x10=100000Bytes=97.65625MBytes；

q  1万个文件真实占用空间大小：10000x4096Bytes=40960000Bytes=40000MBytes=40GB。

q  根据企业实际需求，此时可以将Block设置为1K，从而节省更多的空间。

## 7.2     硬盘Block及Inode详解

通常而言，操作系统对于文件数据的存放包括两个部分：文件内容、权限及文件属性。操作系统文件存放是基于文件系统，文件系统会将文件的实际内容存储到Block中，而将权限与属性等信息存放至Inode中。

在硬盘分区中，还有一个超级区块 (SuperBlock) ，SuperBlock会记录整个文件系统的整体信息，包括 Inode、Block 总量、使用大小、剩余大小等信息，每个 inode 与 block 都有编号对应，方便Linux系统快速定位查找文件。

q  Superblock：记录文件系统的整体信息，包括inode与block的总量、使用大小、剩余大小， 以及文件系统的格式与相关信息等；

q  Inode：记录文件的属性，权限，同时会记录该文件的数据所在的block编号；

q  Block：存储文件的内容，如果文件超过默认Block大小，会自动占用多个Block。

因为每个 inode 与 block 都有编号，而每个文件都会占用一个 inode ，inode 内则有文件数据放置的 block 号码。如果能够找到文件的 inode，就可以找到该文件所放置数据的block号码，从而读取该文件内容。

操作系统进行格式化分区时，操作系统自动将硬盘分成两个区域。一个是数据Block区，用于存放文件数据；另一个是Inode Table区，用于存放inode包含的元信息。

每个inode节点的大小，可以在格式化时指定，默认为128Bytes或256Bytes，/boot分区Inode默认为128Bytes，其他分区默认为256Bytes，查看Linux系统Inode方法如下：

|dumpe2fs   /dev/sda1 \|grep " Inode size "<br><br>tune2fs  -l  /dev/sda1 \|grep " Inode size"<br><br>stat  /boot/\|grep "Inode"|
|---|

格式化磁盘时，可以指定默认Inode和Block的大小，-b指定默认Block值，-I指定默认Inode值，如图7-2所示，命令如下：

|mkfs.ext4 -b 4096 -I 256 /dev/sdb|
|---|

![](学习笔记/Attachments/1737817672192-281d4653-df2a-4655-a8d5-e5dc4581c0a6.png)图7-2 格式化硬盘指定Inode和Block

## 7.3     硬链接介绍

一般情况下，文件名和inode编号是一一对应的关系，每个inode号码对应一个文件名。但UNIX/Linux系统多个文件名也可以指向同一个inode号码。这意味着可以用不同的文件名访问同样的内容，对文件内容进行修改，会影响到所有文件名。但删除一个文件名，不影响另一个文件名的访问。这种情况就被称为硬链接（hard link）。

创建硬链接的命令为：ln  jf1.txt  jf2.txt，其中jf1.txt为源文件，jf2.txt为目标文件。如上命令源文件与目标文件的inode号码相同，都指向同一个inode。inode信息中有一项叫做"链接数"，记录指向该inode的文件名总数，这时会增加1，变成2，如图7-3所示：

![](学习笔记/Attachments/1737817672485-289420ec-3095-47e6-95d3-afa00de5bc81.png)图7-3 jf1.txt jf2.txt硬链接inode值变化

同样删除一个jf2.txt文件，就会使得jf1.txt inode节点中的"链接数"减1。如果该inode值减到0，表明没有文件名指向这个inode，系统就会回收这个inode号码，以及其所对应block区域，如图7-4所示：

![](学习笔记/Attachments/1737817672701-cbbc3e9b-212c-477d-8b6b-62ca5189f857.png)图7-4 删除jf2.txt硬链接inode值变化

实用小技巧：硬链接不能跨分区链接，硬链接只能对文件生效，对目录无效，也即是目录不能创建硬链接。硬链接源文件与目标文件共用一个inode值，从某种意义上来，节省inode空间。不管是单独删除源文件还是删除目标文件，文件内容始终存在。同时链接后的文件不占用系统多余的空间。

## 7.4     软链接介绍

除了硬链接以外，还有一种链接-软链接。文件jf1.txt和文件jf2.txt的inode号码虽然不一样，但是文件jf2.txt的内容是文件jf1.txt的路径。读取文件jf2.txt时，系统会自动将访问者导向文件jf1.txt。

无论打开哪一个文件，最终读取的都是文件jf1.txt。这时，文件jf2.txt就称为文件jf1.txt的"软链接"（soft link）或者"符号链接（symbolic link）。

文件jf2.txt依赖于文件jf1.txt而存在，如果删除了文件jf1.txt，打开文件jf2.txt就会报错："No such file or directory"。

软链接与硬链接最大的不同是文件jf2.txt指向文件jf1.txt的文件名，而不是文件jf1.txt的inode号码，因此文件jf1.txt的inode链接数不会发生变化，如图7-5所示：

![](学习笔记/Attachments/1737817672994-9a3d07b2-3289-4015-9e2b-72fed0c12bed.png)

图7-5 删除jf1.txt源文件链接数不变

实用小技巧：软链接可以跨分区链接，软链接支持目录同时也支持文件的链接。软链接源文件与目标文件Inode不相同，从某种意义上来，会消耗省inode空间。不管是删除源文件还是重启系统，该软链接还存在，但是文件内容会丢失，一旦新建源同名文件名，软链接文件恢复正常。

## 7.5     Linux下磁盘实战操作命令

企业真实场景由于硬盘常年大量读写，经常会出现坏盘，需要更换硬盘。或者由于磁盘空间不足，需添加新硬盘，新添加的硬盘需要经过格式化、分区才能被Linux系统所使用，虚拟机CentOS 7 Linux模拟DELL R730真实服务器添加一块新硬盘，不需要关机，直接插入用硬盘即可，一般硬盘均支持热插拔功能。企业中添加新硬盘的操作流程如下：

（1）      检测Linux系统识别的硬盘设备，新添加硬盘被识别为/dev/sdb，如果有多块硬盘，会依次识别成/dev/sdc、/dev/sdd等设备名称，如图7-6所示：

|fdisk  -l|
|---|

![](学习笔记/Attachments/1737817673203-8645cca4-ee44-4f9c-9595-bc653f37b7bc.png)

图7-6 Fdisk查看Linux系统硬盘设备

（2）      基于新硬盘/dev/sdb设备，创建磁盘分区/dev/sdb1，如图7-7所示：

|fdisk /dev/sdb|
|---|

![](学习笔记/Attachments/1737817673450-14cdb19c-a747-4993-a7b8-8fde018b2310.png)图7-7 Fdisk /dev/sdb分区

（3）      fdisk分区命令参数如下，常用参数包括m、n、p、e、d、w。

|b                            编辑bsd disklabel；<br><br>c                             切换dos兼容性标志；<br><br>d                            删除一个分区；<br><br>g                            创建一个新的空GPT分区表；<br><br>G                            创建一个IRIX（SGI）分区表；<br><br>l                              列出已知的分区类型；<br><br>m                            打印帮助菜单；<br><br>n                             添加一个新分区；<br><br>o                            创建一个新空DOS分区表；<br><br>p                            打印分区表信息；<br><br>q                            退出而不保存更改；<br><br>s                             创建一个新的空的Sun磁盘标签；<br><br>t                             更改分区的系统ID；<br><br>u                             更改显示/输入单位；<br><br>v                             验证分区表；<br><br>w                            将分区表写入磁盘并退出；<br><br>x                             额外功能。|
|---|

（4）      创建/dev/sdb1分区方法，fdisk /dev/sdb，然后按n-p-1-Enter键- +20G-Enter键-w，最后执行fdisk –l|tail -10，如图7-8（a）、图7-8（b）所示：

![](学习笔记/Attachments/1737817673622-f803d9ca-a32e-45e4-946a-79b965e84c73.png)

图7-8（a） Fdisk /dev/sdb创建/dev/sdb1分区

![](学习笔记/Attachments/1737817673819-cc11f5e7-d47f-4fd6-815c-ca6832e74473.png)图7-8（b） Fdisk –l查看/dev/sdb1分区

（5）      mkfs.ext4  /dev/sdb1格式化磁盘分区，如图7-9所示：

![](学习笔记/Attachments/1737817673978-80c3965d-e338-4d76-b304-53665adf2a85.png)图7-9  mkfs.ext4格式化磁盘分区

（6）      /dev/sdb1分区格式化，使用mount命令挂载到/data/目录，如图7-10所示：

|mkdir   -p              /data/               创建/data/数据目录<br><br>mount  /dev/sdb1    /data           挂载/dev/sdb1分区至/data/目录<br><br>df -h                                       查看磁盘分区详情<br><br>echo "mount  /dev/sdb1  /data" >>/etc/rc.local  将挂载分区命令加入/etc/rc.local开机启动|
|---|

![](学习笔记/Attachments/1737817674241-be0df67f-84e9-4e0e-a369-912d9e1f8104.png)图7-10  MOUNT挂载/dev/sdb1磁盘分区

（7）      自动挂载分区除了可以加入到/etc/rc.local开机启动之外，还可以加入到/etc/fstab文件中，如图7-11所示：

|/dev/sdb1                /data/             ext4            defaults        0 0<br><br>mount      -o      rw,remount      /     重新挂载/系统，检测/etc/fstab是否有误。|
|---|

![](学习笔记/Attachments/1737817674431-291e4bc1-e0f3-4b7c-8245-0bd66bee381f.png)

图7-11 /dev/sdb1磁盘分区加入/etc/fstab文件

## 7.6     基于GPT格式磁盘分区

MBR分区标准决定了MBR只支持在2TB以下的硬盘，为了支持能使用大于2T硬盘空间，需使用GPT格式进行分区。创建大于2TB的分区，需使用parted工具。

在企业真实环境中，通常一台服务器有多块硬盘，整个硬盘容量为10T，需要基于GTP格式对10T硬盘进行分区，操作步骤如下：

|parted -s   /dev/sdb  mklabel gpt        设置分区类型为gpt格式；<br><br>mkfs.ext3  /dev/sdb                    基于Ext3文件系统类型格式化；<br><br>mount     /dev/sdb  /data/             挂载/dev/sdb设备至/data/目录。|
|---|

（1）      如图7-12所示，假设/dev/sdb 为10T硬盘，使用GPT格式来格式化磁盘：

![](学习笔记/Attachments/1737817674654-425166ec-06fc-492f-ac66-033a87756ff2.png)图7-12  假设/dev/sdb为10T设备

（2）      执行命令：parted -s  /dev/sdb  mklabel gpt，如图7-13所示：

![](学习笔记/Attachments/1737817674845-e396e7ad-2a19-48fb-963f-5e81d0fa59bf.png)图7-13  设置/dev/sdb为GPT格式磁盘

（3）      基于mkfs.ext3  /dev/sdb格式化磁盘，如图7-14所示：

![](学习笔记/Attachments/1737817675111-3c78e13b-332d-4715-aa3d-574e343a339b.png)图7-14  格式/dev/sdb磁盘

parted命令行也可以进行分区，如图7-15（a）、7-15（b）、7-15（c）所示：

|partedselect /dev/sdbmklabel gptmkpart primary 0  -1print<br><br>mkfs.ext3  /dev/sdb1<br><br>mount     /dev/sdb1  /data/|
|---|

![](学习笔记/Attachments/1737817675267-dc389a3f-0c8d-40fa-a030-6a16cdca5fd1.png)图7-15（a）parted工具执行GPT格式分区

![](学习笔记/Attachments/1737817675474-5a726d68-385e-4393-abdf-2d47308cce76.png)

图7-15（b）parted工具执行GPT格式分区

![](学习笔记/Attachments/1737817675740-bdb79eeb-5c41-40e1-8038-f09160829198.png)

图7-15（c） parted工具执行GPT格式分区

## 7.7     MOUNT命令工具

Mount命令工具主要用于将设备或者分区挂载至Linux系统目录下，Linux系统在分区时，也是基于mount机制将/dev/sda分区挂载至系统目录，将设备与目录挂载之后，Linux操作系统方可进行文件的存储。

### 7.7.1  Mount命令参数详解

如下为企业中Mount命令常用参数详解：

|mount [-Vh]<br><br>mount -a [-fFnrsvw] [-t vfstype]<br><br>mount [-fnrsvw] [-o options [,...]] device \| dir<br><br>mount [-fnrsvw] [-t vfstype] [-o options] device dir<br><br>-V：                              显示mount工具版本号；<br><br>-l：                               显示已加载的文件系统列表；<br><br>-h：                                  显示帮助信息并退出；<br><br>-v：                                   输出指令执行的详细信息；<br><br>-n：                                  加载没有写入文件/etc/mtab中的文件系统；<br><br>-r：                                   将文件系统加载为只读模式；<br><br>-a：                                   加载文件/etc/fstab中配置的所有文件系统；<br><br>-o:                                指定mount挂载扩展参数，常见扩展指令：rw、remount、loop等，其中-o相关指令如下：<br><br>-o atime：                          系统会在每次读取文档时更新文档时间；<br><br>-o noatime：                           系统会在每次读取文档时不更新文档时间；<br><br>-o defaults:                              使用预设的选项 rw,suid,dev,exec,auto,nouser等；<br><br>-o exec                         允许执行档被执行；<br><br>-o user、-o nouser：                使用者可以执行 mount/umount的动作；<br><br>-o remount：                           将已挂载的系统分区重新以其他再次模式挂载；<br><br>-o ro：                               只读模式挂载；<br><br>-o rw：                                   可读可写模式挂载；<br><br>-o loop                          使用loop模式，把文件当成设备挂载至系统目录。<br><br>-t：                                   指定mount挂载设备类型，常见类型nfs、ntfs-3g、vfat、iso9660等，其中-t相关指令如下：<br><br>iso9660                         光盘或光盘镜像；<br><br>msdos                              Fat16文件系统；<br><br>vfat                            Fat32文件系统；<br><br>ntfs                                 NTFS文件系统；<br><br>ntfs-3g                          识别移动硬盘格式；<br><br>smbfs                         挂载Windows文件网络共享；<br><br>nfs                              Unix/Linux文件网络共享。|
|---|

### 7.7.2  企业常用Mount案例

Mount常用案例演示如下：

|mount     /dev/sdb1         /data                挂载/dev/sdb1分区至/data/目录<br><br>mount      /dev/cdrom             /mnt             挂载Cdrom光盘至/mnt目录；<br><br>mount      -t ntfs-3g              /dev/sdc  /data1 挂载/dev/sdc移动硬盘至/data1目录；<br><br>mount   -o remount,rw    /                       重新以读写模式挂载/系统；<br><br>mount      -t  iso9660  -o loop  centos7.iso /mnt 将centos7.iso镜像文件挂载至/mnt目录；<br><br>mount   -t fat32   /dev/sdd1            /mnt    将U盘/dev/sdd1挂载至/mnt/目录；<br><br>mount   -t nfs 192.168.1.11:/data/      /mnt        将远程192.168.1.11:/data目录挂载至本地/mnt目录。|
|---|

## 7.8     Linux硬盘故障修复

企业服务器运维中，经常会发现操作系统的分区变成只读文件系统，错误提示信息为

“Read-only file system”，出现只读文件系统，会导致只能读取，而无法写入新文件、新数据等操作。

造成该问题的原因包括：磁盘老旧长期大量的读写、文件系统文件被破坏、磁盘碎片文件、异常断电、读写中断等等。

以企业CentOS 7 Linux为案例，来修复文件系统，步骤如下：

（1）      远程备份本地其他重要数据，出现只读文件系统，需先备份其他重要数据，基于rsync|scp远程备份，其中/data为源目录，/data/backup/2021/为目标备份目录。

|rsync  -av   /data/       root@192.168.111.188:/data/backup/2021/|
|---|

（2）      可以重新挂载/系统，挂载命令如下，测试文件系统是否可以写入文件。

|mount  -o   remount,rw   /|
|---|

（3）      如果重新挂载/系统无法解决问题，则需重启服务器，以CD/DVD光盘引导进入Linux Rescue修复模式，如图7-16（a）、7-16（b）所示，光标选择“Troubleshooting”,按Enter键，然后选择“Rescue a CentOS system”，按Enter键。

![](学习笔记/Attachments/1737817676066-316b1c27-61d8-4259-8901-0857fad6edb3.png)图7-16（a）  光盘引导进入修复模式

![](学习笔记/Attachments/1737817676274-2e652080-7953-4f39-bf61-18b993adac3b.png)图7-16（b）  光盘引导进入修复模式

（4）      选择“1）Continue”继续操作，如图7-17所示：

![](学习笔记/Attachments/1737817676456-922af01e-8176-46f6-88da-eaee3bf5ba10.png)

图7-17 选择Continue继续进入系统

（5）      登录修复模式，执行如下命令，df –h显示原来的文件系统，如图7-18所示：

|chroot     /mnt/sysimage<br><br>df  -h|
|---|

![](学习笔记/Attachments/1737817676624-eb8aac84-d7f6-41de-972d-5810bf698e54.png)图7-18 切换原分区目录

（6）      对有异常的分区进行检测并修复，根据文件系统类型，执行相应的命令如下：

|umount    /dev/sda3<br><br>fsck.ext4   /dev/sda3     –y|
|---|

（7）      修复完成之后，重启系统即可

|reboot|
|---|

## 7.9     本章小结

通过对本章内容的学习，读者掌握了Linux硬盘内部结构、Block及Inode特性，能够对企业硬盘进行分区、格式化等操作，满足企业的日常需求。

基于mount工具，能对硬盘、各类文件系统进行挂载操作，同时对只读文件系统能快速修复并投入使用。

## 7.10  同步作业

1.      软链接与硬链接的区别是什么？

2.      有一块4T的移动硬盘，如何将数据拷贝至服务器/data/目录，服务器空间为10T，请写出详细步骤？

3.      运维部小刘发现公司IDC机房一台DELL R730服务器，/data/目录不可写，而/boot目录可读可写，请问原因是什么导致，如何修复/data/目录？

4.      公司一台DELL R730服务器/data/images目录存放了大量的小文件，运维人员向该目录写入1M测试文件，提示磁盘空间不足，而通过df –h显示剩余可用空间为500G，请问是什么原因导致，如何解决该问题？

5.      机房一台DELL R730服务器，由于业务需求，临时重启，重启完，20分钟后还无法登陆系统，检查控制台输出，一直卡在MySQL服务启动项，请问如何快速解决让系统正常启动，请写出解决步骤。

# 第8章   NTP服务器企业实战

## 8.1  NTP服务简介

NTP是网络时间协议(Network Time Protocol)，它是用来同步网络中各个计算机的时间的协议。NTP服务器是用于局域网服务器时间同步使用的，可以保证局域网所有的服务器与时间服务器的时间保持一致，某些应用对时间实时性要求高的必须统一时间。

在计算机的世界里，时间非常地重要，例如对于火箭发射这种科研活动，对时间的统一性和准确性要求就非常地高，是按照A这台计算机的时间，还是按照B这台计算机的时间？NTP就是用来解决这个问题的，NTP（Network Time Protocol，网络时间协议）是用来使网络中的各个计算机时间同步的一种协议。

NTP的用途是把计算机的时钟同步到[世界协调时](https://baike.baidu.com/item/%E4%B8%96%E7%95%8C%E5%8D%8F%E8%B0%83%E6%97%B6)UTC，其精度在局域网内可达0.1ms，在互联网上绝大多数的地方其精度可以达到1-50ms。

NTP可以使计算机对其服务器或时钟源（如石英钟，GPS等等）进行时间同步，它可以提供高精准度的时间校正，而且可以使用加密确认的方式来防止病毒的协议攻击。

互联网的时间服务器也有很多，例如ntpdate ntp.sjtu.edu.cn 上海交通大学的NTP免费提供互联网时间同步。

## 8.2  NTP服务器配置

1)      安装NTP软件包

|yum install ntp ntpdate -y|
|---|

2)      修改ntp.conf配置文件

|cp  /etp/ntp.conf /etc/ntp.conf.bak|
|---|

 vi /etc/ntp.conf ，修改如下两行，把#号去掉即可！

|server 127.127.1.0     # local clock<br><br>fudge  127.127.1.0 stratum 10|
|---|

3)      以守护进程启动ntpd

|service ntpd restart|
|---|

4)      客户端配置同步时间，增加一行，在每天的6点10分与时间同步服务器进行同步。

|crontab -e<br><br>10 06 * * * /usr/sbin/ntpdate ntp-server的ip >>/usr/local/logs/crontab/ntpdate.log|
|---|

## 8.3  NTP配置文件

|driftfile /var/lib/ntp/drift<br><br>restrict default kod nomodify notrap nopeer noquery<br><br>restrict -6 default kod nomodify notrap nopeer noquery<br><br>restrict 127.0.0.1<br><br>restrict -6 ::1<br><br>server  127.127.1.0     # local clock<br><br>fudge   127.127.1.0 stratum 10<br><br>includefile /etc/ntp/crypto/pw<br><br>keys /etc/ntp/keys|
|---|

## 8.4  NTP参数详解

|restrict default ignore|# 关闭所有的 NTP 要求封包|
|---|---|
|restrict 127.0.0.1|# 开启内部递归网络接口lo|
|restrict 192.168.0.0 mask 255.255.255.0 nomodify|#在内部子网里面的客户端可以进行网络校时，但不能修改NTP服务器的时间参数|
|server 198.168.0.111|#198.168.0.111作为上级时间服务器参考|
|restrict 198.168.0.111|#开放server 访问我们ntp服务的权限|
|driftfile /var/lib/ntp/drift|在与上级时间服务器联系时所花费的时间，记录在driftfile参数后面的文件内|
|broadcastdelay 0.008|#广播延迟时间|

# 第9章   DHCP服务器企业实战

## 9.1  DHCP服务简介

DHCP(Dynamic Host Configuration Protocol，动态主机配置协议)是一个[局域网](http://baike.baidu.com/view/788.htm)的[网络协议](http://baike.baidu.com/view/16603.htm)，使用[UDP](http://baike.baidu.com/view/30509.htm)协议工作，主要用途：给内部网络或[网络服务](http://baike.baidu.com/view/1279152.htm)供应商自动分配[IP地址](http://baike.baidu.com/view/3930.htm)，DHCP有3个端口，其中UDP67和UDP68为正常的DHCP服务端口，分别作为DHCP Server和DHCP Client的服务端口。

DHCP可以部署在服务器、交换机或者服务器，可以控制一段IP地址范围，客户机登录服务器时就可以自动获得DHCP服务器分配的IP地址和子网掩码。其中DHCP所在服务器的需要安装TCP/IP协议，需要设置静态IP地址、子网掩码、默认网关。

## 9.2  DHCP服务器配置

1)      安装DHCP软件包

|yum  install  dhcp dhcp-devel -y|
|---|

2)      修改DHCP配置文件

|ddns-update-style interim;<br><br>ignore client-updates;<br><br>next-server  192.168.0.79;<br><br>filename "pxelinux.0";<br><br>allow booting;<br><br>allow bootp;      <br><br>subnet 192.168.0.0 netmask 255.255.255.0 {<br><br># --- default gateway<br><br>option routers          192.168.0.1;<br><br>option subnet-mask      255.255.252.0;<br><br>#   option nis-domain       "domain.org";<br><br>#  option domain-name "192.168.0.10";<br><br>#   option domain-name-servers  192.168.0.11;<br><br>#   option ntp-servers      192.168.1.1;<br><br>#   option netbios-name-servers  192.168.1.1;<br><br># --- Selects point-to-point node (default is hybrid). Don't change this unless<br><br># -- you understand Netbios very well<br><br>#   option netbios-node-type 2;<br><br>range  dynamic-bootp  192.168.0.100 192.168.0.200;<br><br>host ns {<br><br>hardware ethernet  00:1a:a0:2b:38:81;<br><br>fixed-address 192.168.0.101;}<br><br>}|
|---|

## 9.3  DHCP参数详解

|选    项|解    释|
|---|---|
|||
|ddns-update-style interim\|ad-hoc\|none|参数用来设置DHCP服务器与DNS服务器的动态信息更新模式：interim为DNS互动更新模式，ad-hoc为特殊DNS更新模式，none为不支持动态更新模式。|
|next-server ip|pxeclient远程安装系统，指定tftp server 地址|
|filename|开始启动文件的名称，应用于无盘安装，可以是tftp的相对或绝对路径|
|ignore client-updates|为忽略客户端更新|
|subnet-mask|为客户端设定子网掩码|
|option routers|为客户端指定网关地址|
|domain-name|为客户端指明DNS名字|
|domain-name-servers|为客户端指明DNS服务器的IP地址|
|host-name|为客户端指定主机名称|
|broadcast-address|为客户端设定广播地址|
|ntp-server|为客户端设定网络时间服务器的IP地址|
|time-offset|为客户端设定格林威治时间的偏移时间，单位是秒|

## 9.4  客户端使用

客户端要从该DHCP服务器获取IP，需要做如下设置：

n  Linux操作系统设置

vim /etc/sysconfig/network-scritps/ifcfg-eth0里BOOTPROTO值修改为dhcp。

n  Windows操作系统设置

需要修改本地连接，把它设置成自动获取IP即可。

# 第10章       Samba服务器企业实战

## 10.1   Samba服务简介

Samba是在Linux和UNIX系统上实现SMB协议的一个免费软件，由服务器及客户端程序构成，

[SMB](http://baike.baidu.com/view/262410.htm)（Server Messages Block，信息服务块）是一种在[局域网](http://baike.baidu.com/view/788.htm)上共享文件和打印机的一种[通信协议](http://baike.baidu.com/view/185322.htm)，它为局域网内的不同计算机之间提供文件及打印机等资源的共享服务。

SMB协议是客户机/服务器型协议，客户机通过该协议可以访问服务器上的共享文件系统、[打印机](http://baike.baidu.com/view/7836.htm)及其他资源。通过设置“NetBIOS over TCP/IP”使得Samba不但能与局域网络主机分享资源，还能与全世界的电脑分享资源。

## 10.2   Samba服务器配置

1)   安装Samba软件包

|yum install  samba -y|
|---|

2)   配置文件修改

|cp /etc/samba/smb.conf /etc/samba/smb.conf.bak ;egrep -v "#\|^$" /etc/samba/smb.conf.bak \|grep -v "^;" >/etc/samba/smb.conf|
|---|

3)   Smb.conf配置文件

|[global]<br><br>        workgroup = MYGROUP<br><br>        server string = Samba Server Version %v<br><br>        security = share<br><br>        passdb backend = tdbsam<br><br>        load printers = yes<br><br>        cups options = raw<br><br>[temp]<br><br>     comment=Temporary file space<br><br>     path=/tmp<br><br>     read only=no<br><br>     public=yes<br><br>[data]<br><br>     comment=Temporary file space<br><br>     path=/data<br><br>     read only=no<br><br>     public=yes|
|---|

4)   根据需求修改之后重启服务：

|service smb restart<br><br>Shutting down SMB services:                                [FAILED]<br><br>Shutting down NMB services:                                [FAILED]<br><br>Starting SMB services:                                     [  OK  ]<br><br>Starting NMB services:                                     [  OK  ]|
|---|

## 10.3   Samba参数详解

|workgroup =|WORKGROUP 设Samba Server 所要加入的工作组或者域。|
|---|---|
|server string = Samba Server Version %v|Samba Server 的注释，可以是任何字符串，也可以不填。宏%v表示显示Samba的版本号。|
|security = user|1.share：用户访问Samba Server不需要提供用户名和口令, 安全性能较低。<br><br>2. user：Samba Server共享目录只能被授权的用户访问,由Samba Server负责检查账号和密码的正确性。账号和密码要在本Samba Server中建立。<br><br>3. server：依靠其他Windows NT/2000或Samba Server来验证用户的账号和密码,是一种代理验证。此种安全模式下,系统管理员可以把所有的Windows用户和口令集中到一个NT系统上,使用Windows NT进行Samba认证, 远程服务器可以自动认证全部用户和口令,如果认证失败,Samba将使用用户级安全模式作为替代的方式。<br><br>4. domain：域安全级别,使用主域控制器(PDC)来完成认证。|
|comment = test|是对该共享的描述，可以是任意字符串。|
|path = /home/test|共享目录路径|
|browseable= yes/no|用来指定该共享是否可以浏览。|
|writable = yes/no|writable用来指定该共享路径是否可写。|
|available = yes/no|available用来指定该共享资源是否可用|
|admin users = admin|该共享的管理者|
|valid users = test|允许访问该共享的用户|
|invalid users = test|禁止访问该共享的用户|
|write list = test|允许写入该共享的用户|
|public = yes/no|public用来指定该共享是否允许guest账户访问|

1)   客户端访问Samba

打开Windows资源管理器，输入[\\192.168.33.10](192.168.149.128) （SMB文件共享服务端IP），如图10-1所示：

![](学习笔记/Attachments/1737817676828-71ba6234-e1b1-4e5e-8e01-74ffb3bd581a.png)

图10-1 访问Samba文件共享

# 第11章        Rsync服务器企业实战

Rsync是Unix/Linux下的一款应用软件，利用它可以使多台服务器数据保持同步一致性，第一次同步时 rsync 会复制全部内容，但在下一次只传输修改过的文件。

Rsync 在传输数据的过程中可以实行压缩及[解压缩](http://baike.baidu.com/view/845020.htm)操作，因此可以使用更少的带宽。可以很容易做到保持原来文件的权限、时间、软硬链接等。

## 11.1   Rsync服务端配置

1)   源码部署安装Rsync服务

|wget  [http://rsync.samba.org/ftp/rsync/src/rsync-3.0.7.tar.gz](http://rsync.samba.org/ftp/rsync/src/rsync-3.0.7.tar.gz) <br><br>tar  xzf  rsync-3.0.7.tar.gz<br><br>cd rsync-3.0.7<br><br>./configure --prefix=/usr/local/rsync<br><br>make<br><br>make install|
|---|

2)   创建配置文件，配置内容为如下:

|vi /etc/rsyncd.conf<br><br>#########<br><br>[global]<br><br>uid = nobody<br><br>gid = nobody<br><br>use chroot = no<br><br>max connections = 30<br><br>pid file = /var/run/rsyncd.pid<br><br>lock file = /var/run/rsyncd.lock<br><br>log file = /var/log/rsyncd.log<br><br>transfer logging = yes<br><br>log format = %t %a %m %f %b<br><br>syslog facility = local3<br><br>timeout = 300<br><br>[www]<br><br>read only = yes<br><br>path = /usr/local/webapps<br><br>comment = www<br><br>auth users =test<br><br>secrets file = /etc/rsync.pas<br><br>hosts allow = 192.168.0.11,192.168.0.12<br><br>[web]<br><br>read only = yes<br><br>path = /data/www/web<br><br>comment = web<br><br>auth users =test<br><br>secrets file = /etc/rsync.pas<br><br>hosts allow = 192.168.0.11,192.168.0.0/24|
|---|

3)   Rsync配置参数含义

|[www]   #要同步的模块名<br><br>path = /usr/local/webapps  #要同步的目录<br><br>comment = www    #注释，解释模块的用于做什么备份<br><br>read only = no   # no客户端可上传文件,yes只读<br><br>write only = no  # no客户端可下载文件,yes不能下载<br><br>list = yes      #是否提供资源列表<br><br>auth users =test  #登陆系统使用的用户名，没有默认为匿名。<br><br>hosts allow = 192.168.0.10,192.168.0.20  #本模块允许通过的IP地址<br><br>hosts deny = 192.168.0.4     #禁止主机IP<br><br>secrets file=/etc/rsync.pas  #密码文件存放的位置|
|---|

4)   启动服务器端RSYNC服务，默认监听端口TCP 873。

|/usr/local/rsync/bin/rsync  --daemon|
|---|

5)   设置rsync服务器端同步密钥，文件内容username:userpasswd (表示用户名：密码)

|vi  /etc/rsync.pas<br><br>jfedu:jfedu999|
|---|

保存完毕，chmod 600 /etc/rsync.pas设置权限为宿主用户读写。

6)   设置客户端配置同步密钥,文件内容userpasswd (表示密码)

|vi  /etc/rsync.pas<br><br>jfedu999|
|---|

7)   客户端执行同步命令，从服务端同步数据至本地

|rsync -aP --delete jfedu@192.168.0.100::www /usr/local/webapps    --password-file=/etc/rsync.pas  <br><br>rsync -aP --delete jfedu@192.168.0.100::web  /data/www/web   --password-file=/etc/rsync.pas|
|---|

注*/usr/local/webapps为客户端的目录，@前jfedu是认证的用户名；IP后面www为rsync服务器端的模块名称。

## 11.2   Rsync 参数详解

|-a, ––archive|归档模式，表示以递归方式传输文件，并保持所有文件属性。|
|---|---|
|––exclude=PATTERN|指定排除一个不需要传输的文件匹配模式|
|––exclude-from=FILE|从 FILE 中读取排除规则|
|––include=PATTERN|指定需要传输的文件匹配模式|
|--delete|删除那些接收端还有而发送端已经不存在的文件|
|-P|等价于 ––partial ––progress|
|-v, ––verbose|详细输出模式|
|-q, ––quiet|精简输出模式|
|––rsyncpath=PROGRAM|指定远程服务器上的 rsync 命令所在路径|
|––password-file=FILE|从 FILE 中读取口令，以避免在终端上输入口令，<br><br>通常在 cron 中连接 rsync 服务器时使用|

## 11.3   Rsync基于SSH同步  

除了可以使用rsync密钥进行同步之外，还有一个比较简单的同步方法就是基于linux ssh来同步。具体方法如下：

|rsync  -aP  --delete  [root@192.168.0.100:/data/www/webapps](mailto:root@192.168.0.10:/data/www/webapps)  /data/www/webapps|
|---|

如果需要每次同步不输入密码，需要做Linux主机之间免密码登录。

## 11.4   Rsync基于Sersync实时同步

在企业日常web应用中，某些特殊的数据需要要求保持跟服务器端实时同步，那我们该如何来配置呢？如何来实现呢？这里可以采用rsync+sersync来实现需求。

同步原理：在同步服务器上开启sersync服务，sersync负责监控配置路径中的文件系统事件变化，如果文件目录一旦发生变化，则调用rsync命令把更新的文件同步到目标服务器。

|#数据推送端（sersync）：192.168.75.121<br><br>#数据接收端（rsync）：192.168.75.122|
|---|
|#部署rsync：<br><br>yum  install rsync  -y<br><br>#修改配置文件：<br><br>vim /etc/rsyncd.conf<br><br># /etc/rsyncd: configuration file for rsync daemon mode<br><br># See rsyncd.conf man page for more options.<br><br>#rsync数据同步用户：<br><br>uid = root<br><br>gid = root<br><br>#rsync服务端口，默认是873，可以省略不写<br><br>port = 873<br><br>#安全设置，限制软连接，如果设置为no，则文件同步到远程服务器后，会在链接文件前面增加一个目录，使链接文件失效，设置为yes则表示不加目录。<br><br>use chroot = yes<br><br>#最大链接数；<br><br>max connections = 100<br><br>#进程pid文件<br><br>pid file = /var/run/rsyncd.pid<br><br>#排除指定目录<br><br>exclude = lost+found/<br><br>#开启日志，默认在系统日志中记录<br><br>transfer logging = yes<br><br>#超时时间<br><br>timeout = 900<br><br>#忽略不可读的文件，如果上面的用户对于同步的文件没有可读权限，则不同步<br><br>ignore nonreadable = yes<br><br>#不压缩指定文件<br><br>dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2<br><br>#允许同步的服务器<br><br>hosts allow = 192.168.75.122<br><br>#不允许同步的服务器<br><br>hosts deny = *<br><br>#关闭只读权限（默认只能将本机文件同步至远程服务器，关闭后可以拉取远程服务器内容到本机）<br><br>read  only = false<br><br>#模块<br><br>[web]<br><br>          # 本机存放同步文件目录<br><br>       path = /data/web<br><br>       # 注释目录作用<br><br>       comment = nginx web data<br><br>       # 用于同步的用户（虚拟用户）<br><br>       auth users = rsync<br><br>       # 用户认证文件<br><br>       secrets file = /etc/rsync.passwd<br><br>#创建存放数据的目录：<br><br>mkdir -p /data/web<br><br># 创建认证文件：<br><br>echo "rsync:123456" > /etc/rsync.passwd<br><br># 设置文件权限：<br><br>chmod 600 /etc/rsync.passwd<br><br># 启动服务：<br><br>systemctl start rsyncd|
|#部署sersync：<br><br># 解压包：<br><br>tar xf sersync2.5.4_64bit_binary_stable_final.tar.gz<br><br># 移到/usr/local/目录下：<br><br>mv GNU-Linux-x86/ /usr/local/sersync<br><br># 备份配置文件：<br><br>[root@www-jfedu-net ~]#  cd /usr/local/sersync/<br><br>[root@www-jfedu-net sersync]# cp confxml.xml confxml.xml.bak<br><br># 修改配置文件：<br><br><?xml version="1.0" encoding="ISO-8859-1"?><br><br><head version="2.5"><br><br>        <!--为插件设置的IP:PORT，与同步没有太大关系--><br><br>    <host hostip="localhost" port="8008"></host><br><br>    <!--是否开启调式模式，默认关闭，如果开启，则进行文件同步时，终端会输出同步的命令--><br><br>    <debug start="false"/><br><br>    <!--如果文件系统是xfs,则需要开启，否则会出问题，比如卡机等状况--><br><br>    <fileSystem xfs="true"/><br><br>    <!--设置过滤规则，默认是关闭，关闭表示所有文件都同步，如果设置为true，则可以根据设置的正则表达式过滤不需要同步的文件--><br><br>    <filter start="false"><br><br>        <exclude expression="(.*)\.svn"></exclude><br><br>        <exclude expression="(.*)\.gz"></exclude><br><br>        <exclude expression="^info/*"></exclude><br><br>        <exclude expression="^static/*"></exclude><br><br>    </filter><br><br>    <inotify><br><br>        <delete start="true"/><br><br>        <createFolder start="true"/><br><br>        <createFile start="false"/><br><br>        <closeWrite start="true"/><br><br><moveFrom start="true"/><br><br>        <moveTo start="true"/><br><br>        <attrib start="false"/><br><br>        <modify start="false"/><br><br>    </inotify><br><br>       <!--sersync命令的配置信息--><br><br>    <sersync><br><br>    <!--设置监控的目录，该目录是本地要进行同步的目录，如果要监控多个目录，可以创建多个xml配置文件，启动多个sersync服务--><br><br>        <localpath watch="/tmp"><br><br>        <!--设置远程服务器的IP与rsync配置文件中同步的模块，web即为模块名--><br><br>            <remote ip="192.168.75.122" name="web"/><br><br>            <!--<remote ip="192.168.8.39" name="tongbu"/>--><br><br>            <!--<remote ip="192.168.8.40" name="tongbu"/>--><br><br>        </localpath><br><br>        <!--rsync命令的配置信息--><br><br>        <rsync><br><br>               <!--默认参数，t保留文件时间属性，u源文件比备份文件新则执行，z传输压缩--><br><br>            <commonParams params="-artuz"/><br><br>            <!--开启认证，默认没有开启（将false改为true），然后指定认证的文件--><br><br><auth start="true" users="rsync" passwordfile="/etc/rsync.passwd"/><br><br>                     <!--设置自定义端口，默认关闭--><br><br>            <userDefinedPort start="false" port="874"/><!-- port=874 --><br><br>            <!--设置超时时间，默认为100s--><br><br>            <timeout start="true" time="100"/><!-- timeout=100 --><br><br>            <!--使用ssh协议进行传输，那么关闭前面的auth认证，并且将name改为路径，不是模块名，远程服务器不需要启动rsync服务--><br><br>            <ssh start="false"/><br><br>        </rsync> <br><br>       <failLog path="/tmp/rsync_fail_log.sh" timeToExecute="60"/><!--default every 60mins execute once--><br><br>       <!--是否开启计划任务，默认关闭（false）,开启则表示每过600分钟，进行一次全同步--><br><br>        <crontab start="true" schedule="600"><!--600mins--><br><br>        <!--因为是全同步，所以如果前面有开启过滤文件功能，这里也应该开启，否则会将不应该同步的文件也进行同步，默认为不开启（false），如果开启了，下面设置的过滤规则尽量与上面的过滤规则保持一致。--><br><br>            <crontabfilter start="false"><br><br>                <exclude expression="*.php"></exclude><br><br>                <exclude expression="info/*"></exclude><br><br>            </crontabfilter><br><br>        </crontab><br><br>                <plugin start="false" name="command"/><br><br>    </sersync><br><br>     </head><br><br># 启动sersync：<br><br>/usr/local/sersync/sersync2 -d -r -o /usr/local/sersync/confxml.xml<br><br>-d: daemon模式，后台运行<br><br>-r: 全同步<br><br>-o： 指定启动的配置文件<br><br># 查看进程：<br><br>ps -ef \|grep sersync<br><br>root      75488      1  0 01:04 ?        00:00:00 /usr/local/sersync/sersync2 -d -r -o /usr/local/sersync/confxml.xml<br><br>root      75503  74825  0 01:05 pts/5    00:00:00 grep --color=auto sersync<br><br># 创建认证文件：<br><br>echo “123456” > /etc/rsync.passwd<br><br># 设置为其他人不可读：<br><br>chmod 600  /etc/rsync.passwd|

## 11.5   Rsync基于Inotify实时同步

Inotify 是一个 Linux特性，它监控文件系统操作，比如读取、写入和创建。Inotify 反应灵敏，用法非常简单，并且比cron 任务的繁忙轮询高效得多。

Rsync安装完毕后，需要安装inotify文件检查软件。同时为了同步的时候不需要输入密码，这样可以使用ssh免密钥方式进行同步。

1)      安装inotify-tools工具

|wget -c [https://jaist.dl.sourceforge.net/project/inotify-tools/inotify-tools/3.13/inotify-tools-3.13.tar.gz](https://jaist.dl.sourceforge.net/project/inotify-tools/inotify-tools/3.13/inotify-tools-3.13.tar.gz)<br><br>tar –xzf  inotify-tools-3.14.tar.gz<br><br>./configure<br><br>make <br><br>make install|
|---|

2)      配置auto_inotify.sh同步脚本

|#!/bin/sh <br><br>src=/data/webapps/www/<br><br>des=/var/www/html/<br><br>ip=192.168.0.100<br><br>inotifywait -mrq --timefmt '%d/%m/%y-%H:%M' --format '%T %w%f' -e modify,delete,create,attrib,access,move ${src} \| while read file <br><br>do<br><br>  rsync   -aP   --delete  $src   root@$ip:$des   <br><br>done|
|---|

3)      服务器端后台运行该脚本，此时服务器端目录新建或者删除，客户端都会实时进行相关操作。

|nohup  sh  auto_inotify.sh &|
|---|

# 第12章        Linux文件服务器企业实战

运维和管理企业Linux服务器，除了要熟练Linux系统本身的维护和管理之外，最重要的是熟练甚至精通基于Linux系统安装配置各种应用软件，对软件进行调优以及软件在使用中遇到各类问题，能够快速定位并解决问题。

本章向读者介绍进程、线程、企业Vsftpd服务器实战、匿名用户访问、系统用户访问及虚拟用户实战等。

## 12.1  进程与线程概念及区别

Linux系统各种软件和服务，存在于系统，必然会占用系统各种资源，系统资源是如何分配及调度的呢，本节将给读者展示系统进程、资源及调度相关的内容。

进程（Process）是计算机中的软件程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是[操作系统](http://baike.baidu.com/view/880.htm)结构的基础。

在早期面向进程设计的计算机结构中，进程是程序的基本执行实体，在当代面向线程设计的计算机结构中，进程是线程的容器。软件程序是对指令、数据及其组织形式的描述，而进程是程序的实体，通常而言，把运行在系统中的软件程序称之为进程。

除了进程，读者通常会听到线程的概念，线程也被称为轻量级进程(Lightweight Process，LWP），是程序执行流的最小单元。一个标准的线程由线程ID，当前指令[指针](http://baike.baidu.com/view/159417.htm)(PC），[寄存器](http://baike.baidu.com/view/6159.htm)集合和[堆栈](http://baike.baidu.com/view/93201.htm)组成。

线程是进程中的一个实体，是被系统独立调度和分派的基本单位，线程自己不拥有操作系统资源，但是该线程可与同属进程的其它线程共享该进程所拥有的全部资源。

程序、进程、线程三者区别如下：

q  程序：程序并不能单独执行，是静止的，只有将程序加载到内存中，系统为其分配资源后才能够执行；

q  进程：程序对一个数据集的动态执行过程，一个进程包含一个或者更多的线程，一个线程同时只能被一个进程所拥有，进程是分配资源的基本单位。进程拥有独立的内存单元，而多个线程共享内存，从而提高了应用程序的运行效率。

q  线程：线程是进程内的基本调度单位，线程的划分尺度小于进程，并发性更高，线程本身不拥有系统资源, 但是该线程可与同属进程的其它线程共享该进程所拥有的全部资源。每一个独立的线程，都有一个程序运行的入口、顺序执行序列、和程序的出口。

如图12-1所示，程序、进程、线程三者的关系拓扑图：

![](学习笔记/Attachments/1737817677069-9ab182be-3ca3-4b9c-a7b6-4640ef801daf.png)图12-1  程序、进程、线程关系图

如上图12-1所示，多进程、多线程的区别如下：

q  多进程，每个进程互相独立，不影响主程序的稳定性，某个子进程崩溃对其他进程没有影响，通过增加CPU可以扩充软件的性能，可以减少线程加锁/解锁的影响，极大提高性能。缺点是多进程逻辑控制复杂，需要和主程序交互，需要跨进程边界，进程之间上下文切换比线程之间上下文切换代价大。

q  多线程，无需跨进程，程序逻辑和控制方式简单，所有线程共享该进程的内存和变量等。缺点是每个线程与主程序共用地址空间，线程之间的同步和加锁控制比较麻烦，一个线程的崩溃会影响到整个进程或者程序的稳定性。

## 12.2  Vsftpd服务器企业实战

文件传输协议（File Transfer Protocol，FTP），基于该协议FTP客户端与服务端可以实现共享文件、上传文件、下载文件。 FTP 基于[TCP](http://baike.baidu.com/subview/32754/8048820.htm)协议生成一个虚拟的连接，主要用于控制FTP连接信息，同时再生成一个单独的TCP连接用于FTP[数据传输](http://baike.baidu.com/view/875888.htm)。用户可以通过客户端向FTP服务器端上传、下载、删除文件，FTP服务器端可以同时提供给多人共享使用。

FTP服务是Client/Server（简称C/S）模式，基于FTP协议实现FTP文件对外共享及传输的软件称之为FTP服务器源端，客户端程序基于FTP协议，则称之为FTP客户端，FTP客户端可以向FTP服务器上传、下载文件。

### 12.2.1        FTP传输模式

FTP基于C/S模式，FTP客户端与服务器端有两种传输模式，分别是FTP主动模式、FTP被动模式，主被动模式均是以FTP服务器端为参照。主被动模式如图12-2（a）、12-2（b）所示，主被动模式详细区别如下：

q  FTP主动模式：客户端从一个任意的端口N（N>1024）连接到FTP服务器的port 21命令端口，客户端开始监听端口N+1，并发送FTP命令“port N+1”到FTP服务器，FTP服务器以数据端口（20）连接到客户端指定的数据端口（N+1）。

q  FTP被动模式：客户端从一个任意的端口N（N>1024）连接到FTP服务器的port 21命令端口，客户端开始监听端口N+1，客户端提交 PASV命令，服务器会开启一个任意的端口（P >1024），并发送PORT P命令给客户端。客户端发起从本地端口N+1到服务器的端口P的连接用来传送数据。

在企业实际环境中，如果FTP客户端与FTP服务端均开放防火墙，FTP需以主动模式工作，这样只需要在FTP服务器端防火墙规则中，开放20、21端口即可。关于防火墙配置后面章节会讲解。

![](学习笔记/Attachments/1737817677316-26a7e9ca-5363-4ea8-a3d0-f960a18f1522.png)

图12-2（a） FTP主动模式

![](学习笔记/Attachments/1737817677507-4f084e1a-4feb-4bac-97e7-36140e678c8c.png)图12-2（b） FTP被动模式

### 12.2.2        Vsftpd服务器简介

目前主流的FTP服务器端软件包括：Vsftpd、ProFTPD、PureFTPd、Wuftpd、Server-U FTP、 FileZilla Server等软件，其中Unix/Linux使用较为广泛的FTP服务器端软件为Vsftpd 。

非常安全的FTP服务进程（Very Secure FTP daemon，Vsftpd），Vsftpd在Unix/[Linux](http://www.ha97.com/category/linux)发行版中最主流的FTP服务器程序，优点小巧轻快，安全易用、稳定高效、满足企业跨部门、多用户的使用（1000用户）等。

Vsftpd基于GPL开源协议发布，在中小企业中得到广泛的应用，Vsftpd可以快速上手，基于Vsftpd虚拟用户方式，访问验证更加安全。Vsftpd还可以基于MYSQL数据库做安全验证，多重安全防护。

### 12.2.3        Vsftpd服务器安装配置

Vsftpd服务器端安装有两种方法，一是基于YUM方式安装，而是基于源码编译安装，最终实现效果完全一致，本文采用YUM安装Vsftpd，步骤如下：

（1）      在命令行执行如下命令，如图12-3所示：

|yum   install    vsftpd*   -y|
|---|

![](学习笔记/Attachments/1737817677874-4d012f7d-ab71-4af4-bc59-4a168ec59256.png)图12-3 YUM安装Vsftpd服务端

（2）      打印vsftpd安装后的配置文件路径、启动Vsftpd服务及查看进程是否启动，如图12-4所示：

|rpm   -ql    vsftpd\|more<br><br>systemctl  restart  vsftpd.service<br><br>ps  -ef \|grep  vsftpd|
|---|

![](学习笔记/Attachments/1737817678047-08025c52-143d-4dcf-b1a1-6c11e40dc99c.png)图12-4 打印Vsftpd软件安装后路径

（3）      Vsftpd.conf默认配置文件详解如下：

|anonymous_enable=YES       开启匿名用户访问；<br><br>local_enable=YES               启用本地系统用户访问；<br><br>write_enable=YES               本地系统用户写入权限；<br><br>local_umask=022               本地用户创建文件及目录默认权限掩码；<br><br>dirmessage_enable=YES       打印目录显示信息，通常用于用户第一次访问目录时，信息提示；<br><br>xferlog_enable=YES             启用上传/下载日志记录；  <br><br>connect_from_port_20=YES   FTP使用20端口进行数据传输；<br><br>xferlog_std_format=YES        日志文件将根据xferlog的标准格式写入；<br><br>listen=NO                     Vsftpd不以独立的服务启动，通过Xinetd服务管理，建议改成YES；<br><br>listen_ipv6=YES                   启用IPV6监听；<br><br>pam_service_name=vsftpd        登录FTP服务器，依据/etc/pam.d/vsftpd中内容进行认证；<br><br>userlist_enable=YES             vsftpd.user_list和ftpusers配置文件里用户禁止访问FTP；<br><br>tcp_wrappers=YES                   设置vsftpd与tcp wrapper结合进行主机的访问控制，Vsftpd服务器检查/etc/hosts.allow 和/etc/hosts.deny中的设置，来决定请求连接的主机，是否允许访问该FTP服务器。|
|---|

（4）      启动Vsftpd服务后，通过Windows客户端资源管理器访问Vsftp服务器端，如图12-5所示：

|ftp://192.168.111.131/|
|---|

![](学习笔记/Attachments/1737817678240-1c7e4ff0-38aa-441e-a35f-df88b58e4abb.png)图12-5 匿名用户访问FTP默认目录

FTP主被动模式，默认为主动模式，设置为被动模式使用端口方法如下：

|pasv_enable=YES<br><br>pasv_min_port=60000<br><br>pasv_max_port=60100|
|---|

### 12.2.4        Vsftpd匿名用户配置

Vsftpd默认以匿名用户访问，匿名用户默认访问的FTP服务器端路径为：/var/ftp/pub，匿名用户只有查看权限，无法创建、删除、修改。如需关闭FTP匿名用户访问，需修改配置文件/etc/vsftpd/vsftpd.conf，将anonymous_enable=YES修改为anonymous_enable=NO，重启Vsftpd服务即可。

如果允许匿名用户能够上传、下载、删除文件，需在/etc/vsftpd/vsftpd.conf配置文件中加入如下代码：

|anon_upload_enable=YES             允许匿名用户上传文件；<br><br>anon_mkdir_write_enable=YES         允许匿名用户创建目录；<br><br>anon_other_write_enable=YES         允许匿名用户其他写入权限。|
|---|

匿名用户完整vsftpd.conf配置文件代码如下：

|anonymous_enable=YES<br><br>local_enable=YES<br><br>write_enable=YES<br><br>local_umask=022<br><br>anon_upload_enable=YES<br><br>anon_mkdir_write_enable=YES<br><br>anon_other_write_enable=YES<br><br>dirmessage_enable=YES<br><br>xferlog_enable=YES<br><br>connect_from_port_20=YES<br><br>xferlog_std_format=YES<br><br>listen=NO<br><br>listen_ipv6=YES<br><br>pam_service_name=vsftpd<br><br>userlist_enable=YES<br><br>tcp_wrappers=YES|
|---|

由于默认Vsftpd匿名用户有两种：anonymous、ftp，所以匿名用户如果需要上传文件、删除及修改等权限，需要ftp用户对/var/ftp/pub目录有写入权限，使用如下chown和chmod任意一种即可，设置命令如下：

|chown      -R  ftp    pub/<br><br>chmod          o+w     pub/|
|---|

如上Vsftpd.conf配置文件配置完毕，同时权限设置完，重启vsftpd服务即可，通过Windows客户端访问，能够上传文件、删除文件、创建目录等操作，如图12-6所示：

![](学习笔记/Attachments/1737817678536-03f239c8-10d7-410b-b77b-c65ed78baaa9.png)

图12-6 匿名用户访问上传文件

### 12.2.5        Vsftpd系统用户配置

Vsftpd匿名用户设置完毕，匿名用户，任何人都可以查看FTP服务器端的文件、目录，甚至可以修改、删除，此方案如适合存放私密文件在FTP服务器端，如何保证文件或者目录专属拥有者呢，Vsftpd系统用户可以实现该需求。

实现Vsftpd系统用户方式验证，只需在Linux系统中创建多个用户即可，创建用户使用useradd，同时给用户设置密码，即可通过用户和密码登录FTP，进行文件上传、下载、删除等操作。Vsftpd系统用户实现方法步骤如下：

（1）      Linux系统中创建系统用户jfedu1、jfedu2，分别设置密码为123456：

|useradd   jfedu1<br><br>useradd   jfedu2<br><br>echo 123456\|passwd --stdin  jfedu1<br><br>echo 123456\|passwd --stdin  jfedu2|
|---|

（2）      修改vsftpd.conf配置文件代码如下：

|anonymous_enable=NO<br><br>local_enable=YES<br><br>write_enable=YES<br><br>local_umask=022<br><br>dirmessage_enable=YES<br><br>xferlog_enable=YES<br><br>connect_from_port_20=YES<br><br>xferlog_std_format=YES<br><br>listen=NO<br><br>listen_ipv6=YES<br><br>pam_service_name=vsftpd<br><br>userlist_enable=YES<br><br>tcp_wrappers=YES|
|---|

（3）      通过Windows资源客户端验证，使用jfedu1、jfedu2用户登录FTP服务器，即可上传文件、删除文件、下载文件，jfedu1、jfedu2系统用户上传文件的家目录在/home/jfedu1、/home/jfedu2下，如图12-7（a）、12-7（b）所示：

![](学习笔记/Attachments/1737817678699-c6b34de0-3f1c-45e4-b583-1fedf9ee60e7.png)图12-7（a） jfedu1用户登录FTP服务器

![](学习笔记/Attachments/1737817678934-a8fc9747-07fb-4c12-958c-bc52c70709c9.png)图12-7（b） jfedu1登录FTP服务器上传文件

### 12.2.6        Vsftpd虚拟用户配置

如果基于Vsftpd系统用户访问FTP服务器，系统用户越多越不利于管理，而且不利于系统安全管理，鉴于此，为了能更加的安全使用VSFTPD，需使用Vsftpd虚拟用户方式。

Vsftpd虚拟用户原理：虚拟用户就是没有实际的真实系统用户，而是通过映射到其中一个真实用户以及设置相应的权限来实现访问验证，虚拟用户不能登录Linux系统，从而让系统更加的安全可靠。

Vsftpd虚拟用户企业案例配置步骤如下：

（1）      安装Vsftpd虚拟用户需用到的软件及认证模块：

|yum  install  pam*  libdb-utils  libdb*  --skip-broken  -y|
|---|

（2）      创建虚拟用户临时文件/etc/vsftpd/ftpusers.txt，新建虚拟用户和密码，其中jfedu001、jfedu002为虚拟用户名，123456为密码，如果有多个用户，依次格式填写即可：

|jfedu001<br><br>123456<br><br>jfedu002<br><br>123456|
|---|

（3）      生成Vsftpd虚拟用户数据库认证文件，设置权限700：

|db_load  -T  -t  hash  -f  /etc/vsftpd/ftpusers.txt  /etc/vsftpd/vsftpd_login.db<br><br>chmod  700  /etc/vsftpd/vsftpd_login.db|
|---|

（4）      配置PAM认证文件，/etc/pam.d/vsftpd行首加入如下两行：

|auth      required        pam_userdb.so   db=/etc/vsftpd/vsftpd_login<br><br>account   required        pam_userdb.so   db=/etc/vsftpd/vsftpd_login|
|---|

（5）      所有Vsftpd虚拟用户需要映射到一个系统用户，该系统用户不需要密码，也不需要登录，主要用于虚拟用户映射使用，创建命令如下：

|useradd    -s   /sbin/nologin    ftpuser|
|---|

（6）      完整vsftpd.conf配置文件代码如下：

|#global config Vsftpd 2021<br><br>anonymous_enable=YES<br><br>local_enable=YES<br><br>write_enable=YES<br><br>local_umask=022<br><br>dirmessage_enable=YES<br><br>xferlog_enable=YES<br><br>connect_from_port_20=YES<br><br>xferlog_std_format=YES<br><br>listen=NO<br><br>listen_ipv6=YES<br><br>userlist_enable=YES<br><br>tcp_wrappers=YES<br><br>#config virtual user FTP<br><br>pam_service_name=vsftpd<br><br>guest_enable=YES<br><br>guest_username=ftpuser<br><br>user_config_dir=/etc/vsftpd/vsftpd_user_conf<br><br>virtual_use_local_privs=YES|
|---|

如上Vsftpd虚拟用户配置文件参数详解：

|#config virtual user FTP<br><br>pam_service_name=vsftpd                  虚拟用户启用pam认证；<br><br>guest_enable=YES                                   启用虚拟用户；<br><br>guest_username=ftpuser                          映射虚拟用户至系统用户ftpuser；<br><br>user_config_dir=/etc/vsftpd/vsftpd_user_conf   设置虚拟用户配置文件所在的目录；<br><br>virtual_use_local_privs=YES                       虚拟用户使用与本地用户相同的权限。|
|---|

（7）      至此，所有虚拟用户共同基于/home/ftpuser主目录实现文件上传与下载，可以在/etc/vsftpd/vsftpd_user_conf目录创建虚拟用户各自的配置文件，创建虚拟用户配置文件主目录:

|mkdir  -p    /etc/vsftpd/vsftpd_user_conf/|
|---|

（8）      如下分别为虚拟用户jfedu001、jfedu002用户创建配置文件：

vim /etc/vsftpd/vsftpd_user_conf/jfedu001，同时创建私有的虚拟目录，代码如下：

|local_root=/home/ftpuser/jfedu001<br><br>write_enable=YES<br><br>anon_world_readable_only=YES<br><br>anon_upload_enable=YES<br><br>anon_mkdir_write_enable=YES<br><br>anon_other_write_enable=YES|
|---|

vim /etc/vsftpd/vsftpd_user_conf/jfedu002，同时创建私有的虚拟目录，代码如下：

|local_root=/home/ftpuser/jfedu002<br><br>write_enable=YES<br><br>anon_world_readable_only=YES<br><br>anon_upload_enable=YES<br><br>anon_mkdir_write_enable=YES<br><br>anon_other_write_enable=YES|
|---|

虚拟用户配置文件内容详解：

|local_root=/home/ftpuser/jfedu002      jfedu002虚拟用户配置文件路径；<br><br>write_enable=YES                          允许登陆用户有写权限；        <br><br>anon_world_readable_only=YES            允许匿名用户下载，然后读取文件；<br><br>anon_upload_enable=YES                    允许匿名用户上传文件权限，只有在write_enable=YES时该参数才生效；<br><br>anon_mkdir_write_enable=YES            允许匿名用户创建目录，只有在write_enable=YES时该参数才生效；<br><br>anon_other_write_enable=YES          允许匿名用户其他权限，例如删除、重命名等。|
|---|

（9）      创建虚拟用户各自虚拟目录：

|mkdir -p /home/ftpuser/{jfedu001,jfedu002} ；chown -R ftpuser:ftpuser /home/ftpuser|
|---|

重启Vsftpd服务，通过Windows客户端资源管理器登录Vsftpd服务端，测试结果如图12-8（a）、12-8（b）所示：

![](学习笔记/Attachments/1737817679111-eb3804dd-8d1c-4b7a-a7b4-04b6b1eed0ee.png)图12-8（a） jfedu001虚拟用户登录FTP服务器

![](学习笔记/Attachments/1737817679335-ba4e95d8-da45-4e8e-a428-f531b0c479a5.png)图12-8（b） jfedu001虚拟用户上传下载文件

# 第13章       大数据备份企业实战

随着互联网不断的发展,企业对运维人员的能力要求也越来越高,尤其是要求运维人员能处理各种故障、专研自动化运维技术、云计算机、虚拟化等，满足公司业务的快速发展。

本章向读者介绍数据库备份方法、数量量2T及以上级别数据库备份方案、xtrabackup企业工具案例演示、数据库备份及恢复实战等。

## 13.1  企业级数据库备份实战

在日常的运维工作中，数据是公司非常重要的资源，尤其是数据库的相关信息，如果数据丢了，少则损失几千元，高则损失上千万。所以在运维工作中要及时注意网站数据的备份，尤其要及时对数据库进行备份。

企业中如果数据量上T级别，维护和管理是非常复杂的，尤其是对数据库进行备份操作。

## 13.2  数据库备份方法及策略

企业中MySQL数据库备份最常用的方法如下：

q  直接cp备份

q  Sqlhotcopy

q  主从同步复制

q  Mysqldump备份

q  Xtrabackup备份

Mysqldump和Xtrabackup均可以备份MYSQL数据，如下为Mysqldump工具使用方法：

通常小于100G的MYSQL数据库可以使用默认Mysqldump备份工具进行备份，如果超过100G的大数据，由于Mysqldump备份方式是采用的逻辑备份，最大的缺陷是备份和恢复速度较慢。

基于Mysqldump备份耗时会非常长，而且备份期间会锁表，锁表直接导致数据库只能访问Select，不能执行Insert、Update等操作，进而导致部分WEB应用无法写入新数据。

如果是Myisam引擎表，当然也可以执行参数--lock-tables=false禁用锁表，但是有可能造成数据信息不一致。

如果是支持事务的表，例如InnoDB和BDB，--single-transaction参数是一个更好的选择，因为它不锁定表。

|mysqldump -uroot -p123456 --all-databases --opt --single-transaction > 2021all.sql|
|---|

其中--opt快捷选项，等同于添加--add-drop-tables --add-locking --create-option --disable-keys --extended-insert --lock-tables --quick --set-charset选项。

本选项能让 Mysqldump 很快的导出数据，并且导出的数据能很快导回。该选项默认开启，但可以用--skip-opt 禁用。

如果运行 Mysqldump没有指定--quick 或 --opt 选项，则会将整个结果集放在内存中。如果导出大数据库的话可能会导致内存溢出而异常退出。

## 13.3  Xtrabackup企业实战

Mysql冷备、Mysqldump、Mysql热拷贝均不能实现对数据库进行增量备份，在实际环境中增量备份非常的实用，如果数据量小于100G，存储空间足够，可以每天进行完整备份，如果每天产生的数据量大，需要定制数据备份策略例如：每周日使用完整备份，周一到周六使用增量备份，或者每周六完整备份，周日到周五使用增量备份。

Percona-xtrabackup是为实现增量备份而生一款主流备份工具，Xtrabackup有两个主要的工具，分别为：xtrabackup、innobackupex。

[Percona XtraBackup](https://www.percona.com/software/mysql-database/percona-xtrabackup)是 Percona 公司开发的一个用于 MySQL 数据库物理热备的备份工具，支持 MySQl、Percona Server及MariaDB，开源免费，是目前互联网数据库备份最主流的工具之一。

Xtrabackup只能备份InnoDB和XtraDB两种数据引擎的表，而不能备份MyISAM数据表，Innobackupex-1.5.1则封装了Xtrabackup，是一个封装好的脚本，使用该脚本能同时备份处理innodb和Myisam，但在处理Myisam时需要加一个读锁。

XtraBackup备份原理，Innobackupex在后台线程不断追踪InnoDB的日志文件，然后复制InnoDB的数据文件。数据文件复制完成之后，日志的复制线程也会结束。这样就得到了不在同一时间点的数据副本和开始备份以后的事务日志。完成上面的步骤之后，就可以使用InnoDB崩溃恢复代码执行事务日志（Redo log），以达到数据的一致性。其备份优点如下：

q  备份速度快，物理备份更加可靠；

q  备份过程不会打断正在执行的事务，无需锁表；

q  能够基于压缩等功能节约磁盘空间和流量；

q  自动备份校验；

q  还原速度快；

q  可以流传将备份传输到另外一台机器上；

q  节约磁盘空间和网络带宽。

Innobackupex工具的备份过程原理，如图13-1所示：

![](学习笔记/Attachments/1737817679517-52352171-7847-4f7d-9aa8-b0336db07bc4.png)

图13-1 Innobackupex备份过程

Innobackupex备份过程中首先启动Xtrabackup_log后台检测的进程，实时检测Mysql redo的变化，一旦发现Redo有新的日志写入，立刻将日志写入到日志文件Xtrabackup_log中，并复制Innodb的数据文件和系统表空间文件idbdata1到备份目录。

Innode引擎表备份完之后，执行Flush table with read lock操作进行MyIsam表备份。拷贝.frm .myd .myi文件，并且在这一时刻获得binary log的位置，将表进行解锁unlock tables，停止Xtrabackup_log进程，完整整个数据库的备份。

## 13.4  Percona-xtrabackup备份实战

基于Percona-xtrabackup备份，需要如下几个步骤：

（1）      官网下载Percona-Xtrabackup

|Percona官方wiki使用帮助：<br><br>[https://www.percona.com/doc/percona-xtrabackup/LATEST/index.html](https://www.percona.com/doc/percona-xtrabackup/LATEST/index.html)|
|---|

（2）      Percona-xtrabackup安装方法如下：

|# 安装Percona XtraBackup仓库：<br><br>yum  install [https://repo.percona.com/yum/percona-release-latest.noarch.rpm](https://repo.percona.com/yum/percona-release-latest.noarch.rpm) -y<br><br># 安装xtrabackup：<br><br>yum  install  percona-xtrabackup-24  -y|
|---|

（3）      MYSQL数据库全备份：

|[root@www-jfedu-net ~]# xtrabackup  --backup --target-dir=/data/backups/<br><br># 看到completed OK，表示备份完成，如果遇到报错，根据错误提示解决：<br><br>210907 15:49:12 [00] Writing /data/backups/backup-my.cnf<br><br>210907 15:49:12 [00]        ...done<br><br>210907 15:49:12 [00] Writing /data/backups/xtrabackup_info<br><br>210907 15:49:12 [00]        ...done<br><br>xtrabackup: Transaction log of lsn (1597945) to (1597945) was copied.<br><br>210907 15:49:12 completed OK!|
|---|

（4）      xtrabackup数据库恢复，恢复前先保证数据一致性，执行如下命令，如图13-3所示：

|[root@www-jfedu-net ~]# xtrabackup  --prepare --target-dir=/data/backups/<br><br># 看到ompleted OK，表示准备成功：<br><br>xtrabackup: starting shutdown with innodb_fast_shutdown = 1<br><br>InnoDB: FTS optimize thread exiting.<br><br>InnoDB: Starting shutdown...<br><br>InnoDB: Shutdown completed; log sequence number 1598504<br><br>210907 15:52:32 completed OK!|
|---|

通常数据库备份完成后，数据尚不能直接用于恢复操作，因为备份的数据时是一个过程，在备份过程中，有任务会写入数据，可能会包含尚未提交的事务或已经提交但尚未同步至数据文件中的事务。

因此此时数据文件仍处理不一致状态，基于--prepare可以通过回滚未提交的事务及同步已经提交的事务至数据文件使数据文件处于一致性状态，方可进行恢复数据。

（5）      删除原数据目录/var/lib/mysql数据，使用参数--copy-back恢复完整数据，授权mysql用户给所有的数据库文件，如图13-4所示：

|# 恢复数据，数据库的datadir必须为空，其应该关闭mysql服务器。<br><br>[root@www-jfedu-net ~]#   rm  -rf  /var/lib/mysql/*<br><br>[root@www-jfedu-net ~]# xtrabackup --copy-back --target-dir=/data/backups/<br><br># 看到completed OK，表示恢复成功！<br><br>210907 15:55:05 [01] Copying ./xtrabackup_info to /var/lib/mysql/xtrabackup_info<br><br>210907 15:55:05 [01]        ...done<br><br>210907 15:55:05 [01] Copying ./xtrabackup_master_key_id to /var/lib/mysql/xtrabackup_master_key_id<br><br>210907 15:55:05 [01]        ...done<br><br>210907 15:55:05 [01] Copying ./ibtmp1 to /var/lib/mysql/ibtmp1<br><br>210907 15:55:05 [01]        ...done<br><br>210907 15:55:05 completed OK!<br><br># 对数据目录重新授权：<br><br>chown  –R  mysql.  /var/lib/mysql/<br><br># 启动数据库服务：<br><br>[root@www-jfedu-net ~]# systemctl start mariadb<br><br>[root@www-jfedu-net ~]# ps -ef  \| grep mysql<br><br>mysql      4436      1  0 16:00 ?        00:00:00 /bin/sh /usr/bin/mysqld_safe --basedir=/usr<br><br>mysql      4598   4436  0 16:00 ?        00:00:00 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --log-error=/var/log/mariadb/mariadb.log --pid-file=/var/run/mariadb/mariadb.pid --socket=/var/lib/mysql/mysql.sock<br><br>root       4634   1120  0 16:00 pts/0    00:00:00 grep --color=auto mysql|
|---|

![](学习笔记/Attachments/1737817679736-053c09c5-5425-4e31-b969-d1d44d9c8bc3.png)

图13-4 xtrabackup 数据恢复

查看数据库恢复信息，数据完全恢复，如图13-5所示：

![](学习笔记/Attachments/1737817679931-d253d9d9-5284-4663-9e69-7410f7742a4c.png)

图13-5 xtrabackup 数据恢复

## 13.5  Innobackupex增量备份

增量备份仅能应用于InnoDB或XtraDB表，对于MyISAM表而言，执行增量备份时其实进行的是完全备份。

（1）      增量备份之前必须执行完全备份，如图13-6所示：

|innobackupex --user=root --password=123456 --databases=wugk01 /data/backup/mysql/|
|---|

![](学习笔记/Attachments/1737817680106-cacf153a-c739-4263-b961-8b60c6e36467.png)

图13-6 Innobackupex 完整备份

（2）      执行第一次增量备份：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456 --databases=wugk01 --incremental /data/backup/mysql/ --incremental-basedir=/data/backup/mysql/2021-12-20_13-01-43/|
|---|

增量备份完后，会在/data/backup/mysql/目录下生成新的备份目录，如图13-7所示：

![](学习笔记/Attachments/1737817680268-9df8c5c7-4713-4e97-810a-461cd5fd59d6.png)

图13-7 Innobackupex 增量备份

（3）      数据库插入新数据，如图13-8所示：

![](学习笔记/Attachments/1737817680438-c6c793a8-b292-4940-910e-6544d0f7dd12.png)

图13-8 数据库insert into新数据

（4）      执行第二次增量备份，备份命令如下，如图13-9（a）、16-9（b）：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456 --databases=wugk01 --incremental /data/backup/mysql/ --incremental-basedir=/data/backup/mysql/2021-12-20_13-07-31/|
|---|

![](学习笔记/Attachments/1737817680695-7fc8f15b-4700-49d9-a0b2-f40d6cab3679.png)

图13-9（a）数据库增量备份

![](学习笔记/Attachments/1737817680903-8618ffe9-7e95-424c-a01a-4a60625cd936.png)

图13-9（b）数据库增量备份

## 13.6  MySQL增量备份恢复

删除原数据库中表及数据记录信息，如图13-10所示：

![](学习笔记/Attachments/1737817681105-513be37e-f65d-4ea9-a394-cd95809925b2.png)

图13-10 删除数据库表信息

MYSQL增量备份数据恢复方法如下步骤：

（1）      基于Apply-log确保数据一致性：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456  --apply-log --redo-only /data/backup/mysql/2021-12-20_13-01-43/|
|---|

（2）      执行第一次增量数据恢复：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456  --apply-log --redo-only /data/backup/mysql/2021-12-20_13-01-43/ --incremental-dir=/data/backup/mysql/2021-12-20_13-07-31/|
|---|

（3）      执行第二次增量数据恢复：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456  --apply-log --redo-only /data/backup/mysql/2021-12-20_13-01-43/ --incremental-dir=/data/backup/mysql/2021-12-20_13-11-20/|
|---|

（4）      执行完整数据恢复：

|innobackupex --defaults-file=/etc/my.cnf --user=root --password=123456  --copy-back /data/backup/mysql/2021-12-20_13-01-43/|
|---|

（5）      测试数据库已完全恢复，如图13-11所示：

![](学习笔记/Attachments/1737817681296-1b6f7c43-815c-4fbd-b9b3-373bf23a0a5d.png)

图13-11 数据库表信息完整恢复

# 第14章       Kickstart企业系统部署实战

## 14.1  Kickstart使用背景介绍

随着公司业务不断增加，经常需要采购新服务器，并要求安装Linux系统，并且要求Linux版本要一致，方便以后的维护和管理，每次人工安装linux系统会浪费掉更多时间，如果我们有办法能节省一次一次的时间岂不更好呢？

大中型互联网公司一次采购服务器上百台,如果采用人工手动一台一台的安装,一个人得搞坏N张光盘,得多少个加班加点才能完成这项”艰巨”的任务呢,我们可以看到全人工来完成这样的工作太浪费人力了,有没有自动化安装平台呢,通过一台已存在的系统然后克隆或者复制到新的服务器呢。Kickstart可以毫不费力的完成这项工作。

PXE(preboot execute environment，预启动执行环境)是由[Intel公司](http://baike.baidu.com/view/8364.htm)开发的最新技术，工作于Client/Server的网络模式，支持[工作站](http://baike.baidu.com/view/7977.htm)通过网络从远端服务器下载映像，并由此支持通过网络启动[操作系统](http://baike.baidu.com/view/880.htm)，在启动过程中，终端要求服务器分配[IP](http://baike.baidu.com/view/8370.htm)地址，再用[TFTP](http://baike.baidu.com/view/23881.htm)（trivial file transfer protocol）协议下载一个启动[软件](http://baike.baidu.com/view/37.htm)包到本机内存中执行。

Kickstart安装平台，完整架构：Kickstart+DHCP+NFS(HTTP)+TFTP+PXE，从架构可以看出，大致需要安装的服务，例如Dhcp、Tftp、Httpd、Kickstart/pxe等。

## 14.2   Kickstart企业实战配置

1）基于CentOS7.x Linux操作系统，YUM安装DHCP、TFTP、HTTPD服务，操作指令如下：

|yum  install  httpd httpd-devel  tftp-server  xinetd  dhcp*   -y|
|---|

2）配置tftp服务，开启tftp服务，操作指令如下：

|cat>/etc/xinetd.d/tftp<<EOF<br><br>service tftp<br><br>{<br><br>disable = no<br><br>socket_type = dgram<br><br>protocol = udp<br><br>wait = yes<br><br>user = root<br><br>server = /usr/sbin/in.tftpd<br><br>server_args = -u nobody -s /tftpboot<br><br>per_source = 11<br><br>cps = 100 2<br><br>flags = IPv4<br><br>}<br><br>EOF|
|---|

3）只需要把disable = yes改成disable = no即可，也可以通过sed命令修改，命令如下：

|sed  -i  ‘/disable/s/yes/no/g’/etc/xinetd.d/tftp|
|---|

## 14.3  Kickstart TFTP+PXE实战

要实现远程Kickstart安装系统，需要在TFTPBOOT目录指定相关PXE内核模块及相关参数，操作方法和指令如下：

|#挂载本地光盘镜像；<br><br>mount /dev/cdrom /mnt<br><br>#安装syslinux必备文件；<br><br>yum install syslinux syslinux-devel -y<br><br>#将tftpboot目录软链接至/根目录；<br><br>ln -s  /var/lib/tftpboot  /<br><br>#创建pxelinux.cfg目录；<br><br>mkdir -p /var/lib/tftpboot/pxelinux.cfg/<br><br>#拷贝必备配置文件；<br><br>\cp /mnt/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default<br><br>\cp /usr/share/syslinux/vesamenu.c32 /var/lib/tftpboot/<br><br>\cp /mnt/images/pxeboot/vmlinuz /var/lib/tftpboot/<br><br>\cp /mnt/images/pxeboot/initrd.img /var/lib/tftpboot/<br><br>\cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/<br><br>#授权default文件权限；<br><br>chmod 644 /var/lib/tftpboot/pxelinux.cfg/default|
|---|

## 14.4  配置Tftpboot引导案例

Tftpboot相关目录和文件创建之后，接下来需要创建default，并且写入如下内容，该文件是Kickstart安装系统核心文件之一。操作指令如下：

|cat>/tftpboot/pxelinux.cfg/default<<EOF<br><br>default vesamenu.c32<br><br>timeout 10<br><br>display boot.msg<br><br>menu clear<br><br>menu background splash.png<br><br>menu title CentOS Linux 7<br><br>label linux<br><br>  menu label ^Install CentOS Linux 7<br><br>  menu default<br><br>  kernel vmlinuz<br><br>  append initrd=initrd.img inst.repo=http://192.168.0.131/centos7 quiet ks=http://192.168.0.131/ks.cfg<br><br>label check<br><br>  menu label Test this ^media & install CentOS Linux 7<br><br>  kernel vmlinuz<br><br>  append initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 rd.live.check quiet<br><br>EOF|
|---|

Default配置文件详解：

|#192.168.0.131是kickstart服务器;<br><br>#centos7是HTTPD共享linux镜像的目录，即linux存放安装文件的路径：<br><br>#ks.cfg是kickstart主配置文件;<br><br>#设置timeout 10 /*超时时间为10S */;<br><br>#ksdevice=ens33代表当我们有多块网卡的时候，要实现自动化需要设置从ens33安装。<br><br>#TFTP配置完毕，由于是TFTP是非独立服务，需要依赖xinetd服务来启动，启动命令为：<br><br>chkconfig    tftp  --level 35 on  && service  xinetd  restart|
|---|

## 14.5  Kickstart+Httpd配置      

Kickstart远程系统安装，客户端需要下载系统所需的软件包，所以需要使用NFS或者httpd把镜像文件共享出来。

|mkdir -p /var/www/html/centos7/<br><br>mount /dev/cdrom /var/www/html/centos7/<br><br>#cp /dev/cdrom/*  /var/www/html/centos7/|
|---|

     配置Kickstart，可以使用system-kickstart系统软件包来配置，ks.cfg配置文件内容如下：

|cat>/var/www/html/ks.cfg<<EOF<br><br>install<br><br>text<br><br>keyboard 'us'<br><br>rootpw www.jfedu.net<br><br>timezone Asia/Shanghai<br><br>url --url=http://192.168.0.131/centos7<br><br>reboot<br><br>lang zh_CN<br><br>firewall --disabled<br><br>network  --bootproto=dhcp --device=ens33<br><br>auth  --useshadow  --passalgo=sha512<br><br>firstboot --disable<br><br>selinux   disabled<br><br>bootloader --location=mbr<br><br>clearpart --all --initlabel<br><br>part /boot --fstype="ext4" --size=300<br><br>part / --fstype="ext4" --grow<br><br>part swap --fstype="swap" --size=512<br><br>%packages<br><br>@base<br><br>@core<br><br>%end<br><br>EOF|
|---|

## 14.6  DHCP服务配置实战

1）DHCP服务配置文件代码如下：

|cat>/etc/dhcp/dhcpd.conf<<EOF<br><br>ddns-update-style interim;<br><br>ignore client-updates;<br><br>next-server 192.168.0.131;<br><br>filename "pxelinux.0";<br><br>allow booting;<br><br>allow bootp;<br><br>subnet 192.168.0.0 netmask 255.255.255.0 {<br><br>#default gateway<br><br>option routers          192.168.0.1;<br><br>option subnet-mask      255.255.255.0;<br><br>range dynamic-bootp 192.168.0.180 192.168.0.200;<br><br>host ns {<br><br>hardware ethernet  00:1a:a0:2b:38:81;<br><br>fixed-address 192.168.0.101;}<br><br>}<br><br>EOF|
|---|

2）重启各个服务，启动新的客户端验证测试，操作指令如下：

|service httpd restart<br><br>service dhcpd restart<br><br>service xinetd restart|
|---|

## 14.7  Kickstart客户端案例

开启新虚拟机，同时设置虚拟机（客户端）BIOS启动项，设置为首选网卡启动，如图14-1（a）、14-1（b）、14-1（c）所示：

![](学习笔记/Attachments/1737817681491-716ae30f-47a1-4cf9-a2d1-1aca6af05e0c.png)

图14-1（a） BIOS设置

![](学习笔记/Attachments/1737817681652-ebe29c66-b00e-44bf-94ad-c74ddd16e92f.png)

图14-1（b） 客户端启动测试

![](学习笔记/Attachments/1737817681831-6b8fb2c3-2d22-4402-9d7b-c6ee9cacc4bb.png)

图14-1（c） 客户端启动测试

如果安装时报错，如图14-2所示：

![](学习笔记/Attachments/1737817682009-b092b582-a14a-4bb5-bfae-10777c80dfa6.png)

图14-2 客户端系统安装报错

如果报如上错误，需要调整客户端虚拟机的内存设置为2G+，然后再次启动客户端远程安装系统，如图14-3所示：

![](学习笔记/Attachments/1737817682182-1b9a388a-5035-4acd-9ca1-369b5b174486.png)

图14-3 客户端系统安装

## 14.8  Kickstart案例扩展

在真实环境中，通常我们会发现一台服务器好几块硬盘，做完raid，整个硬盘有等10T，如果来使用Kickstart自动安装并分区呢；一般服务器硬盘超过2T，如何来使用Kickstart安装配置呢？这里就不能使用MBR方式来分区，需要采用GPT格式来引导并分区。需要在ks.cfg末尾添加如下命令来实现需求：

|%pre  <br>parted  -s  /dev/sdb  mklabel  gpt  <br>%end|
|---|

为了实现Kickstart安装完系统后，自动初始化系统等等工作，我们可以在系统安装完后，自动执行定制的脚本，需要在ks.cfg末尾加入如下配置：

|%post<br><br>mount  -t  nfs 192.168.0.131:/centos/init   /mnt<br><br>cd  /mnt/ ;/bin/sh  auto_init.sh<br><br>%end|
|---|

Kickstart所有配置就此告一段落，真实环境需要注意，新服务器跟Kickstart最后独立在一个网络，不要跟办公环境或者服务器机房网络混在一起，如果别的机器以网卡就会把它的系统重装成Linux系统。