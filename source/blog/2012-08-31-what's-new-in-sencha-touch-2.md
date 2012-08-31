---
title: Sencha Touch 2中的新功能
date: 2012/08/31
tags: sencha touch, javascript
---

这里是Sencha Touch2.0中的一个列表的新特性和功能。

注：本指南是一个仍在更新中，并没有涵盖所有新的Sencha Touch2.0功能。读者需要经常查看更新。Sencha Touch2目前是开发者的预览版。可能其中会有错误，缺少的功能，和不完整的文件。

###欢迎学习Sencha Touch2.0

Sencha Touch是第一个HTML5移动框架和2.0是其最显着的升级。最重要的性能是焦点的释放 - 获得尽可能多应用程序在设备上运行。应用程序启动速度更快，提供了更迅捷的初始渲染和布局，并且完成一个roatated设备的布局。其他重要的增强功能：

* 一个全新的滚轮，为每个平台进行了优化，速度比以往任何时候都要快 - 尤其是在Android设备上。我们已经优化了渲染过程，应用创新技术来重用现有的组件，而无需新的实例化。

* 许多创新的Ext JS4，包括先进的新类系统，可重构的组件和应用程序体系结构的改进。

* 增加设备与WebKit和更强大的平台，随着时间的推移，我们可以用它来支持更多的设备，而不是强调更广泛的支持。
  
让我们仔细看看一个这些和其他不同的增强Sencha Touch2.0。
  
###更小，更快的布局引擎

Sencha Touch，可以很容易地制定出各种设备的形状和大小的应用程序提供了一个非常灵活的布局系统。第2版​​带来的运行更像是优化浏览器的CSS引擎的布局引擎。其结果是极大地提高，表现在几个关键领域：
  
* 应用程序呈现，奠定了更快的启动

* 更新后的画面旋转设备的速度要比Sencha Touch1.x应用程序快的多
    
* 布局引擎要小很多，从而导致更快的下载
    
所有的布局配置选项从Sencha touch1继续使用新的布局引擎,因此你不需要改变的代码行。

结果是极大改进布局性能的整体性。在屏幕出现页面能通过应用程序中导航时,给一个更流畅的感受。最引人注目的进步发生在一个设备布局发生方向改变。新的布局引擎是如此之快,我们不得不使用一个高速摄影机来测量它。这里是厨房的水槽按钮的例子运行在1。x和2。x,减缓到四分之一速度:

  //此处为厨房按钮放缓四分之一的图片
 
###更强大，更聪明的核心

Sencha Touch受益于与Ext JS的共享一个开发环境。 Ext JS4带来了一系列新的创新，建立到Sencha Touch，包括以下内容：
  
* 升级类系统支持动态加载和依赖关系

* 类配置为核心构建的支持 - 为你提供免费的，干净的，一致的API的getter和setter方法

###更快的启动时间

我们的1.x应用程序有太多的启动项，而要用掉大量的启动时间，所以我们正在竭尽所能去优化与启动无关的。到目前为止，我们现在所能看到在设备上进行测试时能够改善10％至25％的启动时间，我们的厨房水槽的例子。这个应用程序是相当大的，它几乎表明每一个组件的框架。On many devices, it loads almost a second faster in 2.x:
//此处为图
  
