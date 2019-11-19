# systemserver系统服务

## 功能介绍
systemserver是一种管理进程的系统服务模块。主要功能如下：

* 维护进程状态，在内存中记录进程需要的property
* 控制进程之间的依赖关系
* 提供进程间消息通讯机制

## 编译方法及相关接口
在工程目录执行make systemserver生成如下文件：

* systemservice：记录进程状态，根据init.rc配置文件维护进程启动顺序和进程间依赖关系
* libcutils.so和libsysutils.so： 提供一种一对多的进程间消息通讯机制，提供property_get和property_set函数接口
* getprop命令：读取进程状态和property
* setprop命令：设置property
* cmdserver和cmdclient：进程间消息通讯的测试程序

## 相关属性
systemservice程序会在内存中维护一个列表，记录进程需要的property，用于进程间消息传递和进程状态的显示。通过getprop命令可以看到系统中所有property的值和进程状态信息，如下：
```
# getprop
[init.svc.adbd]: [running]
[init.svc.bleservice]: [running]
[init.svc.bsaservice]: [stopped]
[init.svc.cmdserver]: [running]
[init.svc.connectserver]: [running]
[init.svc.dbusd]: [stopped]
[init.svc.debuggerd]: [stopped]
[init.svc.dnsservice]: [stopped]
[init.svc.event_manager]: [running]
[init.svc.klogd]: [running]
[init.svc.logd]: [running]
[init.svc.network]: [stopped]
[init.svc.network_manager]: [running]
[init.svc.networkconfig]: [stopped]
[init.svc.printer]: [running]
[init.svc.pserver]: [running]
[init.svc.spiservice]: [stopped]
[init.svc.urandom]: [stopped]
[init.svc.userdata]: [stopped]
[persist.settings.backlight]: [222]
[persist.settings.baseurl]: [http://117.78.40.33:18985/bill_edge]
[persist.settings.volume]: [32]
[persist.update.isForce]: [10]
[sys.backend.spi.clk]: [30000000]
[sys.dev.chipID]: [0d21ca500530340c00000000af992990]
[sys.dev.dns0]: [192.168.17.1]
[sys.dev.dns1]: [192.168.17.1]
[sys.dev.dns2]: []
[sys.dev.gw.eth]: [192.168.17.1]
[sys.dev.gw.wlan]: [192.168.17.1]
[sys.dev.gw]: [192.168.17.1]
[sys.dev.ipaddr.eth]: [192.168.17.115]
[sys.dev.ipaddr.wlan]: [192.168.17.109]
[sys.dev.ipaddr]: [192.168.17.115]
[sys.dev.ipmask.eth]: [255.255.255.0]
[sys.dev.ipmask.wlan]: [255.255.255.0]
[sys.dev.ipmask]: [255.255.255.0]
[sys.dev.mac.eth]: [2E:6C:CC:EC:49:8B]
[sys.dev.mac.wlan]: [28:ED:E0:95:17:A9]
[sys.dev.mqtt.timeout]: [30]
[sys.dev.net]: [OK]
[sys.dev.paytexno]: []
[sys.dev.version]: [1]
[sys.logd.config]: [-n -s 16]
[sys.printer.baseurl]: [http://117.78.40.33:18985/bill_edge]
[sys.printer.mqtturl]: [/pdf/get]
[sys.printer.otadetecturl]: [/sys/checkUpdate]
[sys.printer.register]: [OK]
[sys.printer.registerurl]: [/printer/register]
[sys.printer.topicprefix]: [P]
[sys.printer.typePrinterSn]: [j918_mips_x1000]
[sys.settings.getCommentUrl]: [/printer/getComment]
[sys.settings.saveCommentUrl]: [/printer/saveComment]
[sys.settings.savePrintMethodUrl]: [/printer/savePrintMethod]
[sys.usb.config]: [adb]
[sys.usb.state]: [adb]
```
可见，property根据需要分为以下几种：

* 以init.开头的property：表示由systemservice维护的各个进程的当前状态
* 以persist.开始的property：记录重启后不会被重置的property，systemservice会把这类property记录到/usr/data/property/目录中
* 其他进程自定义的property：进程根据需要自定义的property，通过setprop或getprop命令设置或得到，也可在程序中通过property_get和property_set函数接口设置和获取对应的property，一些固定的property值可以记录到文件系统/etc/build.prop文件中，systemservice会读取该文件设置property

