---
title: "iOS 5.0开启全景拍照（无需越狱）"
date: "2011-11-15"
---

iOS 5.0 被发现是自带全景拍照功能的，只是iOS 默认把这项功能隐藏了。早前就已经有方法开启iOS 5 的全景拍照，但是需要越狱设备。现在又放出了无需越狱的方法。

1. ### 先下载 [iBackupBot for iTunes](http://www.icopybot.com/itunes-backup-manager.htm "iBackupBot for iTunes")。
    
2. ### 把iPhone、iPod Touch 等iOS 5.0 设备连接进电脑，打开iTunes，先将设备备份一次。
    
    [![](images/00001-01-02caea95ddf69936.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-01-02caea95ddf69936.png)
3. ### 打开iBackupBot for iTunes，即可见到刚刚在iTunes 的备份。
    
4. ### 接着找到Library/Preferences/com.apple.mobileslideshow.plist，双击进行编辑。
    
    [![](images/00001-02-c9289aa3bac69116-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-02-c9289aa3bac69116.png)
5. ### 在编辑模式下，按照图中位置插入如下代码，然后保存。
    
    \[xml\] <key>EnableFirebreak</key> <string>YES</string> \[/xml\]
    
    [![](images/00001-03-c5bb49dc85d8dc98-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-03-c5bb49dc85d8dc98.png)
6. ### 返回主界面，按“Restore backup/file to iPhone/iPod Touch”把修改后的备份恢复到你的iOS 5.0 的设备上。整个恢复过程需要一段时间，长短取决于你的备份大小。
    
    [![](images/00001-04-a9819c3df43c0dd7-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-04-a9819c3df43c0dd7.png)
    
    [![](images/00001-05-dcb6f8c29c0d9ac1-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-05-dcb6f8c29c0d9ac1.png)
7. ### 恢复成功后，你的iOS 5.0 设备将会重启，重启后，全景拍照功能就被解开了，Enjoy it~
    
    [![](images/00001-06-f25713b9e13a18a4-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-06-f25713b9e13a18a4.png)
    
    [![](images/00001-07-906171fe79fe3c5b-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-07-906171fe79fe3c5b.png)
    
    [![](images/00001-08-4b02a8afe9366b05-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00001-08-4b02a8afe9366b05.png)
