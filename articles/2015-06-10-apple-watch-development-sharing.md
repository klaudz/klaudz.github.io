# Apple Watch 开发经验分享



又一个迭代结束，终于腾出了时间来写一下关于 Apple Watch 开发的分享。
闲话休题，马上开始第一个环节——晒婊！
<img src="https://klaudz.me/articles/images/2015-06-10/01.jpg" alt="img" style="zoom:33%;" />

晒命完毕，言归正传。



## 几个基础问题

首先针对几个基础的问题来自问自答。

### 1. WatchKit 是什么鬼？

WatchKit 是 Apple 提供给开发者用于开发 Apple Watch 应用（WatchKit App）的 framework。
WatchKit 于 14 年 11 月 19 日跟 Xcode 6.2 Beta、iOS 8.2 Beta 一起问世，集成在 Xcode 6.2 或以上版本的 SDK 中。

### 2. 开发 Watch App 需要什么？

首先，必备

- 一台 Mac 电脑（当然你可以黑苹果或虚拟机，但。。）， Mac OS X 10.10 或以上
- Xcode 6.2 或以上

这样，我们已经可以在 Mac 上通过模拟器开发 Watch App 了。
<img src="https://klaudz.me/articles/images/2015-06-10/02.png" alt="img" style="zoom:30%;" /><img src="https://klaudz.me/articles/images/2015-06-10/03.png" alt="img" style="zoom:30%;" /> 

如果你需要在 iOS 设备上开发和调试 WatchKit，那么你必须有

- 一台 iPhone，iOS 8.2 或以上，iPhone 5 或以上

- 一只苹果婊（洋名 Apple Watch）

- 一个 iOS 开发者帐号（加上上面的设备 UDID，配置一堆相关的开发证书，这里略过）

<img src="https://klaudz.me/articles/images/2015-06-10/04.png" alt="img" style="zoom:20%;" /><img src="https://klaudz.me/articles/images/2015-06-10/05.png" alt="img" style="zoom:40%;" /><img src="https://klaudz.me/articles/images/2015-06-10/06.png" alt="img" style="zoom:30%;" />

### 3. WatchKit 怎么工作？

首先看一幅图。
<img src="https://klaudz.me/articles/images/2015-06-10/07.png" alt="img" style="zoom:40%;" />

然后再看一幅图。
<img src="https://klaudz.me/articles/images/2015-06-10/08.png" alt="img" style="zoom:30%;" />

最后看一幅图。
<img src="https://klaudz.me/articles/images/2015-06-10/09.png" alt="img" style="zoom:50%;" />

从上面的图可以看出，

