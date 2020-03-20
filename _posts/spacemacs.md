
** YOOO
Lisp Machine：http://cnlox.is-programmer.com/posts/34114.html
emacs vs vi：https://blog.csdn.net/oyd/article/details/1511128
http://blog.jobbole.com/86571/
https://www.cnblogs.com/jiqingwu/p/4043512.html

** 代替notepad
1) 安装
  - 26.1之前：https://chriszheng.science/emacs-w64/
  - 26.1以后：https://www.gnu.org/software/emacs/
2) win特有的问题
  https://chriszheng.science/2018/07/12/Why-is-Emacs-so-slow-on-Windows
  http://caiorss.github.io/Emacs-Elisp-Programming/Emacs_On_Windows.html
  https://www.gnu.org/software/emacs/manual/html_mono/efaq-w32.html
  - docker（gui环境难搞，弃用）
    #+BEGIN_QUOTE
    I wonder how much it has to do with the following code and links about docker, spacemacs, x11, and xpra:
    https://github.com/syl20bnr/spacemacs/tree/develop/layers/%2Bdistributions/spacemacs-docker
    The following github project led me to that spacemacs layer:
    https://github.com/JAremko/spacemacs https://github.com/JAremko/docker-emacs
    #+END_QUOTE
    http://lujun9972.github.io/blog/2018/04/23/%E4%BD%BF%E7%94%A8docker%E8%BD%BB%E6%9D%BE%E4%BD%93%E9%AA%8C%E5%A4%9A%E4%B8%AA%E7%89%88%E6%9C%ACemacs/
https://emacs-china.org/t/topic/2754/74
    docker下载：https://blog.csdn.net/enweitech/article/details/52083280
    虚拟机无缝切换（虚拟机linux做服务器，win开机后虚拟机后台自启动）
    https://www.reddit.com/r/emacs/comments/7ne631/whats_the_best_way_to_work_with_emacs_on_remote/
    https://stackoverflow.com/questions/12546722/using-emacs-server-and-emacsclient-on-other-machines-as-other-users
    https://segmentfault.com/a/1190000006248174
    https://emacs-china.org/t/windows-emacs-hyper-v/3235
    https://emacs-china.org/t/emacs-terminal/3140/14
    https://emacs-china.org/t/topic/3276/16
3) 基本操作
  1) CUA模式（Common User Access，将emacs变成可以深度定制的notepad/notepad++）
    https://www.emacswiki.org/emacs/CuaMode
    https://en.wikipedia.org/wiki/IBM_Common_User_Access
    - vi也有类似的（所以说通过捆绑销售强买强卖强迫用户使用别扭的键位这种事情真是...）
      https://www.cnblogs.com/wucg/archive/2011/11/24/2261435.html
  2) 常用快捷键（看新手教程）
    跟CUA模式的快捷键对应
    #+BEGIN_QUOTE
    关闭 = C-x C-c
    复制 = M-w
    剪切 = C-w
    粘贴 = C-y
    #+END_QUOTE
  3) 列模式（编辑多行）
    https://emacs-china.org/t/topic/5741/7
    编辑矩形：https://www.cnblogs.com/ts65214/p/5561646.html
    （C-@设置起点，方向键选择操作范围，C-x r t在每列前插入）
    https://blog.csdn.net/bigmarco/article/details/6727676
    https://www.cnblogs.com/bamanzi/archive/2011/02/28/emacs-cua-rectangle-cool.html
  4) 画图
    http://ergoemacs.org/emacs/emacs_ascii_diagram.html
    https://blog.csdn.net/csfreebird/article/details/40016753
4) 替代notepad
  - 环境变量
    将emacs安装目录添加到环境变量中（貌似可以直接运行bin目录下某个程序，但是没试过）
  - 注册表
    采用Sam Hasler的方法：https://stackoverflow.com/questions/2984846/set-image-file-execution-options-will-always-open-the-named-exe-file-as-defaul
    不用此方法：https://www.emacswiki.org/emacs/EmacsMsWindowsIntegration
  - 快速启动（server-client模式）
    https://scripter.co/emacsclient-on-windows/（采用spcaemacs后不需要考虑服务器问题）
