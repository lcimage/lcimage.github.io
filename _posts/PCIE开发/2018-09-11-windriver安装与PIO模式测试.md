---
layout:     post   				    # 使用的布局（不需要改）
title:      windriver安装与PIO模式测试 				# 标题 
subtitle:        #副标题
date:       2018-09-11 				# 时间
author:     LC 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - pcie
---

>本篇文章参考米联客战神开发板及开发手册进行书写，在此表示感谢。
  
>紧接着上次的教程，大家准备好小板凳，小编这次主要介绍一下PCIE软件开发环境的安装与PCIE在PIO模式下的上板测试。


>温馨提示：对于每次下载完程序后，都需要重启电脑，切记！！！

| 电脑系统  | 开发工具 | 开发软件|设计工程|
| ---      | -----    | ----   |---    |
| win10    | ISE14.7  |  windriver10.21| PCIE的pio模式|

>插播一则灵魂画手的图画，

* PIO模式整个代码架构

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7nz2fmwcj20ic0600x6.jpg)

* PC与PCIE之间的关系
![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7nygbaxcj20c905h0sk.jpg)

我们这次实验使用的是jungo公司开发的驱动软件Windriver，操作简单方便，使用WinDriver的优点是:开发者并不需要熟悉任何内部操作系统或kernel programming或DDK及任何驱动程式.WinDriver同时允许开发者能在自己所熟悉的开发环境下,利用使用者模式(User Mode)如使用MSDEV Visual C/C++,Borland C++Builder,Delphi或任何Win32编译器，同时不需要牵扯到很底层的东西即可在很短的时间内编写驱动程序。

# 一、代码的编译和下载
## 第一步
对于上一遍博文接着来，我们打开ise工程，添加example design中的所有文件，并且修改ucf文件，确保所有的对外信号绑定管脚（参考时钟，复位，接收和发送差分对）。此处对照板卡的原理理仔细比对，查看仔细，如图1所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7mu4h54ej20ky0bcabx.jpg)

## 第二步 
我们对程序进行编译和生成bit流文件
## 第三步
启动Impact程序，也可搜索找到此软件，玩过ISE的朋友应该都晓得，如图2所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7mw8xz9ej20gj0dj75y.jpg)

## 第四步
双击**Create PROM File**，打开后作如下选择，（根据自己板卡及原理图信息进行选择，不要盲目点击）如图3所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7n4rffzkj20qf0fbgnr.jpg)

## 第五步
选择bit文件，如图4所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73qtw4nfj20kz0c3t9y.jpg)

## 第六步
点击**NO**，如图5所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73rpjr62j20dv06r40v.jpg)

## 第七步

点击**OK**，如图6所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73sehkzmj20df05ggnx.jpg)

## 第八步

双击**Generate File** 产生文件，并提示成功。

## 第九步
双击**Boundary Scan**，打开界面并进行初始化，如图7所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73y0a59zj20t90dxwf3.jpg)

## 第十步

双击**SPI/BPI**,添加刚才生成好的`mcs`文件，如图8所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv74316uvmj20ur0dlgn4.jpg)

## 第十一步
选择后进入添加FLASH（此处根据自己原理图的具体型号选择），点击OK，如图9所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7455rfj8j20bw08pglm.jpg)

## 第十二步
选择`program`，下载即可（此处稍等一分钟就会出现下载成功的英文单词success）。

>终于把程序烧到板子里面了，紧接着一口气继续。

# 二、 windriver安装和使用
## 第一步
Windriver软件是官方开发需要收费的软件，但是对于个人开发而言，安装后进行破解就行（此处不做赘述）。
## 第二步
设置禁止使用驱动程序强制签名，再安装驱动文件。
我们自己的主机可能是win10系统或者是win7系统，小编目测很少有人使用win8系统（故对其就不做介绍）。对于这两种系统的话，禁用驱动签名的方式有一点不同。
>对于win10系统而言
* 点击通知，找到并进入“所有设置”，如图10所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72yiu12sj20dw0htq7n.jpg)

* 在所有设置中找到并进入“更新和安全”，如图11所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72zmkjfqj20dv0awdgu.jpg)

* 找到恢复，点击“高级启动”下的“立即重启”，重启电脑，如图12所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv730kujr1j20dv0axjsc.jpg)