1. 目前我们开发的 WatchKit App 并非 Watch 上的 Native App，其本质上一个 App Extension（插件），所以不能单独存在，必须依赖 iOS App。 如果对 App Extension 开发不了解，可以参考我此前写的文章[《iOS 8 Widget 开发经验分享》](https://klaudz.me/?p=299)。

2. WatchKit App 在 Apple Watch 上，只负责存放着与 UI 相关的 Storyboard 和资源文件。

3. iOS App 和 WatchKit Extension 在 iPhone 上，其中 WatchKit Extension 负责给 WatchKit App 执行代码逻辑，并与其进行交互（处理事件和更新界面）。

4. 代码跑在 iPhone 的 WatchKit Extension，意味着 AppGroups 是可用的。关于 AppGroups，也可以参考[《iOS 8 Widget 开发经验分享》](https://klaudz.me/?p=299)。
   所以，WatchKit App 其实只是一个外壳，真正在幕后默默耕耘的是在 iPhone 上的 WatchKit Extension。

### 4. WatchKit App 能让 Watch 干什么？

然而并不能干什么，至少目前是。

上面提到了，我们现在开发的并非 Watch 上的 Native App，而是 iPhone 上的插件，Watch 只是给我们的 App 展示 UI。
因此，代码调用的 API 都由 iOS 提供，而非 Watch OS。我们并不能读取 Watch 的任何信息，也不能使用 Watch 上的任何功能。
<img src="https://klaudz.me/articles/images/2015-06-10/10.png" alt="img" style="zoom:30%;" />

最近有消息称，即将在 6 月举行的 WWDC15，会更新 WatchKit，开发者可以开发 Native Watch App。孰真孰假，也只能坐着这里等了。

### 5. WatchKit App 分为哪几个部分？

1. **主 App**
   主 App 就是主 App 咯~
   <img src="https://klaudz.me/articles/images/2015-06-10/11.png" alt="img" style="zoom:40%;" /><img src="https://klaudz.me/articles/images/2015-06-10/12.png" alt="img" style="zoom:40%;" />
2. **Glance**
   Glance 是 Apple Watch 当中的一种快捷视图功能，它能将应用当中的重要信息提取出来，并以简明的形式呈现。
   <img src="https://klaudz.me/articles/images/2015-06-10/13.png" alt="img" style="zoom:40%;" />
   - **基于模板**。Glance 界面的上下两部分有各自独立的模板。你可以在 Xcode 当中挑选合适的模板，并按照相应的规格设计你的内容。
   -    **不可滚动**。所有信息都要集中呈现在一屏当中。
   -    **只读**。轻点 Glance 界面当中的任何地方都会打开相应的应用。
   -    **非强制**。不是所有的应用都需要 Glance 视图，用户可以自主选择在 Glance 中显示哪些应用的信息。
3.  **Notification**
  作为快速、轻量的互动功能，Apple Watch 上的通知由两部分组成：
  - **Short Look**
    <img src="https://klaudz.me/articles/images/2015-06-10/14.png" alt="img" style="zoom:40%;" />
  - **Long Look**
    <img src="https://klaudz.me/articles/images/2015-06-10/15.png" alt="img" style="zoom:40%;" />



## WatchKit App 架构

### 1. WatchKit App 的启动

当一个 WatchKit app 在 Apple Watch 启动，先加载 Storyboard 对应的 Interface Controller 入口，初始化 UI，
然后与 iPhone 上的 WatchKit Extension 进行通讯，后者执行代码，Interface Controller 依次初始化（init）、唤醒（awakeWithContext:）、激活（willActivate）。
<img src="https://klaudz.me/articles/images/2015-06-10/16.png" alt="img" style="zoom:40%;" />

### 2. Interface Controller 的生命周期

当用户要打开某个页面时，WatchKit App 需要展示某个页面的 UI，对应 WatchKit Extension 中的 Interface Controller 会执行 willActivate。
用户在操作 UI 时，Interface Controller 执行相应的响应方法。

当用户停止当前页面的操作，如 Apple Watch 屏幕关掉、切换到下个页面等，对应的 Interface Controller 会执行 didDeactivate。

如果用户退出 WatchKit App，或 Apple Watch 锁屏，WatchKit Extension 的进程将被挂起。
<img src="https://klaudz.me/articles/images/2015-06-10/17.png" alt="img" style="zoom:40%;" />



## 创建一个 WatchKit App —— Hello WatchKit

下面，我们即将开始开发第一个苹果表应用，并与大家一起去探索 WatchKit 的开发。

1. **Container App**
   首先要有一个 Container App（iOS 主 App）的 project，此处省略一万字。 
   这里，我新建了一个名为“HelloWatchKit”的 project。
<img src="https://klaudz.me/articles/images/2015-06-10/18.png" alt="img" style="zoom:60%;" />
   
2. **新建 WatchKit Target**
   给 project 新建一个 WatchKit App 的 Target。
   <img src="https://klaudz.me/articles/images/2015-06-10/19.png" alt="img" style="zoom:30%;" />
   下一步，对 WatchApp 的 Target 进行配置。
   <img src="https://klaudz.me/articles/images/2015-06-10/20.png" alt="img" style="zoom:30%;" />

3. **创建 Target**
   此时，project 上多了两个目录，一个是 WatchKit App 的目录，一个是 WatchKit Extension 的目录。
   <img src="https://klaudz.me/articles/images/2015-06-10/21.png" alt="img" style="zoom:30%;" />
   如上所述，WatchKit 分为 App 和 Extension 两部分，App 仅仅装载着 StoryBoard 和资源文件，Extension 包含代码实现（主 App、Glance、Notification 等）。

   WatchKit App 的 StoryBoard 被创建，包含了主 App、Glance、Notification 的界面设计。
   <img src="https://klaudz.me/articles/images/2015-06-10/22.png" alt="img" style="zoom:30%;" />
   同时可以看到 Build Scheme 多了三项，WatchKit App、Glance、Notification。
   <img src="https://klaudz.me/articles/images/2015-06-10/23.png" alt="img" style="zoom:30%;" />

4. **添加控件**
   简单地在主界面上增加一个 Label。Interface Builder 的具体操作在此就不细说了。
   <img src="https://klaudz.me/articles/images/2015-06-10/24.png" alt="img" style="zoom:40%;" />

5. **跑在模拟器**
   选择 WatchKit App 和 iPhone 6 模拟器，然后“Run”（Command+R）。
   <img src="https://klaudz.me/articles/images/2015-06-10/25.png" alt="img" style="zoom:50%;" />

   编译完成后，Simulator 弹出。
   如果你只看到 iPhone 6 的模拟器，而不见 Apple Watch 模拟器，在 Simulator 的菜单 Hardware -> External Displays 可以调出 Apple Watch 模拟器，可以选择 38mm 和 42mm 的模拟器。
   <img src="https://klaudz.me/articles/images/2015-06-10/26.png" alt="img" style="zoom:30%;" /><img src="https://klaudz.me/articles/images/2015-06-10/27.png" alt="img" style="zoom:20%;" />



## UI 设计

UI 界面设计主要集中在 WatchKit App。

### 1. 图标

<img src="https://klaudz.me/articles/images/2015-06-10/28.png" alt="img" style="zoom:30%;" />

### 2. 屏幕尺寸


<img src="https://klaudz.me/articles/images/2015-06-10/29.png" alt="img" style="zoom:30%;" />

### 3. UI 控件

WatchKit 现有的 UI 控件屈指可数。

除去前 3 个 Interface Controller（Base，Glance，Notifcation），常用控件只有 11 个（Label、Button、Image、Switch、Slider、Table、Menu 等）。
<img src="https://klaudz.me/articles/images/2015-06-10/30.png" alt="img" style="zoom:30%;" />

在 Interface Builder 可以编辑这些控件的属性。
以下是一个 Label 的可编辑项。
<img src="https://klaudz.me/articles/images/2015-06-10/31.png" alt="img" style="zoom:30%;" />

点击“+”号，可以针对不同的屏幕尺寸对某个属性进行设置。
<img src="https://klaudz.me/articles/images/2015-06-10/32.png" alt="img" style="zoom:30%;" />

### 4. UI 控件接口

提到 UI 控件的接口，也不得不吐槽其“简约”。。

首先看看 `WatchKit.framework` 的头文件。
Watch 上的 UI 方面是完全脱离 UIKit 的，WatchKit 提供了所有 UI 控件的支持，所有 UI 控件均以 WKInterface 为 Prefix。
<img src="https://klaudz.me/articles/images/2015-06-10/33.png" alt="img" style="zoom:50%;" />
<img src="https://klaudz.me/articles/images/2015-06-10/34.png" alt="img" style="zoom:40%;" />

所有控件都以 `WKInterfaceObject` 为父类。
<img src="https://klaudz.me/articles/images/2015-06-10/35.png" alt="img" style="zoom:40%;" />

而 klaudz 觉得一个值得吐槽的点是，属性值只提供了 setter，没有 getter。。。
而且 setter 的接口也是少之又少。
我们看看 `WKInterfaceLabel` 和它父类 `WKInterfaceObject` 的所有 UI 接口。

`WKInterfaceLabel`
<img src="https://klaudz.me/articles/images/2015-06-10/36.png" alt="img" style="zoom:40%;" />

`WKInterfaceObject`
<img src="https://klaudz.me/articles/images/2015-06-10/37.png" alt="img" style="zoom:40%;" />

可以看到，`WKInterfaceLabel` 只有 3 个 setter 方法，`WKInterfaceObject` 的 setter 方法也只有 `hidden`、`alpha`、`width`、`height`，没了。
连当前 `frame`、`text` 等基础属性的值都不能取得。。。

### 5. 布局

首先说明两点。
一，目前 WatchKit 不提供 `-setFrame:` 的接口，
二，没有 `-addSubview:` 和 `-subviews` 之类的接口。

好了，在 Storyboard 上，WatchKit App 的布局有 2 种，水平和竖直。
<img src="https://klaudz.me/articles/images/2015-06-10/38.png" alt="img" style="zoom:50%;" />

而对于每种布局的位置，可以设置“居左/上”、“居中”、“居右/下”3 种。
<img src="https://klaudz.me/articles/images/2015-06-10/39.png" alt="img" style="zoom:50%;" />

`WKInterfaceGroup` 是一个控件容器，可以容纳多个子控件在其中布局。
其他控件都是按照容器的布局进行布局，不能重叠。
以下是 4 个 Labels 按竖直方向的布局，每个 Label 的位置都为居上。
<img src="https://klaudz.me/articles/images/2015-06-10/40.png" alt="img" style="zoom:40%;" />



## 其他注意点与坑

### 1. 开发者帐号加入设备

如果需要在真机上 debug WatchKit App，需要把 Apple Watch 和配对它的 iPhone 的 UDID 都加入到开发者帐号的设备名单中。
所以，这样就占用两个设备的位置。

### 2. Lost Connection

klaudz 刚开始时老是 debug 不了，提示 Lost Connection，搜索了很多问题和问了一些朋友，未果。
<img src="https://klaudz.me/articles/images/2015-06-10/41.png" alt="img" style="zoom:30%;" />

后来发现是项目配置问题，Xcode 6.3 新建 WatchKit 的 Targets 时，`Valid Architectures` 只有 `armv7`，没有 `arm64`，导致 debug 不到。
自行加上即可。
<img src="https://klaudz.me/articles/images/2015-06-10/42.png" alt="img" style="zoom:30%;" />

### 3. iOS Deployment

`iOS Deployment Target` 需设置 `iOS 8.2` 或以下。
Xcode 6.3.2（iOS 8.3 SDK）新建项目时，`iOS Deployment Target` 默认是 `iOS 8.2`，但编译会报错。。。
说 WatchKit 需要支持 iOS 8.2，听他的吧，`iOS Deployment Target` 设置到低于 `iOS 8.2`（含）的版本。
<img src="https://klaudz.me/articles/images/2015-06-10/43.png" alt="img" style="zoom:40%;" />

### **4. 版本号设置**

主 App、WatchKit App、WatchKit Extension 的版本号（Version、Build Version）都要设置到一致，否则编译时会弹出错误。

### **5. 能做动画吗？**

能，然而并不能用 `CoreAnimation`。
目前 WatchKit 不支持 `CoreAnimation`，也是非 Native App 的原因。
所以如果想要做动画，只能用 `CoreGraphics` 把动画的每一帧画成 `UIImage`，然后在 Image 控件上刷新。

### **6. 提交审核的图标和截图**

提交审核的图标和截图需要没有 Alpha 通道，这是 cyan 告诉我的。
Apple Watch 上的截图，是带 Alpha 通道，直接提交，会被打回。
更新：现在貌似提交带 Alpha 通道的截图似乎又可以了。Apple’s bug？



## 补充说明

此文写于 WWDC15 之前，故分享的是 watchOS 1.x 开发的经验，开发背景上 watchOS 2.0 为未知之数。