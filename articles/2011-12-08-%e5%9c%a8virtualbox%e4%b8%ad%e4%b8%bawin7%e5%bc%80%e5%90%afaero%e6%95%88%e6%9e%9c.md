---
title: "在VirtualBox中为Win7开启Aero效果"
date: "2011-12-08"
---

最近在VirtualBox 中安装了Windows 7，安装完后发现Aero 特效无法成功开启。 我的是VirtualBox 4.1.6，Win7 虚拟机也安装了增强工具，但却无法完美切换Aero 主题，如下图。 [![](images/00006-01-6d49e5decfe93434-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-01-6d49e5decfe93434.png)

[![](images/00006-02-26eb6e9e09d48f32-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-02-26eb6e9e09d48f32.png)

后来google 后发现需要设置一些地方，同样安装增强工具的时候也要注意一些步骤，否则将无法开启Aero。

以下是详细步骤：

1. 首先必须确认VirtualBox 的版本在4.1.0 以上，因为早期版本并不支持Win7 Aero。
2. 一开始当然是在VirtualBox 新建虚拟机并安装Windows 7 系统（新建和安装的步骤略） **重点：新建好虚拟机后必须先配置显存，显存至少为128M，并且勾选“启用3D加速”（2D随意）**
    
    [![](images/00006-03-258d036c87128bfa-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-03-258d036c87128bfa.png)
    
    [![](images/00006-04-75c3dece66262610-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-04-75c3dece66262610.png)
3. 配置完毕后，启动Win7 虚拟机。
4. 然后安装增强功能。
    
    [![](images/00006-05-1512ef5992a69b29-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-05-1512ef5992a69b29.png)
5. 安装增强功能的过程中，基本按下一步即可。 **但在选择组件（Choose Components）的时候就需要注意了。** **选择Direct3D Support 的时候，会弹出一个对话框，这里一定要点击否，否则将无法开启Win Aero。**
    
    [![](images/00006-06-212030913927e8ba-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-06-212030913927e8ba.png)
    
    [![](images/00006-07-a772f11c682e0b58-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-07-a772f11c682e0b58.png)
    
    [![](images/00006-08-b29b1c7f1c4f2f98-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-08-b29b1c7f1c4f2f98.png)
    
    [![](images/00006-09-5123b13e7211430f-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-09-5123b13e7211430f.png)
6. 安装增强功能完成后，重启虚拟机。
    
    [![](images/00006-10-a19cdaff76288233-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-10-a19cdaff76288233.png)
7. 重启进入Win7 后，再重新设置一下带Aero 的主题，即可开启Aero 特效。 [![](images/00006-11-bd9da73646f8eab8-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00006-11-bd9da73646f8eab8.png)
