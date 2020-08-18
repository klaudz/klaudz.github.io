# iOS 11 短信拦截开发经验分享



## 前言

2017 年 6 月 5 日，苹果在 WWDC 2017 上正式公布了 iOS 11，并在当天面向开发者提供开发者测试版的下载。

在 iOS 11 中，Apple 向开发者开放了很多新的 SDKs 和 APIs，`IdentityLookup.framework` 便是其中之一。
开发者可以在应用中创建一个 **MessageFilter** 插件，实现实时的短信拦截功能。

与 iOS 10 的电话标记和拦截功能一样，短信拦截功能也是由腾讯手机管家与苹果合作的成果。（←_← 这绝对是一个广告。）
<img src="https://klaudz.me/articles/images/2017-12-08/01.png" alt="img" style="zoom:15%;" /><img src="https://klaudz.me/articles/images/2017-12-08/02.png" alt="img" style="zoom:15%;" />

广告已经得到曝光，那我们就赶紧进入正题吧，哈哈~



## 初窥

暂且把官方文档放一放，我们先使用 Xcode 9 创建一个 MessageFilter 插件，看看官方的模版里有何玄机。
<img src="https://klaudz.me/articles/images/2017-12-08/03.png" alt="img" style="zoom:50%;" />

创建 MessageFilter 插件后，可以看到项目中出现了插件对应的 target，和 3 个文件。
其中主要代码在 `MessageFilterExtension.m` 文件中。
（嗯，看来插件的开发不会太复杂。）
<img src="https://klaudz.me/articles/images/2017-12-08/04.png" alt="img" style="zoom:80%;" />

我们直接看 .m 文件，官方的模版代码如下。
<img src="https://klaudz.me/articles/images/2017-12-08/05.png" alt="img" style="zoom:50%;" />

从上面的模版，我们可以得到以下的信息：

1. 插件可以获得一个 `ILMessageFilterQueryRequest` 对象，里面包含：
   - **短信发送者的号码**
   - **短信内容**
2. 查询分为两种方式：
   - **本地（离线）查询**
   - **云端查询**
3.  可以给系统返回两种结果：
   - **允许**（Allow）
   - **拦截**（Filter）

现在，我们已经大致了解可以干什么了，下面再看一下系统的工作原理，看看我们该怎么干。



## 本地查询


<img src="https://klaudz.me/articles/images/2017-12-08/06.png" alt="img" style="zoom:80%;" />

1. **Query request**

   当 iOS 信息服务接收到一条陌生短信，则传送给 MessageFilter 插件。

2. **Decision**

  插件可以通过查询本地数据库，对当前的短信做一个判决，通过还是拦截，把结果告诉信息服务。

**Q&A**

1. Q：插件可以直接联网更新自己的数据库吗？
   A：不能。苹果考虑了用户的隐私问题，**插件不能直接联网**。
2. Q：那插件如何更新本地的数据库？
   A：插件可以读取 AppGroups 的目录，由**主 app 来更新数据库**，存放**在 AppGroups 目录进行共享**即可。
3. Q：插件的开发者能获得用户的短信内容吗？
   A：由于 iOS 的权限限制，在本地查询中，MessageFilter 插件无法联网，只能从 AppGroups 目录读数据，而**不能写数据**，因此**插件无法把短信数据发送给应用开发者**。



## 云端查询


<img src="https://klaudz.me/articles/images/2017-12-08/07.png" alt="img" style="zoom:80%;" />

1. **Query request**

   当 iOS 信息服务接收到一条陌生短信，则传送给 MessageFilter 插件。

2. **Defer request to server**

   插件此时通过云查接口，告诉系统信息服务，要通过云查的方式查询。

3. **Query request**

   系统服务很乖巧，立即把短信的相关数据发送到 app 关联的服务器（URL）进行查询。

4. **Response**

   服务器对查询请求进行响应，然后把查询的结果以二进制的数据形式返回给系统信息服务。

5. **Response**

   系统服务把服务器返回的数据透传给插件。

6. **Decision**

   插件得到了服务器返回的二进制数据，解析数据得到结果，然后告诉系统服务，通过还是拦截。

**Q&A**

1. Q：插件具有云查能力，苹果如何保障用户的隐私安全？
   A：首先，系统只会把陌生人的短信传送到插件进行查询，**不包括联系人**（发送者在用户通讯录中）**的短信**。
   A：其次，系统 SDK 限制了云查的能力，从上图可知，**app 不能直接向服务器发送网络请求**，**系统信息服务作为中介**，向 app 关联的服务器发送云查请求，系统只会把发送者和短信内容发送到服务器，插件不能增加其他的任何信息。因此，每条云查请求中，**开发者的服务器只能获得发送者和短信内容，而无法定位短信是来自于哪个用户的**。
2. Q：App 是怎样向 iOS 关联云查的服务器和 URL 的？
   A：苹果要求开发者在项目中设置 Associated Domains，通过关联域名、绑定 URL、证书验证等方式，iOS 的服务作为中介，向指定的 URL 发送云查请求。本文不对云查相关的客户端设置和服务器部署进行详述，有兴趣的同学可以查阅[官方文档](https://developer.apple.com/documentation/identitylookup/creating_a_message_filter_app_extension)。后面有时间我也可能会另起一文，对相关的细节和坑点进行详细解说。