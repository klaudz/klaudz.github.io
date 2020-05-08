---
title: "升级iOS 5.0.1 + 完美越狱教程(A4处理器)"
date: "2011-12-29"
---

在12月27日晚，@pod2g 终于把大家期待已久的 iOS 5.0.1 的完美越狱工具 redsn0w 0.9.10b1 放出。 [redsn0w 0.9.10b1 for Windows 下载](http://vdisk.weibo.com/s/1IUAv "redsn0w 0.9.10b1 for Windows") [redsn0w 0.9.10b1 for MacOSX 下载](http://vdisk.weibo.com/s/1IUdh "redsn0w 0.9.10b1 for MacOSX")

此次放出的越狱工具可以越狱 iOS 5.0.1 的非 A5 处理器的设备，包括 iPhone 3GS，iPhone 4，iPhone 4 CDMA，iPad 1，iPod Touch 3G，iPod Touch 4G。 （也就是说，暂时此越狱工具还不能给 iOS 5.0.0 越狱，也不能给 iPhone 4S 和 iPad 2 越狱。） 今天终于抽出了时间进行越狱，并记录一下。 这里仅仅是记录一下我是如何给我的越狱的过程，以简单的文字配上每一个步骤的截图来做说明，所以是以简单为原则，不会作太深奥的文字解释，希望给予新手技术支持。

废话少讲，下面开始。

首先要注意两点：

1. 如果是有锁版 iPhone，需要修改过的固件，所以请勿根据本教程越狱。
2. 如果是之前已经进行过不完美越狱的，可以直接在 Cydia 搜索“corona”插件来进行完美越狱。也可以根据本教程进行越狱，但下面会提到相关注意事项。

## 升级固件

升级到 iOS 5.0.1（如果你的设备已经为 iOS 5.0.1，则直接跳到“开始越狱”）

1. 第一种方法，把设备用 USB 连接到电脑，在 iTunes 上直接【更新】iOS 5.0.1。（iTunes 直接下载更新，更新包约777MB，所以下载慢很慢）[![](images/00009-01-53965432d7c6a2f9-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-01-53965432d7c6a2f9.png)
2. 第二种方法，是通过苹果官方的地址，直接下载 iOS 5.0.1 固件文件（.ipsw），然后把设备恢复成选中固件的版本。 注意：这种方法是恢复模式，恢复前 iTunes 并不会备份数据，如有需要应该先备份一下设备的数据 （如何备份？可以参照[《iOS 5.0 开启全景拍照（无需越狱）》](http://cloud018.sinaapp.com/archives/ios-5-0-%e5%bc%80%e5%90%af%e5%85%a8%e6%99%af%e6%8b%8d%e7%85%a7%ef%bc%88%e6%97%a0%e9%9c%80%e8%b6%8a%e7%8b%b1%ef%bc%89/ "iOS 5.0 开启全景拍照（无需越狱）")的第2个步骤）。 下面提供苹果官方 iOS 5.0.1 的直接下载地址。（用下载工具下载，速度快，推荐）
    
    - [5.0.1 固件 for iPhone 4](http://appldnld.apple.com/iPhone4/041-3309.20111109.64rtg/iPhone3,1_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPhone 4")
    - [5.0.1 固件 for iPhone 4 CDMA](http://appldnld.apple.com/iPhone4/041-3304.20111109.Vgtyh/iPhone3,3_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPhone 4 CDMA")
    - [5.0.1 固件 for iPhone 3GS](http://appldnld.apple.com/iPhone4/041-3307.20111109.5tGhu/iPhone2,1_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPhone 3GS")
    - [5.0.1 固件 for iPad 1](http://appldnld.apple.com/iPhone4/041-3308.20111109.Fvgtr/iPad1,1_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPad 1")
    - [5.0.1 固件 for iPod Touch 4](http://appldnld.apple.com/iPhone4/041-3313.20111109.Azxe3/iPod4,1_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPod Touch 4")
    - [5.0.1 固件 for iPod Touch 3](http://appldnld.apple.com/iPhone4/041-3314.20111109.Mbgh6/iPod3,1_5.0.1_9A405_Restore.ipsw "5.0.1 固件 for iPod Touch 3")
    
    [![](images/00009-02-78a8e8a9e63dee4a.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-02-78a8e8a9e63dee4a.png)
    
    下载完毕后，把设备用 USB 连接到电脑，在 iTunes 上按着【shift】键，然后点击【恢复】，然后选择刚刚下载的 .ipsw 文件。
    
    [![](images/00009-03-07baa55e11986c67-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-03-07baa55e11986c67.png)
    
    [![](images/00009-04-6194db4da8bf9c09-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-04-6194db4da8bf9c09.png)
    
    [![](images/00009-05-d82fcd2c0c6caf19.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-05-d82fcd2c0c6caf19.png)
    
    [![](images/00009-06-ad24cac65d68f740-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-06-ad24cac65d68f740.png)
    
    [![](images/00009-07-206c34041eabd486-medium.jpg)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-07-206c34041eabd486.jpg)
    
    [![](images/00009-08-a3c16cae7fdf9786-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-08-a3c16cae7fdf9786.png)
    
    [![](images/00009-09-770b938ed2dd8238.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-09-770b938ed2dd8238.png)
    
    [![](images/00009-10-ecf3197c766df454-medium.jpg)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-10-ecf3197c766df454.jpg)
    
    [![](images/00009-11-54624121bd7a31a5-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-11-54624121bd7a31a5.png)
    
    [![](images/00009-12-0f316a97d9ba8712-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-12-0f316a97d9ba8712.png)

## 开始越狱

这里以 Windows 7 为例进行教程

1. 下载红雪越狱工具，解压后，以管理员身份运行 redsn0w.exe。 [![](images/00009-13-3f2ac67649095956.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-13-3f2ac67649095956.png)
2. 点击【Extras】 [![](images/00009-14-5405c698302ed758-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-14-5405c698302ed758.png)
3. 点击【Select IPSW】，选择在“升级固件”中提到的下载下来的 iOS 5.0.1 的固件文件。 （固件要对应设备，例如我的是 iPod Touch 4，就要选择“5.0.1 固件 for iPod Touch 4”。） [![](images/00009-15-a0ababd5fd0363d2-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-15-a0ababd5fd0363d2.png)[![](images/00009-16-83e5b507e3516d9e-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-16-83e5b507e3516d9e.png)
4. 选择好固件以后，redsn0w 会提出提示框，显示确认信息，按【确定】，返回按【Back】。 [![](images/00009-17-2f8c57e652f00683-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-17-2f8c57e652f00683.png)
5. 返回 redsn0w 主界面后，按【jailbreak】，等待进度条完成。 [![](images/00009-18-f55ad05c9aab9f6b-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-18-f55ad05c9aab9f6b.png)
6. 进度条消失后，进入下一画面。在这个画面中，【Install Cydia】已经被默认勾选了，其他选项根据需要来选就可以了。 注意：如果已经不完美越狱过的，不要勾选【Install Cydia】 [![](images/00009-19-25376211ee588aff-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-19-25376211ee588aff.png)
7. 点击【Next】，进入 DFU 模式前的提示画面。 这个的提示是，需要越狱的 iOS 设备必须先通过 USB 连接到电脑，连接后【关机】。 [![](images/00009-20-21dc8efeaeb31728-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-20-21dc8efeaeb31728.png)
8. 一切都准备完毕后，点击【Next】就要把设备进入 DFU 模式了。 DFU 模式就是强制升降级模式，其实无须紧张，按照步骤来做就可以了，失败后大不了重来就可以。 如图，redsn0w 会有倒数提示，照着做就没有问题。步骤如下：
    
    1. 先按着【开机】键3秒
    2. 保持按着【开机】键，然后按着【HOME】键10秒
    3. 放开【开机】键，保持按着【HOME】键15秒
    
    [![](images/00009-21-3a2255dd1ba398bf-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-21-3a2255dd1ba398bf.png)
    
    按着顺序做，你会见到电脑的任务栏右下角有小气泡提示正在安装驱动，而 redsn0w 的画面也会转到下一个页面。 这个过程中，iOS 设备就会重启，甚至黑屏，无需惊慌，这是在进行越狱过程。
    
    [![](images/00009-22-a0f3478797f24d6d-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-22-a0f3478797f24d6d.png)
9. 等待。。。不久，redsn0w 会出现“Done!”的提示，但此时还未越狱完成的，你可以见到你的 iOS 设备在命令行的模式（黑屏白字）滚动。 然后就会出现菠萝的界面，这个界面要等待更久，所以慢慢等吧。。。 [![](images/00009-23-c4f697c37179872b-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-23-c4f697c37179872b.png)[![](images/00009-24-f816a47dfbe0b7b2-medium.jpg)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-24-f816a47dfbe0b7b2.jpg)[![](images/00009-25-6d1bb05b77210feb-medium.jpg)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-25-6d1bb05b77210feb.jpg)
10. iOS 设备在完毕以后，会再重启一次。重启后如果见到 Cydia 图标，证明越狱已经完成了。Enjoy it~ [![](images/00009-26-83cf1c494405df8f-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00009-26-83cf1c494405df8f.png)
