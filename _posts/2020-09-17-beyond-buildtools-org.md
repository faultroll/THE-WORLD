
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
   - 脚本语言
     脚本语言的跨平台也是个矛盾点，为了自动编译（CI）/部署/运行，脚本语言一般是不同编译平台不同，不同运行平台也不同。windows上的cmd，*nix上的shell，往往学了编程语言，还得学对应平台的脚本语言。为了统一编译平台的脚本，现在很多主流开源项目都慢慢从cmd/shell往python转变（其实meson就是个python程序）。但是python又不是为了嵌入式这种环境设计的（试着移植一个就知道了，又难又大），这就造成编译时和运行时的不统一（不过也可以说运行时脚本其实是源代码/成果物），还是得多学几门语言。当然有可以统一的语言，比如有cpan支持的perl，有简洁明了的lua（xmake就是用lua开发的，也是个buildtool），但是perl晦涩，lua小众，其他类似ruby/lisp/scheme/haskell这些就更小众晦涩难移植了。

   做梦做到的矛盾很精辟，现在写出来就很拉跨，不知道为什么，感觉总结的没有梦里精彩，毕竟梦里啥都有...

** buidltools
*** 一些资料
    #+BEGIN_QUOTE
    build systems: https://ruudvanasseldonk.com/2018/09/03/build-system-insights
    https://stackoverflow.com/questions/12017580/c-build-systems-what-to-use
    https://stackoverflow.com/questions/4071880/what-are-the-differences-between-autotools-cmake-and-scons
    http://leesei.github.io/build-systems/
    https://carlosvin.github.io/posts/choosing-modern-cpp-stack
    multi-compiler/target/platform: https://stackoverflow.com/questions/1929846/what-is-currently-the-best-build-system
    https://stackoverflow.com/questions/14306642/adding-multiple-executables-in-cmake
    https://stackoverflow.com/questions/636841/how-do-multiple-languages-interact-in-one-project
    https://stackoverflow.com/questions/25365513/how-to-make-cmake-targeting-multiple-platforms-in-a-single-build
    demos: https://github.com/dyu/cpp-build-example
    #+END_QUOTE
    - modern cmake
      #+BEGIN_QUOTE
      cmake config.h.in: https://stackoverflow.com/questions/38419876/cmake-generate-config-h-like-from-autoconf
      https://stackoverflow.com/questions/647892/how-to-check-header-files-and-library-functions-in-cmake-like-it-is-done-in-auto 
      https://github.com/dev-cafe/cmake-cookbook
      cmake: https://axel.isouard.fr/
      #+END_QUOTE
    - meson
      #+BEGIN_QUOTE
      meson: https://gitlab.freedesktop.org/pulseaudio/webrtc-audio-processing
      https://github.com/strfry/webrtc-meson
      #+END_QUOTE
    - gn
      #+BEGIN_QUOTE
      google gn(gyp): https://blog.csdn.net/weixin_44701535/article/details/88355958
      https://www.cnblogs.com/xl2432/p/11844943.html
      https://blog.csdn.net/zhangtracy/article/details/79045363
      https://blog.csdn.net/zhangkai19890929/article/details/81909780
      https://blog.csdn.net/caoshangpa/article/details/53353681
      https://blog.csdn.net/IdeasPad/article/details/100867361
      https://blog.csdn.net/Vincent95/article/details/78499883
      https://blog.csdn.net/define_me_freedom/article/details/104196475
      https://blog.csdn.net/zhangkai19890929/article/details/82013780
      https://blog.csdn.net/foruok/article/details/70050342
      https://blog.simplypatrick.com/posts/2016/01-23-gn/
      https://stackoverflow.com/questions/49860282/how-can-i-create-project-by-chromium-gn-tools
      因为google相关的都被墙（ https://gn.googlesource.com/gn ），故用github上的镜像（ninja就没这问题，说明gn其实也不是个稳定版本？）
      以下每个版本都各有特色，推荐用opera的编译成果物
      build opera gn
      1. cxx=g++ ld=g++ ar=ar --no-last-commit-position
      2. generate last-commit-position.h by yourself (mimic the python script), put it in out/
      3. delete rule build.ninja in ninja.build
      4. ninja -C out/ gn
      https://github.com/operasoftware/gn-opera
      https://github.com/timniederhausen/gn
      https://github.com/o-lim/generate-ninja
      https://github.com/vivaldi/Vivaldi-GN
      https://github.com/chromium/eclipse-gn
      gn坑：https://blog.csdn.net/u014786330/article/details/84562388
      https://www.suninf.net/2017/05/gn-usage.html
      nacl: https://blog.gmem.cc/chrome-native-client-study-note
      goma: https://blog.csdn.net/Vincent95/article/details/78477597
      pgo lto: https://developer.ibm.com/technologies/systems/articles/gcc-profile-guided-optimization-to-accelerate-aix-applications
      kythe: https://maskray.me/blog/2017-07-01-emacs-helm-kythe-and-haskell-xrefs
      rc: https://blog.csdn.net/wangningyu/article/details/4844687
      how webrtc gn compile java and c/c++ in one project 
      build/config/android/internal_rule.gni -- template(compile_java) -- action_with_pydeps -- build/android/gyp/compile_java.py --  build/android/gyp/build_utils.py -- JAVAC_PATH
      so it compiles java directly using python script, not through ninja build
      #+END_QUOTE
