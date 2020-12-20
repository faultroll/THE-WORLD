gcc编译Ti
https://github.com/mwbrown/cc2640r2_blinky

rfcore：
https://github.com/Venemo/cc2640-ble-broadcaster
https://e2echina.ti.com/question_answer/wireless_connectivity/hw_rf_proprietary/f/45/t/137588
https://e2e.ti.com/support/wireless-connectivity/sub-1-ghz/f/156/p/471529/2288482
https://github.com/ChenYu0302/TI_CC2640R2_LaunchPad-iBeacon
https://github.com/ntilau/cc2640-ble-advertising
晶振初始化：https://blog.csdn.net/feilusia/article/details/47003293?utm_source=blogxgwz0

flash烧录工具：
https://www.ti.com/tool/cn/uniflash
（不用这个）https://www.ti.com.cn/tool/cn/flash-programmer
uniflash无法烧录（[ERROR] Cortex_M3_0: File Loader: Verification failed: Values at address 0x00008000 do not match Please verify target memory and memory map.）：将 binary 的勾给去掉
![](https://upload-images.jianshu.io/upload_images/293860-0680e5ea22d3542b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
xds：http://processors.wiki.ti.com/index.php/XDS_Emulation_Software_Package

arm-none-eabi-gcc无法链接libc：
https://stackoverflow.com/questions/23082990/error-compiling-hello-world-program-c-with-arm-none-eabi-gcc

uart：
https://e2e.ti.com/support/wireless-connectivity/bluetooth/f/538/t/614898

linux：
https://github.com/jcormier/TI_BLE_CC2650_Linux_Convert

https://users.ece.utexas.edu/~valvano/arm/