5) theme&lite（放弃，采用spacemacs+自己配置）
  https://www.cnblogs.com/bamanzi/archive/2011/03/07/intro-microemacs.html
  https://www.reddit.com/r/emacs/comments/19ggi5/questions_making_emacs_ui_attractive_lighttable/
  https://blog.csdn.net/u013225150/article/details/51577271
6) 其他资料
  emacs-tw：https://github.com/emacs-tw/emacs-101-beginner-survival-guide
  https://github.com/redguardtoo/mastering-emacs-in-one-year-guide
  https://www.jianshu.com/p/b4cf683c25f3
  https://www.cnblogs.com/tylinux/p/3691909.html
  http://docs.huihoo.com/homepage/shredderyin/emacs.html
  https://www.cs.cmu.edu/~07131/f18/topics/extratations/spacemacs.pdf
  https://blog.csdn.net/mengtianwxs/article/details/53129292
  https://blog.csdn.net/pandora_madara/article/details/45013927
  emacs插件整理：https://www.jianshu.com/p/99c3f6fa6355
  https://www.zhihu.com/question/21943533
  eshell：https://emacs-china.org/t/topic/5362/17
  emacs使用札记：https://legacy.gitbook.com/book/yfwu/emacs-manual
  加速启动（Multi-tty Emacs）：http://blog.chinaunix.net/uid-1877180-id-303301.html
  #+BEGIN_QUOTE
  Emacs Org-mode - a system for note-taking and project planning.rar (91.77 MB)
  xah_emacs_tutorial_2017-02-18_936bddb1.zip (42.73 MB)
  The Org Manual.pdf (1.19 MB)
  The compact Org-mode Guide.pdf (335.65 KB)
  Specialized Emacs Features.pdf (376.38 KB)
  GNU Emacs Manual.pdf (2.52 MB)
  GNU Emacs Lisp Reference Manual.pdf (3.87 MB)
  An Introduction to Programming in Emacs Lisp.pdf (1017.75 KB)
  #+END_QUOTE
  emacs-like / emacs implementations
  https://wiki.gentoo.org/wiki/Project:Emacs/Emacs-like_editors
  https://www.emacswiki.org/emacs/EmacsImplementations
  kawa（jemacs，仅kawa-1.8的成果物可以直接用jemacs，其他版本需要自己编译）：https://www.gnu.org/software/kawa/index.html
  java11不带jre：https://blog.csdn.net/qq_42017152/article/details/90717985
  uemacs：https://github.com/torvalds/uemacs

** 代替editplus
1) 安装
  https://github.com/syl20bnr/spacemacs
  - package等问题
    https://blog.csdn.net/u011729865/article/details/54178388
    https://emacs-china.org/t/topic/2662/14
    https://blog.csdn.net/csfreebird/article/details/52744771
    https://blog.csdn.net/grey_csdn/article/details/79477210
2) dotspacemacs设置问题（emacs模式，mini安装，ivy）：
  dotspacemacs/layers里的内容，dotspacemacs-maximized-at-startup只能直接修改，在dotspacemacs/user-init中修改无效
  yas error（在报error的地方建立目录，然后无视这个error即可）：https://github.com/syl20bnr/spacemacs/issues/10316
  dotspacemacs-excluded-packages ，dotspacemacs-persistent-server
  修改后快速调试（C-x C-e，SPC=M-m，SPC f e R）