## 进程依赖关系设置

 systemservice根据/etc/init.rc配置文件维护进程启动顺序和进程间依赖关系，用户可以通过配置init.rc文件灵活控制各进程启动时先后顺序和进程间的依赖。下面是Init.rc文件的设置示例：
```
 on init
   export LD_PRELOAD /usr/lib/libdebugger.so
   start logd
   start klogd
   start userdata
   start urandom
on property:init.svc.logd=running
   start debuggerd

on property:init.svc.urandom=stopped
   start dbusd
   start network

on property:init.svc.network=stopped
   start networkconfig
   start event_manager

on property:init.svc.networkconfig=stopped
   start network_manager

on property:init.svc.network_manager=running
   start dnsservice

on property:init.svc.userdata=stopped
   load_persist_props

on property:init.svc.dbusd=stopped
    start bsaservice

on property:init.svc.bsaservice=stopped
   start bleservice

on property:sys.usb.config=none
   stop adbd
   write /sys/class/android_usb/android0/enable 0
   
   # adb only USB configuration
# This should only be used during device bringup
# and as a fallback if the USB manager fails to set a standard configuration
on property:sys.usb.config=adb
   write /sys/class/android_usb/android0/enable 0
   write /sys/class/android_usb/android0/idVendor 18d1
   write /sys/class/android_usb/android0/idProduct dddd
   write /sys/class/android_usb/android0/functions ${sys.usb.config}
   write /sys/class/android_usb/android0/enable 1
   start adbd
   setprop sys.usb.state ${sys.usb.config}

service debuggerd /etc/init.d/S22debuggerd start
    class core
    disabled
    oneshot
            
service adbd /usr/bin/adbd
    class core
    disabled
    
service logd /sbin/syslogd -n -s 16
    class log
    disabled

service klogd /sbin/klogd -n
    class log
    disabled

service userdata /etc/init.d/S21mount start
    class device
    disabled
    oneshot

service urandom /etc/init.d/S20urandom start
 class device
    disabled
    oneshot

service dbusd /etc/init.d/S30dbus start
    class core
    oneshot
    disabled

service network /etc/init.d/S40network start
    class core
    oneshot
    disabled

service networkconfig /etc/init.d/S45network.sh
    class core
    oneshot
    disabled

service network_manager /usr/sbin/network_manager -m sta
    class core
    disabled
    onrestart restart connectserver
    onrestart restart bleservice

service event_manager /usr/sbin/event_manager
    class core
    disabled
    onrestart restart bleservice

service avahiservice /etc/init.d/S50avahi-daemon start
    class core
    disabled
    oneshot

service bsaservice /etc/init.d/S60bsa start
    class bluetooth
    disabled
    oneshot

service bleservice /sbin/ble_cfg
    class bluetooth
    disabled

service dnsservice /etc/init.d/S80dnsmasq start
    class core
    disabled
    oneshot
```
 可见，init.rc设置分为如下部分：

 * on init：设置初始化时需要启动的服务，这些服务是并行运行的
 * on property：设置当指定property被设置为某一状态值时，需要做的操作和启动的服务。可以是systemservice维护进程的状态，也可以是用户可在程序中主动设置某一属性的值去启动相关服务
 * service：是设置服务的具体实现方法
 	+ class: 设置一个进程所属的组
 	+ disabled：开机不启动，需要手动触发
 	+ oneshot：只启动一次
 	+ onrestart：设置该服务重启后要做的操作，比如重启其他服务
 	
## 进程间消息通讯机制
systemserver提供了一种一对多的进程间消息机制。用户使用简单，具体实现方法如下：
首先，需要通信的两个进程需要链接libcutils.so和libsysutils.so库，libsysutils.so库实现了FrameworkListener类，server端只要继承该类实现要处理的命令就可以监听处理所有client发送的消息，在client端使用libcutils.so库提供的socket_local_client函数实现消息发送。这样就完成了client和server的消息通讯。具体使用参考packages/kunpeng/system/systemserver/socket-test中的测试例子。此示例在编译后会生成cmdserver和cmdclient两个可执行文件，测试方法如下：
在Init.rc中加如下几行，设置cmdserver service：
```
service cmdserver /usr/bin/cmdserver
    class core 
    socket Demo stream 0666 root root
    console
```
然后在适当时机start cmdserver，重启系统后，cmdserver就运行起来了，此时执行cmdclient aaaa命令（其中aaaa是发要发送给server的命令），cmdserver端就收到了aaaa命令。cmdserver端输出如下：
```
DemoCmd Command:
Demo
aaaa
```