
``` org
** 序
   最近学习、移植、使用了nng、glib、webrtc，分别是用cmake、meson、gn来管理工程，自然也就把这三个buildtool粗略的学了下。结合之前所做的工程，感悟颇多，大致可以归纳为：从buildtool的各个困境看工程上的矛盾。

** buildsystem
   使用buildtool的最终目的便是管理工程，这里面涉及诸多哲学和冲突，我们先从buildtool本身这套构筑系统（buildsystem）的变更说起。
   - 旧构筑系统
     个人阅历原因，只能从vc6盛行的年代讲起，更早的构筑系统不此在叙述
     - win&mac
       图形化界面的构筑系统，以vs&xcode为代表的ide，内部采用的编译、链接等工具皆是黑盒，傻瓜式，操作简单，但局限性大（交叉编译什么的想想就好）
       命令行式的构筑系统，win上用winsdk带的buildtool，命令行运行cl-link或nmake运行makefile脚本编译，也可以用msbuild直接编译vcprj工程
     - *nix
       autotool(m4, pkgconfig, …)+makefile+shellscript的形式，这里已经可以看出构筑系统的所需（工具-工具/文件脚本-脚本生成工具），包管理则是个独立的部分
     cmake则是我所接触的第一个跨平台编译的构筑系统，但是那时候还不是modern-cmake（有多难用不用我说了吧）。
   - 新构筑系统
     gn/meson/modern-cmake（脚本生成、包管理、...）+ninja（工具脚本）+python/lua（文件脚本）
     似乎所有的工作都只是为了跨平台（*nix, win, apple, …）问题，makefile变成ninja，shellscript变成python/lua，...
   但其实工程上遇到的问题还有很多，大部分都涉及到了管理上的哲学，是不可调和的矛盾，也是从未被解决的困境
   - 跨语言
     c&cxx/objc&objcxx/java均支持
   - 跨编译平台
     win/nix/mac均可编译
   - 跨平台运行（交叉编译）
     win/nix/mac/embed均可运行
   - 多工具链
     clang/gcc/msvc均支持
   - 包管理
   - version control
     - debug/release
     - major/minor/patch/build(time/hash/...)
     - flags/rules/cross/toolchain/platform/arch/os/...
     - ...
   - 易用性
     可复用/语言简单/...
   - ...
   如果不重视这些问题，一个工程，在迭代过程中，将会不可避免的，变成“屎山”（前人填坑后人添屎）
*** buildsystem本质
    1. src --f-- dst | project本质：映射（集合），什么都不做的project：自己映射自己
    2. src<a1,a2,a3,...> --f1,f2,f3,...-- dst<b1,b2,b3,...> | deps本质：映射（元素），f1,f2,f3之间可不同，可相同（类似接口的概念）
    3. src<sub1(a1,a2),sub2(a1,a3),sub3(a4),...> --fa1,fa2,fa3,...--, --fb1,fb2,fb3,...-- dst<b1,b2,b3,...> | meson/gn/cmake等buildtool本质：描述映射的语言，将映射转为脚本，即根据deps生成ninja/makefile | 优化：某些映射不是严格分层的，比如src<a1>跟tmp<t1>生成dst<b1>，优化成src<a1>生成tmp<t2>（自己映射自己），tmp<t2>跟tmp<t1>生成dst<b1>，这样利于（并行？）
    eg. 
    #+BEGIN_QUOTE
    1. steps
       0. src<a1.c/a2.c/a3.cc/a4.d/a5.java/a6.html/a7.ts>
       1. sub1<a1.c/a3.cc> sub2<a2.c a4.d> sub3<a2.c> sub4<a5.java> sub5<a6.html a7.ts>
       2. tmp1<b1.so/b2.elf/b3.jar/b4.js>
       3. tmp2<doc<c1.html/c2.js>/env<c3.jar>/lib<c4.so>/tool<c5.elf>>
       4. dst<d1.tar.gz>
       5. deps
    2. mapping
       sub3--f0--b2.elf(image it as a move cmd)
       sub1--f1,f2--b1.so--b2.elf--lib/c4.so--f5--d1.tar.gz
       sub2--f2,f3,b1.so,f4,b2.elf--tool/c5.elf--f5--d1.tar.gz
       sub4--f6--b3.jar--b2.elf--env/c3.jar--f5--d1.tar.gz
       sub5--f7--b4.js--b2.elf--doc/c1.html doc/cj.js--f5--d1.tar.gz
    3. rules
       f0: x86_64-c-compiler
       f1: arm-cxx-compiler
       f2: arm-c-compiler
       f3: arm-d-compiler
       f4: arm-elf-linker
       f5: tar
       f6: java-compiler
       f7: ts-compiler
    #+END_QUOTE

** 矛盾
   按照buildsystem需要解决的问题，看看工程上遇到的矛盾和流派
   - 跨平台（交叉编译）
     假设有个函数叫fff（非语言标准的函数），a平台上自带的库对应函数叫fffa，b平台上自带的库对应函数叫fffb
     为了跨平台，有两种方式（流派）
     - 代码封装一层system_wrapper，叫mmm_fff
       内部按照不同平台不同文件或同文件内不同分支（又是两种流派），后续都调用这个（创造语言+1）
     - 用其他库，ccc_fff
       跨平台前得先编译该库
     跨平台的问题关系到可移植性和代码侵入性，也可以说是方言/语法糖跟portable/耦合性之间的矛盾
     eg. 
     #+BEGIN_QUOTE
     类似
     #define MK_STRUCT(_type, ...) (_type){__VA_ARGS__}
     debug_printf(lvl, ...)
     如果大量运用在代码中，那么将这个当作一个模块，这部分代码就是跟模块强耦合；想单独剥离出代码进行移植，就得先移植这个模块
     #+END_QUOTE
     如何平衡耦合性和移植性之间的矛盾？
     当这些东西运用的多的时候（比如在github上有几千的star），这个就变成一个基础库；更上一层，当这个东西被大机构（比如苹果/firefox）运用的多了，就变成了方言（obj-c/rust前身），当这些方言可以通过bootstrap进行自举，这就变成了新的“语言”（obj-c/rust）
   - 包管理
     假设有个third_party的模块叫mmm，工程依赖于它，有两种流派
     - mmm源码加入工程
     - mmm的地址/版本/hash值加入工程
       编译和运行同平台需先下载成果物
       交叉编译工程前需要先交叉编译mmm
       那么是用自带的编译系统？还是化成工程的编译系统
     包管理的问题关系工程维护，维护难度和维护成本两者的不可协调，比如遇到以下情况，该怎么办
     - 当第三方软件更新时
     - 工程需要改变/调优/bug-fix该模块
     是通过修改源码？还是打patch？当然也可以做个接口wrapper，那问题也会变得跟跨平台遇到的类似了（创造语言+2）
   - 跨语言（多目标）
     总所周知，c是个对字符串十分无力的语言，那么如果a语言适合做字符串，b语言适合底层交互，c语言适合界面开发，...
     是同个语言多个target，多个语言同个target，如何管理工程，有几种流派
     - 不同target不同工程
       中间重合部分怎么办？一份代码更新维护好几个工程？
       维护人员分配给哪个项目？工作量怎么算？
     - 整个target一个工程
       我要单独提取一个部分用于其他工程怎么办？
     多目标的问题有点类似人月神话的问题，glib、webrtc等几个大工程的趋势是多语言，合适的工具做合适的，无他，只因为开源大项目，人多
     同时又涉及到模块化，ffi（语言间接口）这些问题
   - 多工具链
     flag（debug/O0/O1/O2/Os/...）管理，本质问题跟跨平台类似，但也包含部分版本管理/问题回溯的问题
     同时也是不同跨平台中不同流派问题
     eg. 有个函数在win上交fw，在posix上叫fp，如何管理
     - 一个文件
       #if win32
         fw
       #elif posix
         fp
       #else
         error
       #endif
     - 多个文件
       f_win.c和f_posix.c，然后在工具/文件脚本内写
       #if win32
         f_win.c
       #elif posix
         f_posix.c
       #else
         error
       #endif
   - 版本管理
     这个版本管理不是管理代码本身，那是git/svn这些工具做的事情。考虑如下问题，你需要追踪每个生成的成果物，该怎么办
     一个通用的做法是将编译时间等env/工程信息加到代码内，在崩溃时打印/保存这些信息，便于追溯问题
     但是又会出现另一些问题，比如不能reproducible build造成的某次编译出错，但同个版本的源码下次编译又没有问题，这时候该如何管理
     侵入性也是版本管理中的问题，比如获取到的git/svn地址因utf-8/gbk的字符集问题造成了代码崩溃

   做梦做到的矛盾很精辟，现在写出来就很拉跨，不知道为什么，感觉总结的没有梦里精彩，毕竟梦里啥都有...

** buidltools
   TODO 放一些链接和学习笔记
   - modern cmake
   - meson
   - gn

   fini
```
