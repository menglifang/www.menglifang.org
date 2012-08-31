---
title: Sencha Touch 2 入门
date: 2012/08/31
tags: sencha touch, javascript
---

###什么是Sencha Touch

Sencha Touch能使你快速开发基于HTML5，安卓，IOS和Blackberry的工作，并能在本地浏览器中体验的本地应用程序。

###你需要了解的
	
首先，你要去下载Sencha Touch 2 SDK 与 SDK Tools（可以免费下载），除此之外你还要有如下的：
	
* 能在本地计算机运行的WEB服务器
* WEB浏览器，推荐Chrome 与 Safari

如果你使用的是基于Window的IIS，那请注意你必须要添加应用程序/x-json as a MIME。这样Sencha Touch才能正常工作，你可以在以下找到你需要的应用程序[http://stackoverflow.com/a/1121114/273985](http://stackoverflow.com/a/1121114/273985)

###安装

首先，提取你的SDK的zip文件到你的项目目录。在理想的情况下，这个文件夹设为你的HTTP服务器的访问。例如，你应该能够从你的Web浏览器浏览到`http://localhost/sencha-touch-2.0.0-gpl`和Sencha Touch文档。

你还需要运行SDK Tools安装。 SDK工具将Sencha Touch的命令行工具添加到你的路径，这样就可以产生一个新的应用程序模板。其中包括，要检查你是否已经安装了SDK中的工具，改变您的Sencha Touch目录，并运行Sencha Touch命令。例如：

```bash
cd ~/webroot/sencha-touch-2.0.0-gpl/
sencha
Sencha Command v2.0.0 for Sencha Touch 2
Copyright (c) 2012 Sencha Inc.
...
```

注意：在执行Sencha Touch命令前，必须确保下载的SDK目录或者生成的Sencha Touch在应用程序内。

###生成第一个应用程序

现在你有Sencha Touch和JDK安装工具，让应用程序确保在Sencha Touch与SDK文件夹内。输入如下：

```bash
$ sencha generate app GS ../GS
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/.senchasdk
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/index.html
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/app.js
...
```
这将产生一个基于Sencha Touch应用程序的命名空间的GS变量的架构，位于目录/GS（在Sencha Touch SDK的上一层目录）。架构中包括你所有的应用文件，还需要Sencha Touch程序。其中包括默认的index.html，复制的Touch SDK，CSS，图像和包装

你的应用程序中本机配置文件。

接着让我们来检查你的应用程序是否能成功在Web浏览器中打开它。假设你的Web根目录文件夹中提取的SDK，你应该能够到nativate的`http://localhost/GS`和默认的应用程序：
//此处应添加一个web浏览器生成的页面
	
###代码构架的基本构成

用你最喜欢的IDE或文本编辑器打开GS目录，目录结构看起来应该像这样

//GS目录结构
下面是目录和一些文件的具体描述

* 应用程序 - 为你的应用程序目录，其中包含：模型，视图，控制器和存储。
* app.js - 主要的Javascript为您的应用程序的入口点。
* app.json - 你的应用程序的配置文件 - 通过Builder中使用你的应用程序创建一个压缩后的版本。
* index.html - 为你的应用程序的主HTML文件。
* packager.json - 使用的配置文件的Packager创建你的应用程序的原生版本的IOS和Android应用程序商店。
* 资源 - 为你的应用程序目录，其中包含CSS和图像
* SDK - Sencha Touch的SDK。你应不需要更改此文件夹的内容。

  在你的编辑器中打开app.js文档，这是应用程序的主入口点。
  
//此处为app.js文档
激活功能，为您的应用程序的入口点。在默认的应用程序中，我们首先隐藏的应用程序加载指示器，然后创建一个实例，我们的主视图，并把它添加到视口。

视口是镶嵌在布局中，您可以将组件添加到您的应用程序。默认的应用程序添加主视图视口的，所以它在屏幕上是可见的。让我们来看看在主视图里面的代码。

开放的应用程序/视图/ Main.js的代码编辑器，并尝试编辑第10行的内容：

```javascript
title: 'Home Tab'
```

现在改变第19行内容：

```javascript
title: 'Woohoo!'
```

另外，在改变22-26行：
 
```javascript
html: [
  "I changed the default <b>HTML Contents</b> to something different!"
].join("")
```

现在刷新应用程序，你在浏览器中将会看到如下变化:

//浏览器刷新后的web

###下一步

接下来的步骤是按照首次应用指南，建立你刚刚做的，能够在15分钟内引导你创建一个简单但功能强大的应用程序。你也可以参照顶部的本指南视频。
如果你想跳过前面的框架，我们建议你检看下面的指南和资源等方面的详细的信息：
 