*** 开发环境配置
**** 开发目标需求
    - win
      虽然很少有windows上运行程序的开发需求，但如果要开发windows窗口程序（一般是基于qt，当然直接学csharp也可以），也得配置开发环境
      - msvc
        - ide
          直接安装vs2013/2015/...这些ide，优点是方便，但多开发平台难以统一
        - cl&link+winsdk+buildtools
          优点是各个平台一致，但是配置麻烦；msvc通过vs_buildtools获取（选），winsdk直接官网下载（版本是跟msvc对应？还是跟windows对应？需确认），buildtools三大跨平台的都行（cmake/meson/gn，cmake后端需选ninja）
      - mingw/cygwin/msys2/clang/...
        交叉编译，优点是统一至一个开发环境（*nix平台），缺点是慢（哪怕最底层的mingw，跟msvc原生的比，界面方面仍就是慢）
    - *nix
      - gcc/clang
        build-essential, g++, ...
      - linaro/android/...
        交叉编译，一般是跑在板子上，vswork/mips这些架构的交叉编译工具链长啥样没见过，倒是见过ti/stm的裸板工具链（用的是gcc-arm-none-eabi）
    - macos
      不熟，没试过，不想，告辞
      - ide
        macos上跑的程序不都用obj-c的开发工具？
      - 本质上是*nix（或者说*bsd？）
        只要知道交叉编译工具链即可统一至*nix平台
