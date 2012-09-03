---
title:使用和创建Sencha Touch 2构建
date: 2012/08/31
tags: sencha touch, javascript
---

Sencha Touch 2 comes with a brand new class system that features an ability to dynamically load classes when they are needed. This has many benefits in both development and production.

Sencha Touch 2有一个全新的类系统,其特点是当使用到它们的功能时能够动态地加载类。这在开发和生产中这是有许多好处的。
  
In development, dynamic loading means you get a file-by-file stack trace, which makes it much easier to debug problems with your application. For production, we provide a build tool that enables you to easily create a minified custom build that only includes the classes your app actually uses, meaning loading times are often reduced for your users.

在开发中,动态加载意味着你得到一个文件以及它的堆栈跟踪,这使得它更容易调试你的应用程序中的问题。对于生产,我们提供了一个构建工具,使你能够轻松地创建一个缩小的自定义构建,它只包括类在应用程序中实际使用的部分,这意味着能为你的用户减少加载时间
  
Choosing a Build  

###选择一个构建

Sencha Touch 2 ships with 5 builds out of the box. If you just want to get up and running as quickly as possible it's best to simply use sencha-touch-debug.js while developing your app locally then switch to sencha-touch.js in production. The other three builds are good for debugging in production, running in production without a custom build, and migrating your 1.x app to 2.x.

Sencha Touch 2 ships with 5 builds out of the box.。如果你只是想要建立和运行，最好最简单地方法是使用`sencha-touch-debug.js`调试，在本地开发你的应用程序然后切换到`sencha-touch.js`。其他三个构建有利于调试生产，在生产中运行中没有自定义的构建生成和你的1.x的应用程序迁移到2.x的应用程序的构建生成

Because each build is good for a different purpose an created using a different set of build options we've created a simple table below that shows how each one is configured:

需要注意的是,最近3个版本都包含在“构建”目录下下载SDK。如果上面的表不是让你理解的很明白,这里有一个更加详细的说明，说明每个选项的意思:

//此处为FROM表单图

Note that the last 3 builds are contained within the 'builds' directory in the SDK download. If the table above is not self-explanatory, here's a little more detail on what each option means:

需要注意的是,最近3个版本都包含在“构建”目录下的SDK下载。如果上面的表不是让你理解的很明白,这里有一个更加详细的说明，说明每个选项的意思:

Type: either "Core" or "All" - Core includes the base classes but none of the Components, All means everything is included

* type:是“核心”或“所有”——核心包括基类,但没有一个组件。这意味着包括所有的一切类

whether dynamic loading is activated or not. Only sencha-touch.js has this activated by default

* loader:无论自动加载是否被激活。`sencha-Touch.js`这个文件都是被默认激活

Minified: means the build has been compressed with YUI compressor

* minified:意味着构建已经被压缩和YUI的压缩机

Comments: means the build still contains the JSDoc comments (these are usually stripped in production to speed up downloads)

* comments:是指构建包含的的JSDoc评论(这些通常都是指精简了生产已提供更快下载)

Debug: means that the build will give you debug messages such as telling you if you have misconfigured a class

* debug:如果你的配置有错误的话，构建会给你一个类并提示你的错误信息

Compat: means that code to provide backwards compatibility with Sencha Touch 1.x is present in the build

* Compat:在构建中，代码与sencha touch1.x有了更好的兼容性

Again, use sencha-touch-debug.js in development mode then switch to either sencha-touch.js or sencha-touch-all.js plus a custom build in production.

再一次,使用`sencha-touch-debug.js`在开发模式下然后切换到`sencha-touch.js`或`sencha-touch-all.js`在生产加上一个自定义构建。

###创建你自己的构建

In the vast majority of cases a Sencha Touch 2 app should use a custom build in production, for 2 main reasons:

在生产的绝大多数情况下一个Sencha Touch 2应用程序应该使用一个自定义构建，有二个主要原因:

Custom builds include only the framework classes your app is actually using, saving on download time

* 1.自定义构建只包括你的应用程序的框架中实际使用的类,这样就节省了下载时间

A custom build includes all of your app classes in a single file, which means only one network request

* 2.一个自定义构建只创建一个文件，这个文件包括你所有的应用程序类,这意味着网络请求时只有一个网络请求

Reason 2 is especially important. Most applications will have a large number of files (sometimes hundreds), and loading them one by one, especially over a 3g network, can take a long time. That's because each request can add several hundred milliseconds of delay, depending on network conditions, which can easily add several seconds to your application's overall load time.

原因2是特别重要的。大多数应用程序有大量的文件(有时会有数百个),请求时会将它们一个接一个的装入,尤其是在3G网络,需要花很长时间。这是因为每个请求都有可能添加几百毫秒的延迟,这取决于网络的条件,这可以很容易地添加几秒负荷在你的应用程序的整体加载时间上。

To ensure your applications load quickly in production we have created the Sencha SDK Tools, which includes a build script that automatically does all of this:

