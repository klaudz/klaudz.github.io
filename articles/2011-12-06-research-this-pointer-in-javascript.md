---
title: "进一步理解JavaScript中类的this指针"
date: "2011-12-06"
---

刚刚CYY（新名[Winguse](http://winguse.sinaapp.com/ "Winguse's Blog")...） 问了一个JS 的问题，如下图的代码，其中在jQuery 的post 方法中，作为callback 的匿名函数中调用Winguse 类id 属性，输出undefined。但在Winguse 类中的foo() 函数却可以成功输出id 的值。 [![](images/00004-01-b1238e9572efbe05-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00004-01-b1238e9572efbe05.png)

对这个问题，是因为对this 的理解不当引起的问题。于是写了一个类似上面代码的测试代码，代码中已经包含了注释。

\[js\] var test = new Cloud018(); test.foo();

function Cloud018() {

this.id = "Cloud018"; // Cloud018 类的id

this.foo = function () { alert("foo, this.id = " + this.id); } // foo() 函数是Cloud018 类的成员函数 // 所以这里的this 是Cloud018 类的对象，输出"Cloud018"。

post(function () { alert("post, this.id = " + this.id); }); // 这里的匿名函数，作为参数传给了类外的post 函数（或者说是“类”） // 类似于jQuery post 方法中的 callback // 这里的this 被看作是post 类的对象，因此这里输出"post"。 }

function post(callback) {

this.id = "post"; // post 函数（或者说是“类”）的id

callback(); }

/\* 由此可知，以上代码，依次输出： post, this.id = post foo, this.id = Cloud018 \*/ \[/js\]

这就是为什么上面CYY 的代码中无法正确输出id 的原因。 $.post 方法的第三个参数中的匿名函数，里面的this.id 中的this 已经不是Winguse 类的对象，因此id 也不是其属性了，故输出undefined。

\===================================华丽丽的分割线===================================

后来讨论到怎么解决无法访问原对象的数据的问题，想到了一种这样的方法。 因为为了保持原代码的基本结构，而用以下方法仅仅解决了问题，但未必是最优的。 在下面的示例代码，已经尽量降低post 对Cloud018 的依赖，降低耦合程度。

\[js\] var test = new Cloud018(); //test.foo();

function Cloud018() {

this.id = "Cloud018"; // Cloud018 类的id

/\* this.foo = function () { alert("foo, this.id = " + this.id); } \*/

// 把Cloud018 类对象的this 指针作为参数传出 post(this, function () { alert("post, this.obj.id = " + obj.id); // post 类中保存的obj，见下面function post() });

}

function post(obj, callback) {

this.obj = obj; // 保存obj 为类成员

callback(); } \[/js\]
