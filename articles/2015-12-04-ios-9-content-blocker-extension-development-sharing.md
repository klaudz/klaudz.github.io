# iOS 9 广告拦截插件开发经验分享



继续腾些时间写写「iOS 插件开发经验分享」系列文章。
这期为大家介绍的主角是面世没多久的 iOS 9 **Content Blocker Extension**（内容拦截插件）。

需要了解一些与 iOS 插件开发有关的基础，可以先参考前面的文章。
传送门：[《Apple Watch 开发经验分享》](https://klaudz.me/?p=310) [《iOS 8 Widget 开发经验分享》](https://klaudz.me/?p=299)



## 背景

大部分主流的浏览器在好早以前就可以支持扩展插件，例如 Chrome、FireFox，Safari。使用浏览器扩展，每个用户都可以自定义浏览器的偏好。
在电脑浏览器的扩展中，很多浏览器扩展都是基于 JavaScript 开发的，由浏览器加载执行。
十多年前 Mozilla 就使用这种模型，而这种模型也在 WebKit 中使用，最经典的就是给 OSX 中的 Safari 编写浏览器扩展。

通过浏览器扩展，我们可以实现很多功能，例如浏览器翻墙设置、微博插件、广告拦截等等。 
今天我们的主角就是 iOS 上的广告拦截插件。继 Mac OS X 的 Safari 加入广告拦截插件后，iOS 终于也支持开发此插件。
在 iOS SDK 9.0 中，我们可以看到插件类型中增加了名为 Content Blocker Extension 的插件。
<img src="https://klaudz.me/articles/images/2015-12-04/01.png" alt="img" style="zoom:30%;" />

更准确地说，这个插件应该命名为内容拦截插件，而不是广告拦截插件。
苹果提供了 API，开发者可以使用这些 API 对 Safari 访问的页面内容进行有目的性的拦截。

下面我们开始了解一下 Content Blocker Extension。



## Extension & Apps

作为插件，我还是一如既往、不厌其烦地简单说明下 Extension、Container App、Host App 的关系吧。（笑）

### 1. Extension

当然就是 Content Blocker Extension 啦。（笑）
这个 Extension 很特别，他完全没有面目，他只负责执行代码。
<img src="https://klaudz.me/articles/images/2015-12-04/02.png" alt="img" style="zoom:30%;" />

除了“没有面子”外，这个 Extension 极其简单，目前他只能执行一个 API，这个方法可以使 Safari 的服务载入我们制定的拦截规则，仅此而已。
而且这个方法是被动地调起，具体是谁调起的呢？请看 Container App 的介绍。

### 2. Container App

Container App 当然就是我们写的 App 啦。（笑）
Container App 可以干啥呢？
他可以主动地调起 Extension，让 Extension 执行他那唯一的 API，让 Safari 的服务载入我们制定的拦截规则。
<img src="https://klaudz.me/articles/images/2015-12-04/03.png" alt="img" style="zoom:50%;" />（广告植入~~~~~~~~~~~~~~~~）

### 3. Host App

作为 Safari 的内容拦截，Host App 自然是 Safari，当之无愧。（笑）
如果我们制定的拦截规则被成功载入，那 Safari 在访问用户的页面时，就会读取这些规则，然后 Safari 会很机智地明白我们的意思，哪些内容该阻止，哪些内容该放行，清清楚楚，明明白白。
<img src="https://klaudz.me/articles/images/2015-12-04/04.png" alt="img" style="zoom:50%;" />

注：Host App 是 Safari，也就是说我们的插件只能拦截 Safari 的内容，无法拦截第三方 App 的内容。

这时候，你也许会想知道，Safari 有没有给用户拦截内容（有没有罢工），给用户拦截了什么内容（例如你想知道某些 XXOO 网站，你懂的），什么时候拦截了内容，拦截了多少内容，等等，等等。

好吧，你可能想多了，苹果毫无意外地想到了用户的隐私，我们无法从这里获取以上你想知道的信息。
正如上面所介绍的，Extension 只能执行一个 API（加载拦截规则），其他的什么都做不了。
当拦截到内容时，这个 Extension 就像死尸一样，毫无知觉。
因此，Extension 无法做任何与拦截结果有关的数据统计。



## 拦截方式

Content Blocker Extension 的拦截方式主要有三种：

### 1. URL

这种方式可以使目标 URL 直接不下载，效果类似 404。

例如，可以把整个页面给拦下来。
例如，可以拦住某个负责加载广告的 js 文件。
例如，可以拦住某张广告图片。
例如，可以拦截某个虚假色（和谐）情的 ipa 下载地址。
例如，。。。（请开脑洞吧）

*［WebKit 主页 URL 被拦截前］*
<img src="https://klaudz.me/articles/images/2015-12-04/05.png" alt="img" style="zoom:30%;" />

*［WebKit 主页 URL 被拦截后］*
<img src="https://klaudz.me/articles/images/2015-12-04/06.png" alt="img" style="zoom:30%;" />

### 2. Cookies

拦截某个网站的 cookies。

### 3. CSS（Display-None）

使 HTML 的某个 DOM 节点隐藏，理论上就是匹配到这个节点，然后把这个节点的 CSS 值改为 `display: none;`。
因此，与 URL 拦截方式不同的是，整个页面会被下载，只是目标节点不被显示。

例如，可以拦截页面中的某个广告的 DOM 节点。
例如，可以拦截页面中的某个广告的 DOM 节点。
例如，可以拦截页面中的某个广告的 DOM 节点。（重要的事情说三遍，无话可说也说三遍）

*［WebKit 主页的 icon 被拦截前］*
<img src="https://klaudz.me/articles/images/2015-12-04/05.png" alt="img" style="zoom:30%;" />

*［WebKit 主页的 icon 被拦截后］*
<img src="https://klaudz.me/articles/images/2015-12-04/07.png" alt="img" style="zoom:30%;" />



## 拦截规则

### 1. 简单例子

下图为官方提供的拦截规则 Sample，包含了两条简单的拦截规则。

简单讲解下吧。
第一条规则， URL 为 `"webkit.org/images/icon-gold.png"` 的内容都被拦截（`"type": "block"`）。（这条规则官方文档写错了，下面会吐槽。。。）
第二条规则，匹配所有页面（`"url-filter": ".*"`），页面中为 `"http://nightly.webkit.org"` 的超链接标签（`a[href^="http://nightly.webkit.org/"]`）都被隐藏（`"type": "css-display-none"`）。
<img src="https://klaudz.me/articles/images/2015-12-04/08.png" alt="img" style="zoom:70%;" />

### 2. 规则定义

我们先来看看 Content Blocker Extension 的 Apple 官方文档吧。
https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewInSafari/Articles/Safari_9.html#//apple_ref/doc/uid/TP40014305-CH9-SW8
<img src="https://klaudz.me/articles/images/2015-12-04/09.png" alt="img" style="zoom:30%;" />

没错，有关 iOS 上的信息，就这一个屏幕上的截图。。。
目前 Apple 官方提供的文档貌似就仅此而已。。。
而且上面占了半个版幅的 Sample，还有错误。。。（`url-filter` 的值是正则表达式，官方的 `.` 没有转义，囧囧囧。。。）

所以还是听老夫来细细讲解一下吧~

Content Blocker Extension 拦截文件为 JSON 文件，由多条规则组成。
一条规则包含两部分：
<img src="https://klaudz.me/articles/images/2015-12-04/10.png" alt="img" style="zoom:30%;" />

- **Trigger（触发器）**
  触发器定义了触发规则，例如 URL、属性等。
  - `url-filter`（string，必填）
  - `url-filter-is-case-sensitive`（bool，选填）
  - `resource-type`（array\<string>，选填）
    可选值：`document`、`image`、`style-sheet`、`script`、`font`、`raw`、`svg-document`、`media`、`popup`
  - `load-type`（array\<string>，选填）
    可选值：`first-party`、`third-party`
  - `if-domain/unless-domain`（array\<string>，选填）
- **Action（拦截行为）**
  行为定义了该如何拦截，例如 URL 直接拦截、CSS-Display-None 等。
- `type`（string，必填）
    可选值：`block`、`block-cookies`、`css-display-none`
  - `selector`（string，当 `type` 为 `css-display-none` 时必填）

### 3. url-filter 与正则表达式

这里特别讲解一下 `url-filter` 的定义。

**`url-filter` 有以下特点：**

- 所有 `url-filter` 的值都是使用正则表达式解析的。
- Content Blocker 的解析器仅支持正则表达式的子集，使用任何不支持的正则表达式语法将引发错误。
- `url-filter` 仅支持 ASCII 的 URL，使用非 ASCII 字符将引发错误。
- `url-filter` 的 `scheme` 和 `domain` 都应该为小写。


**支持的正则表达式（子集）：**

- 使用 `.` 用于表示所有字符。
- 使用 `[a-b]` 表达式表示范围。
- 支持量化表达式 `?`、`+`、`*`。
- 支持括号 `(` `)`。
- 支持 `^` 起始符、`$` 结束符


再次说明一次，使用任何不支持的正则表达式语法将引发错误！

### 4. 优化性能的建议

- 在 `url-filter` 中尽可能少用 `?`、`+`、`*`。
- CSS 规则最好定义在所有 `ignore-previous-rules` 规则前。
- Trigger 的规则越详尽越好。
- 相同的 Action 尽可能使用括号 `(` `)` 包在同一条规则中。优化性能的建议



# 拦截库

作为内容拦截的工具，如何获得拦截库是一个关键问题。
下面提供一些可用方法。

### 1. AdBlock

AdBlock 这个名字，大家应该不会陌生。
初出茅庐时，是一款 FireFox 插件，后来也兼容 Chrome 平台，命名为 AdBlock Plus。
这里就不过多介绍了。

但大家可知道，AdBlock 有提供免费的拦截源供订阅呢？

AdBlock 提供了一个名为 EasyList 的订阅源，里面有国际基础的拦截规则和多个国家的拦截规则，包括中国区的规则。
而且 EasyList 的更新很及时，每天能有 2-3 次的更新。

可以参见 https://easylist.adblockplus.org/en/，
基础规则 https://easylist-downloads.adblockplus.org/easylist.txt，
中国区规则 https://easylist-downloads.adblockplus.org/easylistchina.txt。

- **规则语法**

   AdBlock 的规则语法，可以参见官方文档：
https://adblockplus.org/zh_CN/filters
   
- **规则语法对比**
  
  下表是 AdBlock 规则与 iOS Content Blocker Extension 规则的语法对比。

  |              | Content Blocker                    | AdBlock                            |
  | ------------ | ---------------------------------- | ---------------------------------- |
  | **规则格式** | JSON                               | Text（Line，Custom）               |
  | **语法格式** | 正则表达式（子集）                 | 通配符 + 自定义规则                |
  | **拦截类型** | URL、Cookie、CSS                   | URL、CSS                           |
  | **资源类型** | Document、Script、Image、RAW、etc. | Document、Script、Image、RAW、etc. |
  | **CSS**      | CSS Display None                   | CSS Display None（高级）           |
  | **扩展**     | Load Type、If/Unless Domain        | Load Type、If/Unless URL           |

- **规则转换**
  
   要在 Content Blocker Extension 插件中使用 AdBlock 的规则，自然需要对规则进行转换。
   我们需要自行编写解析的代码，把 AdBlock 格式的规则文件解析转化为 Content Blocker Extension 格式的规则文件。
   
   以下是一些转换的建议：
   
   - **转义**
     `+` → `\+`
     `?` → `\?`
   - **基本过滤规则**
      `*` → `.*`
   - **例外规则**
      `@@`
   - **匹配网址开头/结尾**
      `|xxx` → `^xxx`
      `xxx|` → `xxx$`
      `||xxx` → `://xxx`
   - **标记分隔符**
      `^` → `[/:?=&]`
   - **注释**
      直接忽略
   - **逗号**
   - **其他**

### 2. 合作

可以通过与其它部门合作，同享比较成熟的拦截库。

### 3. 自行收集



# 问题与坑

1. 用户需要在 Safari — Content Blockers 中打开相应 Extension 的开关，Extension 加载的规则才能生效。
   <img src="https://klaudz.me/articles/images/2015-12-04/11.png" alt="img" style="zoom:30%;" />

2. iOS SDK 没有提供 API 让 App/Extension 得知用户是否打开了 Safari — Content Blockers 中对应 Extension 的开关，因此需要 App 在合理的时机去让 Extension 去 reload/update 规则。

   ```objc
   [SFContentBlockerManager reloadContentBlockerWithIdentifier:blockerBundleId completionHandler:^(NSError * _Nullable error) {
       // Result
   }];
   ```

3. 此插件只支持 64 位 CPU（arm64）的设备。
   在 Extension 的 `Info.plist` 需要设置 `UIRequiredDeviceCapabilities` 支持 `arm64`。（关联下一个问题）
   <img src="https://klaudz.me/articles/images/2015-12-04/12.png" alt="img" style="zoom:50%;" />

4. 必须特别注意！除了要在 Extension 设置 `UIRequiredDeviceCapabilities` 支持 `arm64`，同时也要在 App 上设置。
   而且，App 不能设置 `armv7`/`armv7s` 等！
   

意味着如果我们的 App 支持了 Content Blocker Extension，那么我们的 App 只能支持 arm64 的设备。。。

   如果在 App 上同时设置了 `armv7`、`armv7s`、`arm64`，
   编译、安装、打包都是没问题的，但在向 App Store 提交安装包时，会报错！
   多么痛的领悟！
   <img src="https://klaudz.me/articles/images/2015-12-04/13.png" alt="img" style="zoom:50%;" />