**** 环境配置
     windows宿主机，linux虚拟机；windows上配置简易开发环境，linux上做主力开发环境；当作两台独立主机考虑，两者间命令通过ssh/telnet交互，数据通过samba（windows挂载linux）/nfs（linux挂载windows）或tftp（直传）交互
     - windows
       msvc
       https://stackoverflow.com/questions/49089448/how-to-download-and-install-microsofts-visual-studio-c-c-compiler-without-vis
       https://stackoverflow.com/questions/46684230/visualstudio-build-tools-2017-offline-installer
       （需运行installer目录下的exe，同时建立2019/buildtool目录）
       https://blog.csdn.net/zhangpeterx/article/details/86602394
       https://blog.csdn.net/yxy244/article/details/90368917
       msbuild编译sln工程：https://www.dearcloud.cn/archives/648
       https://blog.csdn.net/cloudspider/article/details/95390120
       按照cmake/gn--sln/vcprj--ninja(nmake)--cl/link/lib(msvc buildtool & winsdk(for libc&libstdc++&...))模式
       - git-bash（代替cmd）
         svn cmd（用VisualSVN维护的apache subversion command line tools）
         https://stackoverflow.com/questions/2341134/command-line-svn-for-windows
         http://subversion.apache.org/packages.html
         git-svn: https://blog.csdn.net/iteye_21309/article/details/81600230
       - vscodium（代码编辑器）
         只作为编辑工具（配合astyle格式化），当然可以用emacs之类的
         #+BEGIN_SRC json
         {
           "window.zoomLevel": 0,
           "extensions.ignoreRecommendations": true,
           "workbench.startupEditor": "none",
           "workbench.colorTheme": "Tiny Light",
           "files.encoding": "utf8",
           "files.autoGuessEncoding": false,
           "files.eol": "\n",
           "editor.minimap.enabled": false,
           "editor.fontFamily": "'Ricty Diminished Discord with Fira Code', 'Sarasa Mono CL'",
           "editor.fontSize": 16,
           "editor.renderControlCharacters": false,
           "editor.tabSize": 4,
           "terminal.integrated.fontFamily": "'Ricty Diminished Discord with Fira Code', 'Sarasa Mono CL'",
           "terminal.integrated.fontSize": 16,
           "markdown.preview.fontFamily": "'Ricty Diminished Discord with Fira Code', 'Sarasa Mono CL'",
           "markdown.preview.fontSize": 16,
           "git.ignoreMissingGitWarning": true,
           "astyle.cmd_options": [
               "--style=linux",
               "--indent=spaces=4",
               "--indent-preproc-block",
               "--indent-preproc-define",
               "--min-conditional-indent=0",
               // "--delete-empty-lines",
               "--unpad-paren",
               "--pad-oper",
               "--pad-header",
               "--align-pointer=name",
               "--align-reference=name",
               "--indent-col1-comments",
               // "--break-blocks",
               // "--add-braces",
               "--indent-switches",
               "--indent-labels",
               "--break-one-line-headers",
               "--attach-return-type",
               "--attach-return-type-decl",
               "--max-code-length=80",
               "--break-after-logical",
           ],
           "files.associations": {
               "*.h": "c"
           },
         }
         #+END_SRC
         测试等宽用的文本（如果用系统自带的字体，可以用simsum-extb+jheng）
         #+BEGIN_QUOTE
         ;; 如果配置好了， 下面20个汉字与40个英文字母应该等长
         ;; here are 20 hanzi and 40 english chars, see if they are the same width
         ;;
         ;; aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa| ;; eng
         ;; 你你你你你你你你你你你你你你你你你你你你| ;; chn
         ;; ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,| ;; comma
         ;; 。。。。。。。。。。。。。。。。。。。。| ;; dot
         ;; 1111111111111111111111111111111111111111| ;; num
         ;; 東東東東東東東東東東東東東東東東東東東東| ;; jp
         ;; ここここここここここここここここここここ| ;; jp
         ;; ｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺｺ| ;; jp
         ;; 까까까까까까까까까까까까까까까까까까까까| ;; korean
         ;; 𠄀𠄁𠄂𠄃𠄄𠄅𠄆𠄇𠄈𠄉𠄀𠄁𠄂𠄃𠄄𠄅𠄆𠄇𠄈𠄄| ;; ext-b
         
         +----------------------------------------------------+
         | 中英文等宽对齐设置：按加号或减号按钮直至此表格对齐 |
         | abcdefjhijklmnoprqstuvwxwyABCDEFJHIJkLMNOPQRSTUVXW |
         | 𠄀𠄁𠄂𠄃𠄄𠄅𠄆𠄇𠄈𠄉𠄀𠄁𠄂𠄃𠄄𠄅𠄆𠄇𠄈𠄄𠄅𠄆𠄇𠄇𠄆 |
         | 英文字号   中文对齐设置    EXT-B 对齐设置    测试  |
         +----------------------------------------------------+
         #+END_QUOTE
         插件配置，安装vsix失败可以解压后导入（跟chrome crx一个尿性）
         TODO linuxkerneldev插件使用
         #+BEGIN_QUOTE
         vscode embedded linux: https://marketplace.visualstudio.com/items?itemName=microhobby.linuxkerneldev
         （bash）https://git-scm.com/download/win
         （ctags，版本按照非win版本的hash搜）https://github.com/universal-ctags/ctags-win32
         https://shellbombs.github.io/vscode-for-linux-kernel/
         https://sthbrx.github.io/blog/2019/05/07/visual-studio-code-for-linux-kernel-development/
         #+END_QUOTE
       - superputty(iputty)（跟linux虚拟机交互）
         iputty配置文件如下，当然也可以用x-term/ms-terminal(with tftp/xftp, x/y/z modem)
         #+BEGIN_QUOTE
         zDownloadDir\E:%5Cuser%5CDesktop\
         szOptions\-e%20-v\
         szCommand\\
         rzOptions\-e%20-v\
         rzCommand\\
         CygtermCommand\\
         Cygterm64\0\
         CygtermAutoPath\1\
         CygtermAltMetabit\0\
         HyperlinkRegularExpression\((((https%3F|ftp):%5C/%5C/)|www%5C.)(([0-9]+%5C.[0-9]+%5C.[0-9]+%5C.[0-9]+)|(([a-zA-Z0-9%5C-]+%5C.)%2A[a-zA-Z0-9%5C-]+%5C.(aero|asia|biz|cat|com|coop|kim|info|int|jobs|mobi|museum|name|net|org|post|pro|tel|travel|xxx|edu|gov|mil|[a-zA-Z][a-zA-Z]))|([a-z]+[0-9]%2A))(:[0-9]+)%3F((%5C/|%5C%3F)[^%20"]%2A[^%20,;%5C.:">)])%3F)|(spotify:[^%20]+:[^%20]+)\
         HyperlinkBrowser\\
         HyperlinkRegularExpressionUseDefault\1\
         HyperlinkBrowserUseDefault\1\
         HyperlinkUseCtrlClick\0\
         HyperlinkUnderline\1\
         SSHManualHostKeys\\
         SSHHostkeyCheck\0\
         ConnectionSharingDownstream\1\
         ConnectionSharingUpstream\1\
         ConnectionSharing\0\
         WindowClass\\
         SerialFlowControl\1\
         SerialParity\0\
         SerialStopHalfbits\2\
         SerialDataBits\8\
         SerialSpeed\9600\
         SerialLine\COM1\
         ShadowBoldOffset\1\
         ShadowBold\0\
         WideBoldFontHeight\0\
         WideBoldFontCharSet\0\
         WideBoldFontIsBold\0\
         WideBoldFont\\
         WideFontHeight\0\
         WideFontCharSet\0\
         WideFontIsBold\0\
         WideFont\\
         BoldFontHeight\0\
         BoldFontCharSet\0\
         BoldFontIsBold\0\
         BoldFont\\
         ScrollbarOnLeft\0\
         LoginShell\1\
         StampUtmp\1\
         BugChanReq\0\
         BugWinadj\0\
         BugOldGex2\0\
         BugMaxPkt2\0\
         BugRekey2\0\
         BugPKSessID2\0\
         BugRSAPad2\0\
         BugDeriveKey2\0\
         BugHMAC2\0\
         BugIgnore2\0\
         BugRSA1\0\
         BugPlainPW1\0\
         BugIgnore1\0\
         PortForwardings\\
         RemotePortAcceptAll\0\
         LocalPortAcceptAll\0\
         X11AuthFile\\
         X11AuthType\1\
         X11Display\\
         X11Forward\0\
         BlinkText\0\
         BCE\1\
         LockSize\0\
         EraseToScrollback\1\
         ScrollOnDisp\1\
         ScrollOnKey\0\
         ScrollBarFullScreen\0\
         ScrollBar\1\
         CapsLockCyr\0\
         Printer\\
         UTF8Override\1\
         CJKAmbigWide\0\
         LineCodePage\\
         Wordness224\2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,2,2,2,2,2,2,2,2\
         Wordness192\2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,2,2,2,2,2,2,2,2\
         Wordness160\1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1\
         Wordness128\1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1\
         Wordness96\1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,1\
         Wordness64\1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,2\
         Wordness32\0,1,2,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1\
         Wordness0\0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0\
         MouseOverride\1\
         RectSelect\0\
         MouseIsXterm\0\
         PasteRTF\0\
         RawCNP\0\
         Colour21\255,255,255\
         Colour20\187,187,187\
         Colour19\85,255,255\
         Colour18\0,187,187\
         Colour17\255,85,255\
         Colour16\187,0,187\
         Colour15\85,85,255\
         Colour14\0,0,187\
         Colour13\255,255,85\
         Colour12\187,187,0\
         Colour11\85,255,85\
         Colour10\0,187,0\
         Colour9\255,85,85\
         Colour8\187,0,0\
         Colour7\85,85,85\
         Colour6\0,0,0\
         Colour5\0,255,0\
         Colour4\0,0,0\
         Colour3\85,85,85\
         Colour2\0,0,0\
         Colour1\255,255,255\
         Colour0\187,187,187\
         BoldAsColour\1\
         Xterm256Colour\1\
         ANSIColour\1\
         TryPalette\0\
         UseSystemColours\0\
         FontVTMode\4\
         FontQuality\3\
         FontUnicodeAdjustment\0\
         FontUnicodeHeight\16\
         FontUnicodeCharSet\134\
         FontUnicodeIsBold\0\
         FontUnicode\Terminal\
         UseFontUnicode\1\
         FontHeight\16\
         FontCharSet\134\
         FontIsBold\0\
         Font\Terminal\
         TermHeight\24\
         TermWidth\114\
         WinTitle\cpl@cpl:%20~\
         WinNameAlways\1\
         DisableBidi\0\
         DisableArabicShaping\0\
         CRImpliesLF\0\
         LFImpliesCR\0\
         AutoWrapMode\1\
         DECOriginMode\0\
         ScrollbackLines\2000\
         BellOverloadS\5000\
         BellOverloadT\2000\
         BellOverloadN\5\
         BellOverload\1\
         BellWaveFile\\
         BeepInd\0\
         Beep\1\
         BlinkCur\0\
         CurType\0\
         WindowBorder\1\
         SunkenEdge\0\
         HideMousePtr\0\
         FullScreenOnAltEnter\0\
         AlwaysOnTop\0\
         Answerback\PuTTY\
         LocalEdit\2\
         LocalEcho\2\
         TelnetRet\1\
         TelnetKey\0\
         CtrlAltKeys\1\
         ComposeKey\0\
         AltOnly\0\
         AltSpace\0\
         AltF4\1\
         WindowIcon\\
         Transparency\250\
         FailureReconnect\0\
         WakeupReconnect\0\
         TrayRestore\0\
         StartTray\0\
         Tray\0\
         StorageType\1\
         NetHackKeypad\0\
         ApplicationKeypad\0\
         ApplicationCursorKeys\0\
         NoRemoteCharset\0\
         NoDBackspace\0\
         RemoteQTitleAction\1\
         NoRemoteClearScroll\0\
         NoRemoteWinTitle\0\
         NoAltScreen\0\
         NoRemoteResize\0\
         NoMouseReporting\0\
         NoApplicationCursors\0\
         NoApplicationKeys\0\
         LinuxFunctionKeys\0\
         RXVTHomeEnd\0\
         BackspaceIsDelete\1\
         PassiveTelnet\0\
         RFCEnviron\0\
         RemoteCommand\\
         PublicKeyFile\\
         SSH2DES\0\
         LogHost\\
         SshProt\3\
         SshNoShell\0\
         GSSCustom\\
         GSSLibs\gssapi32,sspi,custom\
         AuthGSSAPI\1\
         AuthKI\1\
         AuthTIS\0\
         SshBanner\1\
         SshNoAuth\0\
         RekeyBytes\1G\
         RekeyTime\60\
         HostKey\ed25519,ecdsa,rsa,dsa,WARN\
         KEX\ecdh,dh-gex-sha1,dh-group14-sha1,rsa,WARN,dh-group1-sha1\
         Cipher\aes,chacha20,blowfish,3des,WARN,arcfour,des\
         ChangeUsername\0\
         GssapiFwd\0\
         AgentFwd\0\
         TryAgent\1\
         Compression\0\
         NoPTY\0\
         LocalUserName\\
         Auth2FactorString\\
         UserPassword\\
         UserNameFromEnvironment\0\
         UserName\\
         Environment\\
         ProxyLogToTerm\1\
         ProxyTelnetCommand\connect%20%25host%20%25port%5Cn\
         ProxyPassword\\
         ProxyUsername\\
         ProxyPort\80\
         ProxyHost\proxy\
         ProxyMethod\0\
         ProxyLocalhost\0\
         ProxyDNS\1\
         ProxyExcludeList\\
         AddressFamily\0\
         TerminalModes\CS7=A,CS8=A,DISCARD=A,DSUSP=A,ECHO=A,ECHOCTL=A,ECHOE=A,ECHOK=A,ECHOKE=A,ECHONL=A,EOF=A,EOL=A,EOL2=A,ERASE=A,FLUSH=A,ICANON=A,ICRNL=A,IEXTEN=A,IGNCR=A,IGNPAR=A,IMAXBEL=A,INLCR=A,INPCK=A,INTR=A,ISIG=A,ISTRIP=A,IUCLC=A,IUTF8=A,IXANY=A,IXOFF=A,IXON=A,KILL=A,LNEXT=A,NOFLSH=A,OCRNL=A,OLCUC=A,ONLCR=A,ONLRET=A,ONOCR=A,OPOST=A,PARENB=A,PARMRK=A,PARODD=A,PENDIN=A,QUIT=A,REPRINT=A,START=A,STATUS=A,STOP=A,SUSP=A,SWTCH=A,TOSTOP=A,WERASE=A,XCASE=A\
         TerminalSpeed\38400,38400\
         TerminalType\xterm\
         TCPKeepalives\0\
         TCPNoDelay\1\
         PingIntervalSecs\0\
         PingInterval\0\
         WarnOnClose\1\
         CloseOnExit\1\
         PortNumber\22\
         Protocol\ssh\
         SSHLogOmitData\0\
         SSHLogOmitPasswords\1\
         LogFlush\1\
         LogFileClash\-1\
         LogType\0\
         LogFileName\putty.log\
         HostName\10.67.210.17\
         Present\1\
         #+END_QUOTE
     - linux
       在无网络/cpl，虚拟机，可通过从外部拷贝（光盘等）的情况也可以成功配置
       保证hyper-v和virtualbox都可以运行，只能选vhd
       vhd1: sys debian-cd, minimal(no-xfce) 大小2g或4g（留个备份，不同工具链不同，更希望是编译链这种安装在work内，这样只需要1g，而且不用更换）
       https://cdimage.debian.org/debian-cd/10.4.0/amd64/iso-cd/debian-10.4.0-amd64-xfce-CD-1.iso
       注意的点
       - 用二代虚机安装
       - locale默认en_US.UTF-8，否则会出现棱形乱码（用ssh链接的情况下zh_CN.UTF-8能正常显示中文）
       - 安装时linux不要force efi，grub一定要force efi
       - 安装完删除虚机（保留vhd），然后用一代虚机或virtualbox等进行后续操作（二代虚拟机起不来的）
       - CPU至少2核，否则编译很吃力；另外不要选动态内存，固定2g内存，动态内存容易出现因内存泄漏导致host卡死的问题
       vhd2: soft opt
       vhdx: xfer (因为没法apt，所以装包不方便，samba/nfs的模式行不通），作用类似usb，host挂载拷贝后卸载，虚拟机再挂载；要创建scsi驱动器，ide驱动器不支持热插拔
       开启ssh后通过ssh调试即可
       - 坑
         - busybox没有链接命令至/bin/下
           busybox --install
         - 用bash替换dash（source等命令无效）
           https://blog.csdn.net/mvpme82/article/details/7615454
         - 静态ip（/etc/network/interfaces）
           https://blog.csdn.net/guigui_oy/article/details/80913526
           新建虚拟网卡eth0（不能用default-switch，其无法设置成静态ip），设置成静态ip，虚机设成同一网段即可
           host ping虚机可以，虚机ping host不行（防火墙）
           https://blog.csdn.net/hskw444273663/article/details/81301470
           若需要动态ip（建议一张静态一张动态）：https://www.cnblogs.com/hustdc/p/9749012.html
         - 缺工具
           用debian-dvd的3张dvd，然后apt-cdrom更新apt list
           - build-essential(make gcc) g++ autoconf pkgconfig cmake（meson/gn/ninja通过移植方式）
           - samba nfs
           - screen mg（micro-emacs，哪怕emacs-nox也要400mb的空间）
           - lxc
           - tree ntpupdate ...
         - ssh
           /etc/init.d/ssh restart
           可连外网的情况：一块网卡内部，固定ip，一块外部，用于apt
         - samba
           host要开启samba服务，配置map to guest不要设成bad usr，设成never即可
           https://www.jianshu.com/p/be7dc5875923
           不同用户不同目录（%S）：https://blog.csdn.net/JerryGou/article/details/80946858
         - /opt单独磁盘
           lvm相关操作（pv--vg--lv三层，具体百度即可）
           swap分区：https://www.cnblogs.com/gumutianqi/articles/2079635.html
         - 交叉编译工具链安装后显示 no such file or directory
           64位的虚机，工具链需要32位的库（lib32z1 lib32stdc++6 )
           安装完后需要 source /etc/profile 才能让PATH变化生效
           http://www.itskill.cn/ArticleDetail/post-275
         - 中文棱形（乱码），不需要解决（用ssh链接即可）
           没有中文字库（需先配置apt源）：apt-get install fonts-noto-cjk
           更换源：https://www.cnblogs.com/acmexyz/p/12868760.html
           配置apt出现问题：https://blog.csdn.net/zhemejinnameyuanxc/article/details/100030629
           如果字库安装不成功，设置locale切回en_US.UTF-8：https://www.cnblogs.com/linxiong945/p/4200097.html
       - 工作环境刻盘（virtual-box）
         工具链/不常用的工具都可以通过/etc/profile的模式（模仿hisi的工具链）来添加，而不是直接放在/usr/bin下
         vb网络配置（用host-only模式）：https://www.myway5.com/index.php/2020/04/21/virtualbox-%E7%9A%84%E5%87%A0%E7%A7%8D%E7%BD%91%E7%BB%9C%E6%A8%A1%E5%BC%8F/
         工作环境兼容性（win7 64遇到的问题）：字体ttf，vb6.0.x（同时注意6.0.x会跟hyper-v冲突，但6.1.x不会），superputty没有.net