###类系统和应用程序

  Sencha Touch 2使用功能强大的新类系统从Ext JS 4。这提供了动态加载的所有好处,智能构建,只有包括类,那么使用mixin配置,和所有的其他特性的新引擎。有关详细信息,请参见在[http://docs.sencha.com/touch/2-0/#!/guide/class_system](Sencha Touch指南中如何使用类)。
  
  我们还将在Ext JS 4应用程序体系结构,包括ComponentQuery和生产建设支持。我们没有完成一个完整的开发者与模型-视图-控制器(MVC)打包的beta版本,将添加特性,如深度链接/历史支持在即将到来的预览版和测试版。
  
###配置驱动组件

配置驱动组件的一个好处是这个新类系统能够建立“款”——简单的属性是自动提供了getter和setter函数,违约,和其他更多。

Sencha Touch 2利用配置系统在整个框架。你已经知道每当你看到一个配置在一个类,你可以在任何时候重新配置它(即使它呈现)。甚至更好的,因为你已经知道要调用什么函数该配置的setter名称总是遵循相同的模式。

例如,当我们实例化一个文本字段我们可以给[http://docs.sencha.com/touch/2-0/#!/api/Ext.field.Text](一个文本字段标签),然后我们会很容易地知道如何去改变它:

```javascript
var text = Ext.create('Ext.form.Text', {
label: 'My Field'
});
//anything we can configure also has a setter function
//its name always follows the setConfigName pattern
text.setLabel('Another Field');
```
Configs很棒,因为它们给类一个非常干净的API。你看到的每样东西都在“配置选项的API文档对于每个类都有一个真正的配置完成标准化的getter和setter函数。

对于一个完整的概述的新功能看到[http://docs.sencha.com/touch/2-0/#!/guide/class_system](类系统指南)。

###改进的MVC功能

Sencha Touch1 用一个简单的方法来组织你的应用程序在MVC(模型-视图-控制器)行。Sencha Touch 2显著改善,是将全部历史的支持,一个强大的方式来控制组件和一个强大的方法来定制你的应用程序的一个不同的屏幕大小。

在此之上,包的数据已经被移植到使用新的类系统,使其更加灵活和提高性能。对于一个完整的概述在MVC改善Sencha Touch 2见下面的指南:
  
* [http://docs.sencha.com/touch/2-0/#!/guide/Intro to Applications](Intro to Applications)
* [http://docs.sencha.com/touch/2-0/#!/guide/profiles](Profiles)
* [http://docs.sencha.com/touch/2-0/#!/guide/controllers](Controllers)
* [http://docs.sencha.com/touch/2-0/#!/guide/History Support]( History Support)
  
###更好的Android支持

Sencha Touch 2带来一个巨大的进步在Android的性能,特别是当涉及到滚动和动画。在Sencha触摸1。x Android设备滚动大的列表明显慢。动画可能震荡和展览出怪异视觉。
  
Touch2可以让Android优化机制来实现两个平滑滚动和快速流畅的动画。在本月晚些时候我们将深入探讨SenchaCon这些进步的技术,但现在这里是摩托罗拉的Atrix在Android设备显示出更快的2。x感觉:
  
//此处为摩托罗拉的Atrix在在Android设备显示出更快的2。x感觉
  
###本地包装

最常见的问题之一开发者问在构建应用程序是Sencha联系,“在客户面前我如何得到我的应用程序?“在很多情况下,构建和部署应用程序web正是开发者想要与客户期望的。在另一些情况下,让应用程序在应用程序商店是最快的方式接触到消费者。

Sencha Touch 2是难以置信地容易构建和部署应用程序,Android的市场都和iOS应用程序商店。今天,随着Sencha Touch 2预览,我们运输一个开发者预览版本的SDK工具2.0。新的SDK工具包括一个新的sencha包命令,使您可以把你的应用程序和软件包 sencha联系起来作为一个应用程序或一个Android APK iOS。实现它是很容易的:一个命令,你的应用程序准备提交给苹果或谷歌的分布。

这让开发人员的生活变得更容易,在iOS的包装机不需要本机SDK,一个是包不需要原生SDK，这样你就可以打包，而无需下载苹果的SDK。下载SDK工具,您已经准备好构建。如果你部署到Android,您将需要下载从谷歌的Android SDK。开发人员有本机SDK,可以使用SDK工具直接把你的应用程序到iOS和Android模拟器,所以你可以看到你的应用程序将运行在设备。

SDK工具开发人员预览现在可以在Mac OS X,Windows和Linux的支持将很快加入。我们也会添加设备api,这将使它易于使用的本机功能,比如照相机和联系人等。对于所有的细节如何使用这些新包装功能,看到[http://docs.sencha.com/touch/2-0/#!/guide/native_packaging](本地包装为iOS在Mac)和[http://docs.sencha.com/touch/2-0/#!/guide/native_android](原生Android包装)。我们认为你和我们一样兴奋，因为你会发现现在是多么容易构建和打包你的web应用程序对于本机分布。

###检修文档

所有的最广泛使用的类在Sencha Touch 2功能优秀的文档正确的API参考。分布在类文档是几十个生活的例子,可以在你的浏览器中正确的运行,让你看到(甚至修改)示例代码。我们还带来了所有的SASS变量为每个组件API文档,使它更容易明白你可以定制。

We’re shipping 11 [http://docs.sencha.com/touch/2-0/#!/guide](brand new guides) out of the box.。我们有指南,解释核心概念(比如布局、组件和类,其他人,介绍如何使用的组件(比如选项卡面板、表单和旋转木马,和一个新的入门指导,带你通过构建你的第一个应用程序从头开始。
  