3) 中文问题
  https://github.com/hick/emacs-chinese
  - flycheck设置不检测中文（放弃此layer，在中文环境容易造成卡顿）
    需要aspell等，apsell安装完后将bin目录添加到环境变量，并在dotspacemacs-configuration-layers里添加代码
    #+BEGIN_SRC lisp
    (spell-checking :variables
                            ispell-program-name "aspell"
                            ispell-dictionary "american"
                            spell-checking-enable-auto-dictionary t
                            enable-flyspell-auto-completion t
                            spell-checking-enable-by-default nil)
    #+END_SRC
    https://emacs-china.org/t/how-to-disable-flyspell-for-zh-cn/2257/4
    https://superuser.com/questions/193432/ispell-for-windows-7-for-flyspell-mode/193546
    https://stackoverflow.com/questions/3805647/enabling-flyspell-mode-on-emacs-w32
    https://lujun9972.github.io/blog/2018/06/03/%E4%BD%BF%E7%94%A8emacs%E6%94%B9%E8%BF%9B%E4%BD%A0%E7%9A%84%E8%8B%B1%E6%96%87%E5%86%99%E4%BD%9C/
  - chinese layer没必要装
    - M-x describe-char时会遇到类似\303的问题（charset不对）
    - pangu-spacing 会造成中英文混编时英文显示成2个单位
      https://emacs-china.org/t/topic/440/23
      https://emacs-china.org/t/org/1930/9
  - 中文字体设置：cnfonts
    - M-x customize设置cofonts的personal字体，再设置profile
    - 中文无法缩放：用cnfonts的字体缩放而不是emacs自带的缩放即可
    - markdown等模式可以单独设置字体（M-x customize） 
    - 不想用cnfont可以参照这篇设置（将字体设置放在最后，之设置字体不设置大小，这样可以用内置的缩放，但前提是等宽字体）
      https://emacs-china.org/t/daemon-gui-client/2338
      http://baohaojun.github.io/perfect-emacs-chinese-font.html
    - Sarasa-Gothic用cnfonts对不齐
      http://mpwang.github.io/2019/02/06/productivity/
      https://github.com/cjkvi/HanaMinAFDKO
  - 字体推荐
    https://github.com/be5invis/Sarasa-Gothic
    https://blog.typekit.com/alternate/source-han-sans-cht/（难看+没必要显示韩文之类的）
    https://design.ubuntu.com/font/（ubuntu字体，很nice，但中文需要单独配）
  - 合并字体（建议ubuntu+source-han，失败，放弃）
    https://github.com/dfrankland/font-family-merger
    https://fontforge.github.io/
  - win环境中文复制（需要用utf-16-le）
    https://www.zhihu.com/question/35148860
  - fonset对应不同charset
    显示乱码或不显示字符而显示字符码（解决方案：改变字符编码）：https://blog.csdn.net/lidonghat/article/details/52891343
  - 其他资料
    http://blog.sina.com.cn/s/blog_b752b6c00102xx6t.html
    https://blog.lennox67.me/posts/en-hans-monospace-issuse.html
    https://emacs-china.org/t/spacemacs/2509/10
    https://emacs-china.org/t/topic/5306/10
    https://blog.csdn.net/abcamus/article/details/52848459
    http://blog.51cto.com/laokaddk/686817
    http://blog.sina.com.cn/s/blog_1313062e90102w0o7.html
    http://ergoemacs.org/emacs/emacs_list_and_set_font.html
    https://chriszheng.science/2015/04/26/Emacs-font-settings/
    https://emacser.com/torture-emacs.htm
4) 常用命令
  - 替换：C-x-h全选（或者列模式选中一部分），C-s调出swiper，输入被替换的，M-q，输入替换之后的，！替换全部，y替换一处（具体可以输入?来看help）
5) 小说目录生成（暂缺）
  正则搜索：
  [第]*[0123456789零一二三四五六七八九十百千]+[卷章节集][  ]+.*

** 代替vscode
01) 需求
  #+BEGIN_QUOTE
  多窗口界面 (ecb + tabbar)
  project管理
  语法高亮, 自动缩进等编辑功能
  语法提示 (**semantic**)
  代码跳转 (**semantic**)
  自动补全
  自动代码生成
  查看帮助和显示doc-string
  代码质量控制（Static Analyzers等）
  调试--不需要
  解释器--不需要
  #+END_QUOTE
  https://my.oschina.net/alphajay/blog/152599
  http://www.360doc.com/content/12/0531/10/5962017_214925829.shtml
  https://www.zhihu.com/question/276356727
  其他还需要：neotree，文件切换，markdown预览，org mode，project使用等
  ```
  A text editor cannot 'understand' your code, but an ide can. Thus, make a text editor to an ide need external tools.
  ```
