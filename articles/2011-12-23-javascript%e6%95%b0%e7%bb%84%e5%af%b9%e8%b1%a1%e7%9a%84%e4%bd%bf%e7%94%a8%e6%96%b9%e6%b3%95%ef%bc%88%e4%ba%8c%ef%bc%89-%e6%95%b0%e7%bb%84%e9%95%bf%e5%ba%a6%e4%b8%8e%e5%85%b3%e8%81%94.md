---
title: "JavaScript数组对象的使用方法（二）——数组长度与关联数组"
date: "2011-12-23"
---

在上一篇文章[《JavaScript 数组对象的使用方法（一）——数组的声明、访问和遍历》](http://cloud018.sinaapp.com/archives/javascript-%e5%85%a5%e9%97%a8%e4%b9%8b%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%b8%80%ef%bc%89%e2%80%94%e2%80%94%e6%95%b0%e7%bb%84%e7%9a%84%e5%a3%b0/ "JavaScript 数组对象的使用方法（一）——数组的声明、访问和遍历")中，我介绍了 JavaScript 中数组的简单用法。 本篇将继续对数组进行更深入一点的介绍。

1. ### 数组长度的增加
    
    JavaScript 中增加数组长度的方法极其简单，只要用更大的下标就可以对相应的小标进行赋值，并增加长度，看以下代码。
    
    \[js\] var members = new Array( "Indream", "Tao", "BeyondXGB" );
    
    alert(members.length); // 输出“3” members\[3\] = "Cloud018"; // 插入第4个元素 alert(members.length); // 输出“4” members\[9\] = "The 10th"; // 插入第10个元素 alert(members.length); // 输出“10”
    
    for (var i = 0; i < members.length; i++) { alert(members\[0\]); /\* 前四依次输出“Indream”、“Tao”、“BeyondXGB”、“Cloud018” 然后连续输出5个 undefined，因为中间的下标位置未赋值 最后输出“The 10th” \*/ } \[/js\]
    
    从代码中可以看出，我们只需通过下标赋值即可，如果下标超出当前数组长度，则数组的长度会被自动增加。
    
    其实数组对象还有 push 等方法，现在暂不介绍，在下面的文章内容中会介绍到。
2. ### 关联数组
    
    数组的传统方式都是通过下标来对元素进行访问，但 JavaScript 中有另一种方式来访问数组内容，就是关联数组。 我们可以通过自定义的字符串来对数组的元素进行访问。 看下面的代码。
    
    \[js\] var members = new Array();
    
    members\[0\] = "test"; alert(members.length); // 输出“1”
    
    // 下面是以关联数组的方式赋值 members\["CEO"\] = "Indream"; members\["TCE"\] = "Cloud018"; members\["TE-1"\] = "Tao"; members\["TE-2"\] = "BeyondXGB";
    
    alert(members.length); // 关联数组是不占长度的，这里输出“1” alert(members\["TCE"\]); // 输出“Cloud018” \[/js\]
    
    这里必须注意的是，关联数组的元素并不占用数组的长度（length），所以上面代码最后还是输出“1”，也意味着关联数组的元素不能被遍历，但非关联的元素还是可以遍历出来的。

下一节，也就是最后一节，将介绍一下常用的 Array 对象方法，请点击[《JavaScript 数组对象的使用方法（三）——Array 对象方法》](http://cloud018.sinaapp.com/archives/javascript-%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%b8%89%ef%bc%89%e2%80%94%e2%80%94array-%e5%af%b9%e8%b1%a1%e6%96%b9%e6%b3%95/ "JavaScript 数组对象的使用方法（三）——Array 对象方法")。
