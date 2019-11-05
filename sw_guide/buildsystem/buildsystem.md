#系统编译


## 如何编译

### 编译命令：
* 第一次编译需安装必要软件包： $ ./build/core/tools/auto_env_setup.sh 
* 设置环境变量： $ source build/envsetup.sh
* 选择配置： $ lunch
* 开始编译： $ make   （或多线程编译 $make -j）

运行log如下：
``` shell
$ source build/envsetup.sh
including device/kunpeng/vendorsetup.sh

$ lunch
You're building on Linux
Lunch menu... pick a combo:
     1. kunpeng_usb-userdebug
Which would you like? [kunpeng_usb-userdebug] 1
selection is kunpeng_usb-userdebug
product is kunpeng_usb
variant is userdebug
kunpeng
usb
============================================
TARGET_PRODUCT=kunpeng_usb
TARGET_DEVICE=kunpeng
TARGET_STORAGE_MEDIUM=usb
TARGET_EXT_SUPPORT=
TARGET_BUILD_VARIANT=userdebug
TARGET_BUILD_TYPE=release
TARGET_BUILD_APPS=
HOST_ARCH=x86
HOST_OS=linux
HOST_OS_EXTRA=Linux-3.19.0-26-generic-x86_64-with-Ubuntu-14.04-trusty
HOST_BUILD_TYPE=release
OUT_DIR=out
ToolChian_version=mips-gcc520-glibc222
GLIBC_VERSION=5.2.0
============================================

$ make
```
>选择kunpeng_usb-userdebug是X1000_Pear平台热敏打印机：
>

### 目标文件

``` shell
out/product/kunpeng/image/
|-- kernel
|-- kernel-recovery
|-- system.ubifs
`-- u-boot-with-spl.bin
```

### 制作ota包

制作ota命令：
>$ make ota_mkpackage

编译完成后ota目录升级包:
```
out/product/kunpeng/image/ota
`-- ota-package.7z
```


### 模块单独编译
在工程目录下重新编译子模块命令如下：

	+ uboot : make u-boot-clean;make u-boot
	+ kernel : make kernel-clean;make kernel
	+ kernel-recovery : make kernel-recovery-clean;make kernel-recovery
	+ buildroot : make rootfs-clean;make rootfs
	+ system : make rootfs-clean;make rootfs;rm -r out/product/j618/obj/post-intermediate;make


## 代码结构
本工程分为如下几个目录，各目录存放内容如下：

+ build : 整个工程的编译规则
+ buildroot : buildroot源码，提供基础文件系统
+ device/kunpeng : 板级配置信息
+ external : 只读文件系统和程序异常调试信息
+ hardware : kernel、uboot编译和wifi firmware安装
+ package : 存放buildroot之外打印机系统需要的相关模块
+ prebuilts : 存放烧录工具、编译链
+ kernel : kernel源码
+ u-boot : uboot源码


## 设备配置
设备配置文件： device/kunpeng/device.mk

### uboot配置
UBOOT_BUILD_CONFIG:=pear_xImage_sfc_nand
UBOOT_TARGET_FILE:=u-boot-with-spl.bin

### kernel配置
```
KERNEL_BUILD_CONFIG:=pear_v10_linux_sfcnand_ubi_thermal_defconfig
KERNEL_RECOVERY_BUILD_CONFIG:=pear_v10_linux_sfcnand_recovery_defconfig
KERNEL_TARGET_IMAGE:=xImage
```

### 文件系统配置
```
BUILDROOT_CONFIG:=printer_usb_defconfig
BUILDROOT:=buildroot
FILE_SYSTEM_TYPE:=ubifs
ROOTFS_UBIFS_LEBSIZE:= 0x1f000
#ROOTFS_UBIFS_MAXLEBCNT := 2048
ROOTFS_UBIFS_MINIOSIZE := 0x800
#ROOTFS_UBIFS_EBSIZE := 131072
ROOTFS_UBIFS_PAGESIZE := 2048
```
