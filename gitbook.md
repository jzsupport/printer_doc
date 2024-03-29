
# document
  * markdown
  * git（shared repository）
  * markdown editor(remarkable,gitbook-editor,typora)
  * gitbook

------
## github
* https://github.com/jzsupport/printer_doc
* username: jzsupport
* password: jzsupport*123
```shell
$ git clone https://github.com/jzsupport/printer_doc.git
$ cd printer_doc
$ git pull
$ git push

gitconfig示例
$ cat ~/.gitconfig
[credential]
	helper=store
[color]
	ui = auto
[user]
	name = Linggang
	email = linggang.wang@ingenic.com
[push]
	default = simple
```

------
## Markdown format

Markdown格式补充：
```
索引：文件头加入 <!-- toc --> ，当编译的时候回自动在这个位置增加索引
分段： 空一行换的是自然段；
换行： <br/>，或者2个空格"  "
首行缩进： 使用html方法，输入全角或半角占位符&emsp;   &ensp;

图片格式：
<div align=center><img src="res/x1000_printer.jpg" width="50%" height="50%">

<!-- 居中 宽度100 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>

<!-- 居中 宽度200 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="200"> 
</div>

<!-- 居中 宽度300 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="300"> 
</div>

<!-- 居左 宽度100 -->
<div align=left>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>

<!-- 居右 宽度100 -->
<div align=right>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>
————————————————
版权声明：本文为CSDN博主「MrNoboday」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/MrNoboday/article/details/89203688

```

<!-- 居中 宽度100 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>

<!-- 居中 宽度200 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="200"> 
</div>

<!-- 居中 宽度300 -->
<div align=center>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="300"> 
</div>

<!-- 居左 宽度100 -->
<div align=left>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>

<!-- 居右 宽度100 -->
<div align=right>
	<img src="https://www.baidu.com/img/bd_logo1.png" width="100"> 
</div>


------

## gitbook

### install gitbook
* node.js
>https://nodejs.org/en/
node-v12.13.0-linux-x64.tar.xz
* gitbook
` $ npm install gitbook-cli -g`
* ebook-convert
`$ sudo apt-get install calibre`
* set PATH

```
$ NODE_PATH=gitbook/install/node-install/node-v12.13.0-linux-x64/bin
$ NODE_MODULES_PATH=gitbook/install/node-install/node_modules/.bin
$ export PATH=$NODE_PATH:$NODE_MODULES_PATH:$PATH
```

### gitbook commands
生成电子书 (epub, mobi, pdf) 时需要ebook-convert。

### start gitbook
```
$ cd your_book/
$ gitbook serve
按需安装插件
$ gitbook install      (install plugins)
```

### 指定路径参数
```
构建书籍：gitbook build
默认：将生成的静态网站输出到 _book 目录
指定路径：gitbook build [书籍路径] [输出路径]
指定端口：gitbook serve --port 2333
生成pdf格式：gitbook pdf ./ ./mybook.pdf
生成epub格式：gitbook epub ./ ./mybook.epub
生成 mobi 格式：gitbook mobi ./ ./mybook.mobi

注意：如果生成不了，你可能还需要安装一些工具，比如 calibre、ebook-convert，或者在 Typora 中安装 Pandoc 进行导出。

作者：左木北鱼
链接：https://www.jianshu.com/p/dc53e589897a
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
### 参考文章
* GitBook 从懵逼到入门
https://blog.csdn.net/lu_embedded/article/details/81100704
* GitBook 使用教程
https://www.jianshu.com/p/421cc442f06c
* GitBook 简明教程
http://www.chengweiyang.cn/gitbook/index.html

------
## remarkable

 $ dpkg -i remarkable_1.87_all.deb


------

## typora
http://support.typora.io/Typora-on-Linux/
在Ubuntu 14.04 安装typora启动失败。在Ubuntu 16.04 安装typora启动成功。

```
# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update
# install typora
sudo apt-get install typora
```

