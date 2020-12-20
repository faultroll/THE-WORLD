hyper-v：
http://benyouhui.it168.com/thread-4132217-1-1.html

source insight：
https://www.cnblogs.com/olvo/archive/2012/05/04/2483424.html
***
man：https://git.kernel.org/pub/scm/docs/
http://man7.org/linux/man-pages/dir_all_alphabetic.html
http://pubs.opengroup.org/onlinepubs/9699919799/
作为一个新人，怎样学习嵌入式Linux：https://blog.csdn.net/oTangTang1234567/article/details/17304729
韦东山嵌入式Linux第一期视频：https://edu.csdn.net/course/detail/207
EmbeddedLinuxPrimer：http://www.embeddedlinux.org.cn/EmbeddedLinuxPrimer/
嵌入式启动顺序：https://blog.csdn.net/u011011827/article/details/77773095
bootload详细分析——废铁是怎么产生价值的：https://blog.csdn.net/column/details/bootload-spl.html（uboot详解——板子刚上电时都干了些什么：https://blog.csdn.net/lee244868149/article/details/49681987）
![](https://upload-images.jianshu.io/upload_images/293860-e520a817f2d48950.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
AMBA总线协议AHB、APB、AXI对比分析：https://blog.csdn.net/ivy_reny/article/details/56274412
![](https://upload-images.jianshu.io/upload_images/293860-446f1a1275b01ec6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Linux那些事儿导读：https://blog.csdn.net/fudan_abc/article/details/1810000
linux内核调试指南：http://wiki.zh-kernel.org/sniper
ioctl：https://blog.csdn.net/zifehng/article/details/59576539
驱动开发思想：https://blog.csdn.net/zqixiao_09/article/details/51088887
linux学习笔记：https://blog.csdn.net/xukai871105/article/category/2259571/2?
bmp：https://blog.csdn.net/carson2005/article/details/7614125
ARM與Cortex：https://blog.csdn.net/hlchou/
GBK，UTF-8互转：https://blog.csdn.net/csdn_ds/article/details/79077483
一致性模型：https://mingx01.github.io/zh/yi-zhi-xing-mo-xing-consistency-modelxiao-jie.html
Invalid module format：https://blog.csdn.net/zhenxisuiyuan/article/details/5570490
https://blog.csdn.net/zengxianyang/article/details/50505998
同步，mutex：https://www.zhihu.com/question/39850927
计时：http://blog.sina.com.cn/s/blog_4b1849e4010115hb.html
Linux内核时钟系统和定时器实现：https://blog.csdn.net/anonymalias/article/details/52022787
线程绑定core：https://www.cnblogs.com/x_wukong/p/5924298.html
硬核绑定：https://www.jianshu.com/p/3903284d3190
内存使用与性能优化：https://www.cnblogs.com/arnoldlu/p/6874328.html
linux学习：https://www.cnblogs.com/vamei/tag/Linux/default.html?page=2
linux系统编程：https://blog.csdn.net/tianttt/article/category/2915399
linux编程深入：https://blog.csdn.net/column/details/linux666.html?&page=3
cond_wait：https://www.cnblogs.com/zhchoutai/p/6938188.html
rmmod无效：https://blog.csdn.net/dog250/article/details/6430818
disk sleep：https://blog.csdn.net/davion_zhang/article/details/48268319
gettimeofday、clockgettime 以及不同时钟源的影响：
https://blog.habets.se/2010/09/gettimeofday-should-never-be-used-to-measure-time.html
https://www.cnblogs.com/raymondshiquan/articles/gettimeofday_vs_clock_gettime.html
UNIX环境高级编程：https://blog.csdn.net/longbei9029/article/category/6718981
SMP：https://blog.csdn.net/u013836909/article/category/9072851
小话c语言：https://blog.csdn.net/column/details/c-small-analyse.html
每天进步一点点——Linux中的线程局部存储（一）：https://blog.csdn.net/cywosp/article/details/26469435
bogoMIPS：http://tinylab.org/explore-linux-bogomips/
```
Note that ARM CPUs are just cores, the clock control is part of the larger SoC built around them, and differs from vendor to vendor (i.e. it matters far more that this is an OMAP-something than that it contains a Cortex-A8). Without a cpufreq driver which actually knows the relationship between that clock controller's parameters and the resulting CPU frequency, you're left fumbling around in the dark trying to infer things. If you're lucky, your kernel might have perf event support so you can estimate via counting cycles.
```

operation system
http://jpkc.scezju.com/czxtyl/
http://cse.seu.edu.cn/personalpage/xfliu/en/id-5.html
https://cseweb.ucsd.edu/classes/fa06/cse120/lectures/
https://www.csie.nuk.edu.tw/~wuch/course/csc061/
操作系统核心原理：https://www.cnblogs.com/edisonchou/p/4996163.html
进程管理与调度：https://blog.csdn.net/gatieme/article/details/51456569
进程间通信：http://man7.org/conf/lca2013/IPC_Overview-LCA-2013-printable.pdf
https://docplayer.net/11085524-Shared-memory-segments-and-posix-semaphores-1.html
http://fac-staff.seattleu.edu/zhuy/web/teaching/Winter07/csse340/sharemem.htm
https://www.slime.com.tw/tutorial/bgnet_2014-Jun-zhtw.pdf
semaphore：https://www.cnblogs.com/LubinLew/p/POSIX-semaphores.html
共享内存：https://www.ibm.com/developerworks/cn/linux/l-ipc/part5/index1.html
https://www.ibm.com/developerworks/cn/linux/l-ipc/part5/index2.html
https://blog.csdn.net/sweetfather/article/details/80035967
https://blog.csdn.net/andylauren/article/details/78821655
定时器（timer）：https://stackoverflow.com/questions/5755181/need-help-applying-timer-in-c-in-linux
https://stackoverflow.com/questions/10812858/how-to-use-timers-in-linux-kernel-device-drivers
https://www.ibm.com/developerworks/cn/linux/l-time/index.html
https://kernhack.hatenablog.com/entry/2013/12/04/165249
https://blog.csdn.net/yichigo/article/details/23459613
https://www.cnblogs.com/U201013687/p/5540539.html
https://stackoverflow.com/questions/12392278/measure-time-in-linux-time-vs-clock-vs-getrusage-vs-clock-gettime-vs-gettimeof
https://stackoverflow.com/questions/13474000/arm-performance-counters-vs-linux-clock-gettime
https://stackoverflow.com/questions/20483534/clock-gettime-still-not-monotonic-alternatives
http://www.wowotech.net/basic_tech/tech_discuss/timer_subsystem/posix-clock.html
https://www.zhihu.com/question/28559933
Linux程序设计学习笔记：https://blog.csdn.net/column/details/linuxprogramming.html
screen管理屏幕（screen --> C-a c --> C-a C-a --> screen -x）：https://www.ibm.com/developerworks/cn/linux/l-cn-screen/

进程/线程/pthread/同步
pthread：https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_72/apis/rzah4mst.htm
https://www.cppblog.com/JonsenElizee/archive/2010/11/01/131984.html
https://blog.csdn.net/ithomer/article/details/6063067
pthread cancel：https://blog.csdn.net/M_jianjianjiao/article/details/84204482
https://blog.csdn.net/qq_35865125/article/details/81178818
https://blog.csdn.net/huangshanchun/article/details/47420961
posix_spwan：https://blog.csdn.net/linux_ever/article/details/50316905
进程：https://www.cnblogs.com/linquan/p/5001310.html
bootloader和bootstrap的区别（embedded linux primer）：https://blog.csdn.net/gaoxuelin/article/details/9624677
boot block：https://blog.csdn.net/jamestaosh/article/details/4394961
Linux开机启动：https://www.cnblogs.com/vamei/archive/2012/09/05/2672039.html
细说Linux内核中断架构：https://blog.csdn.net/kerneltea/article/details/7664014
操作系统为什么要分用户态和内核态：https://blog.csdn.net/liuyueyue0921/article/details/48225533
Linux探秘之用户态与内核态：https://www.cnblogs.com/bakari/p/5520860.html
多道批处理、单道批处理、分时系统和实时操作系统的特点：http://blog.sina.com.cn/s/blog_155aff35b0102x27i.html
![](https://upload-images.jianshu.io/upload_images/293860-42ee6e6a05c7276e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Linux线程总结：https://blog.csdn.net/a1414345/article/details/70947396
Java多线程同时启动：https://blog.csdn.net/hi_jess/article/details/4599375
线程池 探究：https://www.cnblogs.com/life2refuel/p/5322567.html
获取线程tid：https://stackoverflow.com/questions/21091000/how-to-get-thread-id-of-a-pthread-in-linux-c-program
https://zrj.me/archives/1048
线程等待：https://www.cnblogs.com/jiuyueguang/archive/2013/04/09/3009820.html
https://blog.csdn.net/lisayh/article/details/76684750
https://blog.csdn.net/joker0910/article/details/7240940
https://www.cnblogs.com/skyfsm/p/7079458.html
并发与并行：https://blog.csdn.net/qq_33290787/article/details/51790605
https://www.jianshu.com/p/892aa4af8b8e
sleep造成阻塞：https://blog.csdn.net/xuzhouweihao/article/details/41851357
https://blog.csdn.net/zxjcarrot/article/details/9877027
线程间通信：https://blog.csdn.net/a987073381/article/details/52029070
https://blog.csdn.net/lovecodeless/article/details/24968369
《Linux内核设计与实现》读书笔记（十）- 内核同步方法：https://www.cnblogs.com/wang_yb/archive/2013/05/01/3052865.html
无锁编程：https://www.ibm.com/developerworks/cn/linux/l-cn-lockfree/
atomic：https://stackoverflow.com/questions/2353371/how-to-do-an-atomic-increment-and-fetch-in-c
https://blog.csdn.net/littlefang/article/details/6104679
atomic与pthread_mutex性能对比：http://www.gpfeng.com/?p=451
对LINUX内核各种锁的理解：https://blog.chinaunix.net/uid-24716553-id-4549342.html
rwlock：https://www.ibm.com/developerworks/cn/linux/l-cn-rwspinlock1/index.html
进程间互斥锁：https://blog.csdn.net/u011244446/article/details/47313963
http://deepfuture.iteye.com/blog/760860
pthread_create传参
https://www.cnblogs.com/shuqingstudy/p/10395962.html
需要传值，而非指针（局部变量释放）
``` c
// parent
int param = 10;
pthread_create(&pid, NULL, son, (void *)(intptr_t)param);

// son
pthread_detach(pthread_self);
int param = (int)(intptr_t)arg;
```
另外pthread_join后pthread_detach也不能结束join状态
``` c
// parent
if (pthread_join(pid, NULL) != 0) {
    perror("pthread_join");
}
// son detach后也不会结束，除非son结束

// son
if (pthread_detach(pthread_self) != 0) {
    perror("pthread_detach");
}
```

文件操作：
「复制、拷贝、替身、软连接、硬连接」区别详解：https://blog.csdn.net/woodpeck/article/details/78761219
生成文件：（dd）https://blog.csdn.net/wuxingpu5/article/details/65631559
（truncate）https://blog.csdn.net/qq_15760109/article/details/79458848
（touch）https://blog.csdn.net/zx1669323685/article/details/79383082
关于硬链接与软连接占用磁盘空间问题的分析研究：http://blog.51cto.com/jk6627/1949090 
ln -L：https://superuser.com/questions/337292/what-is-ln-l-logical-for
通过c语言高效拷贝文件（零拷贝）：https://stackoverflow.com/questions/7463689/most-efficient-way-to-copy-a-file-in-linux
https://blog.csdn.net/lk_wkqd/article/details/50244463
https://stackoverflow.com/questions/2180079/how-can-i-copy-a-file-on-unix-using-c
比较文件：https://stackoverflow.com/questions/6163611/compare-two-files
https://stackoverflow.com/questions/12402391/compare-two-files-c-code


磁盘管理：
https://www.cnblogs.com/along21/category/1096459.html
https://blog.csdn.net/silenceyea/article/details/51773687
***

cli（command-line-interface）
ubuntu：ctrl+alt+f1~f6切至终端，startx进入shell，ctrl+alt+f7切回图形
apt：https://www.cnblogs.com/kulin/archive/2012/07/31/APT_GET_HowWork.html

ir：
https://blog.csdn.net/andyhuabing/article/details/7717200
https://blog.csdn.net/jerryutscn/article/details/7201217

帧结构：
https://wenku.baidu.com/view/823be52b4b73f242336c5f4f.html
时分多址是基于时间分割信道。即把时间分割成周期性的时间段（时帧），对一个时帧再分割成更小的时间段（间隙），然后根据一定的分配原则，使每个用户在每个时帧内只能按指定的时隙收发信号。GSM的时帧结构有5个层次，分别是高帧、超帧、复帧、TDMA时帧和时隙。  
时隙是构成物理信道的基本单元，8个时隙构成一个TDMA时帧。TDMA时帧构成复帧，复帧是业务信道和控制信道进行组合的基本单元。由复帧构成超帧，超帧构成高帧，高帧是TDMA帧编号的基本单元，即在高帧内对TDMA帧顺序进行编号。  
1高帧＝2048个超帧＝2715648个TDMA帧，高帧的时长为3小时28分53秒760毫秒。高帧周期与加密及跳频有关，每经过一个高帧时长会重新启动密码与跳频算法。  
1个超帧＝1326个TDMA帧，超帧时长为6.12秒。  
复帧有两种结构，一种用于业务信道，其结构形式是由26个TDMA帧构成的复帧；另一种用于控制信道，其结构为51个TDMA帧构成的复帧。  
1个TDMA帧＝8个时隙，其时帧长度为4.615毫秒，1个时隙长度为0.577ms，在时隙内传送数据脉冲串，称为突发（Burst），一个突发包含156.25位数据。

PCIe
https://wenku.baidu.com/view/a13bc1c20722192e4436f617.html
https://www.cnblogs.com/yuanming/p/6757997.html
![](https://upload-images.jianshu.io/upload_images/293860-470e2c434065d750.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
https://blog.csdn.net/xuriwuyun/article/details/42240921
![](https://upload-images.jianshu.io/upload_images/293860-42e35bc211a7b005.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
https://blog.csdn.net/scarecrow_byr/article/details/41259139
![](https://upload-images.jianshu.io/upload_images/293860-1e9bbc1796962eee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

PHY：
https://www.intel.com/content/www/us/en/io/pci-express/phy-interface-pci-express-sata-usb31-architectures.html
![](https://upload-images.jianshu.io/upload_images/293860-ed2764e5fab6fc3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
https://blog.csdn.net/xyluoyu/article/details/52760195
https://www.jianshu.com/p/7c86478be839
![](https://upload-images.jianshu.io/upload_images/293860-92cc3fd4589b660d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
网口扫盲：https://www.cnblogs.com/jason-lu/p/3198424.html
https://blog.csdn.net/pengpengjy/article/details/75126648

交叉编译
https://blog.csdn.net/pengfei240/article/details/52912833
https://www.crifan.com/summary_cross_compile_library_note/
![](https://upload-images.jianshu.io/upload_images/293860-f9ab0ee5c16b1980.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/293860-b9d99cda67e3183b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
busybox：https://www.ibm.com/developerworks/cn/linux/l-busybox/
ncurses：https://blog.csdn.net/Mary_Jane/article/details/50769631
arm-linux平台下交叉编译使用libxml2：https://blog.csdn.net/gq1900/article/details/51366279
yaml（./configure -h→./configure --host=）：https://github.com/yaml/libyaml

开发环境（宿主机，虚拟机，目标板）
https://blog.csdn.net/cugxueyu/article/details/1965374
cramfs：https://blog.csdn.net/lurayvis/article/details/10242245
nfs：https://www.cnblogs.com/lifexy/p/7049743.html
https://blog.csdn.net/yiyeguzhou100/article/details/52201976
https://wenku.baidu.com/view/03f0df29aaea998fcc220e32.html
https://blog.csdn.net/keheinash/article/details/50642822
文件系统：https://www.cnblogs.com/shangye/p/6177993.html
U-BOOT移植过程详解：https://blog.csdn.net/liuxinjohn/article/category/1863861
uboot：https://blog.csdn.net/sinat_36184075/article/details/56684537
https://blog.csdn.net/ooonebook/article/category/6484145
uboot下的命令行：https://www.cnblogs.com/PengfeiSong/p/6388521.html
ifconfig：https://www.cnblogs.com/JohnABC/p/5951340.html

memory
Linux内存使用调整：https://blog.csdn.net/coroutines/article/details/39345835
https://blog.csdn.net/vanbreaker/article/details/7505743
https://blog.csdn.net/gatieme/article/details/52384791
https://unix.stackexchange.com/questions/5124/what-does-the-virtual-kernel-memory-layout-in-dmesg-imply
https://blog.csdn.net/sunny04/article/details/40627311
https://blog.csdn.net/laiqun_ai/article/details/8528366
http://blog.2baxb.me/archives/1065
https://www.cnblogs.com/hanyifeng/p/6915399.html
内存带宽计算：http://blog.yufeng.info/archives/1511
https://blog.csdn.net/subfate/article/details/40343497
cache一致性：https://blog.csdn.net/lu_embedded/article/details/78416041
进程概述和内存分配：https://blog.csdn.net/zhangyifei216/article/details/51423580
内存分配方式详解：https://blog.csdn.net/duan19920101/article/details/50989431
为什么struct要malloc：https://stackoverflow.com/questions/3856317/linked-lists-in-c-without-malloc
Memory Leak Detection in Embedded Systems：https://www.linuxjournal.com/article/6059
https://blog.csdn.net/mishifangxiangdefeng/article/details/50552687
https://blog.csdn.net/youbingchen/article/details/52002778
mmap：https://stackoverflow.com/questions/11891979/how-to-access-mmaped-dev-mem-without-crashing-the-linux-kernel
https://blog.csdn.net/skyflying2012/article/details/47611399
https://blog.csdn.net/mantis_1984/article/details/19043111
https://blog.csdn.net/ACHelloWorld/article/details/41828033
https://blog.csdn.net/qq_31833457/article/details/78512945
https://developer.blackberry.com/playbook/native/reference/com.qnx.doc.neutrino.lib_ref/topic/m/mmap.html
https://blog.csdn.net/hhhanpan/article/details/80548687
模拟VirtualAlloc：https://blog.csdn.net/cjfeii/article/details/9122279
https://stackoverflow.com/questions/15261527/how-can-i-reserve-virtual-memory-in-linux
clear cache, posix_fadvise, /proc/sys/vm/drop_caches
https://insights.oetiker.ch/linux/fadvise.html
https://blog.csdn.net/sky_qing/article/details/8988461
https://github.com/leonHua
https://github.com/lnmcc
https://stackoverflow.com/questions/55975346/clearing-os-cache-from-mem-mapped-files-without-file-handle
https://blog.csdn.net/vah101/article/details/7317557

framebuffer
http://en.wikipedia.org/wiki/Linux_framebuffer
http://en.wikipedia.org/wiki/X_server
显示卡刚发明出来的那个年代，CPU 很慢，内存很少，带宽很低，没有 DMA 功能 I/O 都是 CPU 来做，等等。那个时候还处在混乱时期，标准很多时候不统一。以下以原理为主，其实现细节可能差异很大。因此，领会精神！
framebuffer 是一种图形模式，在此之前，那当然就有 Text 模式。当时显示卡是不能显示图形的，只能以一些不同的模式来显示文字，比如 24 行80 列，那么显示卡只需要 24 x 80 = 1920 字节的显存。这是半导体工业的限制。当你需要在 0 行 2 列显示一个 'a' 字符的时候，你就往显存的第 3 个字节写入 'a' 的 ASCII 码。显示卡内部固化有 'a' 在这个模式下如何绘制的代码，这样就能显示文本了。半导体制程进步了，能用得起大显存，提供得了带宽，这时候有了直接控制屏幕上每一个像素的能力。比如 640 x 480 像素 8 位色的显卡需要 640 x 480 个字节的显存。因为具体到实现，很多时候显卡有 A / B 两个 buffer。首先，显卡显示 A 的内容，这时程序写入 B，写入完毕设置个寄存器，显卡就改为显示 B 的内容，而程序则可以写入 A 了。所以我们称这 640 x 480 字节显存为 framebuffer。因此 framebuffer 是很古老的一种 2D 显示技术。
后来 3D 技术出现后，显卡主要会提供 OpenGL/DirectX 这样的硬件接口，同时保留 framebuffer 作为兼容之用。同时 framebuffer 比较简单，也可以用在一些低级代码中，这时候 OpenGL/DirectX 还没有初始化，不可用。
所以，主要区别就是，基于 framebuffer 的 GUI 是 2D 的，比较新的 GUI 比如 cluter / QT 直接基于 3D 绘制系统 EGL / OpenGL 之类的。2D 比较简单，3D 比较复杂，有些嵌入式芯片只支持 framebuffer 不支持 OpenGL之类。2D 比较慢，3D 比较快，因为做显卡的人主要关注 3D 性能。至于 “那这个图形 是怎么显出来的”，和平常一样，一票人设计出一个规范，比如framebuffer / OpenGL / DirectX，做显卡的会实现硬件 + 软件驱动，应用程序开发人员去调用这个规范提供的 API 就好了。前面 MMMIX 说使用 X 也不算错，但个人觉得没有说到点子上。framebuffer 和非 framebuffer 都是访问硬件的方法。X 的绘制功能可以跑在 framebuffer 上，也可以跑在非 framebuffer 上。现在的趋势是能跑在 3D API 上的尽量跑在 3D API 上，X 都是在往 OpenGL 迁移的。简单来说，framebuffer 是前浪，QT 用的 OpenGL 是后浪，明白了？
https://blog.csdn.net/gqb_driver/article/details/9338679
http://bbs.chinaunix.net/thread-1932291-1-1.html
https://blog.csdn.net/u011412769/article/details/37959321
https://www.cnblogs.com/EaIE099/p/5175979.html
字体：
https://blog.csdn.net/YXFLINUX/article/details/9219685
https:///forum.php?mod=viewthread&tid=440931
https://www.cnblogs.com/Leo_wl/archive/2013/05/13/3076215.html
https://wenku.baidu.com/view/04f17d73561252d380eb6e37.html
https://blog.csdn.net/jiangyaoyan/article/details/5572493
https://blog.csdn.net/liuchao35758600/article/details/6936832
https://blog.csdn.net/zxx2011/article/details/16967885
https://blog.csdn.net/WY1468840047/article/details/53130169
https://www.cnblogs.com/wainiwann/p/5900643.html
https://blog.csdn.net/rikpan/article/details/3867961
http://chanae.walon.org/pub/ttf/ttf_glyphs.htm
![](https://upload-images.jianshu.io/upload_images/293860-8f8006a07e8d3a6a.gif?imageMogr2/auto-orient/strip)
![](https://upload-images.jianshu.io/upload_images/293860-f97d4cb41dfb86fb.gif?imageMogr2/auto-orient/strip)
mouse：
https://blog.csdn.net/zpf1217/article/details/4679734
https://blog.csdn.net/qq_21792169/article/details/50809605
https://blog.csdn.net/A694543965/article/details/79834008
https://stackoverflow.com/questions/11451618/how-do-you-read-the-mouse-button-state-from-dev-input-mice

链接库：
.a .so：https://www.cnblogs.com/laojie4321/archive/2012/03/28/2421056.html
https://blog.csdn.net/zhanglianpin/article/details/50491958
make .a：https://blog.csdn.net/u011964923/article/details/73297443
https://blog.csdn.net/haojiahuo50401/article/details/7101617
https://blog.csdn.net/lanmanck/article/details/4659603
https://www.cnblogs.com/orlion/p/5350775.html
装载动态库：https://www.ibm.com/developerworks/library/l-dynamic-libraries/index.html
https://blog.csdn.net/test1280/article/details/78306142
https://blog.csdn.net/test1280/article/details/78306655
https://blog.csdn.net/test1280/article/details/78306795
不同进程和线程调用so：https://blog.csdn.net/u010312436/article/details/81263980
静态库依赖动态库：https://blog.csdn.net/newchenxf/article/details/51735600
将静态库编译成动态库：https://stackoverflow.com/questions/655163/convert-a-static-library-to-a-shared-library

core_dump
https://blog.csdn.net/a1414345/article/details/74311741
https://www.cnblogs.com/nufangrensheng/p/3509262.html

errorno：https://blog.csdn.net/fly__chen/article/details/53101576
https://www.cnblogs.com/acool/p/4708483.html

signal：
https://www.cnblogs.com/mickole/category/496206.html
https://www.cnblogs.com/gogly/articles/2416989.html
https://www.cnblogs.com/52php/p/5813867.html
https://blog.csdn.net/jnu_simba/article/details/8947652
siginfo_t：https://www.mkssoftware.com/docs/man5/siginfo_t.5.asp
ucontext_t：https://www.ibm.com/developerworks/cn/linux/l-sigdebug.html
https://segmentfault.com/p/1210000009166339/read
https://www.cnblogs.com/clover-toeic/p/3949896.html
‘mcontext_t {aka struct sigcontext}’ has no member named ‘gregs’; did you mean ‘regs’?：https://stackoverflow.com/questions/29390618/retrieving-sigcontext-during-backtrace

udev和netlink：
https://stackoverflow.com/questions/22803469/uevent-sent-from-kernel-to-user-space-udev
https://stackoverflow.com/questions/3299386/how-to-use-netlink-socket-to-communicate-with-a-kernel-module
https://stackoverflow.com/questions/14972584/how-do-the-files-in-dev-match-linuxs-model-of-a-device
http://olegkutkov.me/2018/02/14/monitoring-linux-networking-state-using-netlink/
https://blog.csdn.net/Swallow_he/article/details/84073545
parse（writing_udev_rules）：https://blog.csdn.net/qq_29214249/article/details/74909160
https://insujang.github.io/2018-11-28/udev-function-flow-for-kobjectuevent-kernel-group-message/
https://codereview.stackexchange.com/questions/60398/parsing-of-a-linux-netlink-hotplug-uevent-packet/60408
usb hotplug/热插拔：http://blog.chinaunix.net/uid-24943863-id-3223000.html
umount2：https://stackoverflow.com/questions/30547553/umount-doesnt-work-with-device-in-c-but-it-works-in-terminal
getmntent：https://blog.csdn.net/lixiaogang_theanswer/article/details/79431318
libudev：https://github.com/systemd/systemd/tree/master/src/libudev
usb通过/sys找到/dev路径（netlink）：https://stackoverflow.com/questions/15541088/how-to-determine-sysfs-devpath-from-usb-device-vid-and-pid-in-python
https://github.com/libusb/libusb/blob/master/libusb/os/linux_netlink.c
```
通过udev来将/sys和/dev联系起来
rm /etc/udev/rules.d/* -f; /* udev的rule，用内核默认 */
- usb storage(block): /sys/DEVPATH/sda/sdaX --> /dev/sdaX
- keyboard/mouse(input): /sys/DEVPATH/inputX/eventY --> /dev/eventY
```
刷新udev：https://unix.stackexchange.com/questions/39370/how-to-reload-udev-rules-without-reboot
https://serverfault.com/questions/497436/can-udev-be-restarted-without-a-reboot
netlink报错（Address already in use）：https://blog.csdn.net/zhao_h/article/details/80943226

shellscript：
https://segmentfault.com/blog/for_fun
函数：https://www.cnblogs.com/L-dongf/p/9018169.html
循环：https://www.cnblogs.com/chaoguo1234/p/5720949.html
数组：https://blog.csdn.net/potato512/article/details/52397780

输出重定向
freopen：https://www.cnblogs.com/airfand/p/5021104.html
dup2：https://blog.csdn.net/tiandc/article/details/81489447
https://stackoverflow.com/questions/11285621/redirecting-one-file-to-another-using-dup2-and-strtok
open file twice：https://stackoverflow.com/questions/17353461/open-what-happens-if-i-open-twice-the-same-file
https://stackoverflow.com/questions/6686446/using-fopen-twice-on-the-same-file-with-different-access-flags
linkat：https://stackoverflow.com/questions/17127522/create-a-hard-link-from-a-file-handle-on-unix
path of fd：https://stackoverflow.com/questions/1188757/retrieve-filename-from-file-descriptor-in-c
https://stackoverflow.com/questions/11221186/how-do-i-find-a-filename-given-a-file-pointer

Hisi
https://blog.csdn.net/u011011827/article/details/54232434
https://blog.csdn.net/zuodenghuakai/article/details/79581705
基于海思开发板的屏幕截图程序：https://blog.csdn.net/joeblackzqq/article/details/8559662
Hi3531：https://blog.csdn.net/mao0514/article/category/1596019
HI3531例子程序说明：https://yq.aliyun.com/articles/48281
矢量中文字显示：http://bbs.ebaina.com/thread-39483-1-2.html
![](https://upload-images.jianshu.io/upload_images/293860-bcc297efb279086e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/293860-a9dd63a5c3093de9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/293860-3ba573e21079d3fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/293860-38b68755fe8aa114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
串口传文件：https://blog.csdn.net/dragon101788/article/details/30477679
timeref不能为odd：http://bbs.ebaina.com/thread-13424-1-1.html
hisi3516&3536sdk：https://sourceforge.net/projects/hisi/files/SDK/
https://pan.baidu.com/s/1oovJy_gE7u1e1wRNGp8xYA
kbj8

peta-linux
libgcc_s：https://blog.csdn.net/zhu_zhu_2009/article/details/87692304

飞腾（ft）交叉编译: https://blog.csdn.net/macaiyun0629/article/details/106797847/
https://www.linaro.org/downloads/

makefile中echo -e不正常
https://blog.csdn.net/benkaoya/article/details/12410295
判断文件/命令存在（命令也是文件，万物皆文件）
https://blog.csdn.net/qiaoliang328/article/details/7568141


shell按行读文件
https://www.cnblogs.com/chenxiaomeng/p/9638823.html
一行写while：http://blog.sina.cn/dpool/blog/s/blog_ac843e330101c55g.html
while read line; do patch -p1 -i debian/patches/$line; done < debian/patches/series
hunk succeed: https://blog.csdn.net/woshisunyizhen/article/details/109517720
https://blog.csdn.net/longerzone/article/details/16967579

