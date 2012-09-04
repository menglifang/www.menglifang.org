---
title: Sencha Touch 2的应用
date: 2012/09/03
tags: sencha touch, javascript
---

Sencha Touch 2 is optimized around building applications that work across multiple platforms. To make the writing of applications as simple as possible, we provide a simple but powerful application architecture that leverages the MVC (Model View Controller) pattern. This approach keeps your code clean, testable and easy to maintain, and provides you with a number of benefits when it comes to writing your apps:

为了让写应用变的更简单，Sencha Tcouch 2构建了跨越多个平台的优化程序。利用MVC(模型-视图-控制器)模型，为我们提供了一个简单但又非常强大的应用程序体系。这种方式让你在书写代码的时候能够很整洁，可测试性强和易于维护，为你在写应用程序的时候提供了很多好处。

History Support: full back button support inside your app, and any part of your app can be linked to

* History Support:在你的应用程序的任何部分都支持返回功能，只要你单击后退按钮就可以返回你以前浏览过的应用程序。

Deep Linking: share deep links that open any screen in your app, just like linking to a web page

* Deep Linking:通过分享的深度链接，可以在如何一个屏幕上打开你的应用程序。就想直接链接到一个web首页一样

Device Profiles: easily customize your application's UI for phones, tablets and other devices while sharing all of the common code

* Device Profiles:能够轻松的定义你的应用程序在什么样的用户界面上显示如手机,平板电脑或是其他设备。同时也能共享所有的通用代码

###应用程序的结构

An Application is a collection of Models, Views, Controllers, Stores and Profiles, plus some additional metadata specifying things like application icons and launch screen images.

一个应用程序是一个集合，它包括Models, Views, Controllers, Stores and Profiles,在加上一些额外的应用程序图标和启动画面图像数据中指点的类

Models: represent a type of object in your app - for example an e-commerce app might have models for Users, Products and Orders

* Models:在你的应用程序中是一个类型的对象，例如一个电子商务应用程序中可能是用户，产品订单

Views: are responsible for displaying data to your users and leverage the built in Components in Sencha Touch

* Views:利用内置的Sencha Touch组件，负责为用户显示数据

Controllers: handle interaction with your application, listening for user taps and swipes and taking action accordingly

* Controllers:处理每次用户要处理的事件，和应用程序交付并采取相应的处理

Stores: are responsible for loading data into your app and power Components like Lists and DataViews

* Stores:负责加载数据到应用程序和组件中。如列表和DataViews

Profiles: enable you to easily customize your app's UI for tablets and phones while sharing as much code as possible

* Profiles:够让你轻松定义你的应用程序在何种界面上显示，如平板电脑或者手机。同时能共享你的共用代码

The Application is usually the first thing you define in a Sencha Touch application, and looks something like this:

通常你可以先确定一个Sencha Touch应用程序，可以和这个例子一样

```javascript
Ext.application({
    name: 'MyApp',
    models: ['User', 'Product', 'nested.Order'],
    views: ['OrderList', 'OrderDetail', 'Main'],
    controllers: ['Orders'],

    launch: function() {
        Ext.create('MyApp.view.Main');
    }
})
```

The name is used to create a single global namespace for your entire application, including all of its models, views, controllers and other classes. For example, an app called MyApp should have its constituent classes follow the pattern MyApp.model.User, MyApp.controller.Users, MyApp.view.Main etc. This keeps your entire app under a single global variable so minimizes the chance of other code on the page conflicting with it.

这个[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-name](名字)是为整个应用程序创建一个单一的全局命名空间,包括所有的models, views, controllers and other classes。例如,一款叫做“MyApp的应用程序应该遵的循模式MyApp.model.User、Myapp.controller.User、Myapp.view.Main等等。这使你的整个应用程序处于在一个全局变量,这会让你的代码和其他页面上的代码有最少可能的相互冲突。

