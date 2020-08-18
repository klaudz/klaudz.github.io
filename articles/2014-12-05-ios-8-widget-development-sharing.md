# iOS 8 Widget 开发经验分享



Extension 开发是 iOS 8 和 Mac OS X 10.10 新增的开发功能，Widget 是其中的一种 Extension。

Widget 又名 Today Extension（今天插件），是在通知中心“今天”栏展示的插件。
用于快速展示简单的任务或视图。 我们可以在 Widget 中展现一些数据，例如天气、流量、网速等等。
下图为手机管家 iOS AppStore v5.0 的 Widget 截图。
<img src="https://klaudz.me/articles/images/2014-12-05/01.png" alt="img" style="zoom: 30%;" />

下面分享一下 Widget 插件开发的一些经验。



## 前言

本文仅分享 Widget 开发的一些注意点，以及开发过程中取得的经验。
如需系统的入门教程，可以参考苹果文档，文末也分享一些优质的传送门。



## 一、一些名词

### 1. Container App

字面意思就是容器应用，即装着插件的应用，通俗点称呼其为主 App 吧，也就是我们自己开发的 App。
Extension 会嵌入于 Container App 的安装包中，Container App 安装时 Extension 也会一并安装。

### 2. Extension

插件，就是插件咯。。。
也是我们自己开发的，嵌入在 Container App 中。（在 Project General 中可以看到）
<img src="https://klaudz.me/articles/images/2014-12-05/02.png" alt="img" style="zoom: 40%;" />

Extension 的 Bundle ID 一般以 Container App 的 Bundle ID 为前缀，在其后再加字段。
例如你的主 App 的 Bundle ID 为 com.tencent.xxx，那么其 Widget 的 Bundle ID 可以为 com.tencent.xxx.today。

### 3. Host App

宿主 App，即把插件跑起来的应用。
例如 Widget 的 Host App 是 Today（通知中心的“今天”），Photo Editing Extension（图片编辑插件）的 Host App 是 Photos（系统的照片 App）。
那 Custom Keyboard 的 Host App 呢？当然是当前使用这个 Keyboard 插件的应用啦。
<img src="https://klaudz.me/articles/images/2014-12-05/03.png" alt="img" style="zoom: 60%;" />

在 Debug Extension 的时候可以选择 Host App 进行调试。

图中显示的是三者的关系。
<img src="https://klaudz.me/articles/images/2014-12-05/04.png" alt="img" style="zoom: 33%;" />

其中 Extension 和 Host App 是直接通讯的。
而 Extension 和 Container App 虽然同在一个安装包，运行时却是相互独立的进程，两者可以在共享的数据区（AppGroups）共享数据。



## 二、数据共享（AppGroups）

AppGroups 始于 Mac OS X 10.7.5、Mac OS X 10.8.3 和 iOS 7，用于多 App、多 Extension 之间的数据共享。
必须有相同 AppGroup 证书的 App 和 Extension 才可以访问指定的 AppGroup。

AppGroup 的 Bundle ID 一般以“group”为前缀，加上主 App 的 Bundle ID。
例如你的主 App 的 BundleID 为 com.tencent.xxx，那么其 AppGroup 的 Bundle ID 建议为 group.com.tencent.xxx。

以下是两种 AppGroups 常用的访问方式：

### 1. NSUserDefaults

```objc
+ [NSUserDefaults standardUserDefaults];
```

在 App 或 Extension 沙箱内的 `NSUserDefaults` 单例，位于其沙箱内的 Librariy。

```objc
- [NSUserDefaults initWithSuiteName:];
```

取得 AppGroup 共享的 `NSUserDefaults` 实例，`suiteName` 对应目标 AppGroup 的 Bundle ID。

### 2. 文件共享

```objc
- [NSFileManager containerURLForSecurityApplicationGroupIdentifier:]; 
```

取得 AppGroup 的文件目录的绝对路径，GroupIdentifier 对应目标 AppGroup 的 Bundle ID。



## 三、生命周期

Widget 的生命周期主要需要关注两个点，Extension 进程和主 UIViewController。
<img src="https://klaudz.me/articles/images/2014-12-05/05.png" alt="img" style="zoom: 50%;" />

### 1. Extension 进程

当用户选择了显示 Widget，Today 将启动 Widget 的进程。
Widget 在 Today 中展示时，进程处于活动状态。
Today 或 Widget 不被展示时，进程将被挂起，一动不动，但不会被立刻关掉，可能长时间待命。
当 Widget 不需要被展示，且系统需要内存，或其他一些情况下，系统才会通知 Today 把 Widget 的进程关掉。
下次用户有需要展示 Widget 时 Today 再启动 Widget 的进程。

### 2. 主 UIViewController

Extension 的 UI 入口都是一个 `UIViewController`，在 `Info.plist` 中的 NSExtension 中可以设置。

对于 UI，当 Widget 需要展示时，会创建一个 `UIViewController` 的实例。
Widget 消失时此实例会立即被回收（`-dealloc`）。
当下次展示时再创建一个新的实例。
<img src="https://klaudz.me/articles/images/2014-12-05/06.png" alt="img" style="zoom: 40%;" />



## 四、UI 那些事儿

Widget 用于快速展示简单的任务或视图，原则上是做得简洁。

### 1. 原则

- 确保展现的内容看起来都是最新的。
- 适当地响应用户的交互。
- 表现良好。 这里尤其要注意，应该要明智地使用内存，否则系统可能会把它 kill 掉。

