参考资料
arm linux 中断：https://blog.csdn.net/WSYW126/article/details/51803727
ioctl：http://slucx.m.m.blog.chinaunix.net/uid-28385692-id-3499034.html
bsp：https://wenku.baidu.com/view/434be34ca517866fb84ae45c3b3567ec102ddcf1.html
ARM体系结构与编程
禁止内核打印（dmesg可以打印内核信息）：https://blog.csdn.net/Michelle_li7/article/details/78135518
内核：https://ww2.cs.fsu.edu/~stanovic/teaching/ldd_summer_2014/references.html
https://blog.csdn.net/cug_fish_2009/article/list/2
http://www.kerneltravel.net/（若https会被重定向到https://helight.info/）
LDD（Linux Device Driver）：https://lwn.net/Kernel/LDD3/
https://www.cs.fsu.edu/~baker/devices/notes/
https://www.nxp.com/files-static/soft_dev_tools/doc/ref_manual/Linux%20Device%20Drivers.pdf
arm内核解析：
https://www.bbsmax.com/A/D85473x3JE/
xilinx：https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18841873/Linux+Drivers 
https://www.cnblogs.com/KcMeterCEC/category/855954.html
linux diagram：https://www.thomas-krenn.com/en/wikiEN/index.php?title=Linux_Storage_Stack_Diagram
Linux内核大讲堂
kernel-api：https://ww2.cs.fsu.edu/~rosentha/linux/2.6.26.5/docs/DocBook/kernel-api/index.html
kernel-doc：https://elixir.bootlin.com/linux/latest/source/include/linux/i2c.h

misc, platform, /dev/..：不同的bus，就有对应不同的dev和drv
device-bus-driver：https://blog.csdn.net/lizuobin2/article/details/51570196
modules：另一个维度的概念（bus, driver, device都能用module来插入内核）
file_oper：另一个维度？？，文件操作（/dev/）
ldd3 example：https://github.com/duxing2007/ldd3-examples-3.x