* 重启后选择“疑难解答”，如图13所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv731ptcjrj20dy06e41u.jpg)

* 选择“高级选项”，如图14所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73283itij20du06cwi5.jpg)

* 选择“启动设置”，如图15所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv733ubbpbj20dm06htcx.jpg)

* 点击“重启”，如图16所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv734djs8kj20e005z77h.jpg)

* 按提示输入“7”禁用驱动程序强制签名，如图17所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv734wp73zj20du0680wg.jpg)

>对于win7系统
* 开机到系统选项时，马上按F8键，进入安全模式，可提早按F8键，以免错过时间，如图18所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv736ke3rcj20dt08tgpi.jpg)

* 进入高级模式后，用键盘上的上下方向键移动光标到下面，找到并选择“禁用驱动程序签名强制”，按Enter键确定，就可进入系统，然后更新硬件的驱动程序就无须签过名的驱动了，如图19所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv7373rkzcj20dv08y763.jpg)

* 上面是一次性解决方法，如果想长期禁用驱动程签名强制，就要用下面这个方法，从开始处打开“所有程序”，再从附件中找到“命令提示符”，右键，以管理员身份运行，如图20所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv738703hgj20dp092gqm.jpg)

* 进入命令提示符窗口后，在光标处，输入bcdedit.exe -set loadoptions DDISABLE_INTEGRITY_CHECKS命令，回车。

如果禁用驱动程签名强制成功，就会显示“操作成功完成”，以后安装所有的驱动程，都不会被驱动程序签名阻止了。

## 第二步
我们安装好软件后，关闭电脑，然后插上我们的S6 系列的PCIE板卡，再次开启主机，用管理员方式打开刚才安装好的windriver，查看如图21所示列表，选择`New host driver project`。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv729q1hzgj20j10b80tg.jpg)

## 第三步
点进入之后我们会看到所有的PCI设备，这时我们会发现xilinx的PCIE设备，如图22所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72bjq7wtj20py0g6aat.jpg)

>如果这时打开的界面没有我们要找的PCIE设备的话，小编觉得问题可能就出现目前所用的板卡以前没有烧写过flash，以至于PC端不能识别，因此先必须找一个能用的demo工程，生成`.mcs`文件并烧写至板卡的flash中，然后再重启电脑，再次打开本驱动软件，就可以在列表中找到xilinx的PCIE板卡。

## 第四步

我们需要生成一个驱动文件，点击**Generate .INF
 file**(点击此处时，打开这个软件一定要用管理员身份打开，此后每次就可以直接打开即可)，我们需要填写`Manufacturer name`和`Device name`（此处填写不限制），并勾选`Automatically install the INF file`，然后点击**Next**，如图23所示。

 ![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72l69exjj20e90g4mxl.jpg)

## 第五步

紧跟着上一步，我们需要对需要文件起名字，并保存，如图24所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72p8ja8wj20k70ecwfz.jpg)

## 第五步
选择始终安装此驱动程序的软件，如图25所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv73bhgq1wj20p20drjv8.jpg)

## 第六步
看到以下自己命名的驱动文件时，表示成功，如图26所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv72rpe52cj20pq0g2mxv.jpg)

## 第七步
查看设备管理器驱动文件是否安装成功，如图27所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv74ahenqcj20ct0ekjs2.jpg)

>终于搞定windriver的安装了，接下来就是见证奇迹的时刻了。
# 三、测试
## 第一步
对此处双击，如图28所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv74ffy45yj20fm01ljr7.jpg)

## 第二步
进去之后，按如下提示进行选择，我们可以对地址0写入一个32bit的数据，比如写入数据为12345678，如图29所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv74nevnwvj213v0g6gnv.jpg)

## 第三步 
我们选择读数据，就可在Information Panel，界面看到我们写入和对出的数据，对于以前写入和读出数据，可以在不同地址进行测试，如图30所示。

![](http://ww1.sinaimg.cn/large/ebdd169ely1fv74qos2lpj20jg05yq2t.jpg)

>本来此处还需要在**chipscope**中抓取数据、地址及一些指示信号，但是小编觉得PIO模式应该不难，留给读者自己，后续在DMA模式下小编会详细抓取一些发送和接收引擎的信号，大家拭目以待吧。