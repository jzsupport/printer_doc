# buildroot

* 项目介绍：[buildroot](https://buildroot.org/)
  >buildroot是Linux平台上一个构建嵌入式Linux系统的框架。整个Buildroot是由Makefile脚本和Kconfig配置文件构成的。你可以和编译Linux内核一样，通过buildroot配置，menuconfig修改，编译出一个完整的可以直接烧写到机器上运行的Linux系统软件(包含boot、kernel、rootfs以及rootfs中的各种库和应用程序)。

君正打印机项目使用buildroot生成设备运行时的rootfs。

------
* 参考文档 
[buildroot使用介绍](https://www.cnblogs.com/arnoldlu/p/9553995.html)

------

## 代码和编译路径
* 代码：*buildroot/*
* 编译位置：*buildroot*
* 配置文件：*device/kunpeng/printer_usb_defconfig*

工程第一次make完毕后，基于device/kunpeng/printer_usb_defconfig生成buildroot/.config。如果没有生成.config的话，通过命令手动配置
```
$ cp device/kunpeng/printer_usb_defconfig buildroot/configs/printer_usb_defconfig
$ cd buildroot
$ make printer_usb_defconfig
```
* 若需修改配置，配置命令为：
`$ make menuconfig 或$ make xconfig`

------
## 软件包介绍
主要软件包介绍
