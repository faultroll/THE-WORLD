appweb+php7（依赖: zlib+libxml2+openssl+sqlite3(tclsh)+iconv）
（其实webserver应该叫httpserver，对应rtspserver/rtmpclient，都是某个协议server/client）
https://blog.csdn.net/ysj265/article/details/7251080
https://github.com/faultroll/appweb-7.1.0
- mpr-version/tick_run/ar等问题
修改project下对应的.mk文件
- __aarch64__问题
头文件内判断宏用错，应该时__aarch64__，而不是__arch64__
- libmod_php.so崩溃
phpHandle7.c初始化正确，正确的初始化步骤参考https://github.com/php/php-src/blob/master/sapi/下的几个工程
- 整合最新appweb7相关的修改
https://github.com/embedthis/appweb-gpl
模仿debian的形式，通过生成.patch文件的方式来修bug
- listen的backlog不能为0
- xmlrpc-c的format要有括号
- mpr_alloc_trace宏应为me_mpr_...
rpc
https://blog.csdn.net/feibuhui123/article/details/10251465
http://www.imooc.com/article/10371
https://bbs.csdn.net/topics/390268641
https://blog.csdn.net/shallnet
https://blog.csdn.net/a600423444/article/details/8835801
https://segmentfault.com/q/1010000004046215
https://blog.csdn.net/u014530704/category_6989350.html
https://www.zhihu.com/question/20028868
https://www.hackster.io/sanyaade1/php-embedded-php4mcu-2bf22c
https://cloud.tencent.com/developer/article/1335999
https://stackoverflow.com/questions/33401895/how-to-set-up-communication-between-php-and-c
https://stackoverflow.com/questions/1746207/how-to-ipc-between-php-clients-and-a-c-daemon-server
rpc: https://www.jianshu.com/p/7d6853140e13
rpc: http://blog.sina.com.cn/s/blog_e59371cc0102v81w.html
restful: https://www.zhihu.com/question/28557115/answer/47846156
jquery界面
下拉框：https://www.cnblogs.com/ychunblog/p/10268177.html
https://www.jianshu.com/p/46c90ff507c4
jquery&vue/react/angular: https://www.zhihu.com/question/317271206
https://www.cnblogs.com/zhuzhenwei918/p/7447434.html
https://blog.csdn.net/tsinbo1314/article/details/80367452
原生js: https://www.jianshu.com/p/117d0d4497f9
js和webapi: https://stackoverflow.com/questions/43017576/what-is-the-difference-between-fetch-and-jquery-ajax
https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction
```
Client-side JavaScript, in particular, has many APIs available to it — these are not part of the JavaScript language itself, rather they are built on top of the core JavaScript language, providing you with extra superpowers to use in your JavaScript code.
```
```
fetch (unlike Promise) is not a javascript language feature but a web api feature.
```
https://segmentfault.com/q/1010000006813975/a-1020000006814400
fetch: https://stackoverflow.com/questions/59463417/how-to-replace-fetch-with-ajax-method-in-javascript
https://stackoverflow.com/questions/62943493/send-multiple-files-to-php-with-jquery-and-fetch
https://segmentfault.com/q/1010000004254003
typescript: https://www.runoob.com/typescript/ts-tutorial.html
vanilla/native js: https://stackoverflow.com/questions/7022007/what-is-native-javascript
https://stackoverflow.com/questions/20435653/what-is-vanillajs
https://github.com/bradtraversy/vanillawebprojects
https://github.com/snipcart/learn-vanilla-js
https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore
es/js/ts: https://zhuanlan.zhihu.com/p/69171948
https://www.zhihu.com/question/361303428
jquery document: https://github.com/jquery/api.jquery.com
js数据绑定：https://zhaomenghuan.js.org/blog/javascript-mvvm-vue.html
https://stackoverflow.com/questions/16483560/how-to-implement-dom-data-binding-in-javascript
json2h5table: https://stackoverflow.com/questions/5180382/convert-json-data-to-a-html-table
https://stackoverflow.com/questions/53877282/display-json-on-an-html-table-using-vanilla-js
https://stackoverflow.com/questions/1391278/contenteditable-change-events
同一文件响应不同的请求（反射）：https://www.cnblogs.com/benz/archive/2009/05/17/1458559.html
http://yanue.net/post-107.html
（$_SERVER）https://www.php.net/language.variables.superglobals
https://stackoverflow.com/questions/10999293/is-serverrequest-method-still-viable
ajax post PHP后台接收不到数据：https://blog.csdn.net/qq_41293288/article/details/107211298
https://stackoverflow.com/questions/10955017/sending-json-to-php-using-ajax
https://www.cnblogs.com/evaling/p/7607603.html
https://stackoverflow.com/questions/33439030/how-to-grab-data-using-fetch-api-post-method-in-php
vanillajs helper: https://github.com/DimiMikadze/vanilla-helpers

php json_decode object: https://blog.csdn.net/panglongyouhui/article/details/45114499
http://www.nowamagic.net/academy/
restful php: https://www.runoob.com/php/php-ajax-php.html
http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html

mvvm&mvc&mvp in c: https://github.com/zlgopen/awtk-patterns
```
mvc --> mvp : view和model解耦（各自给出回调，presenter来粘连）
mvp --> mvvm : presenter写的太累，用自然语言（开发新语言）代替（类似makefile --> cmake的性质， mvvm框架干的就是这事，本质上是字符串处理（一般用通用的json/xml语法））
```
https://github.com/NicolasVanBossuyt/shop.c
http://www.yhthu.com/2016/09/22/201609221/

https://github.com/nemoTyrant/manong
https://blog.csdn.net/siren0203/article/details/8085813

os和browser的思考
```
1.
client(os) -- server
web(browser) --http-- server(处理http协议)
一次http访问（http(s)://...）
    - 请求前台web（html/js/css/...）
        httpserver（移植）根据请求及对应规则（开发）进行处理（传输/转发/...）
    - web加载（开发）
        - html（document）
        - css（渲染）
        - js（后台交互）
    - 后台交互（开发）
        - php/...（移植）
            底层交互（开发）
2.
livestream
- 发现
    独立
- 连接
    rtmp/rtsp/http/...
- 传输
    tcp/udp/sctp/...
- 播放
    rtp/flv/...
    h.264/mp4/...
    aac/g.711/...
    - 有插件（flash/...）
    - 无插件（html5原生支持）
3.
web开发
- mvc/mvp/mvvm
- restful
4.
移植过程
    独立
    - buildsystem
        autotools/cmake/...
    - 依赖
    - ...
消息拓扑
    独立
    - req/rep
    - sub/pub
    - ...

```

