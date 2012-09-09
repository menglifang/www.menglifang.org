---
title: 使用和创建Sencha Touch 2构建
date: 2012/09/01
tags: sencha touch, javascript
---

**声明**：本文翻译自Sencha Touch官方文档。

**英文原版地址**：[http://docs.sencha.com/touch/2-0/#!/guide/building](http://docs.sencha.com/touch/2-0/#!/guide/building)

Sencha Touch 2有一个全新的类系统，其特点是能够按需动态地加载类。在开发和生产过程中，这能带来许多的好处。
  
开发过程中，动态加载意味着你得到一个个文件的堆栈跟踪，这使得你能更容易调试应用程序中的问题。对于生产环境下，我们提供了一个构建工具，使你能够轻松地创建一个最小化的自定义构建，它只包括在应用程序中实际被使用的类，这意味着能为你的用户减少加载时间。
  
###选择一个构建

Sencha Touch 2提供了5个不同的构建。如果你只是想快速的搭建并运行一个Sencha Touch应用实例，那么最好的方法就是使用`sencha-touch-debug.js`在本地进行应用开发，待部署到生成环境时，切换到`sencha-touch.js`。其他三个构建有利于在生成环境下进行调试，或在没有自定义构建的情况下运行生成环境，又或是将1.x的应用程序迁移到2.x的应用程序。

因为每个构建都有其特定的使用目标，所以每个构建都有不同的构建选项。下表可以简单的说明每个构建是被如何配置的：

//此处为FROM表单图

需要注意的是,最近3个版本都包含在下载的SDK的`builds`目录下。如果上面的表不是让你理解的很明白，这里有一个更加详细的说明，说明每个选项的意思:

* **Type:** 是`Core`或`All` ——
  `Core`包括基类，不包括组件。`All`则包含了所有。

* **Loader:** 无论自动加载是否被激活。只有`sencha-touch.js`默认激活。

* **Minified:** 是指构建由YUI的压缩机进行压缩。

* **Comments:**
  是指构建依然包含JSDoc注释（为了在生产环境下的下载速度，注释通常会被删除）

* **Debug:** 是指构建会给你提示你的错误信息，例如当你错误的配置了一个类时。

* **Compat:** 是指与`Sencha Touch 1.x`有了更好的兼容性。

最后，我们应该在开发环境中使用`sencha-touch-debug.js`，然后在生产环境下切换到`sencha-touch.js`或`sencha-touch-all.js`，或者一个自定义的构建。

###创建你自己的构建

在生产的绝大多数情况下，一个Sencha Touch 2应用程序应该使用一个自定义构建，有二个主要原因:

* 自定义构建只包括你的应用程序中实际使用的框架类，这样节省了下载时间。

* 一个自定义构建只创建一个文件，这个文件包括你所有的应用程序类，这意味着网络请求时只有一个网络请求。

原因二特别重要。大多数应用程序有大量的文件（有时会有数百个），请求时会将它们一个接一个的加载，在3G网络，这需要花很长时间。因为由于不同的网络环境，每个请求都有可能导致几百毫秒的延迟，这很容易你的应用程序的整体加载时间增加好几秒。

为确保你的应用程序在生产环境中加载变快，我们创建了[http://www.sencha.com/products/sdk-tools](Sencha SDK Tools)，其中包括一个构建脚本，该脚本会自动执行下面这些动能：

* 指出哪些框架类是在应用程序实际被使用的。

* 指出哪些应用程序类是在应用程序启动时要被加载的。

* 以类的正确顺序，将它们组合在一起放在同一个文件里。

* 删除了所有的JSDoc注释，减少文件的大小使其尽可能小。
  
###安装SDK工具

如果你还没有安装`Sencha SDK Tool`，那么你需要先安装，以便你可以创建一个构建。一个快速检验是否已经安装了SDK的方法是打开命令行终端，输入`sencha` —— 如果已经安装SDK工具你应该可以看到类似如下的信息：

//输入“sencha”有SDK工具就可以看到的内容。图

如果你看到的是错误信息，那么可能是因为没有安装SDK工具。你只需点击下载链接[http://www.sencha.com/products/sdk-tools](http://www.sencha.com/products/sdk-tools)，双击下载的文件安装，然后重试`sencha`命令，这时所有的一切都应该能正常工作了。
  
###生成一个构建

**注意：** 建立构建步骤预计将在下一个版本中被简化(2.0.0 Beta
2)，如果这个版本已经发布，请重新查看本文。

我们将假设你已经有一个在本地可用的应用程序，你只是想为生产环境建立构建。如果你还没有一个应用程序或不知道这到底是怎么回事，请看[Sencha Touch 2入门](http://www.menglifang.org/blog/2012/08/31/getting-started-with-sencha-touch-2/)。

如果你已经有一个在本地工作的应用程序，那让我们继续。我们将使用`Sencha Touch SDK`中的Twitter例子来说明这是如何工作的。首先，让我们熟悉这个文件例子的`index.html`文件：
  
```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <title>Twitter</title>

  <link rel="stylesheet" href="resources/css/application.css" type="text/css">

  <script type="text/javascript" src="touch/sencha-touch-debug.js"></script>
  <script type="text/javascript" src="app.js"></script>
</head>
<body></body>
</html>
```

注意，我们加载了`sencha-touch-debug.js`和`app.js`，这种组合能让我们的程序在开发环境中使用动态加载机制，并且是SDK工具可以生成一个最小化的构建的基础。一会儿我们将回到这个html文件的内容。

回到命令行，首先你使用`cd`进入到你的应用程序在硬盘上的目录：

```bash
cd ~/path/to/my/app
```

接下来，你需要为你的应用程序生成一个jsb文件。这个jsb文件列出了应用程序中用到的所有的类。你应该值得庆幸，SDK工具为你做了这件事：

```bash
sencha create jsb -a index.html -p app.jsb3
```

这个命令需要用到你的`index.html`文件（在开发过程中，你会用浏览器去浏览的那个文件），找出所有的类的依赖关系并把它们写入到一个名为`app.jsb3`的文件。到此，我们只需要再执行一个命令来生成这个构建：

```bash
sencha build -p app.jsb3 -d ./
```

这个最后的命令将jsb文件中列出的所有文件组合在一个文件中，并将其尽可能的最小化。该命令的输出是一个名为`all-classes.js`的文件，这个文件包含了所有的框架类以及你的应用程序类。
  
###更新你的HTML文件

最后一步，你需要更新你的应用程序在生产环境中使用的HTML文件，用`sencha-touch.js` 代替`sencha-touch-debug.js`，加载你新生成的`all-classes.js`。这样我们就完成了Twitter示例:
 
```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <title>Twitter</title>

  <link rel="stylesheet" href="resources/css/application.css" type="text/css">

  <script type="text/javascript" src="touch/sencha-touch.js"></script>
  <script type="text/javascript" src="all-classes.js"></script>
  <script type="text/javascript" src="app.js"></script>
</head>
<body></body>
</html>
```

为了不总是改变`index.html`文件，通常的做法是创建一个名为`index-production.html`的文件，该文件中包含如上所述的代码。许多开发人员会编写一个简单的部署脚本，将应用程序复制到一个部署文件夹中，并自动将`index-production.html`重命名为`index.html`，以便与这个构建可以很容易的上传。
  
###对于开发者来说的变化

虽然这只是几个步骤，我们期望下一个Sencha Touch版本能够同时改进工作流程和输出。如果你正在使用Sencha Touch 2.0 beta 1构建，当你升级了以后，请务必重新阅读该文档，因为它的步骤和输出可能会稍有变化。
