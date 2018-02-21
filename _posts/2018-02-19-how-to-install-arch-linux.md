---
layout:     post
title:      "Arch Linux 装机全攻略"
subtitle:   " \"详细记录archlinux安装的每一步，linux零基础用户也看得懂\""
date:       2018-02-19 19:12:00
author:     "Zhang Shuyang"
header-img: "img/post-bg-2015.jpg"
tags:
    - linux
    - arch
    - archlinux
---

#### 0.前言

本文最初写于2012年11月6日，发表在我的人人网主页，由于人人凉了，之后重新发表在了我的新浪博客上，这次是第三次重新发表了，因为我觉得 Jekyll+GitHub pages非常酷！以后的技术类博客和一些生活随想也会发表在这里。本文适合各类linux用户查看（从一无所知到高端），写作的主要目是对自己装Archlinux的过程进行记录，方便自己日后查阅，也方便其他喜欢linux或准备学习linux或者体验过其他发行版的linux的用户安装Archlinux时的参考。

#### 1.什么是Linux？

Linux是一种自由和开放源码的类Unix操作系统，存在着许多不同的Linux版本，但它们都使用了Linux内核。绝大多数服务器架设均使用Linux，其优点主要主要是免费，开源，安全，稳定，多平台。

#### 2.什么是ArchLinux？

Linux有很多发行版，比如大名鼎鼎的ubuntu和redhat，Arch是其中一种，Arch的设计思想是：简洁！Arch Linux 将简洁定义为：去除任何不必要的添加、修改和复杂，提供一个轻量级的类 Unix 系统，每个用户都可以以此为基础，打造出适合自己的系统。简而言之，即优雅、极简之道。一个新安装的 Arch Linux 系统仅包含基本的核心组件，不会执行任何自动配置。用户可以按自己意愿配置系统。从一开始，每个系统组件都可以很容易立即删除或者用其它组件替代。Arch Linux 软件仓库里大量的软件包也让你有更多的自由选择。仓库中既提供了开源、自由的软件，也提供了闭源软件给想使用它们的人。一切都由用户自己决定。正如 Arch Linux 项目的创立者 Judd Vinet 所说：“它（Arch）完全由你自己塑造而成。"



#### 3.我从未接触过Linux系统，可以配置属于自己的ArchLinux吗？

Arch是一个简单、轻量级、适合计算机水平较高用户使用的发行版。虽然这么说，但是绝不影响你将Arch作为你的第一个Linux发行版来安装。就Linux各个发行版的上手程度上来说，从简单到难顺序大致为：ubuntu，debian，fedora，redhat，SUSE，centOS，arch，slackware，gentoo。而且arch，slackware和gentoo的用户亲和力比较差，请做好心理准备，但是看了本帖相信就算你计算机水平较低也能顺利配置好你的Arch。



#### 4.安装一个ArchLinux大约需要多久？

可以这么说，ArchLinxu大概就像一个空操场，安装时间要根据你准备安装什么设施决定，不过如果只想给操场上铺一层塑胶，安装一些简单的锻炼器材，大约只需要15-20分钟（当然要求你对linux命令和arch相对熟悉），如果完全零基础按照本帖来做，大约1小时即可完成。



#### 5.我可以安装ArchLinux和Windows双系统吗？

当然可以，我就是Arch+Win8+Win7三系统，没有任何问题，不过需要一些软件来实现，下文会提到。如果你实在害怕你把Windows玩坏的话，可以尝试安装虚拟机。（我在安装arch之前也是先在虚拟机上安装，然后在U盘上安装，后来尝试在硬盘上安装，其中硬盘安装前前后后反反复复安了3次，最后一次非常完美，用时20min。）



**不多说废话了，切入正题。**

> 首先要明确Arch是基于网络安装的，请保证你的网络通畅（如果有ipv6更好，速度会更快）



#### 1.准备


下载Arch的最新镜像并制作安装介质：Arch的官网archlinux.org右上角的download既可下载其最新iso镜像，并使用ultraliso等工具刻录到你的U盘/移动硬盘/光盘等硬件载体上，如果会使用虚拟光驱当然更好（由于制作安装介质属于准备工作因此不赘述，其方法与制作windows安装盘类似，可以直接百度iso安装盘制作指南）

准备一个硬盘分区用来安装Arch：制作分区可以用很多windows平台的软件，因此也不赘述了，不过需要提醒的是，linux的文件管理和windows不同，linux可以访问windows的硬盘分区而windows不能访问linux的硬盘分区，这一点请注意！因此一定要防止重要文件丢失，以免得不偿失。



#### 2.格式化分区


现在进入插上你刚制作好的安装介质（U盘/光盘/移动硬盘），重启计算机，如果是U盘或移动硬盘的话，需要改变BIOS从U盘/移动硬盘启动。方法为开机后按F12，然后选择你的介质，比如我用的是u盘，我就会看到一个kingston 8GB USB设备，然后点击，即可从U盘启动，这样我们就进入了安装arch的引导界面。