02) 语法高亮
  http://tuhdo.github.io/c-ide.html
  https://blog.csdn.net/sealingdust/article/details/3982192
  https://writequit.org/denver-emacs/
  http://ju.outofmemory.cn/entry/149090
  注意：c-c++ layer不需要
03) 代码格式化
  astyle or 自带
  https://blog.csdn.net/github_37986351/article/details/77822254
04) project管理，多窗口界面
  - 显示行号
    https://chriszheng.science/2017/07/27/Emacs-now-supports-native-line-number/
    字体导致行号不全：https://unix.stackexchange.com/questions/29786/font-size-issues-with-emacs-in-linum-mode/146781
  - neotree（弃用）
    先在layer里添加spacemacs-ui-visual
    https://blog.csdn.net/grey_csdn/article/details/79736940
    https://blog.csdn.net/sjhuangx/article/details/51288689
  - emacs内置的speedbar（sr speedbar package）+semantic layer
    code outline：https://segmentfault.com/a/1190000000508954
05) 自动补全，自动生成
  - 补全插件（后端、前段等概念）：http://tieba.baidu.com/p/3360335690
    snails（代码补全）架构设计：https://manateelazycat.github.io/emacs/2019/07/23/snails-framework.html
    company, ivy, helm：https://www.reddit.com/r/emacs/comments/6x7ph2/is_company_different_from_helm_and_ivy/
    They do different things. Company-mode is a package for in-buffer code completion, while Helm/Ivy are general narrowing-completion frameworks. Essentially, any time that you have to make a choice from a list of candidates, Helm/Ivy will turn that action into a narrowing completion list. This can be things like M-x, find-file or switch-buffer.
    https://blog.csdn.net/u011729865/article/details/75725595
    1. framework-ivy（ivy layer）
    2. code completion-company（auto-completion layer）
    3. snippet
  - 注释生成
    1) SRecode（不建议）：
      https://blog.csdn.net/lujun9972/article/details/46002951
    2) doxymacs（用这个）
      https://www.cnblogs.com/cudafans/archive/2012/11/05/2754476.html
      http://tigersoldier.is-programmer.com/posts/2243.html
06) 代码跳转（tags，code outline）
  #+BEGIN_QUOTE
  1) 生成tags（一般的名字是某tag，或者semantic），补全也是要先生成tag
    etag：https://www.cnblogs.com/siyuan/archive/2011/04/28/2031847.html
  2) 堆栈等记录位置实现跳转和跳回
  3) 可以在此基础上生成code outline
  #+END_QUOTE
  1) 跳转到实现
    semantic
    https://blog.csdn.net/csfreebird/article/details/71036826
    https://emacser.com/cedet.htm/comment-page-2
  2) 文件间（工程内）跳转，工程内搜索
    win下搜索不支持grep（finstr效果不好，github上的el文件未测试）：https://emacs.stackexchange.com/questions/30196/how-to-search-in-multiple-files-with-emacs-without-grep
    可以参考此人的配置，他把几个linux下的bin文件直接拿出来了（based on CEDET）：https://github.com/meteor1113/dotemacs
  3) 跳回
    semantic：https://www.cnblogs.com/yangyingchao/archive/2011/09/04/2178379.html
07) codemap
  cogre in CEDET
  https://emacser.com/built-in-cedet.htm
08) 窗口管理（待整理）
  layout：https://blog.csdn.net/u011729865/article/details/71076054
  https://blog.csdn.net/csfreebird/article/details/71194235
  https://blog.csdn.net/jameschen1988/article/details/7541013
  https://blog.csdn.net/mscf/article/details/52471047
  http://blog.chinaunix.net/uid-24704319-id-2594442.html
