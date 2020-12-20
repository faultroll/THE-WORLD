toybrick/firefly/friendlyarm

yocto/buildroot-rockchip
petalinux-xilinx

TB-RK3399ProD: http://t.rock-chips.com

根据
https://github.com/rockchip-toybrick/manifest/blob/master/release/rk3399pro_release-V1.5.xml
中的revision和upstream
1. uboot&kernel
选择u-boot和kernel对应的96ai（3399proD用stable特定的md5）
https://github.com/rockchip-toybrick/u-boot/tree/96ai
https://github.com/rockchip-toybrick/kernel/tree/96ai
根据make.sh中的TOOLCHAIN_ARM32和TOOLCHAIN_ARM64
https://releases.linaro.org/components/toolchain/binaries/6.3-2017.05
选择对应的arm-linux-gnueabihf和aarch64-linux-gnu
- uboot编译
需rkbin
- kernel编译
需apt包: libssl-dev（openssl头文件）, liblz4-tool（lz4c工具）
linux-event-code.h --> rk-input.h
https://blog.csdn.net/qq_36956154/article/details/101687577
dts生成dtb：http://wiki.100ask.org/%E7%AC%AC%E4%BA%8C%E8%AF%BE:%E8%AE%BE%E5%A4%87%E6%A0%91%E7%9A%84%E8%A7%84%E8%8C%83(dts%E5%92%8Cdtb)
2. tools&sdk
RKDocs&RKNPUTool&RKTools&...，通过添加/tree/revsion访问
https://github.com/rockchip-toybrick/RKNPUTool/tree/5ccbe26c8063273f51ef93b4e0d26e0f2718e936
https://github.com/rockchip-toybrick/RKTools/tree/b0c8a69ce401577ef2fe988bb299f47b2b4b22ca
剩余的类似hardware-xxx/external-xxx多为android的，不是必须（存疑）
类似camera()/audio（rk817/rk809/...）/...等驱动，见doc（走的都是linux的开源协议v4l2/alsa/...），编解码走的内部的asic（通过v4l2调用）
rknn
http://t.rock-chips.com/forum.php?mod=viewthread&tid=1290
https://github.com/rockchip-toybrick/RKNPUTool
mpp
http://t.rock-chips.com/forum.php?mod=viewthread&tid=2177
http://t.rock-chips.com/forum.php?mod=viewthread&tid=336
https://github.com/HermanChen/mpp
用build下的脚本编译（直接用cmake命令会有问题）：http://t.rock-chips.com/forum.php?mod=viewthread&tid=336&extra=&page=15

p7zip(apt-get install p7zip-full): https://blog.csdn.net/frozleaf/article/details/89149014

烧写工具
http://t.rock-chips.com/wiki.php?mod=view&id=14
1. USB驱动
2. 切换linux分区表，烧写uboot&kernel
    - 或只烧uboot，再通过tftp/...烧kernel（1500000波特率真坑）
修改波特率（本质就是改dts，通用方法）：https://blog.csdn.net/GCQ19961204/article/details/108886338
 
patch
- uboot
1. ls error
make.sh --> pack_trust_image
2. baudrate
configs/rk3399pro_defconfig --> BAUDRATE=115200 & 删除重复的OPTEE_CLIENT/OPTEE_V1（reassigning to symbol）
- kernel
1. modpost warning
drivers/net/wireless/rockchip_wlan/rtl8723du/os_dep/linux/usb_intf.c --> 参考rtl8188eu删除__init &__exit 
2. baudrate
arch/arm64/boot/dts/rockchip/rk3399-android.dtsi --> baudrate=<115200>