Loadable Kernel Module：
https://www.cnblogs.com/biaohc/category/967860.html
https://www.cnblogs.com/deng-tao/category/898891.html
https://wenku.baidu.com/view/10043eaef524ccbff1218421.html
device-bus-driver：http://thomas.enix.org/pub/conf/rmll2010/kernel-architecture-for-drivers.pdf
https://blog.csdn.net/lizuobin2/article/details/51570196
https://github.com/torvalds/linux/tree/master/Documentation/driver-model
https://www.mjmwired.net/kernel/Documentation/driver-model/bus.txt
https://www.oreilly.com/openbook/linuxdrive3/book/
https://blog.csdn.net/xiezhi123456/article/details/80459690
https://blog.csdn.net/Guet_Kite/article/details/78368928
https://www.cnblogs.com/gdt-a20/archive/2011/05/17/2291988.html
简单驱动（无应用交互）：https://linux.cn/article-3251-1.html
简单驱动（register_chrdev）：https://www.cnblogs.com/icelemon/archive/2012/06/13/2547954.html
https://blog.csdn.net/HERGhost/article/details/51385569
https://blog.csdn.net/xiahouzuoxin/article/details/8899847
https://blog.csdn.net/Guet_Kite/article/list/3
struct file_operations：
https://www.cnblogs.com/xiaojiang1025/p/6363626.html
![](https://upload-images.jianshu.io/upload_images/293860-3829724dbb408d59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
mknod：https://www.cnblogs.com/pokerface/p/6542392.html
https://segmentfault.com/a/1190000009039011
自动创建节点：https://bbs.csdn.net/topics/330248519

混杂设备、字符设备、平台设备：
https://blog.csdn.net/linxiaowu66/article/details/7639590
https://stackoverflow.com/questions/8676306/linux-device-driver-registration-procedure
https://www.kernel.org/doc/ols/2002/ols2002-pages-368-375.pdf

注册dev：
https://blog.csdn.net/mmhhj/article/details/61618383
https://www.cnblogs.com/mylinux/p/5520125.html
https://www.eefocus.com/spencer/blog/13-11/300459_174ec.html
https://blog.csdn.net/Richard_LiuJH
https://www.tuicool.com/articles/J32i2m
bus, device, driver：
https://blog.csdn.net/pen_cil/article/details/79763106
https://blog.csdn.net/gdt_A20

ko编译：
https://www.cnblogs.com/skyred99/p/5683710.html
https://www.cnblogs.com/zhangjiebin/archive/2013/03/21/2972588.html
https://blog.csdn.net/qq_28992301/article/details/52287792
https://blog.csdn.net/jiankangshiye/article/details/6665179
https://blog.csdn.net/qq_25556149/article/details/79284563
https://blog.csdn.net/tommy_wxie/article/details/7280463

内核异常处理：
https://blog.csdn.net/qianlong4526888/article/details/11347659
unhandled alignment fault (0x92000061)：
https://stackoverflow.com/questions/41093013/linux-on-arm64-sendto-causes-unhandled-fault-alignment-fault-0x96000021-wh
https://happyseeker.github.io/kernel/2016/02/26/alignment-fault-in-arm64.html
__attribute__ ((packed))：https://community.arm.com/community-help/f/discussions/8021/unhandled-fault-alignment-fault-0x92000061-at-0x00000000fff0f729
synchronous external abort：https://stackoverflow.com/questions/49521095/sigbus-on-arm-cortex-when-accessing-external-device
https://stackoverflow.com/questions/27507013/synchronous-external-abort-on-arm
https://www.cnblogs.com/charlesblc/p/6262783.html
null pointer：https://blog.csdn.net/rikeyone/article/details/80021720
core_dump：https://happyseeker.github.io/kernel/2016/03/04/core-dump-mechanism.html
https://blog.csdn.net/zzwdkxx/article/details/73788226

dma cache：
https://blog.csdn.net/guyongqiangx/article/details/52045849
http://www.voidcn.com/article/p-fnhhormx-ry.html
https://stackoverflow.com/questions/9544399/dma-vs-cache-difference
https://gauss.ececs.uc.edu/Courses/c4029/lectures/dma.pdf
sync：
https://www.2cto.com/os/201409/339460.html
https://www.cnblogs.com/KevinT/p/3823281.html
一致性：
https://blog.csdn.net/giantpoplar/article/details/80392967
https://blog.csdn.net/maokelong95/article/details/80727952
https://stackoverflow.com/questions/7132284/dma-cache-coherence-management
https://blog.csdn.net/skyflying2012/article/details/48023447

64位虚拟地址：
https://stackoverflow.com/questions/18310485/which-address-space-is-occupied-by-the-kernel-in-64-bit-linux
https://stackoverflow.com/questions/25706656/linux-virtual-memory-user-kernel-space-split-in-x86-64
https://events.static.linuxfound.org/images/stories/pdf/lcna_co2012_marinas.pdf
linux内存管理：
https://www.cnblogs.com/arnoldlu/p/8051674.html
https://blog.csdn.net/jasonchen_gbd/article/details/79461385
devmem：https://www.w3xue.com/exp/article/201810/4671.html
mmap：https://stackoverflow.com/questions/12040303/how-to-access-physical-addresses-from-user-space-in-linux

文件系统：
https://dq.tieba.com/p/2852126057
mtd：https://www.jianshu.com/p/89a94c1d3e72
procfs：https://unix.stackexchange.com/questions/4884/what-is-the-difference-between-procfs-and-sysfs
yaffs2：https://blog.csdn.net/itismine/article/details/4799770

flash：
https://forums.xilinx.com/t5/Embedded-Linux/MTD-flash-read-write-operation-problem/td-p/494164
https://opensourceforu.com/2012/01/working-with-mtd-devices/
http://www.itkeyword.com/doc/1144854217375446101/flash-linux-user
MEMERASE：http://bbs.chinaunix.net/thread-2033356-1-1.html
http://bbs.chinaunix.net/thread-1986105-1-1.html
lseek：https://www.cnblogs.com/jikexianfeng/p/7085998.html

i2c：
https://blog.csdn.net/lizuobin2/article/details/51694574
https://blog.csdn.net/hello2mao/article/list/3
设备地址：https://blog.csdn.net/u010783226/article/details/76063077
读写：https://stackoverflow.com/questions/39409124/access-devices-register-i2c/39411931
https://stackoverflow.com/questions/9974592/i2c-slave-ioctl-purpose
https://blog.csdn.net/u011479494/article/details/80812760
struct i2c_rdwr_ioctl_data：https://blog.csdn.net/luckywang1103/article/details/16810833
I2C_SMBUS：https://www.cnblogs.com/xuyh/p/4959499.html
http://blog.chinaunix.net/uid-26902809-id-4154586.html
16-bit reg：https://stackoverflow.com/questions/9871289/passing-32-bit-register-address-to-i2c-rdwr
https://blog.csdn.net/psvoldemort/article/details/40300715
i2c_add_driver：https://blog.csdn.net/neverbefat/article/details/53392365
gpio模拟i2c：https://blog.csdn.net/subfate/article/details/53524887
https://blog.csdn.net/qqliyunpeng/article/details/53118861
https://github.com/kadamski/i2c-gpio-param/

usb
ehci, ohci, xhci：https://www.crifan.com/files/doc/docbook/usb_basic/release/webhelp/four_hci_relations.html
https://stackoverflow.com/questions/15541088/how-to-determine-sysfs-devpath-from-usb-device-vid-and-pid-in-python
https://blog.csdn.net/LoongEmbedded/article/details/85098793
https://stackoverflow.com/questions/33140787/determine-usb-device-file-path
open_memstream, fmemopen：https://stackoverflow.com/questions/29849749/what-is-the-difference-between-fmemopen-and-open-memstream
https://stackoverflow.com/questions/5901181/c-string-append
statfs, statvfs：https://stackoverflow.com/questions/1653163/difference-between-statvfs-and-statfs-system-calls
get filesystem type before mount：https://stackoverflow.com/questions/11397813/autodetect-filesystem-on-mount
https://stackoverflow.com/questions/25639180/in-linux-in-c-c-how-to-determine-filesystem-type-of-a-mounted-or-unmounted-par
https://superuser.com/questions/328974/understanding-how-filesystem-type-is-determined-before-it-is-mounted
https://stackoverflow.com/questions/12495537/how-to-programmatically-discover-the-filesystem-without-mounting-the-device-lik
mutual exclusion software solutions： https://exp.newsmth.net/topic/article/6c7bd9f4a056f7533660aa914504d320
ntfs：https://blog.csdn.net/Hilavergil/article/details/82622470
simple filesystem：https://www3.nd.edu/~pbui/teaching/cse.30341.fa17/project06.html
SO_REUSEADDR：http://hea-www.harvard.edu/~fine/Tech/addrinuse.html

uart/serial
UART传文件：https://blog.csdn.net/gin_love/category_7965967.html
http://www.kermitproject.org/ek.html
https://stackoverflow.com/questions/9611000/understanding-the-zmodem-protocol
lrzsz：https://blog.csdn.net/coding__madman/article/details/51084711
CFLAGS=-O2 CC=arm-hisiv300-linux-gcc ./configure --prefix=`pwd`/out/

自动光圈：
https://blog.csdn.net/u010538116/article/details/80259197
https://www.amobbs.com/thread-4296383-1-1.html

kbd, input, tty：https://blog.csdn.net/lizuobin2/article/details/52599055 

平台：硬件+bsp
开发：多进程多线程等（os-concept），控制（控制论），传输（各种协议，zigbee，蓝牙，网络等）
arm linux（64位，多核，simd，fpu）：cpp控制算法库，滤波算法库，opencv+simd优化
模块（模块输出不同，一般至少要raw数据和处理好的数据）：
视频采集模块（120fps，isp，形态运算等前处理）
定位模块（gps，9轴陀螺仪，罗盘，单个raw数据，融合数据，滤波数据）

总线？供电？
eg.
1. 加个电机
硬件怎么弄，挂总线上怎么挂？
bsp要做驱动，然后app直接控制转速
2. 再加个码盘采集速度
bsp做驱动，然后app调用滤波库，再调用控制库对电机进行控制

市场调研：
学校专业需求，模块覆盖面，以研究生专业为试点？
SMART-SMART makes a research touchable
竞争可以有，开发感受不到，研发更加没有



pcie
http://www.ssdfans.com/?p=8216
http://blog.chinaaet.com/justlxy/p/5100053251
