为确保你的应用程序在生产负载中变快，我们已经创建了[http://www.sencha.com/products/sdk-tools](Sencha SDK Tools),其中包括一个构建脚本,该脚本会自动执行下面这些动能:

Figures out which framework classes your application is actually using

* 1.找出哪些框架类是在应用程序实际被使用的

Figures out which application classes are loaded when your application boots up

* 2.找出哪些应用程序类是在应用程序启动是要被加载的

Combines all of these into a single file, with the classes in the right order

* 3.以类的正确顺序找出所有文件，将它们组合在一起放在同一个文件

Strips out all of the JSDoc comments, then minifies the file so it's as small as possible

* 4.剔除了所有的JSDoc评论,减少文件的数量使其尽可能小
  
###安装SDK工具

If you don't already have the Sencha SDK Tools installed, you'll need to install them before you can create a build. A quick way to check to see if the tools are already installed is to open up your command line terminal and type in 'sencha' - if the SDK Tools are present you should see something like this:

如果你还不确定是否安装了Sencha SDK Tool,你可以在安装他们之前可以创建一个构建来检查。一个快速检验是否已经安装了工具的方法是打开命令行终端,输入“sencha”——如果已经安装SDK工具你应该可以看到类似如下的信息:

//输入“sencha”有SDK工具就可以看到的内容。图


If you get an error instead you probably don't have the SDK Tools installed. Just hit the download button on http://www.sencha.com/products/sdk-tools, double-click the downloaded file to install then try the sencha command again and everything should work.

如果你没有得到以上信息，可能是因为没有安装SDK Tool。你只需点击下载链接[http://www.sencha.com/products/sdk-tools](http://www.sencha.com/products/sdk-tools),双击下载的文件安装然后再一次调试sencha命令就能确保一切正常工作
  
  
###生成一个构建

Note: The build steps are expected to be simplified in the next release (2.0.0 Beta 2) so please check this page again once that release is available.

注意:建立构建步骤预计将在下一个版本中被简化(2.0.0 Beta 2),请检查这个页面是否被释放是不是可用的。

We're going to assume you have an app that works locally already and that you just want to build it for production. If you don't have an app yet or don't know what this is all about, check out the getting started guide.

我们将假设你已经有一个在本地使用的应用程序和你只是想为生产建立构建。如果你没有一个应用程序或不知道这到底是怎么回事,请看看[http://docs.sencha.com/touch/2-0/#!/guide/apps_intro](入门指南)。

Assuming you app does work locally though, let's proceed. We're going to use the Twitter example that comes with the Sencha Touch SDK to illustrate how this works. Firstly let's familiarize ourselves with that example's index.html file:

假设你已经有一个在本地工作应用程序，那让我们继续。我们将使用Twitter的例子是与Sencha touch SDK来说明这是如何工作的。首先让我们熟悉这个文件例子.html文件:
  
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


Notice that we're loading sencha-touch-debug.js and app.js - this combination is what allows us to use dynamic loading while developing our app, and is the basis for the SDK Tools' ability to generate a minimal build. We'll come back to this html file's contents shortly.

注意,我们装载`sencha-touch-debug.js`和`app.js` -这种组合能让我们的程序在开发中动态的加载,并且在SDK Tool的基础上产生最小的一个构建。不久我们将回到这个html文件的内容。

Back to the command line - first you'll need to cd into to the directory that your app can be found in on your hard drive:

回到命令行——首先你使用cd进入到你的硬盘并找到应用程序的目录:

```bash
cd ~/path/to/my/app
```

Next you'll need to generate a jsb file for your app. A jsb file is basically a list of all the classes that your application uses. Thankfully, the SDK Tools do this for you:

接下来,你需要为你的应用程序生成一个jsb文件。这个jsb文件基本上列出了应用程序中用到的所有的类,供你的应用程序使用。你应该值得庆幸的是,SDK Tool为你做这件事:

```bash
sencha create jsb -a index.html -p app.jsb3
```

This command takes your index.html file (the same file you use in your browser while developing your app), figures out all of the class dependencies and writes them out into a file called app.jsb3. From here we just need one more command to actually generate the build itself:

这个命令需要用到你的.html文件(同一个文件可以用浏览器开发你的应用程序),找出所有的类的依赖关系并把它们写入到一个文件中称`app.jsb3`。从这里我们只需要一个命令来实际生成这个构建:

```bash
sencha build -p app.jsb3 -d ./
```

This final command takes all of the files listed in the jsb and combines them into a single file, which it then minifies to make as small as possible. The output is a file called all-classes.js, which contains all of the framework classes plus your application classes.

最后的命令需要列出所有文件的信息并将它们组成一个单一的文件,并使尽可能小。输出是一个文件名为 `all-classes.js`,它包括了所有的框架类和你的应用程序类。
  
  
###更新你的HTML文件

The final step to prepare your app for production is to update your HTML file to use sencha-touch.js instead of sencha-touch-debug.js, and to load your newly-generated all-classes.js. Here's how our twitter example file ends up:

最后一步更新你的应用程序生产的HTML文件使用`sencha-touch.js` 代替`sencha-touch-debug.js`,加载你的新生成的`all-classes.js`。这样我们就结束了的twitter示例:
 
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

Rather than change your main index.html file all the time, it's common to create a duplicate called index-production.html that looks like the file above. Many developers will produce a simple deploy script that copies the app into a deploy folder and renames index-production.html to index.html automatically so that the build can simply be uploaded.

这不是改变你的`index.html`文件,它的共同创建一个重复的`index-production.html`,看起来就像上述文件。许多开发人员将产生一个简单的部署脚本复制到应用部署文件夹，重命名为`index.html` `index-production.html`，这样就可以建立简单地上传。
  
###对于开发者来说的变化

Although it's only a few steps, we'd like to improve both the workflow and the output of the builder for the next release of Sencha Touch. If you're building using Sencha Touch 2.0 beta 1 please be sure to check back when you upgrade to a later release as it is likely the steps and output will have changed slightly.

虽然这只是几个步骤,我们想为下一个Sencha Touch版本能够同时提高开发者的工作流程和输出。如果你正在构建使用Sencha touch2.0 beta 1请务必核对当你升级到最新的版本中,因为它的步骤和输出可能会稍有变化。
