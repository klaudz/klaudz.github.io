---
title: "JavaScript数组对象的使用方法（三）——Array对象方法"
date: "2011-12-23"
---

前两节（[《JavaScript 数组对象的使用方法（一）——数组的声明、访问和遍历》](http://cloud018.sinaapp.com/archives/javascript-%e5%85%a5%e9%97%a8%e4%b9%8b%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%b8%80%ef%bc%89%e2%80%94%e2%80%94%e6%95%b0%e7%bb%84%e7%9a%84%e5%a3%b0/ "JavaScript 数组对象的使用方法（一）——数组的声明、访问和遍历")和[《JavaScript 数组对象的使用方法（二）——数组长度与关联数组》](http://cloud018.sinaapp.com/archives/javascript-%e6%95%b0%e7%bb%84%e5%af%b9%e8%b1%a1%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95%ef%bc%88%e4%ba%8c%ef%bc%89%e2%80%94%e2%80%94%e6%95%b0%e7%bb%84%e9%95%bf%e5%ba%a6%e4%b8%8e%e5%85%b3%e8%81%94/ "JavaScript 数组对象的使用方法（二）——数组长度与关联数组")）我们说一下 JavaScript 数组的基本使用方法。 这篇文章中，我们来探究一下常用的数组 Array 对象方法。

1. ### push、pop、shift、unshift
    
    (1) push 方法用于在数组的尾部追加一个或多个值，并返回当前数组的长度。
    
    \[js\] var members = new Array(); var n = 0;
    
    members.push("Indream"); // push 加入一个新元素 members.push("Tao", "BeyondXGB"); // push 加入多个新元素 n = members.push("Cloud018"); // push 返回值为当前数组的长度
    
    // 此刻数组的内容从下标0开始列举如下： // "Indream", "Tao", "BeyondXGB", "Cloud018"
    
    alert(members\[3\]); // 输出“Cloud018” alert(members.length); // 输出“4” alert(n); // 输出“4” \[/js\]
    
    (2) pop 方法把数组最后的元素删除，并返回这个被删除的元素。
    
    \[js\] // 续上面的代码。
    
    var last = null;
    
    alert(members\[3\]); // 输出“Cloud018” last = members.pop(); // 删除数组最后一个元素，并返回，保存在 last 中 alert(members\[3\]); // 由于 members\[3\] 以被弹出，输出 undefined
    
    // 此刻数组的内容从下标0开始列举如下： // "Indream", "Tao", "BeyondXGB"
    
    alert(last); // 输出“Cloud018” alert(members.length); // 输出“3” \[/js\]
    
    (3) shift 方法把数组第一个元素删除，并返回这个被删除的元素
    
    \[js\] // 续上面的代码。
    
    var first = null;
    
    alert(members\[0\]); // 输出“Indream” first = members.shift(); // 删除数组第一个元素，并返回，保存在 first 中 alert(members\[0\]); // 输出“Tao”
    
    // 此刻数组的内容从下标0开始列举如下： // "Tao", "BeyondXGB"
    
    alert(first); // 输出“Indream” alert(members.length); // 输出“2” \[/js\]
    
    (4) unshift 方法从数组的开头插入一个或多个元素，并返回当前数组的长度
    
    \[js\] alert(members\[0\]); // 输出“Tao” n = members.unshift("Indream", "Cloud018"); // 在数组头部插入两个元素，并返回当前数组长度 alert(members\[0\]); // 输出“Indream”
    
    // 此刻数组的内容从下标0开始列举如下： // "Indream", "Cloud018", "Tao", "BeyondXGB"
    
    alert(n); // 输出“4” alert(members.length); // 输出“4” \[/js\]
2. ### splice
    
    splice 方法用于插入、删除、替换数组元素 splice 方法的语法如下： arrayObject.splice(startIndex, deleteCount, replaceElements); 参数如下： (1) startIndex，必选，起始下标 (2) deleteCount，必选，删除个数 (3) replaceElements，可选，用一个或多个值来替换被删除的内容
    
    \[js\] var members = new Array( "Indream", "Tao", "BeyondXGB", "Cloud018" );
    
    alert(members); // 输出“Indream,Tao,BeyondXGB,Cloud018”
    
    members.splice(1, 2); // 删除从下标1开始的2个元素 alert(members); // 输出“Indream,Cloud018”
    
    members.splice(1, 0, "Tao", "BeyondXGB"); // 删除从下标1开始的0个元素，并替换为“Tao”和“BeyondXGB” // 就相当于是从下标1后插入“Tao”和“BeyondXGB” alert(members); // 输出“Indream,Tao,BeyondXGB,Cloud018” \[/js\]
3. ### sort
    
    sort 方法对数组的元素通过字符串比较方式进行排序
    
    \[js\] var members = new Array( "Indream", "Tao", "BeyondXGB", "Cloud018" );
    
    members.sort(); // 对 members 进行排序
    
    alert(members); // 输出“BeyondXGB,Cloud018,Indream,Tao” \[/js\]
    
    非常值得我们注意的是，sort 方法的排序方式是按照字符串的方式排序，所以会出现以下的情况。 对数值排序的问题，排序得出的结果是 1，10， 6，明显是按照字符串的方式排序得出的结果。
    
    \[js\] var numbers = new Array(1, 10, 6); numbers.sort();
    
    alert(numbers); // 输出“1,10,6” \[/js\]
    
    要解决这个问题，就要用到 sort 方法的参数，这个参数必须是一条函数，用来自定义排序的规则。 这个作为参数的函数又我们自定义，但这个函数有两个特点： (1) 函数有两个参数，就是 sort 排序时每一轮比较的的两个值 (2) 函数的返回值小于0，则 sort 把两个参数在数组中对调位置，否则位置不变。
    
    以下代码来进行数值的排序。
    
    \[js\] function sortNumbers(a, b) { return a - b; // 由于减法，所以这里已经强制转换为数值类型 }
    
    var numbers = new Array(1, 10, 6); numbers.sort(sortNumbers); // 为 sort 指定比较时调用的比较函数
    
    alert(numbers); // 输出“1,6,10” \[/js\]
4. ### reverse
    
    reverse 方法用于颠倒数组里的顺序。
    
    \[js\] var members = new Array( "Indream", "Tao", "BeyondXGB", "Cloud018" );
    
    members.reverse(); // 颠倒 members 的元素
    
    alert(members); // 输出“Cloud018,BeyondXGB,Tao,Indream” \[/js\]