09) ecb/cedet
  https://stackoverflow.com/questions/9877180/emacs-ecb-alternative
  https://www.emacswiki.org/emacs/PracticalECB
  http://jixiuf.github.io/blog/cedet-%E7%9A%84%E5%AE%9A%E5%88%B6%EF%BC%8C%EF%BC%88%E8%AF%91%E6%96%87%EF%BC%89/
  http://blog.chinaunix.net/uid-24948934-id-59822.html
  https://emacs.stackexchange.com/questions/474/using-emacs-as-a-full-featured-c-c-ide/481
  https://blog.csdn.net/foreverkongxincai/article/details/9027823
  ecb-activate即可开启ecb模式
  跳转：https://stackoverflow.com/questions/18230838/semantic-cedet-how-to-force-parsing-of-source-files
  etags（dir /b /s *.c *.h | etags -）：https://www.emacswiki.org/emacs/RecursiveTags
  https://emacs.stackexchange.com/questions/3360/emacs-tags-file-in-windows
  https://blog.csdn.net/way88liu/article/details/38224353
  etags生成tag后即可跳转
  https://stackoverflow.com/questions/12922526/tags-for-emacs-relationship-between-etags-ebrowse-cscope-gnu-global-and-exub
  tabNine：https://manateelazycat.github.io/emacs/2019/07/17/tabnine.html
10) my layers：layer单独写（自己的layer）
  - coding layer
  - theme layer(font, charset, theme, etc)
  https://www.jianshu.com/p/bdd64fecddce
  https://github.com/appleshan/my-spacemacs-config
11) 其他资料
  emacs-ide
  http://syamajala.github.io/c-ide.html
  https://eide.frama.io
  https://segmentfault.com/q/1010000005718869
  https://blog.csdn.net/elloop/article/category/6108198
  https://emacs-china.org/t/topic/5150/82

** GTD功能
1) org-mode
  https://blog.csdn.net/nbtsz/article/details/44623287
  https://www.cnblogs.com/holbrook/archive/2012/04/14/2447754.html（https://holbrook.github.io/）
  https://www.cnblogs.com/bamanzi/p/org-mode-tips.html
  org管理配置：https://github.com/vapniks/org-dotemacs
  https://emacs-china.org/t/topic/8692/113
  生成pdf：https://blog.csdn.net/u011729865/article/details/72892235
  https://www.cnblogs.com/visayafan/archive/2012/06/16/2552023.html
  latex中文：https://www.jianshu.com/p/0cba74d6ad45
  https://wenku.baidu.com/view/dabb3bc15fbfc77da269b159.html
  下划线等：https://blog.csdn.net/csfreebird/article/details/43580679
  长行：emacs默认会用多行来显示非常长的行，但是org mode 默认会打开 truncate-lines 模式。(setq truncate-lines nil)。
  引用换行：https://emacs.stackexchange.com/questions/21556/org-mode-export-how-to-force-newline-on-lines-between-paragraphs
  latex头（#+OPTIONS: \n:t 会遇到问题）：
  #+BEGIN_SRC org
  #+TITLE: Emacs
  
  #+OPTIONS: ^:nil
  #+OPTIONS: \n:t
  
  #+LATEX_HEADER: \usepackage{xeCJK}
  #+LATEX_HEADER: \setmainfont{Ubuntu Mono}
  #+LATEX_HEADER: \setsansfont{Ubuntu Mono}
  #+LATEX_HEADER: \setmonofont{Ubuntu Mono}
  #+LATEX_HEADER: \setCJKmainfont{Source Han Sans HW}
  #+LATEX_HEADER: \setCJKsansfont{Source Han Sans HW}
  #+LATEX_HEADER: \setCJKmonofont{Source Han Sans HW}
  #+END_SRC
  https://www.kohn.com.cn/wordpress/?p=78
  https://blog.poi.cat/post/spacemacs-plus-org-mode-plus-latex
  https://blog.csdn.net/u014801157/article/details/24372485
  https://blog.csdn.net/fanfan4569/article/details/79140470
  画图：https://blog.csdn.net/u011729865/article/details/79059391
  https://www.cnblogs.com/chenfanyu/archive/2013/01/27/2878845.html
  显示图片：https://stackoverflow.com/questions/17621495/emacs-org-display-inline-images
  https://www.reddit.com/r/emacs/comments/55zk2d/adjust_the_size_of_pictures_to_be_shown_inside/