rootfs
https://blog.csdn.net/m0_38096844/article/details/97786761
为什么需要：libc/tools/…？（并不，空的也是可以的，单纯是个根文件系统，为了加载后续文件系统用，参考lxc的container）
跟ramfs的关系
怎么制作（buildroot--rootfs/yocto--kernel）
模仿hisi用busybox制作：config文件怎么写（通用的即可）？怎么被调起（地址？bootparam/bootcmd里面设置）
https://blog.csdn.net/subfate/article/details/75137866
https://unix.stackexchange.com/questions/479415/how-does-linux-know-where-the-rootfs-is
https://unix.stackexchange.com/questions/136278/what-are-the-minimum-root-filesystem-applications-that-are-required-to-fully-boo
rockchip buildroot: https://github.com/yunzhaoyu2050/buildroot
https://github.com/ADVANTECH-Rockchip/linux_rootfs/tree/rk3399_linux_v231_rockchip
https://embed-linux-tutorial.readthedocs.io/zh_CN/latest/building_image/building_rootfs.html

arm-trusted-firmware 
https://blog.csdn.net/shuaifengyun/article/details/72468109
http://harmonyhu.com/2018/06/23/Arm-trusted-firmware/

initramfs: https://blog.csdn.net/ooonebook/article/details/52624481
https://blog.csdn.net/rikeyone/article/details/52048972
https://blog.csdn.net/m0_38096844/article/details/99968912
https://stackoverflow.com/questions/6405083/is-it-possible-to-boot-the-linux-kernel-without-creating-an-initrd-image

mali gpu: https://blog.iotwrt.com/linux/2017/03/08/How-to-choose-display-backend/

rk3399踩坑记录：https://github.com/lanseyujie/tn3399_v3
https://www.jianshu.com/p/5e9d96b273aa
alpine-rootfs: https://github.com/alpinelinux/alpine-make-rootfs
https://www.crifan.com/try_use_qemu_emulate_arm_board_to_load_and_run_uboot_kernel_rootfs/
https://zasdfgbnm.github.io/2017/06/29/能当主力，能入虚拟机，还能随时打包带走，Linux就是这么强大/

bootm启动kernel
https://stackoverflow.com/questions/41230606/cannot-boot-from-fit-image
- legacy
https://blog.csdn.net/qq_18077275/article/details/108942796
1. recovery模式烧录bootloader, uboot, trust
因为没有hiburn/flymcu这种直接烧flash的，所以只能通过自带的recovery功能来烧uboot
后续开发uboot添加类似updateb的功能即可
2. 修改bootargs指定rootfs为/dev/mmblk16（原rootfs）
3. tftp下载boot.img/zboot.img（其实就是kernel）和.dtb（设备树见kernel目录下的ext_linux，最后跟initramfs一起打包成不知用途的boot_linux.img）
4. bootm kernel地址 - dtb地址
- fit

rk-android
https://github.com/rockchip-android

maskrom
https://blog.csdn.net/qq2010899751/article/details/81133742

文档
https://rockchip.fr/

http://wiki.100ask.org/100ask_roc-rk3399-pc
https://blog.csdn.net/rikeyone/article/details/88378085
https://www.zephray.me/post/build_busybox_filesystem_with_linaro
recovery：需通过此方法烧录boot/kernel(dtb)/rootfs，用mmc write等不行（原因待查）
rootfs：runtime-lib, init
static link warning: https://github.com/longsleep/build-pine64-image/issues/9
uboot默认env: https://www.cnblogs.com/zhyy-mango/p/9470845.html
busybox shell无响应：http://m.blog.chinaunix.net/uid-26598889-id-4027308.html
无响应原因是getty需要登录导致，修改inittab的askfirst和respawn为-/bin/sh，同时删除group/passwd使其不用登录即可：https://blog.csdn.net/andylauren/article/details/52006456
linaro sysroot库为硬链接，需转换为软链接：https://unix.stackexchange.com/questions/171788/convert-a-hardlink-into-a-symbolic-link
太大需裁剪，参考hisi-sdk的runtimelib
sysroot（glibc）各个库作用：https://blog.csdn.net/jzwjzw19900922/article/details/103833615
https://bf.mengyan1223.wang/lfs/zh_CN/10.0/chapter08/glibc.html
https://blog.csdn.net/mybelief321/article/details/9896311
eglibc（已经停止维护，alpine用的是musl-libc，hisi用的是uclibc）:  https://github.com/Xilinx/eglibc
https://drewdevault.com/2020/09/25/A-story-of-two-libcs.html
```
patch -p1 -E -i ***.patch
通过-E删除文件
```
busybox: https://www.ibm.com/developerworks/cn/linux/l-lwl1/index.html

