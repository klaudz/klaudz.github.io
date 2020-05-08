---
title: "Ubuntu下gedit中文乱码的解决方法"
date: "2011-12-07"
---

有的文本文件，例如txt文本、歌词文件、字幕文件，在Windows下能正常浏览，但在Ubuntu 下用gedit 打开却乱码，这是由于文件编码问题。 可以通过以下方法解决：

1. 在终端打开gconf-editor，这是打开了配置编辑器；
2. 在左边栏找到 '/apps/gedit-2/preferences/encodings/auto\_detected' 这个键值；
3. 双击auto\_detected，就会弹出“编辑键”的输入框，然后添加 'GBK'，并把其移动最上，确定即可。

[![](images/00005-01-24d52a75c5da98bf-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00005-01-24d52a75c5da98bf.png)

现在再重新打开gedit，就可以正常浏览之前显示乱码的中文文本文件了~~~