引导界面的第一条为安装64位arch，第二条为安装32位arch，选择前请一定确认你的CPU是否支持64位哦。选择后会引导安装界面（本质上是进入你安装介质的arch系统，在这个系统中安装你硬盘的arch），这是一个有点像dos的纯文本界面

可以先输入

`fdisk /dev/sda `      ##进入交互式界面

`p`                                           ##显示分区信息


这样会输出/dev/sda1  /dev/sda2  /dev/sda3等等的信息，这些sda x代表你硬盘的分区，后面显示的数字，例如/dev/sda1 56G 22G 35G 39% 代表sda1（一般对应你的C盘），共56G，已用22G，可用35G，已用39%

可以通过这些数据确定你之前准备好的硬盘分区是 sda几，以后就要格式化这块硬盘。记住之后输入

`q`                                           ##退出fdisk交互界面

输入

`mkfs.ext4 /dev/sdax`      ##sdax中的x为对应数字，例如我就是sda6，这句话为以ext4形式格式化sda6硬盘


#### 3.挂载分区


输入

`mount /dev/sdax /mnt `       ##sdax中的x同上文，比如对我就是指把dev下的sda6分区挂载到mnt目录下


#### 4.更改网络镜像地址

输入

`vi /etc/pacman.d/mirrorlist `    ##找到中国的镜像地址，删除前面的两个#号，#号代表注释，删掉之后后面的内容就可以被执行了，类似于C++里的//,这里推荐mirror.163.com 和mirror.ustc.edu.cn分别是网易和中科大的镜像站点，网速较快，其实还有清华和北京交大的站点，速度都不是很快，因此不推荐了。具体删除的方法是光标找到那行，按i进入编辑模式，然后按del键删掉，然后输入：wq回车即可退出编辑模式。



#### 5.安装基本系统

输入
`pacstrap /mnt base base-devel`    ##安装最基本的Arch系统，即文字界面（类似与现在你所在的环境，其实现在所处的安装环境就是一个基本的arch系统，只不过是安装在你的U盘/移动硬盘/光盘上而已）

#### 6.配置系统

输入

`genfstab -p /mnt >> /mnt/etc/fstab`            ##这里牵扯到一个重定向的管道>>，如果你懂C++可以理解为C++里的流（其实就是嘛，linux就是用C写的。。。。）（比如C++中的I/O流 cin cout）



#### 7.拷贝一下镜像设置

输入

`cp /etc/pacman.d/mirrorlist /mnt/etc/pacman.d/`     ##解释下这步，就是说刚才第4部其实改的是你的U盘或光盘里那个Arch系统（安装环境）的镜像设置，现在拷贝到的目录是你已经装好的硬盘里的Arch系统的目录，这样一会进入你安好的系统中就不用再设置啦



#### 8.切换根目录


输入

`arch-chroot /mnt`               ##刚才你的根目录在u盘或光盘的Arch系统下，现在相当于切换到硬盘你刚才安装好的arch系统下


#### 9.选择需要的字库并安装

输入

`vi /etc/locale.gen `         ##找到en-US（英语的）和zh-CN（中文的）开头的内容，删除前面的##，方法同第4步

输入

`locale-gen  `      ##即安装你刚才选好的英语和中文字库



#### 10.重启


输入

`reboot `      ##刚才几步之后，我们的Arch系统已经算是安好到你的硬盘里了，不过别着急，这个系统才是个框架，只有文字界面，就好比前文说的空操场，现在我们要做的是给他铺塑胶，并且设置基本的运动器材！



> 重启后，你会发现你进入了你的windows！而且系统根本没有提示让你选择进入windows还是arch，你可以会怀疑刚才真的装好了一个archlinux？是的，只是我们没有改引导区，也就是说你的计算机不知道你还有一个Arch系统，现在我们要做的就是通过grub来引导进入你的Arch（说白了，就是实现双系统！）



#### 11.Grub设置

Arch官方WIKI上提供了Grub2和EFI引导的方式，这里提供一种最简单易行的方式，通过windows平台的软件EasyBCD实现。

方法大致为，下载并安装EasyBCD，英语不好的同学可以装汉化版的。运行EasyBCD，选择‘添加新条目‘，选择‘NeoGrub’，选择‘添加’，这是C盘根目录下会多一个‘NST’的文件夹，内有2个文件‘menu.list’'NeoGrub.mbr',这时用文本编辑器编辑menu.list文件，添加

`root (hd0,x-1)`

`kernel /boot/vmlinuz-linux root=/dev/sdax ro`

`initrd /boot/initramfs-linux.img`