image生成流程：https://blog.csdn.net/sinat_20184565/article/details/89341000
dtb/uImage: https://www.cnblogs.com/iot-yun/p/11403498.html
https://www.denx.de/wiki/pub/U-Boot/Documentation/multi_image_booting_scenarios.pdf
fit image: https://elinux.org/images/f/f4/Elc2013_Fernandes.pdf
https://blog.csdn.net/JerryGou/article/details/85170949
https://blog.csdn.net/rikeyone/article/details/86594196
https://blog.csdn.net/ooonebook/article/details/53495002
移植linux到stm32：https://amobbs.com/thread-5731948-1-1.html
itb制作warning: https://patchwork.kernel.org/project/linux-arm-kernel/patch/1477280428-30558-1-git-send-email-wangkefeng.wang@huawei.com/

address（看u-boot/include/configs下对应的板子头文件）
https://www.cnblogs.com/cyyljw/p/11008332.html
https://stackoverflow.com/questions/28128321/understanding-linux-load-address-for-u-boot-process
https://stackoverflow.com/questions/43204778/what-is-the-difference-between-the-kernel-loadaddress-and-entry-point
https://stackoverflow.com/questions/55303762/relationship-between-u-boot-config-sys-text-base-and-sdram

driver编入kernel：https://blog.csdn.net/marz07101/article/details/7647400
emmc下/proc/mtd为空（用/proc/emmc或/proc/partitions）：https://stackoverflow.com/questions/15846294/how-to-read-a-specific-region-in-android-emmc
mdev -s自动生成/dev节点：http://m.blog.chinaunix.net/uid-20678569-id-1574819.html
passwd出错（保留group和passwd，供telnet/ssh使用）：https://blog.csdn.net/qq132132132/article/details/39482333

mpp坑
- dtsi中rkvdec的status是disable状态（复用/重写）
https://blog.csdn.net/u012041204/article/details/88382686
- ***-supply/regulator和power-domains
电源框架：https://www.cnblogs.com/connectfuture/p/3721900.html
supply: https://www.cnblogs.com/-rzx-/p/11621931.html
https://blog.csdn.net/m0_37166404/article/details/54380065
power-domains: https://elixir.bootlin.com/linux/v4.4.240/source/Documentation/devicetree/bindings/power/power_domain.txt
http://www.wowotech.net/pm_subsystem/pm_domain_overview.html
exmaple: http://wiki.t-firefly.com/AIO-3399J/driver_gpio.html
https://blog.csdn.net/huang_165/article/details/86550606
https://e2e.ti.com/support/power-management/f/196/t/535886?How-do-you-setup-the-regulators-in-the-device-tree-

```
dtb其实类似数据库，没有所谓的载入顺序
driver通过of_xxx的函数来搜索dtb，达到使用的目的
dtsi描述通用性质的（比如power-domains），dts描述特定板子的（比如power-supply）
```

0. power supply和domains都需要指定
1. passwd & group
root
@@cetc52
2. user
telnet 
3. saveenv & bootcmd/bootargs & emmc write itb

dts基础： https://blog.csdn.net/zhengnianli/article/details/105680154
更新linux&u-boot：https://blog.csdn.net/andyshrk/article/details/103422993
rk的vpu/vcodec是hantro的，5.4.x的linux中相关驱动在drivers/staging/media/hantro内，不像4.4.x在drivers/video/rockchip/vcodec内
mpp: http://opensource.rock-chips.com/wiki_Mpp
flex&bison: https://blog.csdn.net/youngtiger86/article/details/7578175
gcc internal error: https://blog.csdn.net/qq_36558538/article/details/88394318
pm: http://www.wowotech.net/pm_subsystem/pm_architecture.html
kernel&boot
https://github.com/vitkyrka/kninja
https://blog.mbedded.ninja/programming/embedded-linux/u-boot/
https://stackoverflow.com/questions/48429210/understanding-u-boot-configuration-step
https://blog.csdn.net/Jason_416/article/details/106772698
https://www.cnblogs.com/topeet/p/13201977.html
of_match_table
https://blog.csdn.net/zerolity/article/details/101353728


