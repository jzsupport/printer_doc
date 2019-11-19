# printerGUI程序

##介绍
本方案集成了qt环境，并基于qt做了一个简单的gui界面。该界面展现打印机的打印功能、二维码识别、生成二维码功能，以及打印状态的实时更新等功能。代码在packages/kunpeng/testqt/qtmain目录下，直接在工程根目录下执行make qtmain命令即可生成qtmain可执行程序。界面主要分为主扫码和被扫码两部分，下面分别介绍。

* 被扫码
系统启动后默认进入被扫码界面，在该界面，输入框中输入链接地址，点击‘产生’按钮，会在输入框下面显示生成的二维码图片。如果输入框中没有地址，默认点击‘产生’按钮，会显示 "http://www.ingenic.com.cn/" 链接的二维码。此时按打印按钮，会打印生成的二维码。
* 主扫码
点击‘主扫码’按钮，进入主扫码界面，该界面回显camera灰度数据，用户可将二维码通过camera进行扫码识别，如果识别成功，会在输入栏中显示扫码界面。此时按打印按钮，会打印输入栏中的扫码界面。

具体测试细节请参考printer设备使用方法中的热敏打印机测试项。
##qtmain和scanner-test通信方式
qtmain调用scanner-test实现二维码的识别和camera数据灰度图回显。具体关于scanner-test部分实现和编译参考qrcode章节。在此主要介绍qtmain和scanner-test通信部分。
qtmain通过调用scanner-test /dev/video1 1 1 &命令启动二维码识别程序。其scanner-test最后两个参数设置为1，是指need_result和need_yuv，need_result设置为1，scanner-test会将扫描结果写入/tmp/myfifo中。need_yuv设置为1，scanner-test会将camera中的数据经处理成灰度图像写入共享内存。
qtmain将共享内存中数据回显部分代码如下：
```
void MainWindow::masterDraw()
{
    qDebug()<<"masterDraw";
//    FILE *fp = nullptr;
//    int pixSize = 1; //像素大小Grayscale8就是一个字节
//    uchar *buf = new uchar[640*480*pixSize];
//    if((fp=fopen("/home/user/18.yuv","rb"))==nullptr)
//           qDebug()<<"open 18.yuv error!\n";
//    fread(buf,1,640*480,fp);
//    fclose(fp);
    //qDebug()<<buf;

//    QImage img = QImage(buf, 640, 480, QImage::Format_Grayscale8);
    QImage img = QImage(p, 640, 480, QImage::Format_Grayscale8);
    QPainter painter;
    painter.begin(ui->widgetMaster);
    painter.drawImage(QPoint(0, 0), img);
    painter.end();
}
```
qtmain读取/tmp/myfifo中二维码识别结果部分代码如下：
```
void MainWindow::getresult_func()
{
    qDebug()<<"start getresult_func";
    while(1)
    {
        if(inMasterscanner){

            memset(result, 0, 1024);
            int len = ::read(fifofd, result, 1024);
            if(len > 0 && strlen(result))
            {
                qDebug()<<"addr:"<<result;
                ui->lEditAddrMaster->setText(result);
                sem_post(semcontinue);
            }
        }else
            sleep(1);
    }
}
```
##qtmain生成二维码方式
qtmain通过调用writer程序将输入框中的链接生成二维码，writer会将生成的二维码图片保存为/tmp/a.bmp，关于writer介绍参考qrcode章节。qtmain调用函数如下：
```
void MainWindow::on_btnGenerate_clicked()
{
    qDebug()<<"on_btnGenerate_clicked";

    if(strlen(ui->lEditAddrSlave->text().toStdString().c_str()))
    {
        char cmd[1024];
        sprintf(cmd,"writer '%s'",ui->lEditAddrSlave->text().toStdString().c_str());
        system(cmd);
    }else{
        system("writer 'http://www.ingenic.com.cn/'");
    }

    paintflag = 1;
    update(); //手动产生绘图事件
}
```
此时会生成/tmp/a.bmp二维码图片，然后通过下面函数显示二维码如下：
```
void MainWindow::slaveDraw()
{
    QPainter painter(ui->widgetSlave);
    QPixmap pix;
    pix.load("/tmp/a.bmp");
    painter.translate(10,10); //将（10，10）设为坐标原点
    int size = ui->widgetSlave->frameGeometry().width() > ui->widgetSlave->frameGeometry().height() ?
                ui->widgetSlave->frameGeometry().height(): ui->widgetSlave->frameGeometry().width();
    painter.drawPixmap(0,0,size-30,size-30,pix);
}

```