2) GTD
  gtd：https://www.lijigang.com/blog/从零开始配置spacemacs/
  https://blog.csdn.net/qq_28797197/article/details/78940210
  http://www.fuzihao.org/blog/2015/02/19/org-mode教程/
  http://members.optusnet.com.au/~charles57/GTD/orgmode.html
  https://blog.15cm.net/2016/12/10/emacs-notes-system/
  https://www.cnblogs.com/yangruiGB2312/p/9101838.html
  https://blog.aaronbieber.com/2016/09/24/an-agenda-for-life-with-org-mode.html
  https://lengyueyang.github.io/Life/use-org-mode-to-GTD.html
  https://www.tuicool.com/articles/vMZVZj
  https://www.ideaio.ch/posts/my-gtd-system-with-org-mode.html
  https://blog.csdn.net/u011729865/article/details/54236547
3) markdown
  https://emacs-china.org/t/topic/1549
4) 联动github repository
  - 存储设置和笔记（加密）
  - gitpage（github.io）
5) 其他资料
   知识消化系统：https://emacs-china.org/t/v1/8218/53
   文学编程：https://emacs-china.org/t/topic/5355/31

** OTHER
*** 其他emacs版本
    centaur emacs：https://emacs-china.org/t/topic/3802/50
    Vincent Goulet的优化版本：http://vgoulet.act.ulaval.ca/en/home/
    ESS：http://ess.r-project.org/index.php
    https://blog.csdn.net/u014801157/article/category/2215799
    http://esnm.sourceforge.net/EmacsPortable.html
*** 工作环境
   这篇文章完成的时候（2018）刚好是spacemacs最火的时候，但是由于某些原因，一直未推出新的release版本
   spacemacs现在已经变成滚动更新，不再发布release版本了，个人已经改用 https://github.com/meteor1113/dotemacs 的版本（对windows用户比较友好）
   #+BEGIN_QUOTE
   tortoisesvn1.7（要跟服务器大版本一致）：https://osdn.net/projects/tortoisesvn/storage/Archive/1.7.15/
   iputty+superputty
   tftpw64：http://tftpd32.jounin.net/tftpd32_download.html
   vscodium：https://github.com/portapps/vscodium-portable
   emacs（ess+meteorliu的配置）：https://github.com/meteor1113/dotemacs
   关闭自动截断：toggle-text-mode-auto-fill
   font（iosveka+sarasa或ubuntu+youyuan）：http://mpwang.github.io/2019/02/06/productivity/
   .emacs如下：
   ``` elisp
   (add-to-list 'load-path "~/dotemacs")
   (load "init-emacs" 'noerror)
   (load "init-note" 'noerror)
   (load "etc/sample/proj" t)

   (custom-set-variables
   ;; custom-set-variables was added by Custom.
   ;; If you edit it by hand, you could mess it up, so be careful.
   ;; Your init file should contain only one such instance.
   ;; If there is more than one, they won't work right.
   '(cnfonts-personal-fontnames (quote (("Iosevka SS09") ("Sarasa Mono CL") nil)))
   '(cnfonts-profiles (quote ("sarasa" "ubuntu")))
   '(column-number-mode t)
   '(cua-mode nil)
   '(ecb-source-path nil t)
   '(global-display-line-numbers-mode t)
   '(show-paren-mode t)
   '(size-indication-mode t)
   '(tool-bar-mode nil))
   ```
   #+END_QUOTE
   





















