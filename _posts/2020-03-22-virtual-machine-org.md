
``` org
** docker
   http://dockone.io/article/783
   https://blog.csdn.net/21cnbao/article/details/56275456
   https://blog.csdn.net/xiangxiezhuren/article/category/7542528
   https://www.voidcn.com/blog/lcxinxi0613/article/p-1541530.html
   https://www.kancloud.cn/kancloud/docker-theory-install/50263
   http://www.sqlsec.com/2017/01/docker.html
   https://www.cntofu.com/user/22.html

   - docker on windows
   要login无视就好，powershell正常使用（pull会报错，用daocloud.io镜像）
   要注册docker需要人机验证（谷歌访问助手 or 翻墙）
   windows pull报错：
   https://stackoverflow.com/questions/48066994/docker-no-matching-manifest-for-windows-amd64-in-the-manifest-list-entries
   docker hub镜像：
   https://dashboard.daocloud.io/
   #+BEGIN_QUOTE
     "registry-mirrors": [
       "https://36aec6ba.m.daocloud.io"
     ],
     "insecure-registries": [
       "36aec6ba.m.daocloud.io"
     ],
   #+END_QUOTE
   docker run（建议用alpine来测试，比helloworld小，而且不用login）：
   http://www.runoob.com/docker/docker-image-usage.html
   #+BEGIN_SRC sh
   docker ps -a
   docker image ls -a
   docker save -o alpine.docker alpine（将image打包至docker文件，实际上是tar文件）
   docker load -i alpine.docker（load至image）
   docker run -t -i alpine（自动创建新container）
   #+END_SRC
   ![](https://upload-images.jianshu.io/upload_images/293860-b7b016c55afb5033.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
   #+BEGIN_SRC sh
   docker run -i -t -d -v /d/netdisk/docker:/mnt --name mnt_test silex/emacs:26.1-alpine
   //C-x C-c
   //docker start mnt_test
   //有-d后台运行了
   docker exec -i -t mnt_test sh -c "env LANG=C.UTF-8 && emacs --daemon"
   //每次运行，如何传递参数？bat文件？
   mklink /H "D:\netdisk\docker\test.txt" "D:\test.txt"
   docker exec -i -t mnt_test sh -c "cd /mnt && emacsclient test.txt" // -c新frame
   // docker exec -i -t mnt_test sh 进入linux进行操作
   #+END_SRC
   - 问题&解决
   登陆 or pull不成功（509，401），将 http://xxxx.m.daocloud.io 添加到insecure-registeris
   https://segmentfault.com/a/1190000006799421
   https://blog.csdn.net/bericb310/article/details/72357080
   https://www.cnblogs.com/panjunbai/p/7106215.html
   Docker Pull Image : error pulling image configuration: unexpected EOF：https://ningyu1.github.io/site/post/83-docker-pull-error/
   离线下载image（没办法，还是要注册）：
   https://devops.stackexchange.com/questions/2731/downloading-docker-images-from-docker-hub-without-using-docker
   试用spacemacs发现是非gui版本，无法安装package，而且不支持中文，打算自己制作镜像
   https://www.cnblogs.com/linjj/p/5606911.html
   - GUI
   https://www.lainme.com/doku.php/blog/2018/07/%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E7%9A%84%E5%9C%A8windows_10%E4%B8%8A%E8%A3%85x
   https://stackoverflow.com/questions/39182483/how-to-use-x-windows-with-emacs-on-windows-10-bash
   https://stackoverflow.com/questions/16296753/can-you-run-gui-apps-in-a-docker-container
   https://ask.helplib.com/others/post_357836
   > https://www.cnblogs.com/Rong-/p/7670752.html
   https://blog.csdn.net/weixin_37728669/article/details/59548605
   https://www.aliyun.com/jiaocheng/873748.html
   - image支持中文
   https://segmentfault.com/a/1190000005026503
   https://www.jianshu.com/p/6fe582ba6d3d
   - 传递参数
   https://yanbin.blog/pass-arguments-to-docker-container/#more-8608
   https://blog.csdn.net/wsbgmofo/article/details/79173920
   https://www.louisvv.com/?p=695
   - 文件传递
   https://blog.csdn.net/yangzhenping/article/details/43667785
   https://blog.csdn.net/ap10062kai/article/details/79232582
   https://blog.csdn.net/sportshark/article/details/52714128
   #+BEGIN_QUOTE
   The cleanest way is to mount a host directory on the container before running your command:
     #+BEGIN_SRC sh
     {host} docker run -v /path/to/hostdir:/mnt --name my_container my_image
     {host} docker exec -it my_container bash
     {container} cp /mnt/sourcefile /path/to/destfile
     #+END_SRC
   #+END_QUOTE
   #+BEGIN_QUOTE
   可用命令：docker run -t -i -v /d/netdisk/docker:/mnt --name mnt_test debian 创建共享目录的docker container（名字只能小写，/d/netdisk/docker即d:\netdisk\docker）
   然后用mklink /H "D:\netdisk\docker\1.h" "D:\netdisk\material\VdecTestMulti.h"将要编辑的文件进行硬链接
   用docker exec -it mnt_test sh -c "cd /mnt && cat 1.h" （确保container是运行（up）的，不用docker attach mnt_test） 
   #+END_QUOTE
   - 问题
     1) exec之后会直接退出docker（必须保证前天有程序，如/bin/bash）
     https://segmentfault.com/q/1010000004310264?_ea=610309（ctrl-d退出exec）
   https://blog.csdn.net/jiangh_catr/article/details/52782815
     2) emacs自动备份导致硬链接无效（关闭自动备份）
     https://blog.csdn.net/grey_csdn/article/details/78898095
     3) 配置问题（例如关闭自动备份的设置）
     将emacs.d（或spacemacs等）配置用docker compose做到镜像里面即可解决
     4) 共用剪贴板
   - exec
   https://www.cnblogs.com/xhyan/p/6593075.html
   https://blog.csdn.net/taiyangdao/article/details/71598935
   - 清理
   https://www.v2ex.com/t/328017
   - server（退出不关闭）
   https://blog.csdn.net/O1_1O/article/details/52710733
   https://blog.csdn.net/leoe_/article/details/78685186
   - 轻量docker对比
   https://qiita.com/A-I/items/3d2aedbad4f10141cef3
   alpine：
   http://blog.51cto.com/laodou/2156254
   https://wiki.alpinelinux.org/wiki/Running_glibc_programs
   https://github.com/frol/docker-alpine-glibc
   busybox（busybox:glibc）：
   https://github.com/docker-library/busybox
   https://blog.csdn.net/chengqiuming/article/details/79313539
   https://tonybai.com/2017/12/21/the-concise-history-of-docker-image-building/
   https://alitrack.com/2016/10/12/docker%E7%98%A6%E8%BA%AB%E8%AE%B0/
   gcc：
   https://hub.docker.com/r/frolvlad/alpine-gcc

** lxc(embedded linux)
   docker on embedded linux / embedded container / lxc - linux container
   https://blog.csdn.net/caofengtao1314/category_7902354.html
   https://stackoverflow.com/questions/34010832/docker-on-embedded-systems-why-not
   https://blog.feabhas.com/2017/09/introduction-docker-embedded-developers-part-1-getting-started/
   https://bbs.csdn.net/topics/392339719
   https://3mdeb.com/firmware/optimize-performance-in-docker-containers/
   https://blog.csdn.net/talkxin/article/details/83011017
   https://elinux.org/images/2/22/Evalution_of_linux_container_on_embedded_linux.pdf
   http://www.diva-portal.se/smash/get/diva2:951819/FULLTEXT01.pdf
   https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/26500/lammi.pdf
   https://blog.csdn.net/yaojiawan/article/details/91361271
   https://blog.csdn.net/lhl_blog/article/details/99436441
   hal/gpio控制：https://stackoverflow.com/questions/30059784/docker-access-to-raspberry-pi-gpio-pins
   ko：https://stackoverflow.com/questions/24438386/kernel-module-inside-of-lxc-container
   kvm：https://stackoverflow.com/questions/20578039/difference-between-kvm-and-lxc
   #+BEGIN_SRC sh
   /* 环境准备 */
   lxcfs -s -f -o allow_other /var/lib/lxcfs/
   mount -t cgroup -o cpu,cpuset,memory,devices cgroup /sys/fs/cgroup
   /* 启动 */
   lxc-create -n demo_lheopuart -t /var/lib/lxc/lxc-busybox
   lxc-start -n demo_lheopuart -d
   lxc-attach -n demo_lheopuart -u 0 -g 0 -b -- demo_lheopuart
   /* 停止 */
   lxc-stop -n demo_lheopuart -t 1
   lxc-wait -n demo_lheopuart -s STOPPED -t 15
   lxc-destroy -n demo_lheopuart
   rm -rf /home/share/demo_lheopuart
   #+END_SRC sh
   pam
   https://www.cnblogs.com/kevingrace/p/8671964.html 

** hyper-v(windows)
   需求（已检测到虚拟机监控程序）：https://www.skyfinder.cc/2018/11/19/windows10hypervsystemrequirements/
   复制（用查分vhdx：hyper-v→操作→新建→硬盘，再通过此方法导入，然后删除导入目录下多生成的父vhdx）：http://blog.51cto.com/lumay0526/2049064
   网络：https://shiyousan.com/post/636364159616645479
   宿主机做虚拟路由（公司网不支持网络共享，修改deb源）：
   https://www.cnblogs.com/xiaodiejinghong/p/3149198.html
   修改deb源（http://mirrors.hikvision.com.cn/help/ubuntu.html）：
   https://www.cnblogs.com/TechSnail/p/7754969.html
   删除vm文件夹：https://social.technet.microsoft.com/Forums/windowsserver/en-US/4c4f2535-81ee-4a21-a920-b5632de2be37/hyperv-old-vm-folders-cannot-be-deleted
   ssh（apt-get install openssh-server）：https://blog.csdn.net/lifengxun20121019/article/details/13627757
   could not get lock：https://www.cnblogs.com/yun6853992/p/9343816.html
   sdl：https://www.cnblogs.com/dylancao/p/9039632.html
   - hyper-v装windows
   第二代的vm要在提示boot from dvd出来的时候按任意键才会从dvd启动…如果没有按键就会报错- -'
   https://social.technet.microsoft.com/Forums/office/en-US/29574a12-7528-4f49-ae12-6fe1d85b3732/hyperv-vm-generation-2-how-to-boot-from-iso
   https://blogs.technet.microsoft.com/jhoward/2013/11/11/hyper-v-generation-2-virtual-machines-part-9/
   https://www.reddit.com/r/HyperV/comments/3mfinz/installing_windows_10_iso_to_a_new_hyperv_machine/
   - 嵌套虚拟化（nested）
   https://docs.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
   https://www.sysgeek.cn/hyper-v-nested-virtualization/
   https://blogs.technet.microsoft.com/gbanin/2013/06/25/how-to-install-hyper-v-on-a-virtual-machine-in-hyper-v/
   RSAT：https://serverfault.com/questions/713187/powershell-install-windowsfeature-and-family-missing-on-windows-10

```

