# u-boot

## 代码和编译路径
* u-boot代码：*u-boot/*
* u-boot配置：*u-boot/boards.cfg*
>boards.cfg文件左侧第一列为板级配置名称，x1000打印机板级配置为： pear_xImage_sfc_nand


## 编译方法
* 设置环境变量后
` $ source build/envsetup.sh`
* 启动编译命令
` $ make u-boot-clean; make u-boot`
或者
` $ cd u-boot; make pear_xImage_sfc_nand`
* 目标文件为： u-boot/u-boot-with-spl.bin
或 out/product/kunpeng/image/u-boot-with-spl.bin
