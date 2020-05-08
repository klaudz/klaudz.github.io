---
title: "解决iOS 5越狱后Cydia弹出键盘崩溃的问题"
date: "2012-01-21"
---

iOS 5 的完美越狱以后，在中文环境下，在 Cydia 中点击输入框之类的是会 Crash 掉的，在英文环境下则没事。 例如在中文环境下点击搜索框，Cydia 会立即崩掉。 [![](images/00001-01-92aadf8c72d8e8d6-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2012/01/00001-01-92aadf8c72d8e8d6.png)

## 本教程需要 iTools，可以到**[这里](http://itools.hk/tscms/ "iTools 下载")**下载

以下是解决方法：

1. 把设备通过 USB 连接到电脑
2. 打开 iTools，然后在左侧栏找到你的苹果移动设备下的【文件管理】，并在中间栏找到【越狱系统】
    
    [![](images/00001-02-f75b5116a70adc5c-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2012/01/00001-02-f75b5116a70adc5c.png)
3. 找到以下路径“/var/mobile/Library”中的“Keyboard”，右键【导出】保存起来（或把“Keyboard”从 iTools 拖到电脑的某个文件夹中）
    
    [![](images/00001-03-5cf05dbe058becb6-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2012/01/00001-03-5cf05dbe058becb6.png)
4. 找到以下路径“/var/root/Library”，然后【导入】【文件夹】选中刚刚上一步保存的“Keyboard”文件夹（或把“Keyboard”从电脑中直接拖到 iTools 这个文件夹中）
    
    [![](images/00001-04-0340151fdd2568dc-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2012/01/00001-04-0340151fdd2568dc.png)
5. 重新打开 Cydia 即可
    
    [![](images/00001-05-5c35ea2270c0a189-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2012/01/00001-05-5c35ea2270c0a189.png)
