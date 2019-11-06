# linux-kernel

## 代码和编译路径
* kernel代码：*kernel/*
* kernel编译位置：*out/product/kunpeng/obj/kernel-intermediate/*
* kernel配置：*device/kunpeng/pear_v10_linux_sfcnand_ubi_thermal_defconfig*

### ota kernel编译位置：
* kernel代码：*kernel/*
* ota kernel编译位置：*out/product/kunpeng/obj/kernel-recovery-intermediate/*
* ota kernel配置：*device/kunpeng/pear_v10_linux_sfcnand_recovery_defconfig*


## 编译方法
* 设置环境变量后启动编译命令
` $ source build/envsetup.sh`
` $ make kernel-clean; make kernel`
* 目标文件为： out/product/kunpeng/image/kernel

此整体编译方法操作简单但耗时较长。


## 快速编译方法
开发kernel驱动过程中，经查需要频繁的修改代码编译验证，可分步编译来加快编译时间，步骤如下：

* 修改kernel/驱动代码。
* 切换至kernel编译路径执行：
```
$ cd out/product/kunpeng/obj/kernel-intermediate/
$ make xImage
```
* 目标文件为：
`out/product/kunpeng/obj/kernel-intermediate/arch/mips/boot/compressed/xImage`