### 2. Today 模板

在 Xcode 模版中创建一个 Today 插件。
<img src="https://klaudz.me/articles/images/2014-12-05/07.png" alt="img" style="zoom: 30%;" />

Today 模板默认是使用 StoryBoard 的，在 `info.plist` 中的 `NSExtensionMainStoryboard` 指向 StoryBoard 文件。

如果你不希望使用 StoryBoard，而用代码取而代之，那么把 `NSExtensionMainStoryboard` 改为 `NSExtensionPrincipalClass`，并把值改为你的 `UIViewController` 子类的类名即可。（如［三.2］的图）

### 3. 一些主要的 UI 方法

```objc
- [UIViewController<NCWidgetProviding> widgetMarginInsetsForProposedMarginInsets:];
```

 子类实现，用于设置 Widget 外边距（Margin）。
根据 iOS 8 通知中心的设计，Widget 有默认的外边距，左边距和下边距。

```objc
@interface UIViewController
@property (nonatomic) CGSize preferredContentSize;
@end
```

用于设置 Widget 的大小。
注意，Widget 不能通过修改 `UIViewController` 的 `view.frame` 来实现调整大小。 

```objc
- [UIViewController<NCWidgetProviding> widgetPerformUpdateWithCompletionHandler:];
```

子类实现，用于更新 UI。
Today 维护了 Widget 的 snapshot，通过此方法可以在 iOS 系统读取 snapshot 前告知 Today 是否读取 snapshot 还是直接更新 UI。

- `NCUpdateResultNewData`：widget 有新的内容，请求重绘界面。
- `NCUpdateResultNoData`：widget 不需要更新界面。
- `NCUpdateResultFailed`：widget 更新数据时发生了错误。



## 五、经验分享

在两个迭代的开发中，我们积累了以下的一些值得注意的经验，特此分享。

### 1. 进程与界面

Widget 的进程由通知中心的 Today 管理，常驻内存。

Widget 的 UI（`UIViewController`）会经常销毁和在必要是重新创建。
甚至在拖动通知中心，Widget 的主 `UIViewController` 都可能会 `-dealloc` 和重新 `-init`。

### 2. 编译架构

Extension 必须支持 arm64 的 CPU 架构，以确保在不同机型都可以正常运行。
如果 Extension 只支持 armv7(s) 而不支持 arm64，则在 CPU 架构为 arm64 的设备（如 iPhone 5s）上，可以正常安装，但插件不会被正常显示。

### 3. openURL

在 Extension 中没有 `UIApplication`，如果要在插件中响应用户的交互来打开 App，需要使用 `UIViewController` 实例的 `extensionContext` 的 `- [NSExtensionContext openURL:completionHandler:]` 方法实现。

```objc
// `UIViewController` 内的代码
NSURL *url = [NSURL URLWithString:urlString]; // 获得需要跳转的 App 的 URL 地址
[self.extensionContext openURL:url completionHandler:^(BOOL success) {
    // Done
}];
```

### 4. `+[UIImage imageWithContentFile:]` VS. `+[UIImage imageNamed:]`

`UIImage` 的主要加载方式如上两个，前者通过图片资源的名字加载，且有缓存，而后者通过图片资源的绝对路径加载，没有缓存。

那么问题来了，在 iOS 8 中，覆盖安装时会改变 App 的安装路径，而 Today 中的旧 Widget 的进程还驻留在内存中，未得到更新。
此时使用 `+[UIImage imageWithContentFile:]` 方法，图片资源的绝对路径为旧的路径，可能不能取得目标资源。

而 `+[UIImage imageNamed:]` 方法由于缓存了图片，所以仍然可以正常显示图片。

### 5. Touch 事件

在 Widget 中，无论是 `UIControl`、`UIGestureRecognizer` 还是 `UIView` 的 touch 事件，只要区域是“空白的”（没有被渲染的部分，图中的红色区域），是不会被点击到的。

只有“充实”的区域（`UIImageView`、`UILabel` 或有颜色的 `UIView` 等有被渲染的部分，图中的黄色区域）是可以被点击的。
原因是 iOS 考虑到效率问题，对于 SpringBoard 上的多种层级的 view，不会检测“空白区域”的 touch 事件。
<img src="https://klaudz.me/articles/images/2014-12-05/08.png" alt="img" style="zoom: 30%;" />

要解决这个问题，有两种简便的方法。

1. 对目标的 `UIView` 子类实现空的 `-[UIView drawRect:]` 方法即可，此时整个 `UIView` 实例都可被点击。

   ```objc
   - (void)drawRect:(CGRect)rect
   {
       // 实现了 `-drawRect:`，`UIView` 会被渲染
   }
   ```

   值得注意，`-drawRect:` 会消耗比较多的内存，与 `UIView` 的面积成正比。

2. 贴一张图作为背景。

   例如透明背景图，用一张 1px 透明的 PNG 拉伸即可。



## 优质传送门

1. [Apple: App Extension Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)
2. [Apple: Adding an App to an App Group](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
3. [WWDC 2014 Session笔记 - iOS 通知中心扩展制作入门](http://onevcat.com/2014/08/notification-today-widget/)
4. [How to create iOS 8 Today extension and share data with containing app – tutorial](http://www.glimsoft.com/06/28/ios-8-today-extension-tutorial/)

