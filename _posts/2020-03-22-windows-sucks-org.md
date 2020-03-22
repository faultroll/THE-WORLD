
``` org
* 准备
** 理论知识
*** 启动概念
    TODO 启动顺序图
    http://www.wowotech.net/linux_kenrel/UEFI.html
    https://blog.csdn.net/wojiaodaier/article/details/71885560
    https://blog.csdn.net/JIA_GUOQIANG/article/details/53149314
    https://www.quora.com/What-is-the-difference-between-the-BIOS-and-a-boot-loader
    http://blog.sina.com.cn/s/blog_56799b500102vnop.html
*** pe
    http://blog.sina.cn/dpool/blog/s/blog_a0c06a350102wc0n.html
    - aomei
      https://sspai.com/post/41960
*** 无盘系统
    https://bbs.txwb.com/thread-1896414-1.html
    http://blog.51cto.com/3387405/1043616
    https://www.zhihu.com/question/28887635
*** 笔记本（选购）
    主要看模具、主板（散热，升级），其次CPU、显卡（性能），最后才是内存、硬盘（添头）
** 制作U盘系统
   http://wuyou.net/forum.php?mod=viewthread&tid=388634
   http://wuyou.net/forum.php?mod=viewthread&tid=381242
*** 注意
    - diskgenius每次改变分区就要保存一下，否则会报错
    - diskgenius分配盘符只能分配一个，如果给NTFS也分配盘符会报错，不分配就好
    - 要将menu.lst拷贝至根目录

* 整体构架
  分区（命名参考《恶魔书》）
  目标：文件少（wim，vhd），可移植（sysperp），易备份（wim），占用空间少（wim），可以随便折腾（wim，vhd）
  经过数次折腾（做好系统进不去，差分文件损坏，...）最终选用的是wimboot+vhd的方案
  - 4k对齐（可以通过diskgenius查看是否对齐，win10磁盘管理分出来的驱动都是4k对齐的）
    https://www.thomas-krenn.com/en/wiki/Partition_Alignment
    https://www.i3geek.com/archives/1275
    https://blog.csdn.net/haiross/article/details/38843201
  - diskpart
    https://jingyan.baidu.com/article/08b6a591c82df414a8092224.html
    https://blog.csdn.net/u012757487/article/details/53181425
** BOOT
   100mb，不分配盘符，用bootice隐藏
*** bcd文件
    https://blog.csdn.net/wolfsun3/article/details/48828929
** PE-S
   20gb-18gvhdx+工具--bootice，aida，dism++，...
   1) U盘启动
   2) 参照U盘系统，将硬盘内的系统分区，拷贝vhdx
   3) 步骤变更，U盘系统制作时选择主引导为grub4dos，但是硬盘上没法选择；将此步骤改为分区引导（只需要选择FAT32的分区为grub4dos即可），其余步骤依旧
** SYS&SOFT-R
   wimboot+vhd
*** vhdx+wimboot（后面有更好的方法，基础思路一样，不过是在虚拟机内制作）
    这里的内容选用的是ltsb2016（2018年完成），后续都改用了神州网信版本，仅作记录
    1) wimboot（3个index）：install.wim+优化（补丁等）+驱动装好，软件装好单独一个文件
    http://bbs.pcbeta.com/viewthread-1504823-1-1.html
    http://bbs.pcbeta.com/viewthread-1495215-1-1.html
    http://wuyou.net/forum.php?mod=viewthread&tid=368146
    http://bbs.pcbeta.com/viewthread-1690989-1-3.html
    - wimboot+vhd
      优势--体积小（与纯vhd和差分vhd）、备份vhd就能解决驱动无法备份的问题，劣势--速度略慢，增量&差分
      http://wuyou.net/forum.php?mod=viewthread&tid=403822&extra=page%3D1
      cgi：http://www.zdfans.com/6431.html
      wimboot：http://chenall.net/post/windows7_wimboot/
      是否支持wimboot：http://bbs.pcbeta.com/viewthread-1511770-1-1.html
      TODO：(测试)
      install.wim+vhdx指针（install.wim）内增加test1.txt并制作wimboot1.wim（在install.wim内）+vhdx指针（wimboot1.swm）内增加test2.txt并制作wimboot2.swm（在install.wim内）
      1. 看能否用指针vhdx启动wimboot2.swm
      2. 看两个txt内容是否正确
      vhdx（对应驱动装好的index）：动态（50gb）or固定（30gb），看速度；指针vhdx能否差分便于备份？
    - ltsb2016安装踩到的坑
      1) 无法安装积累更新
         必须先安装堆栈服务：https://support.microsoft.com/zh-cn/help/4338814/windows-10-update-kb4338814
         http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1791096&highlight=
         http://bbs.pcbeta.com/viewthread-1760603-1-1.html
         https://www.catalog.update.microsoft.com/Search.aspx?q=1607
      2) 无法用mtp（单纯安装wpdmtp.inf不能解决问题--会出现设备管理器能识别但无法打开盘符的问题？？？；google usb driver能解决问题么--adb不是mtp；不建议安装media-feature-pack）
        - adb（能解决copy的问题-- adb devices（连接设备，需要设备端-开发者模式-usb调试打开）; adb push -p d:/download/UniversalAdbDriverSetup.msi /storage/self/primary/_bak（push: PC至设备, pull 设备至PC），但mtp仍然不行）
          https://github.com/koush/UniversalAdbDriver
          https://blog.csdn.net/signjing/article/details/51835017
          https://github.com/labo89/adbGUI
          wifi热点 adb：https://blog.csdn.net/guojinpeng1/article/details/50722036
        - mtp（dism装.mum文件）
          https://www.tenforums.com/drivers-hardware/72798-media-transfer-protocol-device-mtp-device-support-windows-10-n.html
          https://forums.mydigitallife.net/threads/windows-10-enterprise-2016-ltsb-n-media-feature-pack.70784/
          https://www.zhihu.com/question/49805367
          https://support.microsoft.com/en-us/help/3145500/media-feature-pack-list-for-windows-n-editions
          mtp porting kit：https://www.microsoft.com/en-us/download/details.aspx?id=19153
        - adb和mtp互不影响：
          https://android.stackexchange.com/questions/96914/using-adb-without-mtp-support 
      3) 小破船（k690e g6d2）vga驱动问题：
         会报该机器不满足最低要求的错误，查看setupif文件发现是未加入8代U的判断（readme里实际是支持的，安装好后也没问题）；如果不安装，则无法调节分辨率（默认1920×1080），但分辨率大于屏幕分辨率，会导致卡顿严重（特别是玩游戏时）；安装好后将分辨率调节至1366×768（与15.6寸的分辨率匹配），这样便不会产生卡顿了
      4) vhd无法进入桌面（最终确定为盘符问题）
         - vhd显卡驱动问题（安全模式能启动，但卸载显卡驱动仍然无法在正常模式下进入系统）
           http://wuyou.net/forum.php?mod=viewthread&tid=326600
           安装完显卡后重启时-按F8（这是win7，可以按F8进入高级启动选项，win10可以在bootice设置进入safeboot）-选择以640\*480显示启动-成功进入桌面后再设定高解像度显示
           win8vhd装显卡驱动：http://bbs.zol.com.cn/nbbbs/d160_162854.html
           dism++驱动导出：http://tieba.baidu.com/p/3662213088
           显卡驱动卸载：https://zhuanlan.zhihu.com/p/29832338
         - sysperp（未测试）：https://blog.csdn.net/AloneSword/article/details/7268986
           需要windows自带的bcd存在才能进行
           之前无法登录应该是未进行sysprep的问题，因为很多注册表信息用的是绝对路径（c:\\...）而不是相对路径或者参数（%system）导致的
         - 看看能不能更改启动盘符
           按下面的方法改了启动盘符成功进入系统（而且显卡驱动都没卸载，仅仅修改了盘符；之前因为safeboot能进而normal不行以为是显卡驱动或者账号激活之类的问题，但其实就是盘符问题，毕竟是在生成wim文件的电脑上进行wimboot启动）
           http://blog.sina.cn/dpool/blog/s/blog_56799b500102vnf3.html
         - 若移动过wim文件位置，则需要重新apply至vhd（或者修改注册表中wimboot相关项，没找到方法…），否则会报0xc000025的错误
      5) 机械硬盘启动速度慢：无解，硬伤，考虑加入ramdisk
         尽量放在前面分区内：http://www.cnblogs.com/TianFang/p/4248227.html

*** 差分vhdx（弃用方案，仅作记录，原因：太吃空间，差分vhdx也不怎么）
    1) vhdx安装并激活server 2016（总管理，纯净，只升级和安装基本驱动）
       - 先尝试vhdx（母盘非差分）激活2016\*2（需要先把ltsb的vhdx弄好，以免出问题，移动硬盘拷贝），若不行则hdd上装2016\*2然后激活，差分的vhdx大小不固定
         server 2016：https://imagine.microsoft.com/zh-cn/catalog/product/524
         结果：win server仍然无法激活，win10可以在vhdx内激活（但不能在差分vhdx内激活）
       - ssd.System-ltsb&server?(可以现装，看看是不是数字，不是的话还是做个vhdx u盘建个存储池然后通过uefi+grub4dos?启动多系统－－可不可以通过Linux?)；
         server2016：
         https://www.jianshu.com/p/09d8fc782e9e
         http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1784159
         https://blog.csdn.net/meigang2012/article/category/7119268
         https://zhuanlan.zhihu.com/p/23302415
         https://github.com/m2nlight/WindowsServerToWindowsDesktop
         https://bbs.aliyun.com/read/273416.html
         https://blogs.technet.microsoft.com/technet_taiwan/2016/01/05/compute-windows-server-2016-hyper-v/
         https://blogs.technet.microsoft.com/technet_taiwan/2016/02/23/storage-windows-server-2016-storage-qos/
         以server2016为核心的家庭数据中心：https://post.smzdm.com/p/635872/
         http://blog.leijun.me/2017/02/15/KVM%E5%AE%89%E8%A3%85windows%20server%202016%E5%B9%B6%E5%90%AF%E7%94%A8WSUS%E6%9C%8D%E5%8A%A1/
         远程桌面服务配置和授权激活：https://blog.csdn.net/h8178/article/details/78354248
     
    2) vhdx安装ltsb 2016（实际使用，vhdx0-父，未安装）
       - 在server2016的基础上，拷贝vhdx至特定位置，bootice设置
       - diff vhdx（差分vhdx），要跟parent在同一个目录下
       - bootice添加引导，分别至父vhdx（pe）和子vhdx（真正使用），子vhdx不要勾选测试模式和winpe，关闭影子系统，即可正常使用
         TODO 如何激活，子vhdx的大小调整（要不要一开始就最大？）
         https://bbs.luobotou.org/thread-7995-1-1.html
       - 注意
         - 差分vhdx要跟父vhdx在一个目录，子vhdx不要勾选测试模式和winpe
         - wim解压至vhdx，用bootice引导，进入之后遇到报错（oobe）
       https://blog.csdn.net/gogcc/article/details/50844641
       https://answers.microsoft.com/zh-hans/windows/forum/windows8_1-windows_install-wininstalls/vhdx%e5%ae%89%e8%a3%8581%e5%87%ba%e7%8e%b0windows/0929ff38-9d92-407c-a1eb-f2169ee6ac18
       vhdx启动：https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/boot-to-vhd--native-boot--add-a-virtual-hard-disk-to-the-boot-menu
       http://bbs.pcbeta.com/viewthread-1099053-1-1.html
       http://bbs.pcbeta.com/viewthread-1577401-1-1.html
       https://www.zhihu.com/question/61091614
       http://www.cnblogs.com/cxchanpin/p/7055542.html
       kms：https://03k.org/
       esd：https://www.iruanmi.com/everything-about-windows-8-esd-image-files/
       http://tieba.baidu.com/p/3366067581
       http://bbs.ithome.com/thread-643237-1-1.html
       LTSB／LTSC序列：https://www.kechuang.org/t/82260
       win10 LTSB 2016 geek精简：http://bbs.pcbeta.com/viewthread-1774229-1-1.html
       ltsb优化：https://bbs.kafan.cn/thread-2121575-1-1.html
       avast+comodo：https://forum.avast.com/index.php?topic=196929.0
       https://bbs.kafan.cn/thread-2100263-1-1.html
       avast 离线安装包：把在线安装包地址中的online改成offline
       数字权利激活：http://bbs.pcbeta.com/viewthread-1786685-1-2.html
       win10 下载器：http://bbs.pcbeta.com/viewthread-1784485-1-3.html
       安装语言包要先将默认语言改成非要安装的语言并重启后再进行安装，否则有可能安装失败
** 存储池
   vhdx存储池：https://bbs.saraba1st.com/2b/thread-1296624-1-1.html
   https://www.xieyidian.com/4413
   https://www.v2ex.com/t/387385
   虚拟化：http://liuqunying.blog.51cto.com/3984207/1385861/
   ssd.缓存？shared data?；
   存储池是否操作系统无关？
   http://codefine.site/1222.html
   http://www.codeclip.com/313.html
   http://www.cnblogs.com/mslagee/articles/6136334.html
   - 注意
     留10G+SSD为未分区，留给存储池用
** 注意
   可以隐藏vhdx和boot文件所在盘符，仍能正常使用，并防止误操作

* 优化&美化
  vhdx1-子-安装，按用户
  基于用户（注册表，win+r runas，cmdkey，mklink等）
** 系统优化
   - 删除休眠文件
     https://blog.csdn.net/qq_35733535/article/details/78968394
   - directsubehanced/sub
   - 字体：字体安装能否用硬链接（/H）
     mactype（感觉没使用的必要）
     https://zhuanlan.zhihu.com/p/22269604
     https://bbs.kafan.cn/thread-1394358-1-1.html
     https://github.com/Tatsu-syo/noMeiryoUI
     https://www.sevenforums.com/tutorials/1175-fonts-change.html?filter
   - emacs取代notepad，vscode和editplus
     见spacemacs文章
   - powershell和cmd
     https://coolcode.org/2018/03/16/how-to-make-your-powershell-beautiful/
     https://blog.csdn.net/itanders/article/details/75305163
     #+BEGIN_SRC sh
     * chcp 65001
     colortool -b ayu
     Import-Module DirColors
     Update-DirColors C:\Users\liuguangyuan5\Documents\WindowsPowerShell\dir_colors\dircolors.ansi-dark
     Import-Module posh-git
     if (!(Get-SshAgent)) {
         Start-SshAgent
     }
     Import-Module oh-my-posh
     Set-Theme PowerLine
     Screenfetch
     #+END_SRC
     http://www.powershellmagazine.com/2014/03/17/pstip-reading-file-content-as-a-byte-array/
     https://blog.csdn.net/powershell/article/category/324943
     https://www.cnblogs.com/fungapapp/archive/2012/05/17/2506201.html
     https://windowsreport.com/change-windows-10-default-font/
     http://www.windowszj.com/news/20837.html
     https://superuser.com/questions/1211023/fontlink-fontlink-systemlink-in-registry-is-not-working-as-expected-in-window
   - shift+f10转移文件夹
     要转移的文件夹（user+font+SoftwareDistribution+Temp+Installer）：https://bbs.saraba1st.com/2b/thread-933858-1-1.html
     https://blog.csdn.net/CrowNAir/article/details/78533051
     https://wenku.baidu.com/view/ae7bb4154431b90d6c85c760.html
     https://www.tenforums.com/tutorials/1964-move-users-folder-location-windows-10-a.html
     Win10转移用户文件夹：https://blog.csdn.net/sinat_38799924/article/details/74059037
     https://social.technet.microsoft.com/Forums/zh-CN/5fb22bb2-7b01-40a7-a537-568d29904b31/-win-7-8-program-filesdocuments-and-settingsd?forum=window7betacn
     不能安装字体：直接通过注册表添加即可
   - theme
     Numix theme for Windows
     http://static.simpledesktops.com/uploads/desktops/2015/04/30/Solarized-Mountains.png
** 软件（vhdx2-子）
   software：免安装+安装至vhdx
   baiduPan：https://github.com/iikira/BaiduPCS-Go
   7z压缩tar：https://www.cnblogs.com/jinjiangongzuoshi/p/3778926.html
   vscode&editplus
   配色方案-安卓（高对比度 solarized ，monokai）：http://color-themes.com
   potplayer
   alipay&qq&weixin
   fdm&thunder
   java&daemontools
   cajviewer&office
   sbeam
   - chrome
     centbrowser&xx-net
     chrome报错：https://superuser.com/questions/1208867/chromium-not-loading-any-page-after-moving-user-folder/1208877
     noCoin：https://chrome.google.com/webstore/detail/no-coin/gojamcfopckidlocpkbelmpjcgmbgjcl
** linux
   debian+xfce
   （vhdx?hyper-v?wsl?docker?）
   armlinux
   https://stackoverflow.com/questions/17138970/is-it-possible-to-emulate-arm-on-windows-8
   - docker
     具体见virtual-machine文章
     https://zhuanlan.zhihu.com/p/29486013
     https://www.zhihu.com/question/22969309/answer/286142439

* 实际使用
  多次尝试后，最后一直使用的方式
  1) 神州网信版本
  2) hyper-v内安装系统得到vhdx
  3) 创建wimboot
     基底，可以用来部署到其他机器上
  4) wimboot+vhdx指针
     部署至其他机器上
** 流程
   以神州网信1803G的版本为例
   http://bbs.pcbeta.com/viewthread-1803772-4-1.html
   #+BEGIN_QUOTE
   0. 支持dism, hyper-v, vhdx的win10, win10iso（推荐win10g-神州网信政府版）, bootice（boot文件夹）, diskgenuis（或傲梅分区助手之类能对硬盘做操作的）
   1. 创建 虚拟机系统（带网卡，30g动态）
   之所以要在虚拟机内安装，是为了与硬件无关，同时自动更新时不会安装驱动
   2. 优化 虚拟机系统 
   2.1 删除之前的用户（建议默认用administrator，后续步骤再建立新用户）
   密码策略（不需要这么复杂的密码，不过administrator还是推荐密码复杂点）：组策略（win+r --> gpedit.msc）--> 计算机配置 --> windows配置 --> 安全设置 --> 密码策略
   2.2 更新（可能要先激活，需要 kmspico）
   ctrl+alt+del无法输入，需要直接用hyper-v的菜单功能来输入
   2.3 安装 .net3.5（需要到官网下载安装包，或在Windows10_V0-H.1020.000.iso\sources\sxs\下有相应的文件，个人用到dism++，是否能直接通过dism安装未确认）
   挂载vhdx，然后将临时的东西先拷到特定的文件夹内，然后再分离vhdx
   2.4 运行库（需要 directX enhanced）
   2.5 关闭休眠（POWERCFG /HIBERNATE OFF），删除网卡，关闭superfetch
   2.6 关闭recovery
   高级系统设置 --> 系统保护 --> 系统还原
   然后删除recovery分区和隐藏的recovery文件夹
   2.7 嵌套虚拟化开启（需要用到hyper-v/docker的要做此步骤）
   https://docs.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
   其中开启hyper-v功能后，会发现创建了虚拟网卡，不要禁用（禁用了还会继续创建）
   2.8 其他优化
   2.8.1 增加语言，默认输入法修改为英文
   2.8.2 （未在虚拟机内测试能否开启）ipv6开启（xx-net用绿色方式运行），关闭ipv6协议
   teredo已启用但ipv6 failed（netsh interface teredo set state server=teredo.remlab.net）：https://github.com/XX-net/XX-Net/wiki/IPv6-Win10
   （20190922更新）teredo已经基本废弃，改用isatap：https://github.com/tuna/ipv6.tsinghua.edu.cn/blob/master/isatap.md
   2.9 磁盘清理
   系统自带的即可
   3. 创建wimboot（base，硬件无关，无驱动）
   3.1 win10下挂载vhdx（假设挂载在d盘）
   dism /Capture-Image /ImageFile:e:\win10G1803.wim /CaptureDir:d:\ /Name:win10G1803base /Description:Windows10_V0-H.1020.000 /WIMBoot /CheckIntegrity /Verify
   3.2 看有没有wimboot（假设为e:\win10G1803.wim，可引导：是）
   dism /Get-ImageInfo /ImageFile:e:\win10G1803.wim /index:1
   3.3 创建vhdx（磁盘管理，20g动态，假设挂载在d盘，叫win10G1803.vhdx）
   dism /Apply-Image /ImageFile:e:\win10G1803.wim /Index:1 /ApplyDir:d:\ /WIMBoot /CheckIntegrity /Verify
   msr分区构建（4k对齐）：会发现vhdx内的msr分区不是4k对齐的（自动生成的，跟vhdx大小有关，貌似无法通过对虚拟机操作来影响），不影响使用，故暂未尝试重新创建
   3.4 用bootice设置从win10G1803.vhdx启动
   3.4.1 创建fat32格式分区boot（64m即可）放入boot.rar内的东西（根目录下即可），bootice不用修改分区引导记录（NTLDR）
   3.4.2 创建ntfs格式分区sys（64gb即可），bootice修改分区引导记录（BOOTMGR）为GRUB4DOS（0.4.6a）
   64g的大小是为了备份方便，比如带驱动的wim大小7g（base.wim大小5g，在其上append的），1个vhdx大小20g。这时候要备份这个vhdx，按照步骤4，最多需要7g+20g+（7+x）g（复制）+20g（新的vhdx）=（54+x）g，然后再删除之前的7g+20g，增量备份的x一般不会太大，64g足够。如果需要保留一个base以防无法启动，则需要多200mb（格式化后的vhdx大小）
   3.4.3 BCD编辑 --> 智能编辑 修改未对应的vhdx（TODO 具体见图）
   3.4.4（做了步骤2.7 嵌套虚拟化开启才需要此步骤） BCD编辑 --> 高级编辑 新建参数（3.4.3步骤中对应的项目）hypervisorlaunchtype --> auto
   4. 驱动
   4.1 安装驱动
   nv驱动要网上搜下哪个版本是最稳定的（存在负优化的情况）：https://www.nvidia.cn/object/icafe-cn.html
   https://forums.guru3d.com/forums/videocards-nvidia-geforce-drivers-section.21/
   Clean Version（guru3d上下载，推荐417.71+417.75hotfix）：
   nvidiaProfileInspector：https://github.com/Orbmu2k/nvidiaProfileInspector
   4.2 （可选）增加用户，常用软件，自行需求的优化
   4.3 磁盘清理
   4.4 创建wimboot（user，硬件相关，带驱动）
   更换系统（之前的系统），复制一份wimboot（假设为e:\systems\win10G1803.wim），然后挂载vhdx（假设挂载在d盘）
   dism /Append-Image /ImageFile:e:\systems\win10G1803.wim /CaptureDir:d:\ /Name:win10G1803user /Description:Windows10_V0-H.1020.000 /WIMBoot /CheckIntegrity /Verify
   4.5 看有没有wimboot
   dism /Get-ImageInfo /ImageFile:e:\systems\win10G1803.wim /index:2
   4.6 创建vhdx（假设挂载在d盘）
   dism /Apply-Image /ImageFile:e:\systems\win10G1803.wim /Index:2 /ApplyDir:d:\ /WIMBoot /CheckIntegrity /Verify
   4.7 设置引导，并删除步骤3中的wimboot和vhdx（e:\win10G1803.wim可以备份一个，因为其与硬件无关）
   #+END_QUOTE
```

