---
title: "设置Eclipse默认打开代码文件的编码方式"
date: "2011-12-22"
---

Eclipse 打开一个文本文件时，默认以 GBK 编码打开并读取文件的内容。 如果所打开的文本文件并非以 GBK 编码，例如 UTF-8，则显示非英文的时候就会出现乱码。 **注意：以下均用 UTF-8 作为例子。 ** 大家可能会问以下的问题：

1. 问：那以后新建、保存文件时都统一用 GBK 就可以了吧？ 答：Eclipse 可以用来写网页代码文件（如 html、css、js、php 等），如果一个站点、页面设置了以 UTF-8 编码方式被浏览，则把文件编码改为 GBK 后，别人浏览你的网页时则有可能显示乱码。
2. 问：在文本编辑器（如记事本、EmEditor 等）把文件“另存为”，选择 UTF-8 编码不就可以了吗？ 答：这样如果是设置了 UTF-8 编码的站点、页面，的确可以被正常浏览，但因为 Eclipse 默认以 GBK 方式打开文件，所以在 Eclipse 中，非英文则会显示乱码。

当然，解决方法肯定是有的。

Eclipse 的每个项目中都有一个 .settings 文件夹，里面有一个 org.eclipse.core.resources.prefs 文件，这个文件记录着整个项目中每个文件的记录方式，一旦一个文件的编码被改变为 Eclipse 非默认打开的编码，则会记录在这个文件中。

但这个方法是比较笨拙的，因为每次更改都要记录，显然很不方便。而且如果一个项目中有多个文件要被更改，就会很麻烦了。 当然，肯定有更好的解决方法的。

### 以下就说一下设置 Eclipse 默认打开代码文件的编码方式的方法。

 

1. ### 首先把某一类文件全部转化为某一种编码（这里以 php 文件转化为 UTF-8 为例）
    
    Windows -- Preferences -- General -- Content Types 选择所需要更改的文件类型（这里是 php 文件），在 Default encoding 中输入我们想要的编码（这里是 UTF-8），按下 Update，则所有 php 文件将被自动转化编码。
    
    [![](images/00008-01-09b37343138c57db.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00008-01-09b37343138c57db.png)
    
    [![](images/00008-02-0931674ee2cff4b6-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00008-02-0931674ee2cff4b6.png)
2. ### 然后就是更改工作区的编码（即打开、读取文件的编码方式）
    
    Windows -- Preferences -- General -- Workspace 在 Text file encoding 中选择 Other，并输入我们想要的编码（这里是 UTF-8），按确定即可。
    
    [![](images/00008-03-55899f8d01043174-medium.png)](http://cloud018-wordpress.stor.sinaapp.com/uploads/2011/12/00008-03-55899f8d01043174.png)
