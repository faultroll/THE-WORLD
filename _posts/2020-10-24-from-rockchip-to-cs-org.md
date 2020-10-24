
``` org
** 序
   hisilicon的板子停产了，公司打算切换到rockchip，最近在进行相关的预研。之前hisi/xilinx的板子也做过相关预研，只不过更多的是dsp/app层面的预研，对bsp层面的预研还是第一次。由于公司不想用android（主要是下不下来），所以大部分东西都不能按照官方给的datasheet来做，基本是重新搞了一遍。越是重头搞起，越感觉跟hisi的bsp类似，又查了相关资料，可能整个嵌入式（甚至整个computer system）都是一套思路吧。

** 层级
   无论hisi/xilinx/rk的arm，还是pc的x86，似乎都是一样的套路
   - bootloader
     miniloader/bios/...
   - boot
     u-boot/...
   - kernel
     *nix/win/...
   曾有人问我单片机和嵌入式最根本的区别是什么，个人感觉，单片机只到boot层（甚至只有bootloader），而嵌入式是到kernel层，而如hisi的vpu/isp/nnie/...，xilinx的fpga，rk的vpu/npu/...，都可以归为asic/外设/...，对boot/kernel来说，是一样的。

** 原因
   产生多级结构的原因，个人认为有以下几个
   - 技术
     历史原因（类似软件的版本迭代），或者高速的做不大（cpu三级cache/dma/...），做大了散热跟不上之类的技术原因
     - 连接
       为了联系版本更迭产生的compat层（如boot），现在kernel要往微内核（即boot方向）发展（虽然也没见多少），boot打算涵盖kernel的作用（看看ucos）
     - 通用
       跟软件开发一样，提取一个hal层，linux的dtb现在干的就是这样，fit-image就是通过boot来决定调哪个kernel/dtb
   - 成本
     nor-flash很贵，且其读写速度也很慢，但就是因为不容易坏，所以bootloader一般都存储在nor里（维护成本）
     emmc便宜/速度快，手机一般都用这个（所以现在手机大"内存"便宜了）

** 其他
   TODO rk预研过程
   TODO driver-device-bus

   fini
```