`boot`

 解释一下第一行的x-1就是你第2步sdax里的x－1，为什么－1呢，因为他是从0开始排的，比如我就是root(hd0,5)

第二行的x就是刚才的x，比如我是sda6。

编辑好之后保存，重启计算机，这样开机就会提示你进Arch了～，选择Arch进入



#### 12.设置主机名并添加用户


输入

`echo 'pcname' >> /etc/hostname   `       ##这里的pcname里就写你要给计算机起的名字,比如我就给计算机叫pc那么就是echo 'pc' >> /etc/hostname

`adduser user    `       ##这里的user就是你想创建的用户名.比如我的叫arch,那么就是adduser arch.这一步是交互式的,系统会提示你设置一些东西,包括密码,跟着一步一步填就OK,这里不赘述了



#### 13.配置网络


输入

`ip link set eth0 up`

`dhcpcd eth0   `                    ##这两步就是强制联网,因为之后要下载一些网络文件,并且设置成开机自动联网,因此这几步以后在也不用重复作了

`pacman -S ifplugd`

`cd /etc/network.d`

`ln -s examples/ethernet-dhcp .    ` ##注意后面有个空格和点,别忘了.

`systemctl enable net-auto-wired.service `  ##这一步就是把开启以太网卡做成开机启动模块,以后会自动联网



#### 14.下载sudo并且设置权限和密码


输入

`pacman -S sudo    `         ##不解释了，Arch的包管理器pacman，－S是下载并安装

`chmod +w /etc/sudoers  `        ##给sudoer这个文件增加可写权限

`vi /etc/sudoers    `          ##和前文几步很像,用vi编辑这个文件,找到root ALL=(ALL) ALL这一行,给这行后面添加一行user ALL=NOPASSWD: ALL 注意这个user是第12步你创建的用户名称,比如我创建的叫arch,那么这行就是arch ALL=NOPASSWD: ALL

`chmod -w /etc/sudoers   `            ##不解释了,改完之后再把他的可写权限去掉


> 到此为止,你的空操场算是铺设好了,不过现在还是文字界面,没有桌面环境和窗口管理器,这些都习都可以自己定制,所以这也体现了Arch简洁的特点,其系统根本不内置任何个性化的东西,个性化的东西都由用户来配置,下文推荐一下一种轻量级的桌面环境lxde,不过linux平台最有名的桌面环境是gnome和kde,这两个桌面环境非常漂亮,后面后介绍如何装这两种,这里先谈轻量级的lxde.



#### 15.下载并安装xorg


输入

`pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils mesa alsa-utils`



#### 16.下载显卡驱动


输入

`pacman -S xf86-video-intel xf86-video-nouveau `    ##注,xf86-video-noveau是N卡的开源驱动,如果你的PC是A卡的,请把这个改成xf86-video-ati,如果只有intel集成显卡,就只下载xf86-video-intel,对于兼容机没有intel集成显卡的用户下载对应的N卡或A卡驱动即可



#### 17.其他驱动,字体


输入

`pacman -S xf86-input-synaptics ttf-dejavu wqy-zenhei`



#### 18.下载桌面环境(lxde)


输入

`pacman -S lxde obconf gamin`



#### 19.退出root登录,进入你的用户登录(前面几步一直在root下进行的)


输入

`exit `

这是系统会出现login:然后输入你刚创建的用户名和密码,比如我就输入arch回车 密码回车

这时候你会发现你输入命令的前面从刚才的root@pcname变成了user@pcname(比如我arch@pc)



#### 20.设置openbox和lxde


输入

`mkdir -p .config/openbox    `          ##注意config前面有个点

`cp /etc/xdg/openbox/{menu.xml,rc.xml,autostart} .config/openbox  `       ##注意花括号里的点和逗号别弄错了

`echo 'exec startlxde' > .xinitrc   `          ##注意xinitrc前面有个点



#### 21.进入桌面环境(图形界面)


输入

`startx`



> OK,经过21步的长途跋涉和设置,我们终于进到了图形界面中!后面的配置就会更简单,看到这么一个完全由自己配置的图形环境,虽然前面过程很麻烦,不过很有成就感哈!~



#### 22.设置语言(需要root登录,root登录的方法是在终端中输入sudo su)


进到图形界面里,点击左下角的按钮(类似windows里的开始),选择'附件','LX终端',这也就是我们常说的terminal,有点像windows里的cmd,不过这个功能无比强大,完爆cmd

输入

`sudo su  `    ##root登录,之所以不直接在后面的命令前面加sudo是因为sudo+命令的权限和root登录有一点区别,更改语言sudo权限不够,必须root登录

`echo "LANG=zh_CN=UTF-8" > /etc/locale.conf`



#### 23.设置为开机登录后自动进入图形界面(需要root登录)


可以直接在刚才的终端中继续输入(也可以再打开一个然后sudo su)

