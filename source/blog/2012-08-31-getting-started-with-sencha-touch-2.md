---
title: Sencha Touch 2 入门
date: 2012/08/31
tags: sencha touch, javascript
---

**声明**：本文翻译自Sencha Touch官方文档。

**英文原版地址**：[http://docs.sencha.com/touch/2-0/#!/guide/getting_started](http://docs.sencha.com/touch/2-0/#!/guide/getting_started)

###什么是Sencha Touch

Sencha
Touch可以让你快速的开发出基于HTML5技术的移动应用，应用可以同时支持安卓、iOS和黑莓平台。并且使基于浏览器的应用能够提供与本地应用一样的用户体验。

###你需要的东西
	
首先，你要去Sencha官网下载[Sencha Touch 2 SDK](http://www.sencha.com/products/touch/download/) 与[SDK Tools](http://www.sencha.com/products/sdk-tools/download)（可以免费下载），除此之外你还需要：
	
* 能在本地计算机运行的WEB服务器
* WEB浏览器，推荐Chrome与Safari

如果你使用的是基于Windows的IIS，那请注意你必须将`application/x-json`添加为MIME类型，以便Sencha Touch正常工作。下面的链接将指导你如何去完成：[http://stackoverflow.com/a/1121114/273985](http://stackoverflow.com/a/1121114/273985)

###安装

首先，解压SDK文件到你的项目目录。通常应该配置HTTP服务器使其能够访问该文件夹。例如，你应该能够从你的Web浏览器访问`http://localhost/sencha-touch-2.0.0-gpl`来查看Sencha Touch文档。

你还需要运行SDK Tools安装程序。 SDK Tools会将`sencha`这个命令行工具添加`path`下，这样就可以通过`sencha`命令生成一个新的应用程序模板。要检查你是否已经安装了SDK Tools，切换到Sencha Touch目录，并运行`sencha`命令。例如：

```bash
cd ~/webroot/sencha-touch-2.0.0-gpl/
sencha
Sencha Command v2.0.0 for Sencha Touch 2
Copyright (c) 2012 Sencha Inc.
...
```

**注意：** 在执行Sencha Touch命令前，必须确保在SDK目录或者生成的Sencha Touch应用程序目录下。

###生成第一个应用程序

现在你已经成功安装Sencha Touch和SDK Tools，那我们现在就来创建一个应用程序。请确定你在Sencha Touch SDK目录下，并执行如下命令：

```bash
$ sencha generate app GS ../GS
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/.senchasdk
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/index.html
[INFO] Created file /Users/nickpoulden/Projects/sencha/GS/app.js
...
```

这将目录../GS下（在Sencha Touch SDK的上一层目录）生成一个命名空间为GS（Getting Started的缩写）的Sencha Touch应用程序。生成的应用程序中包括你构建一个Touch应用程序需要的所有文件，其中包括默认的`index.html`，一份Touch SDK的拷贝，CSS，图片和用于将你的应用程序打包成本地应用的配置文件。

接着让我们来检查你的应用程序是否能成功在Web浏览器中打开。假设你将SDK解压到了你的web根目录，那么你应该能够通过`http://localhost/GS`访问你生成的应用程序：

![](/images/blog/201208310101.png)
	
###代码构架的基本构成

用你最喜欢的IDE或文本编辑器打开GS目录，目录结构看起来应该像这样：

//GS目录结构

下面是目录和一些文件的具体描述

* app - 应用程序目录，其中包含：模型，视图，控制器和存储。
* app.js - 应用程序的入口。
* app.json - 应用程序的配置文件 -
  Builder用这个文件来生成压缩版的应用程序。
* index.html - 应用程序的HTML文件。
* packager.json - 配置文件，Packager使用该文件创建可以发布到应用商店的iOS和安卓的本地应用。
* resources - 资源目录，其中包含CSS和图像
* sdk - Sencha Touch SDK的一份拷贝。你应该不需要更改此文件夹里的内容。

在你的编辑器中打开app.js，这是应用程序的主入口点。
  
//此处为app.js文档

`launch`函数为应用程序的入口点。在默认的应用程序中，我们首先隐藏的应用程序加载指示器，然后创建一个主视图的实例，并把它添加到`Viewport`中去。

`Viewport`是一个`Card layout`, 你可以将组件添加到应用程序中。默认的应用程序将主视图添加到了`Viewport`，所以它在屏幕上是可见的。让我们来看看在主视图里面的代码。

在代码编辑器中打开`app/view/Main.js`，并尝试编辑第10行的内容：

```javascript
title: 'Home Tab'
```

现在编辑第19行内容：

```javascript
title: 'Woohoo!'
```

另外，在编辑22-26行：
 
```javascript
html: [
  "I changed the default <b>HTML Contents</b> to something different!"
].join("")
```

现在刷新应用程序，你将看到你对应用程序的修改。

//浏览器刷新后的web