The Application uses the defined models, views and controllers configurations to automatically load those classes into your app. These follow a simple file structure convention - models are expected to be in the app/model directory, controllers in the app/controller directory and views inside the app/view directory - for example app/model/User.js, app/controllers/Orders.js and app/view/Main.js.

应用程序使用已经定义好的[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-models](models), [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-views](views) and [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-controllers](controllers) configurations来自动加载这些类到你的应用程序。这些应该遵循一个简单的文件结构公约——模型预计在app/model directory、控制器在app / controllerdirectory和视图在app /view directory——例如app /model/User.js,app / controllers /Order.js和app /view/Main.js。

Note that one of the models we specified was different to the others - we specified the full class name ("MyApp.model.nested.Order"). We're able to specify the full class name for any of those configurations if we don't follow the normal naming conventions. See the Dependencies section of the Ext.app.Application docs for full details on how to specify custom dependencies.

注意,我们指定的一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-models](模型)是和其他人不同的——我们指定完整的类名称(“MyApp.model.nested.Order”)。如果我们不遵循正常的命名约定，我们能够自己指定完整的类名的配置。看[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application](依赖关系部分)的[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application](Ext.app.Application)。应用程序文档详细介绍如何指定自定义依赖项。

###控制器

Controllers are the glue the binds an application together. They listen for events fired by the UI and then take some action on it. This helps to keep our code clean and readable, and separates the view logic from the control logic.

控制器是一个应用程序绑定在一起的粘合剂。它们监听用户界面触发的事件,采取不同的处理。这样有助于使我们的代码清晰和可读性,同时从控制逻辑中分离出视图逻辑。

For example, let's say you require users to log in to your app via a login form. The view in this case is the form with all of its fields and other controls. A controller should listen to tap event on the form's submit button and then perform the authentication itself. Any time we deal with manipulating data or state, the controller should be the class that activates that change, not a view.

