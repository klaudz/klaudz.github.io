---
title: "Visual Studio编写C++控制台程序时去掉stdafx.h（取消预编译）"
date: "2011-12-01"
---

我们用Visual Studio 编写C++ 控制台程序的时候，必须加上 #include "stdafx.h"，否则默认情况下不能通过编译。 首先我们要了解stdafx.h 这个头文件有什么作用。

可以参考一下[维基百科（英文）](http://en.wikipedia.org/wiki/Precompiled_header#stdafx.h "预编译头文件（stdafx.h）")对stdafx.h 的解释。

stdafx 的全称是“STanDard Application Framework eXtensions”，用于头文件预编译。这个stdafx.h 文件包含了一些MFC 标准的头文件，而预编译则是事先把这些头文件编译好，工程不再对这些头文件进行编译，提高了编译速度，从而节省时间。

但如果是普通的控制台程序，不需要用到MFC 相关类库，就没有必要引用这个预编译头文件了。但Visual Studio 默认工程是要进行这项预编译的，如果没有引用stdafx.h 则不能通过编译。

以下通过用Visual Studio 2010 讲解如何关闭一个项目的预编译。

[![](images/00002-01-ee7cead0fa758518-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00002-01-ee7cead0fa758518.png)

[![](images/00002-02-24fa7745962f6b30-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00002-02-24fa7745962f6b30.png)

[![](images/00002-03-af915115fedd9d16-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00002-03-af915115fedd9d16.png)

[![](images/00002-04-6be5b7e72ef25940-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00002-04-6be5b7e72ef25940.png)

[![](images/00002-05-958d844e42249b32-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00002-05-958d844e42249b32.png)