https://github.com/rockchip-toybrick/kernel
https://github.com/rockchip-linux/kernel/tree/stable-4.4-rk3399pro-linux
https://github.com/FireflyTeam/kernel/tree/rk3399pro/firefly

u-boot&emmc&partition&env
（总览）https://zhuanlan.zhihu.com/p/71789194
（blkdevparts分区emmc）http://blog.gqylpy.com/gqy/20227/
（env位置）https://blog.csdn.net/ivychend/article/details/79296463
（env位置&saveenv）https://www.cnblogs.com/lifexy/p/8302690.html
（saveenv）https://blog.csdn.net/qq_40897531/article/details/106155218
（eMMC硬件分区）https://www.dazhuanlan.com/2020/02/02/5e3670b0f0421
（烧写emmc）https://blog.csdn.net/qq_41133610/article/details/80936361


2020.10的uboot在env/nvedit.c内（不在cmd没内），地址CONFIG_ENV_OFFSET（在include/configs或configs内，没有设置则用？）
uboot调试（不破坏旧的uboot）：https://blog.csdn.net/guolele2010/article/details/7051099
https://blog.csdn.net/qiaoliang328/article/details/4368951
为什么uboot和kernel维护两套dtb：https://stackoverflow.com/questions/30711327/why-device-tree-structure-dts-file-is-needed-both-in-bootloader-and-kernel-sou
distro_bootcmd .conf
https://blog.csdn.net/agave7/article/details/108774714
https://developer.solid-run.com/knowledge-base/cex7-atf-u-boot-and-linux-kernel/

u-boot(bootloader, miniloader(bootrom/primary bootloader)+secondary bootloader) & bios(Legacy&UEFI)+grub/Bootmgr
https://blog.csdn.net/liao20081228/article/details/81297143
https://stackoverflow.com/questions/41783528/how-arm-cpu-loading-bootloader
https://stackoverflow.com/questions/15665052/what-is-the-difference-between-a-bootrom-vs-bootloader-on-arm-systems
https://blog.csdn.net/weixin_39655765/article/details/80058644
https://rose.telecom-paristech.fr/2012/wp-content/uploads/2012/03/Firmwares-and-bootloaders.pdf

kernel解析dtb（加载drive）
https://blog.csdn.net/weixin_43898067/article/details/106116117
https://blog.csdn.net/forever_2015/article/details/52885847
https://www.cnblogs.com/unclemac/p/12783391.html
https://blog.csdn.net/qq_18077275/article/details/108935413
https://blog.csdn.net/u014674293/article/details/103469658

env地址未指定：https://blog.csdn.net/a_flying_bird/article/details/77113262
https://blog.csdn.net/gzxb1995/article/details/109122168
新旧版宏位置：https://www.cnblogs.com/comeonjiji/p/3623514.html
tpl(spl升级版？类似ssl和tls): http://www.denx.de/wiki/pub/U-Boot/MiniSummitELCE2013/tpl-presentation.pdf
spl&sys test_base: https://stackoverflow.com/questions/56508485/build-u-boot-spl-for-custom-board
http://blog.chinaunix.net/uid-28458801-id-3486399.html
Kconfig: https://www.cnblogs.com/idyllcheung/p/11382972.html
通过ram调uboot在新版uboot上未调通（需要更多改动？）

mmc subsystem
https://www.cnblogs.com/linhaostudy/category/1454587.html
https://blog.csdn.net/huang_165/article/details/86550606

新旧版宏位置：https://www.cnblogs.com/comeonjiji/p/3623514.html
tpl(spl升级版？类似ssl和tls): http://www.denx.de/wiki/pub/U-Boot/MiniSummitELCE2013/tpl-presentation.pdf
spl&sys test_base: https://stackoverflow.com/questions/56508485/build-u-boot-spl-for-custom-board
http://blog.chinaunix.net/uid-28458801-id-3486399.html