例如,让我们假设你需要你的用户通过你的应用程序中的一个表单登录到你的应用程序。在这种情况下视图就是在本例中所有字段和其他控件的一个集合体。一个控制器应该[http://docs.sencha.com/touch/2-0/#!/api/Ext.Button-event-tap](监听)表单的提交[http://docs.sencha.com/touch/2-0/#!/api/Ext.Button](按钮)的提交事件,然后去执行身份验证。任何时候我们处理操作数据或状态,控制器应该是类,激活,改变,而不是一个视图。

Controllers expose a small but powerful set of features, and follow a few simple conventions. Each Controller in your application is a subclass of Ext.app.Controller (though you can subclass existing Controllers, so long as it inherits from Ext.app.Controller at some point). Controllers exist in the MyApp.controller.* namespace - for example if your app had a Sessions controller it would be called MyApp.controller.Sessions and exist in the file app/controller/Sessions.js.

控制器拥有一个小而强大的的特性集,并且它还要遵循一些简单的惯例。在你的应用程序中每个控制器的一个子类都有[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller](Ext.app.Controller)(尽管你现有的子类可以有控制器,但在某种程度上，控制器只它继承自[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller](Ext.app.Controller))。控制器存在于myapp.controller.*namespace——例如,如果你的应用程序有一个会话控制器将被称为MyApp.controller.Sessions。会话控制器存在于文件app / controller /Sessions.js。

Although each Controller is a subclass of Ext.app.Controller, each one is instantiated just once by the Application that loaded it. There is only ever one instance of each Controller at any one time and the set of Controller instances is managed internally by the Application. Using Application's controllers config (as we do above) loads all of the Controllers and instantiates them automatically.

尽管每个控制器的一个子类[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller](Ext.app.Controller)。每个控制器在[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application](Application)中只有一次被实例化去加载它。There is only ever one instance of each Controller at any one time and the set of Controller instances is managed internally by the Application.。使用应用程序的控制器配置(如我们做以上)自动加载所有的控制器和实例化它们。

一个小例子

Here's how we might quickly define the Sessions controller described above. We're using 2 Controller configurations here - refs and control. Refs are an easy way to find Components on your page - in this case the Controller will look for all Components that match the formpanel xtype and assign the first one it finds to the loginForm property. We'll use that property in the doLogin function later.

这是我们如何快速定义上面描述的会话控制器。我们在这里使用的是2控制器配置——[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-refs](裁判)和[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-control](控制)。裁判会使用一个简单的方法来找到你的页面上的组件——在本例中,控制器将寻找所有匹配的[http://docs.sencha.com/touch/2-0/#!/api/Ext.form.Panel](formpanel) xtype组件和找到配置的第一个loginForm部分。我们将在doLogin函数之后使用这个属性。

The second thing it does is set up a control configuration. Just like refs, this uses a ComponentQuery selector to find all formpanel xtypes that contain a button inside them (for example, this will find the Submit button in our hypothetical login form). Whenever any button of this type fires its tap event, our Controller's doLogin function will be called:

第二件事就是为它建立一个控制配置。就像裁判,这使用了一个ComponentQuery选择器来找到所有formpanel xtypes,这里面包含一个按钮(例如,这将找到我们登录表单中Submit按钮)。每当任何此类的按钮触发事件被激活,我们控制器的doLogin函数将被调用:

```javascript
Ext.define('MyApp.controller.Sessions', {
    extend: 'Ext.app.Controller',

    config: {
        refs: {
            loginForm: 'formpanel'
        },
        control: {
            'formpanel button': {
                tap: 'doLogin'
            }
        }
    },

    doLogin: function() {
        var form   = this.getLoginForm(),
            values = form.getValues();

        MyApp.authenticate(values);
    }
});
```

The doLogin function itself is quite straightforward. Because we defined a 'loginForm' ref, the Controller automatically generates a getLoginForm function that returns the formpanel that it matches. Once we have that form reference we just pull the values (username and password) out of it and pass them to an authenticate function. That's most of what Controllers ever do - listen for events fired (usually by the UI) and kick off some action - in this case authenticating.

doLogin函数本身是很简单的。因为我们定义了一个“loginForm“ref,控制器会自动生成一个getLoginForm函数,它返回formpanel进行匹配。一旦我们有,我们只需要将其引用,并将其值(用户名和密码)传递到一个身份验证功能。这是大多数做控制器——侦听事件(通常通过UI)和启动一些事件的开始——在这种情况下进行身份验证。

For more on what Controllers are and what capabilities they possess see the controllers guide.

更多关于控制器和控制器具体有那些功能，请查看[http://docs.sencha.com/touch/2-0/#!/guide/controllers](控制器指南)。

###Store

Stores are an important part of Sencha Touch and power most of the data-bound widgets. At its simplest, a Store is not much more than an array of Model instances. Data-bound Components like List and DataView just render one item for each Model instance in the Store. As Model instances are added or removed from the Store events are fired, which the data-bound Components listen to and use to update themselves.

Stores are an important part of Sencha Touch and power most of the data-bound widgets. At its simplest。简单来说,商店是不超过一个数组的模型实例。数据绑定的组件比如[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.List](list)和[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.DataView](DataView)只是呈现一个Stores对每个模型的实例化。如果Store的事件被触发作为模型的实例将被添加或删除,而监听数据绑定的组件将自己更新。

While the Stores guide has much more information on what Stores are and how they fit in with Components in your app, there are a couple of specific integration points with your Application instance that you should be aware of. 

尽管[http://docs.sencha.com/touch/2-0/#!/guide/stores](Stores guide)有很多关于store的信息以及它们如何去适应你的应用程序中的组件,还是有几个特定的集成点与你的[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application](应用程序)实例,你应该知道的。

###配置文件

Sencha Touch operates across a wide range of devices with differing capabilities and screen sizes.  A user interface that works well on a tablet may not work very well on a phone and vice versa so it makes sense to provide customized views for different device types.  However, we don't want to have to write our application multiple times just to provide a different UI - we'd like to share as much code as possible. 

Sencha Touch能在不同的功能和屏幕尺寸的设备上工作。但一个运作良好的用户界面,在平板电脑上能很好的工作却在电话上不能良好的工作,反之亦然,因此有必要为不同的设备提供定制的视图。然而,我们不想在我们的应用程序中写多次仅是为了提供一个不同的UI——我们想尽可能多的分享代码。

Device Profiles are simple classes that enable you to define the different types of devices supported by your app and how they should be handled differently.  They are opt-in, so you can develop your app without profiles at first and add them in later, or never use them at all. Each profile simply defines an isActive function that should return true if that profile should be active on the current device, plus a set of additional models, views and controllers to load if that profile is detected.
 
为不同的设备写配置文件是一个简单类,使你能够定义支持不同类型的设备,让应用程序以能够选择它们应该如何处理。所以你在开始开发你的应用程序的时候可以没有配置文件,以后在为它们添加,或从不使用它们。每个概要文件仅仅定义了一个函数,如果概要文件在当前设备上运行或者一组额外的[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Profile-cfg-models](models), [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Profile-cfg-views](views) and [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Profile-cfg-controllers](controllers)来加载，那么它应该返回[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Profile-method-isActive](true isActive)。

To app Profile support to your app you just need to tell your Application about those Profiles and then create Ext.app. Profile subclasses for them: 

应用程序的配置文件要支持你的应用程序,你只需要告诉你的应用程序这些配置文件,然后创建[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Profile](ext.app.profile)为它们的子类:

```javascript
Ext.application({
    name: 'MyApp',
    profiles: ['Phone', 'Tablet'],

    //as before
});
```

By defining the profiles above the Application will load app/profile/Phone. js and app/profile/Tablet.js.  Let's say that the tablet version of the app enables additional capabilities - for example managing groups.  Here's an example of how we might define the Tablet profile: 

通过定义概要文件应用程序将加载app/ profile /Phone.js和app / profile /Table.js。这就是说,平板电脑版本的应用程序允许有额外的功能,例如管理组织。下面是一个示例,介绍我们如何定义平板电脑配置文件:

```javascript
Ext.define('MyApp.profile.Tablet', {
    extend: 'Ext.app.Profile',

    config: {
        controllers: ['Groups'],
        views: ['GroupAdmin'],
        models: ['MyApp.model.Group']
    },

    isActive: function() {
        return Ext.os.is.Tablet;
    }
});
```

The isActive function will return true whenever the application is run on what Sencha Touch determines to be a tablet.  This is a slightly subjective determination because there is a near-continuous spectrum of device shapes and sizes with no clear cutoff between phones and tablets.  Because there is no foolproof way to state which devices are tablets and which are phones, Sencha Touch's Ext.os.is. Tablet is set to true when running on an iPad and false otherwise.  If you need more fine grained control it's easy to provide any implementation you like inside your isActive function, so long as it returns true or false. 

如果应用程序决定运行是Sencha Touch平板电脑，那么isActive函数将返回true。这是一个有点主观判断,因为有一个不明确的频谱形状和尺寸的设备在手机和平板电脑不断切换。因为没有绝对安全方式说明哪些设备是平板电脑和那些是手机,当运行在一台iPad的时候Sencha Touch的ext.os.is设置为true时,否则则返回false。如果你需要更多精细的控制也可以很容易实现，只要你喜欢在你isActive函数,它只返回true或false。

You should make sure that only one of your Profiles returns true from its isActive function.  If more than one of them returns true, only the first one that does so will be counted and the rest ignored.  The first one that returns true will be set as the Application's currentProfile, which can be queried at any time. 

你应该确保你的配置文件中，只有一个返回true 的isActive功能。如果不止一个返回true,那么只有第一个,会被系统记录,其余的会被忽视。第一个返回true将被设置为应用程序的[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-currentProfile](currentProfile),这样它就能被随时的查询。

If the detected currentProfile has defined additional models, views, controllers and stores these will be automatically loaded by the Application, along with all of the models, views and controllers defined on the Application itself.  However, all of the dependencies named in the Profile will be prepended with the Profile name unless the fully-qualified class name is provided.  For example: 

如果检测到currentProfile附近有定义的models, views, controllers and stores这些将会被自动加载到应用程序,以及应用程序本身所有定义的models, views and controllers。然而,所有的依赖在命名文件中的文件将被添加到配置文件,除非完全合格的类名。例如:

* views: ['GroupAdmin'] will load app/view/tablet/GroupAdmin.js
* controllers: ['Groups'] will load app/controller/tablet/Groups.js
* models: ['MyApp.model.Group'] will load app/model/Group.js

Most of the time a Profile will only define additional controllers and views as the models and stores are typically shared between all variants of the app. For a more detailed discussion of Profiles see the device profiles guide. 

大多数时候一个配置文件只会额外定义的控制器和视图的模型，和商店通常是通过不同的应用能够程序来共享应用。如果需要更详细的讨论配置文件可以去看[http://docs.sencha.com/touch/2-0/#!/guide/profiles](设备配置指导)。


###启动

Each Application can define a launch function, which is called as soon as all of your app's classes have been loaded and the app is ready to be launched.  This is usually the best place to put any application startup logic, typically creating the main view structure for your app. 

当你所有的应用程序的类都已经加载和应用程序都准备好了，那么应用程序可以定义一个启动功能。这通常是最好的场合来存放你的应用程序启动逻辑,让你的应用程序创建主要视图结构。

In addition to the Application launch function, there are two other places you can put app startup logic.  Firstly, each Controller is able to define an init function, which is called before the Application launch function.  Secondly, if you are using Device Profiles, each Profile can define a launch function, which is called after the Controller init functions but before the Application launch function.
 
除了应用程序启动功能,另外还有两个地方你可以定义应用程序启动逻辑。首先,每个控制器要能够定义一个init函数,它被称为应用程序启动之前的函数。其次,如果你正在使用的设备配置文件,每个文件也可以定义一个启动功能,但在应用程序启动功能前，之后调用控制器init函数。

Note that only the active Profile has its launch function called - for example if you define profiles for Phone and Tablet and then launch the app on a tablet, only the Tablet Profile's launch function is called.
 
注意,只有当前文件有被称为启动功能——例如,如果你定义配置文件为手机和平板电脑,然后启动应用程序在平板电脑上,只有平板电脑的启动函数被调用。

* 1.Controller#init functions called
* 2.Profile#launch function called
* 3.Application#launch function called
* 4.Controller#launch functions called

When using Profiles it is common to place most of the bootup logic inside the Profile launch function because each Profile has a different set of views that need to be constructed at startup. 

大部分使用配置文件的地方是启动逻辑在配置文件启动功能,因为每个配置文件都有一组不同的视图,需要建立在启动。

###Routing and History Support

Sencha Touch 2 has full Routing and History support.  Several of the SDK examples, including the Kitchen Sink, use the history support to enable the back button to easily navigate between screens - especially useful on Android.
 
Sencha Touch 2 has full Routing and History support。SDK的几个例子已经能说明,包括厨房的水槽,使用历史支持使后退按钮来轻松地完成导航屏幕之间切换——尤其是在Android上更有用。

There will be full documentation on the history support from beta 1 onwards.  As of 2.0.0 PR4 the best place to learn about Sencha Touch 2's history support is kitchen sink example, which features lots of documentation on the routing and state restoration required for history support. 

从beta 1起将会有完整文档的历史支持从beta 1起。截至PR4 2.0.0最好了解Sencha Touch 2的历史支持是厨房的水槽示例,该功能很多的文档是由路由和状态恢复历史所支持的。
