输入

`echo 'startx' >> /etc/profile`


#### 24.设置中文输入法

重新打开一个终端(因为不需要root登录,只是需要sudo权限)

输入

`sudo pacman -S scim scim-pinyin`     ##其中scim-pinyin是拼音输入法,习惯使用五笔的同学把这个改为scim-tables

`vi .xinitrc  `             ##编辑.xinitrc,你会发现里面有一行exec startlxde,对,这句话是刚才咱们加进去的,给他前面加5行内容export LC_CTYPE="zh_CN.UTF-8" 回车 export XMODIFIERS=@im=SCIM 回车export GTK_IM_MODULE="scim" 回车export QT_IM_MODULE="scim" 回车scim -d ,重启之后会生效,切换的默认热键是ctrl+space,这个可以用户根据自己习惯再改



> 好了,到此位置,我们的操场上的塑胶和基本的运动设施全部搭载完毕,后面的软件什么的就完全有用户你去定制咯!这样才是属于你的系统,这里会推荐一些软件,下载方法就是"sudo pacman -S 软件名",非常简单!



**输入法**:ibus(刚我们装的是scim,不喜欢的话可以换成ibus)

**web浏览器**:firefox  chrome  opera

**Email客户端**:thunderbird  kmail

**文案工作**:libreoffice  koffice  wpsforlinux

**文本编辑**:gvim(就是我们刚一直用的vi的升级版)  gedit(推荐)  editor

**PDF阅读器**:kpdf  adobereader(这个是闭源软件,因此不推荐)

**字典**:goldendict  stardict

**CAD软件**:draftsight(只支持2D,不过用起来和autoCAD很像,推荐)  freeCAD(支持3D,推荐)

**音乐播放器**:rhythmbox(推荐)  amarok  exaile

**视频播放器**:kaffeine(推荐)  vlc(支持硬解码)  realplayer

**开发环境**:gcc(这个名气太大不说了,不过只能用C和C++)  kdevelop4(支持多种语言,推荐)

**游戏**:大名鼎鼎的doom3有linux平台的.还有一条消息"Steam重申对Linux系统的重视，疏远苹果和微软系统,Valve是真的很讨厌Windows 8，他们对微软操作系统的炮轰从未停止，对苹果Itunes的炮轰也是一样了。如你所知，苹果和微软都基本上全权控制着自己的操作系统和对应的App Store，而Steam不会这么做。Steam表示:我们要继续对开放平台的开发，我们发现很显然Linux是最具灵活性的平台，所以我们现在正在努力尽可能让Steam在Linux下支持更多游戏。我们想消除Linux与玩家之间的最大隔阂，让这个开源平台继续生存。如果微软最后变得和苹果一样，成了全封闭式的系统，那Linux无疑就将成为人们最好的选择了。所以我们要尽所能完善它。”什么?你不知道Steam公司.CS你玩过把?上面写的啥?



> 现写到这里吧,如果你感觉刚才那个图形界面不好看,太屌丝(因为是一个轻量级的桌面环境,因此不是很华丽),你可以尝试安装gnome或者kde,这两个非常漂亮,具体教程正在编辑中,稍等............



**附录一些TIPS**

如果你是给笔记本上装ArchLinux，那么自然少不了触摸板和音量热键的设置（亮度设置内置于BIOS因此不用设置）



#### 1.触摸板设置

`sudo vi /etc/X11/xorg.conf.d/10-synaptics.conf   `              ##用vi编辑配置文件

本身的内容中几行

`Option "TapButton1" "1"       `           ##保持这行不变

`Option "TapButton2" "2"       `           ##这行说的是两指触控 后面的参数"2"为开,若要关闭改为"0"

`Option "TapButton3" "3"       `           ##同理三指触控开关 开为"3",关为"0"

给这几行下面加这么几行

`Option "VertEdgeScroll" "on"     `             ##竖直方向右侧栏滚动开关   "on"为开,若要关闭改为"off"

`Option "VertTwoFingerScroll" "on"     `          ##竖直方向双指滚动开关  同理"on"为开,"off"为关

`Option "HorizEdgeScroll" "on"         `          ##同理,水平方向的下侧栏滚动开关

`Option "HorizTwoFingerScroll" "on"    `               ##同理,水平方向双指滚动开关



#### 2.键盘控制音量(音量热键)

`vi .config/openbox/lxde-rc.xml   `    ##用vi编辑热键配置文件

找到文件的断 添加如下代码

`amixer set Master 5%+ amixer set Master 5%-   `
这些代码分别代表windows键+上键调高5%音量,windows键+下键降低5%音量

如果你的笔记本有额外的热键专门控制音量和静音,可以直接将下面代码添加进配置文件 


`amixer set Master 5%+ unmute `

`amixer set Master 5%- unmute `

`amixer set Master toggle `

 


