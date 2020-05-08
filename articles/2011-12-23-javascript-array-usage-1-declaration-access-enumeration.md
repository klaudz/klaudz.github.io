---
title: "JavaScript数组对象的使用方法（一）——数组的声明、访问和遍历"
date: "2011-12-23"
---

写这篇文章的初衷是给 Ucity 技术部的新人学习的，因为学期末培训停止了，所以给大家补充一下营养。 这篇文章主要提供给 JavaScript 的初学者学习与交流，高手请勿吐槽~

首先字符串、数值、布尔值都属于离散值，这样的变量的某一时刻只可以表示一种值。当需要储存一组数据的时候，就需要用到数组（Array）了。 数组是多个值的集合，数组中的每一个值就是这个数组的元素。 以下用一组成员的名字的例子加以讲解。

1. ### 数组的声明
    
    声明数组需要用到 Array 关键字。 另外，数组对象拥有 length 属性，可以看到数组的长度，即元素个数。
    
    以下代码新建一个拥有4个元素的数组。
    
    \[js\] var members = new Array(4); alert(members.length); // 这里输入“4” \[/js\]
    
    有些时候，我们在声明数组对象的时候，并不知道以后会有多少个数组元素，所以在声明数组对象时可以不指定元素的个数。
    
    \[js\] var members = new Array(); alert(members.length); // 这里输入“0” \[/js\]
    
    我们还可以在初始化数据的时候就给数组赋上初值。
    
    \[js\] var members = new Array("Indream", "Tao", "BeyondXGB", "Cloud018"); alert(members.length); // 这里输入“4” \[/js\]
    
    另外，我们还可以用 JSON 的方式简便地声明数组。 什么是 JSON？希望了解请点击[这里](http://www.json.org/json-zh.html "JSON")。
    
    \[js\] var members = \[\]; alert(members.length); // 这里输入“0” \[/js\]
    
    当然，用 JSON 的方式声明数组时，也可以赋予初值。
    
    \[js\] var members = \["Indream", "Tao", "BeyondXGB", "Cloud018"\]; alert(members.length); // 这里输入“4” \[/js\]
2. ### 数组元素的访问
    
    数组的每一个元素都有一个位置存放数据，需要通过下标（index）来访问这个位置。 值得注意的是，下标从0开始计算，并非从1。
    
    以下代码展示数组元素的赋值与读取。
    
    \[js\] var members = new Array(4);
    
    members\[0\] = "Indream"; // 给第1个元素赋值 members\[1\] = "Tao"; // 给第2个元素赋值，以此类推 members\[2\] = "BeyondXGB"; members\[3\] = "Cloud018";
    
    alert(members.length); // 输出数组的长度 alert(members\[3\]); // 读取第4个元素并输出，“Cloud018” \[/js\]
    
    同样，在声明数组的时候就给数组赋初值的话，则按照顺序来分配下标。
    
    \[js\] var members = new Array("Indream", "Tao", "BeyondXGB", "Cloud018"); /\* 0 1 2 3 \*/
    
    alert(members\[3\]); // 读取第4个元素并输出，“Cloud018” \[/js\]
    
    JavaScript 的一个数组对象中，可以存放不同的类型，你可以将不同数据类型存放在不同的元素中，如数值、布尔值、字符串甚至对象。 但必须注意，一般情况下不建议在同一个数据放置不同的类型，因为这样的编码规范不便于阅读和存取数据，容易造成混乱。
    
    \[js\] var test = new Array(18, true, "Cloud018", new Array()); \[/js\]
3. ### 数组的遍历
    
    要遍历数组，可以通过数组的长度（length 属性）和 for 循环来进行。
    
    \[js\] var members = new Array( "Indream", "Tao", "BeyondXGB", "Cloud018" );
    
    // 从下标0开始，直到下标(length-1)，对数组进行遍历 for (var i = 0; i < members.length; i++) {
    
    alert(members\[i\]); // 输出下标为 i 的元素 } \[/js\]
    
    除此之外，我们还可以通过 for in 来遍历数组。
    
    \[js\] for (var i in members) { // 自动遍历 members 的下标，以此赋值给 i
    
    alert(members\[i\]); // 输出下标为 i 的元素 } \[/js\]

由于篇幅太长，所以本篇到此结束，点击[这里](http://cloud018.sinaapp.com/archives/javascript-%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%ba%8c%ef%bc%89%e2%80%94%e2%80%94%e6%95%b0%e7%bb%84%e9%95%bf%e5%ba%a6%e4%b8%8e%e5%85%b3%e8%81%94/ "JavaScript 数组对象的使用方法（二）——数组长度与关联数组")看一下节内容：[《JavaScript 数组对象的使用方法（二）——数组长度与关联数组》](http://cloud018.sinaapp.com/archives/javascript-%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%ba%8c%ef%bc%89%e2%80%94%e2%80%94%e6%95%b0%e7%bb%84%e9%95%bf%e5%ba%a6%e4%b8%8e%e5%85%b3%e8%81%94/ "JavaScript 数组对象的使用方法（二）——数组长度与关联数组")
