---
permalink: /wiki/server/pxe/faq/
---

# 南阳理工学院校园网 PXE 服务 FAQ

## 一般用户

### 什么是 PXE

PXE(Pre-boot Execution Environment)是由 Intel 设计的协议，它可以使计算机通过网络启动。协议分为 client 和 server 两端，PXE client 在网卡的 ROM 中，当计算机引导时，BIOS 把 PXE client 调入内存执行，并显示出命令菜单，经用户选择后，PXE client 将放置在远端的操作系统通过网络下载到本地运行。 — 摘自 [IBM 中国](http://www-900.ibm.com/developerWorks/cn/linux/l-tip-prompt/l-pex/index.shtml#2)

In the mid-90's, Compaq, Dell, HP, Intel, and Microsoft jointly released a system design guide for building Net PC systems. The guide describes a method of booting the operating system from a network server. This method eventually became known as the Preboot Execution Environment (PXE). Several manufacturers have created implementations of the PXE specification, and include a PXE compliant BootROM on their network cards. Most computer motherboards with embedded network interfaces include PXE in the BIOS.

PXE is designed to load a small (32kb or less) image called a Network Bootstrap Program (NBP). The NBP is then responsible for loading the operating system image. When using PXE to boot an LTSP workstation, the choices for an NBP are PXELINUX and Etherboot. — 摘自 [LTSP](http://wiki.ltsp.org/twiki/bin/view/Ltsp/PXE#Introduction_to_PXE)

### PXE.NYIST 有什么用处？

用途举例：

- 安装新系统 – 安装 Linux/Windows，但是电脑没有软驱/光驱，有没有 USB 启动功能；
- 修复老系统 – 电脑因为病毒/引导程序损坏等原因，无法启动了；
- 学习 Linux – 试用 GNU/Linux，跳过安装/配置门槛；
- Cluster – 快速部署和简单管理；
- 教学实验 – 在公共机房中创建 UNIX/C/C++/Java/Matlab 等的教学实验平台。

### PXE.USTC 提供了哪些系统/工具？

| linux                      | 一个 Debian GNU/Linux 系统，主要用于学习目的，目前只提供命令行界面                                                                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| knoppix                    | 一个基于 Debian 的 LiveCD，主要用于桌面 如果您的网卡不被支持，可以试试 allnic，其中包含了所有网卡驱动，副作用是 initrd 加载会慢一些                                                         |
| debian/redhat/mandrake     | Debian/Fedora/Mandrake 网络安装程序                                                                                                                                                         |
| rip                        | Recovery Is Possible, 基于 Linux 的小型急救系统(http://www.tux.org/pub/people/kent-robotti/looplinux/rip/)                                                                                  |
| pxes                       | 一个微型的 Linux 终端系统(http://pxes.sourceforge.net/)                                                                                                                                     |
| stresslinux                | 用于对硬件系统进行高负荷性能及稳定性测试(http://www.stresslinux.org/)                                                                                                                       |
| mcafee/fprot/antivir/avast | 几个病毒检测程序，个人版，同时包含在 dosnet 的 N:盘上病毒库每月更新                                                                                                                         |
| freedos                    | FreeDOS, 兼容 MSDOS 的自由 DOS。不要试图在 FreeDOS 中安装 Windows                                                                                                                           |
| dos                        | 一个 8MB 的虚拟软盘，提供了很多工具                                                                                                                                                         |
| dosnet                     | 一个支持 TCP/IP 网络的 FreeDOS 虚拟软盘，它的 MSLAN 网络驱动器 N: 上的程序是 dos 的超集。如果它启动时无法自动加载网卡驱动，可以试试手动选择网卡列表中最后的 UNDIS/UNDIS3C 两种 PXE 网卡驱动 |

其中的很多系统工具有很大的危险性，在使用之前请仔细阅读它们的文档并小心操作。使用这些工具而造成的任何损害，均由使用者本人负全部责任。

各类系统工具的主要来源是:

- [Ultimate Boot CD](http://ubcd.sourceforge.net/index.html)，这里存有部分[本地文档](http://pxe.ustc.edu.cn/ubcd/index.html)。
  Ultimate Boot CD contains close to 100 tools that are extremely useful when setting up or maintaining a PC.

另外以下站点还有很多不错的软件资源：

- [UTILITIES FOR 16 BIT DOS](http://www.undercoverdesign.com/dosghost/dos/utildos.htm) [DOS 之家软件站](http://doshome.net/soft/)

dosnet 是由以下工具整合而成：

- [Bart's Network Boot Disk](http://www.nu2.nu/bootdisk/network/)
  A highly professional network boot disk for connecting to a network share on a Windows 9x/ME/NT4/2000/XP or Linux Samba machine.
  Modular design - based on [modboot](http://www.nu2.nu/bootdisk/modboot/) . This means you can customize the bootdisk yourself by adding only the modules you need.
- [Universal TCP/IP Network Bootdisk](http://www.netbootdisk.com/)
  A DOS bootdisk that provides TCP/IP networking support.
- Intel PXE PDK 2.0

### 享受 PXE.NYIST 服务需要什么软硬件环境？

您的电脑最好内置有 PXE Boot Agent，并且已经[激活](https://cips.nyist.edu.cn/wiki/server/pxe/faq#如何激活我电脑中的_pxe_boot_agent)；没有现成支持的话，有[几种解决办法](https://cips.nyist.edu.cn/wiki/server/pxe/faq#我的电脑没有内置_pxe_boot_agent_我该怎么做)。
您所在的网段最好有能提供正确的 PXE 信息的 DHCP 服务器，以及可以通过 TFTP 数据包的网关；没有的话可以请管理员加以[设置](https://cips.nyist.edu.cn/wiki/server/pxe/faq#我们实验室有自己的网关和dhcp服务器_该如何设置以便子网内的计算机能够访问pxe服务)。
如果您所在实验室用地址转换/伪装技术建立了自己的子网，则需要[配置](https://cips.nyist.edu.cn/wiki/server/pxe/faq#我们实验室有自己的网关和dhcp服务器_该如何设置以便子网内的计算机能够访问pxe服务)。

### 如何激活我电脑中的 PXE Boot Agent？

目前有不少主板内部已经集成了 PXE 启动代码，一般的判别准则是：集成了网卡芯片的主板通常都会在 BIOS 中包含 PXE Boot Agent。
很多主板内置了 PXE Boot Agent 但是没有激活它，激活标志通常是：在电脑启动自检后在屏幕上会出现 PXE 字样。
为了激活 PXE Boot Agent，需要修改一到两处 BIOS 设置。具体的方法各个主板不尽相同，一般需要检查如下两项：

- 把 Onboard LAN 和 Onboard LAN Boot ROM (或类似的选项)设为 Enabled。
- 把 LAN (或类似的设备)设成第一启动设备 （如果你的电脑在启动时显示 “Press XX to boot from LAN” 字样，则可省略这一步）

升技 NF7 系列主板的设置：

```
Integrated Peripherals->OnChip PCI Device->
        LAN Controller          [Enabled]
        - LAN Boot ROM          [Enabled]

Advanced BIOS Features->
        First Boot Device       [LAN]
```

微星 NEO 2 主板设置：

```
Integrated Peripherals->
        Onboard LAN             [Enabled]
        Load onboard LAN BIOS   [Enabled]

Boot Device Prioty:
        LAN
```

## 管理员

### 出于管理的需要，我们机房希望只为内部网络提供受限的 PXE 服务，或加以密码保护

您有两种选择：

- 自己动手，设置网关、 DHCP、TFTP 服务器，提供自己的启动菜单。
- 申请网络中心为你们的 MAC 地址或 IP 地址提供特殊服务。

### 我们实验室有自己的网关和 DHCP 服务器，该如何设置以便子网内的计算机能够访问 PXE 服务？

需要做两个方面的设置：

- 网关: 允许 tftp 数据包顺利通过

  ```
  # modprobe ip_nat_tftp ip_conntrack_tftp
  ```

  可以在系统启动的时候自动加载这两个内核模块：

  ```
  # echo ip_nat_tftp >> /etc/modules
  # echo ip_conntrack_tftp >> /etc/modules
  ```

### 如何把一张 Linux/BSD 的 Live CD 做成可网络启动？

可以参考 knoppix Live CD 中的 /usr/sbin/knoppix-terminal-server 脚本，以及它所创建出来的 initrd。
大体上需要对原有系统的 initrd 做如下两个修改：

1. 创建一个更大的 initrd，并将原 initrd 中的全部内容，以及各种网卡驱动内核模块、NFS 网络驱动模块、静态链接的 ifconfig/pump 等网络配置程序拷贝进去；
2. 修改其中 /linuxrc (或类似文件)， 把其中 mount cdrom 的命令改写为 mount nfs 的命令，并在这之前加入加载驱动模块，配置网络的命令。

对于 BSD 系统，其中的内核模块不妨交由 loader 来加载。

### 如何把一个安装在服务器硬盘上的 Linux 系统做成可网络启动？

可以仍然使用用 initrd：

- [LTSP](http://www.ltsp.org/) 项目提供了相关的支持工具；
- Etherboot 的 contrib/initrd 目录下包含的 mkinitrd-net 工具也可参考使用；
- Debian 还提供了工具包 initrd-netboot-tools。

或者可以直接使用 NFS-ROOT 方案。基本思路是：

- 服务端：

1. 在服务器上运行 NFS Server，把此系统所在目录共享；
2. 在此系统中放一个 tmpfs 的处理脚本，动态的在内存文件系统中加载 /etc, /var, /root, /home 目录。
   可参考 (NFS)pxe.ustc.edu.cn:/croot/etc/rcS.d/S03nfsboot.sh 脚本。

- 客户端：

1. 定制编译一个包含所有必要的网卡驱动，以及如下选项：

   ```
   # Networking options
   # Under "Device Drivers ---> Networking support ---> Networking options"

   #  Kernel level IP autoconfiguration
   CONFIG_IP_PNP=y
   #  DHCP support
   CONFIG_IP_PNP_DHCP=y

   # File systems
   # Under "File systems --> Pseudo filesystems"

   #  Virtual memory file system support
   CONFIG_TMPFS=y
   # #  /dev file system support
   # CONFIG_DEVFS_FS=y
   # # Automatically mount devfs at boot time
   # CONFIG_DEVFS_MOUNT=y

   # Network File Systems
   # Under "File systems --> Network File Systems"

   #  NFS file system support
   CONFIG_NFS_FS=y
   #  Provide NFSv3 client support
   CONFIG_NFS_V3=y
   #  Root file system on NFS
   CONFIG_ROOT_NFS=y
   ```

2. 启动此内核时使用参数：

   ```
   root=/dev/nfs nfsroot=202.38.73.198:/croot,rsize=8192,wsize=8192,tcp ip=dhcp
   ```

## 相关链接

- Etherboot

  - Homepage http://www.etherboot.org/
  - Wiki http://wiki.etherboot.org/

- Diskless Linux http://frank.harvard.edu/~coldwell/diskless/
- PXE Documentation Version 1.0（缺少链接）
- HOWTO setup a PXE 2.x server under Linux http://clic.mandrakesoft.com/documentation/pxe/

- 基于 NFS\*ROOT 的 [Gentoo Linux 的 PXE 安装方法](http://www.gentoo.org.tw/doc/altinstall.xml#doc_chap5)
- Diskless Nodes with Gentoo http://www.gentoo.org/doc/en/diskless-howto.xml
- FreeBSD Diskless Clients http://the-labs.com/FreeBSD/Diskless/
- Network Booting i386 Unix Variants http://www.munts.com/diskless/netboot.html
- Booting FreeBSD via PXE http://www.tnpi.biz/computing/freebsd/pxe-netboot.shtml
- [Remote Network Boot via PXE](http://www.kegel.com/linux/pxe.html)
- [Index of /tweaks/dhcp3](http://debian.jones.dk/tweaks/dhcp3/)
- [如何远程安装 Linux](http://www-900.ibm.com/developerWorks/cn/linux/l-tip-prompt/l-pex/index.shtml)
- [Diskless Linux](http://frank.harvard.edu/~coldwell/diskless/)
- [Etherboot Wiki - Main.HomePage](http://wiki.etherboot.org/)
- [PXE Documentation Version 1.0](http://clic.mandrakesoft.com/documentation/pxe/)
- [Ultimate Boot CD - Overview](http://ubcd.sourceforge.net/index.html)
- [Bootable CD Utilities by reanimatolog - Образы загрузочных дискет и жестких дисков](http://bootcd.narod.ru/images.htm)
- [All-drivers Etherboot floppy & how to install etherboot to a hard disk](http://etherboot.anadex.de/)
- [ROM-o-matic.net](http://rom-o-matic.net/)
- [:: ThinStation - a light, full-featured thin client OS ::](http://thinstation.sourceforge.net/wiki/index.php/ThIndex)
- [Index of /debian/dists/unstable/main/installer-i386/current/images/netboot](http://ftp.us.debian.org/debian/dists/unstable/main/installer-i386/current/images/netboot/)
- [YAGI - [Deep Space DaN\]](http://dan.deam.org/yagi.php)
- [Pollix LiveCD](http://moe.tnc.edu.tw/~kendrew/pollix/)
- [Bart's Network Boot Disk](http://www.nu2.nu/bootdisk/network/)
- [Universal TCP/IP Network Bootdisk for M\$ Networks](http://www.netbootdisk.com/download.htm)
- [DOS and W31 utilities for MSDOS, DRDOS, FreeDOS, PCDOS, OpenDOS, and PTSDOS 16 bit DOS versions.](http://www.undercoverdesign.com/dosghost/dos/utildos.htm)
- [Install GNU/Linux without any CD, floppy, USB-key, nor any other removable media](http://marc.herbert.free.fr/linux/win2linstall.html#grub-for-nt)
- [Boosting the power of a DOS image](http://people.cs.uchicago.edu/~gmurali/gui/isodos.html)
- [GRUB4DOS and WINGRUB Project Homepage](http://grub4dos.sourceforge.net/)
- [UBCD for Windows](http://www.ubcd4win.com/downloads.htm)
- [The Labs: FreeBSD Diskless](http://the-labs.com/FreeBSD/Diskless/)
- [Gentoo Linux Documentation -- Diskless Nodes with Gentoo](http://www.gentoo.org/doc/en/diskless-howto.xml)
- [Network Booting Unix Variants](http://www.munts.com/diskless/netboot.html)
- [The Network People, Inc. - FreeBSD 5 PXE boot recipe](http://www.tnpi.biz/computing/freebsd/pxe-netboot.shtml)
- [MorphixWiki! - Qemu](http://am.xs4all.nl/phpwiki/index.php/Qemu)
- [Index of /qemu/utilities/QEMU-HD-Mounter](http://dad-answers.com/qemu/utilities/QEMU-HD-Mounter/)
- [DistroWatch.com: clusterKNOPPIX](http://distrowatch.com/table.php?distribution=clusterknoppix)
- [[Wolves\] using GNU Screen to join two terminals?](http://mailman.lug.org.uk/pipermail/wolves/2004-November/010482.html)
- [Linuxeden 的 GRUB 专区 : 首页](http://grub.linuxeden.com/wakka.php?wakka=%CA%D7%D2%B3)
- [ParallelKnoppix: Bootable CD to Create a Linux Cluster in 15 Minutes](http://pareto.uab.es/mcreel/ParallelKnoppix/)
- [PXE 網路開機實作](http://cha.homeip.net/blog/archives/2006/08/pxe.html)\*
