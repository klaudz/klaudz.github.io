---
title: "JavaScript用cloneNode方法克隆节点"
date: "2011-12-03"
---

很多时候我们需要通过HTML DOM 的方式，用JavaScript 动态生成很多相同的节点，包括其子节点。 很多时候我们会用for 来生成多个结构相同的节点结构，这样我们需要写很多createElement、setAttribute、appendChild 等代码。 但其实我们只需要有一个html 的模板，就可以用cloneNode 方法对已有的节点进行克隆，包括其子节点。

以下是cloneNode 方法原型：

\[c\] newElement oldElement.cloneNode(bool deep); \[/c\]

这个方法只有一个参数deep，布尔值，如果为true，则克隆oldElement 这个及其子节点，否则只可能这个节点本身。 返回值就是一个克隆的节点newElement。

\====================华丽丽的分割线====================

以下是测试代码，test.htm 和test.js 文件。

\[html\] <!-- test.htm -->

<html> <head> <title>Test of cloneNode Method</title> <script type="text/javascript" src="test.js"></script> </head> <body> <div id="main"> <div id="div-0"> <span>Cloud018 said, </span> <span>"Hello World!!!"</span> </div> </div> </body> </html> \[/html\]

\[js\] // test.js

window.onload = function () {

var sourceNode = document.getElementById("div-0"); // 获得被克隆的节点对象

for (var i = 1; i < 5; i++) { var clonedNode = sourceNode.cloneNode(true); // 克隆节点 clonedNode.setAttribute("id", "div-" + i); // 修改一下id 值，避免id 重复 sourceNode.parentNode.appendChild(clonedNode); // 在父节点插入克隆的节点 } } \[/js\]

网页加载的结果如下：

[![](images/00003-01-53a09ef02a93452e.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00003-01-53a09ef02a93452e.png)

用Google Chrome 的开发人员工具可以看出，div-0 的节点结构都被复制了。

[![](images/00003-02-db2c0e3b0875f0b2-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00003-02-db2c0e3b0875f0b2.png)

\====================华丽丽的分割线====================

而当把cloneNode 的deep 参数设为false 的时候，仅仅div-0 这个节点本身被克隆，而他的子节点（即其内容）是没有被复制的。

\[js\] var clonedNode = sourceNode.cloneNode(false); \[/js\]

[![](images/00003-03-7cdc96d352029fb2-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00003-03-7cdc96d352029fb2.png)