maskrom模式下无法烧录（vendor分区内容被擦除导致？）
http://wiki.t-firefly.com/zh_CN/Core-3308Y/faq.html#xie-hao-gong-ju
https://blog.csdn.net/kris_fei/article/details/79580845
https://blog.csdn.net/qq_40658985/article/details/93716909
https://blog.csdn.net/qq_30624591/article/details/105681859

自制rootfs踩坑：https://blog.csdn.net/computerinbook/article/details/107143804
fhs: https://www.pathname.com/fhs/
https://unix.stackexchange.com/questions/5915/difference-between-bin-and-usr-bin
https://blog.csdn.net/sunqian666888/article/details/84555146

arm grub
http://wuyou.net/forum.php?mobile=2&mod=viewthread&tid=387179

v4l2
https://blog.csdn.net/u013904227/article/details/80718831
https://www.jianshu.com/p/0ac427d267d4
https://blog.csdn.net/weixin_42462202/article/details/99680969
固定节点（video_register_device第三个参数）：https://www.cnblogs.com/zou107/p/7895111.html
https://blog.csdn.net/al86866365/article/details/100034124

ko&dts(platform device&normal device ?)
https://stackoverflow.com/questions/18512421/device-tree-and-manual-registration
https://stackoverflow.com/questions/15610570/what-is-the-difference-between-a-linux-platform-driver-and-normal-device-driver
https://stackoverflow.com/questions/46539478/built-in-kernel-driver-still-need-device-tree
https://stackoverflow.com/questions/22944401/accessing-platform-device-from-userpace

dtc syntax error: https://stackoverflow.com/questions/50658326/device-tree-compiler-not-recognizes-c-syntax-for-include-files

isp: http://opensource.rock-chips.com/wiki_Rockchip-isp1
npu（看脚本rk3399pro的npu类似独立设备，adb通信，需要uboot/kernel这些；rk1808是普通asic，insmod galcore.ko即可）: https://blog.csdn.net/SMF0504/article/details/108845293
http://repo.rock-chips.com/debian/pool/main/r/rknn-rk3399pro/
https://github.com/rockchip-linux/rknpu-fw

https://github.com/SoCXin
http://repo.rock-chips.com/
https://dl.radxa.com/


dtb关键词：model, chosen, aliases
uart-->pinctrl&clocks&uart&syscon这几个驱动

1. prebuilts(tools)
	- uncompress
	- cross-compiler
	- partition
2. uboot
	- loader
	- uboot
	- trust
3. kernel
	- kernel
	- dtb
	- built-in driver
4. rootfs
	- fstools
	- busybox/...
5. app
	- loadable driver(kernel-space)
	- tools/sdk/usrapp(user-space)
6. 附录 
- 烧录
 recovery.img需android相关才能编译（待确认），可直接使用官方的成果物（当作loader即可）
- 调试
 - 命令行
  - telnet/ssh（网络）
  - screen/com（串口）
 - 文件
  - 直传
   - tftp（网络）
   - XYZ modem（串口）
  - 挂载（mount）
   - nfs（板子挂主机）
   - samba（主机挂板子）

1. 调试踩坑过程（rk/xilinx到hisi模式）
   - rootfs(busybox)/fit
   - 分区表
   - ko/...
2. 升级版本（boot/kernel哪些是要注意的，dts/driver/config/...）


不同固件需要不同版本烧录工具？maskrom可以从空emmc开始烧？
http://wiki.t-firefly.com/zh_CN/ROC-RK3328-CC/flash_emmc.html
http://wiki.t-firefly.com/zh_CN/Core-3399pro-JD4/upgrade_table.html
```
烧写原始固件：AndroidTool_v2.39
烧写 RK Android7.1 固件：AndroidTool_v2.38
烧写 RK Android8.1 固件：AndroidTool_v2.58
烧写 RK Linux(GPT) 固件：AndroidTool_v2.58
```