*** 我的选择
    cmake/meson/gn都使用了一段时间，最后还是定下以meson为主（其实仍在gn和meson间纠结）
    qt关于buildtools的选择（替换qbs）：https://www.phoronix.com/forums/forum/software/desktop-linux/1037141-what-build-system-should-qt-6-use
    #+BEGIN_QUOTE
    CMake is the safe pick. It is likely the best pick today. If you aren't going to put time into the build system (which is a fair decision), then choose CMake. 
    Meson doesn't fit the requirements. Try to build a current meson project (a real one, not hello world) with a meson build from two years ago and watch it burn.
    GN is unusable for projects that aren't Chromium (and AFAIK that's intentional).
    #+END_QUOTE
    18年的文章，看看就好，meson进步挺大，但上面的说法也很有道理，最终还得看个人喜爱
    - why not gn
      太多功能是为了google自己设计（goma/abseil/...），compile-check（这个可能是没找到），资料少，不是真正的多语言支持；当然优点也很多，设计之初就是为跟ninja配套（名字就是generate ninja），抽象的非常nice（多平台），成果物单文件（类似ninja）
      gn除了chromium，同时也是llvm期望使用的buildtool: https://github.com/llvm/llvm-project/tree/master/llvm/utils/gn
      关于compile-check，gn是通过exec_script运行python脚本实现，没有meson方便（毕竟本身就是python程序）
      #+BEGIN_QUOTE
      compiler checks. In GN, these could use exec_script to identify the host compiler at GN time.
      #+END_QUOTE
    - why not modern cmake
      compile-check，没有多语言支持（名字就是c/c++ makefile）；相对其他两个的优点就一个，资料很多（因为用的人多）

   fini
```


