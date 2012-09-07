---
title: Sencha Touch 2 控制器
date: 2012/09/04
tags: sencha touch, javascript
---

**声明**：本文翻译自Sencha Touch官方文档。

**英文原版地址**：[http://docs.sencha.com/touch/2-0/#!/guide/controllers](http://docs.sencha.com/touch/2-0/#!/guide/controllers)




Controllers are responsible for responding to events that occur within your app. If your app contains a Logout button that your user can tap on, a Controller would listen to the Button's tap event and take the appropriate action.  It allows the View classes to handle the display of data and the Model classes to handle theloading and saving of data - the Controller is the glue that binds them together. 

控制器负责响应你的应用程序中发生的事件。如果你的应用程序包含一个注销按钮,那么当你的用户可以点击该按钮,控制器会监听该按钮的点击事件,并采取适当的处理。它允许视图类来显示处理的数据和模型类来处理theloading与保存的数据——控制器是他们维系在一起的粘合剂。

###Relation to Ext.app.Application

Controllers exist within the context of an Application.  An Application usually consists of a number of Controllers, each of which handle a specific part of the app. For example, an Application that handles the orders for an online shopping site might have controllers for Orders, Customers and Products.
 
在一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application](应用程序)中，控制器是无处不在。一个应用程序通常含有多个控制器,每个控制器都会处理特定的一部分应用程序。例如，一个在线购物的应用程序网站，可能有订单，客户和产品的控制器。

All of the Controllers that an Application uses are specified in the Application's Ext.app.Application. controllers config.  The Application automatically instantiates each Controller and keepsreferences to each, so it is unusual to need to instantiate Controllers directly.  By convention each Controller is named after the thing (usually the Model) that it deals with primarily, usually in the plural - for example if your app is called 'MyApp' and you have a Controller that manages Products, convention is to create a MyApp.controller. Products class in the file app/controller/Products.js.
 
配置应用程序使用的所有控制器都应指定[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Application-cfg-controllers](Ext.app.Application.controllers)应用。。应用程序自动实例化到每个控制器和每个keepsreferences,但做到这样是不容易的,所以需要直接实例化控制器。按照惯例每个控制器命名的东西(通常是模型),通常在复数它是以处理为主——例如,如果你的应用程序被称为‘MyApp’,那么你会有一个控制器,用于管理产品,习惯上是创建一个'MyApp.controller'。产品类文件'app/controller/Products.js'。

###Launching

There are 4 main phases in your Application's launch process, 2 of which are inside Controller.  Firstly, each Controller is able to define an init function, which is called before the Application launch function.  Secondly, after the Application and Profile launch functions have been called, the Controller's launch function is called as the last phase of the process: 

在你的应用程序的启动过程中有4个主要阶段,其中2是在控制器。首先,每个控制器定义一个init函数,它被称为应用程序启动之前的函数。其次,是在应用程序和配置文件启动功能,控制器的整个启动功能的过程被称为最后阶段:

* 1.Controller#init functions called

* 2.Profile#launch function called

* 3.Application#launch function called

* 4.Controller#launch functions called

Most of the time your Controller-specific launch logic should go into your Controller's launch function.  Because this is called after the Application and Profile launch functions, your app's initial UI is expected to be in place by this point.  If you need to do some Controller-specific processing before app launch you can implement a Controller init function. 

大多数时间你的控制器会按特定的启动逻辑进入相应的控制器启动功能。这是因为按照相应的应用程序和配置文件而运行的启动功能,你的应用程序会预先制定这一UI的初始。在应用程序启动前，如果你需要做一些控制器特殊处理，你可以定义一个控制器的init函数。

###refs and control

The centerpiece of Controllers is the twin configurations refs and control.  These are used to easily gain references to Components inside your app and to take action on them based on events that they fire.  Let's look at refs first: 

控制器的二个核心配置是[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-refs](refs)和[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-control](control)。这些都能轻松的获取你的应用程序中的组件和事件。关于它们，让我们看看一些[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-refs](refs):

Refs

Refs leverage the powerful ComponentQuery syntax to easily locate Components on your page.  We can define as many refs as we like for each Controller, for example here we define a ref called 'nav' that finds a Component on the page with the ID 'mainNav'.  We then use that ref in the addLogoutButton beneath it: 

在你的页面上Refs会利用其强大的[http://docs.sencha.com/touch/2-0/#!/api/Ext.ComponentQuery](ComponentQuery)语法能够很容易找到页面上的组件。我们可以为我们喜欢的控制器定义为许多refs,例如这里我们定义了一个名为“导航”的控制器,refs会在页面上找到一个ID“mainNav”的组件。然后,我们在addLogoutButton使用ref:

```javascript
Ext.define('MyApp.controller.Main', {
    extend: 'Ext.app.Controller',

    config: {
        refs: {
            nav: '#mainNav'
        }
    },

    addLogoutButton: function() {
        this.getNav().add({
            text: 'Logout'
        });
    }
});
```
Usually, a ref is just a key/value pair - the key ('nav' in this case) is the name of the reference that will be generated, the value ('#mainNav' in this case) is the ComponentQuery selector that will be used to find the Component. 
通常,一个ref对应的只是一个键/值,键(在“导航”这种情况下)引用该名称,将生成的参考值(在”# mainNav”这种情况下)送[http://docs.sencha.com/touch/2-0/#!/api/Ext.ComponentQuery](ComponentQuery)选择器下,该选择器将用来查找组件。

Underneath that, we have created a simple function called addLogoutButton which uses this ref via its generated 'getNav' function.  These getter functions are generated based on the refs you define and always follow the same format - 'get' followed by the capitalized ref name.  In this case we're treating the nav reference as though it's a Toolbar, and adding a Logout button to it when our function is called.  This ref would recognize a Toolbar like this: 

下面,我们创建了一个简单的函数,名为addLogoutButton.它会通过使用ref其生成的getNav”功能。这些基于你refs定义生成的getter函数总是遵循相同的格式——“get”紧随其后的是大写的ref的名字。在这种情况下我们对待nav参考好像它是一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.Toolbar](工具栏),并添加一个注销按钮。当我们的函数被调用，这refs会默认一个像这样的工具栏:

```javascript
Ext.create('Ext.Toolbar', {
    id: 'mainNav',

    items: [
        {
            text: 'Some Button'
        }
    ]
});
```
Assuming this Toolbar has already been created by the time we run our 'addLogoutButton' function (we'll see how that is invoked later), it will get the second button added to it. 

假设这个工具栏在被创建的时候,我们已经运行我们的“addLogoutButton”功能(我们将会看到以后如何被调用),它将获得它的第二个按钮。

Advanced Refs

Refs can also be passed a couple of additional options, beyond name and selector.  These are autoCreate and xtype, which are almost always used together: 

Refs除了名称和选择器之外还有几个额外的选项。如autoCreate和xtype,它们总是一起被使用:

```javascript
Ext.define('MyApp.controller.Main', {
    extend: 'Ext.app.Controller',

    config: {
        refs: {
            nav: '#mainNav',

            infoPanel: {
                selector: 'tabpanel panel[name=fish] infopanel',
                xtype: 'infopanel',
                autoCreate: true
            }
        }
    }
})
```

We've added a second ref to our Controller.  Again the name is the key, 'infoPanel' in this case, but this time we've passed an object as the value instead.  This time we've used a slightly more complex selector query - in this example imagine that your app contains a tab panel and that one of the items in the tab panel has been given the name 'fish'.  Our selector matches any Component with the xtype 'infopanel' inside that tab panel item. 

我们添加第二个ref到我们的控制器。Again the name is the key, 'infoPanel' in this case, but this time we've passed an object as the value instead。这次我们已经使用了一个稍微复杂的选择器查询——在这个示例中应用程序包含一个选项卡面板和选项卡面板中的项目，被称作“fish”。我们的选择器匹配任何组件与xtype“infopanel”里面的选项卡面板项目。

The difference here is that if that infopanel does not exist already inside the 'fish' panel, it will be automatically created when you call this. getInfoPanel inside your Controller.  The Controller is able to do this because we provided the xtype to instantiate with in the event that the selector did not return anything. 

不同的是,如果这infopanel已经不存在“fish”面板中的时候,当你调用它的时候它会被自动创建。你的getInfoPanel控制器是可以做到这一点,因为我们提供了xtype实例化的事件，选择器不返回任何值。

控制

The sister config to refs is control.  Control is the means by which your listen to events fired by Components and have your Controller react in some way.  Control accepts both ComponentQuery selectors and refs as its keys, and listener objects as values - for example:
 
The sister config to [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-refs](refs) is [http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Controller-cfg-control](control.Control)的方式是监听启动组件的事件和你的控制器的某种方式做出的反应。控制接受ComponentQuery选择器和refs作为其键值,以及侦听器对象值,例如:

```javascript
Ext.define('MyApp.controller.Main', {
    extend: 'Ext.app.Controller',

    config: {
        control: {
            loginButton: {
                tap: 'doLogin'
            },
            'button[action=logout]': {
                tap: 'doLogout'
            }
        },

        refs: {
            loginButton: 'button[action=login]'
        }
    },

    doLogin: function() {
        // called whenever the Login button is tapped
    },

    doLogout: function() {
        // called whenever any Button with action=logout is tapped
    }
});
```

Here we have set up two control declarations - one for our loginButton ref and the other for any Button on the page that has been given the action 'logout'.  For each declaration we passed in a single event handler - in each case listening for the 'tap' event, specifying the action that should be called when that Button fires the tap event.  Note that we specified the 'doLogin' and 'doLogout' methods as strings inside the control block - this is important. 

这里我们已经声明了建立了两个控制——一个用于我们的loginButton ref和其他任何按钮的页面,该页面已经考虑到处理的“注销”。对于每个声明我们通过一个事件处理程序,在每种情况下听“logout”事件,指定的行为被称为'tap'事件，当触发该按钮的点击事情的时候，指定的处理行为应该被执行。注意,我们这里指定了“doLogin”和“doLogout”方法作为字符串在控制块——这是很重要的。

You can listen to as many events as you like in each control declaration, and mix and match ComponentQuery selectors and refs as the keys. 
你可以为你喜欢的控制声明许多事件,去混合和匹配ComponentQuery选择器和refs作为控制的键。

###Routes

As of Sencha Touch 2, Controllers can now directly specify which routes they are interested in. This enables us to provide history support within our app, as well as the ability to deeply link to any part of the application that we provide a route for.
 
在Sencha Touch 2中控制器现在可以直接指定哪些路线是它们感兴趣。这使我们能够在我们的应用程序中提供对历史支持,提供了这样一个路径能够深入的链接到任何应用程序的任何一部分。

For example, let's say we have a Controller responsible for logging in and viewing user profiles, and want to make those screens accessible via urls.  We could achieve that like this: 

例如,假设我们有一个控制器负责控制登录并查看用户档案,并希望可以能通过url去访问那些页面。我们现在可以做这样一点去实现:

```javascript
Ext.define('MyApp.controller.Users', {
    extend: 'Ext.app.Controller',

    config: {
        routes: {
            'login': 'showLogin',
            'user/:id': 'showUserById'
        },

        refs: {
            main: '#mainTabPanel'
        }
    },

    // uses our 'main' ref above to add a loginpanel to our main TabPanel (note that
    // 'loginpanel' is a custom xtype created for this application)
    showLogin: function() {
        this.getMain().add({
            xtype: 'loginpanel'
        });
    },

    // Loads the User then adds a 'userprofile' view to the main TabPanel
    showUserById: function(id) {
        MyApp.model.User.load(id, {
            scope: this,
            success: function(user) {
                this.getMain().add({
                    xtype: 'userprofile',
                    user: user
                });
            }
        });
    }
});
```

The routes we specified above simply map the contents of the browser address bar to a Controller function to call when that route is matched.  The routes can be simple text like the login route, which matches against http://myapp.com/#login, or contain wildcards like the 'user/:id' route, which matches urls like http://myapp.com/#user/123.  Whenever the address changes the Controller automatically calls the function specified. 

我们在上述简单的路线图中规定，其内容可以通过浏览器地址栏映射到控制器要调用的函数进行路径匹配。这个路线可以是简单的文本，如登录路线,它匹配对http://myapp.com/ #login,或包含通配符像“use/:id“ route,它匹配的url像http://myapp.com/ #user/ 123。每当地址改变控制器自动调用指定的函数。

Note that in the showUserById function we had to first load the User instance.  Whenever you use a route, the function that is called by that route is completely responsible for loading its data and restoring state.  This is because your user could either send that url to another person or simply refresh the page, which we wipe clear any cached data you had already loaded.  There is a more thorough discussion of restoring state with routes in the application architecture guides. 

注意,在showUserById函数我们必须首先加载用户实例。每当你使用一个route,调用route的函数,被称为是完全负责装载它的数据和恢复状态。这是因为你的用户可以发送url给其他人或简单的刷新页面,我们可以清除你已经加载的任何数据缓存。在route的应用程序架构指南中有一个更深入的讨论关于恢复状态与路线。

###Before Filters

The final thing that Controllers provide within the context of Routing is the ability to define filter functions that are run before the function specified in the route.  These are an excellent place to authenticate or authorize users for specific actions, or to load classes that are not yet on the page.  For example, let's say we want to authenticate a user before allowing them to edit a Product in an e-commerce backend: 

控制器能提供在路由范围内的最后一个功能是能够定义过滤功能，这是路由在之前运行中指定的功能。这是一个很好的功能就是能够对特定的用户进行身份验证或授权,或在尚未加载累的页面上加载类。例如,假设我们想验证一个用户是否能允许他在一个电子商务网站的后台编辑一个产品:

```javascript
Ext.define('MyApp.controller.Products', {
    config: {
        before: {
            editProduct: 'authenticate'
        },

        routes: {
            'product/edit/:id': 'editProduct'
        }
    },

    // this is not directly because our before filter is called first
    editProduct: function() {
        //... performs the product editing logic
    },

    // this is run before editProduct
    authenticate: function(action) {
        MyApp.authenticate({
            success: function() {
                action.resume();
            },
            failure: function() {
                Ext.Msg.alert('Not Logged In', "You can't do that, you're not logged in");
            }
        });
    }
});
```
Whenever the user navigates to a url like http://myapp.com/#product/edit/123 the Controller's authenticate function will be called and passed the Ext.app. Action that would have been executed if the before filter did not exist.  An Action simply represents the Controller, function (editProduct in this case) and other data like the ID parsed from the url. 

每当用户浏览到诸如这样的url，http://myapp.com/#product/edit/123控制器的验证函数会就被调用,通过[http://docs.sencha.com/touch/2-0/#!/api/Ext.app.Action](Ext.app. Action)就会被执行,如果在过滤器并不存在。一个行动仅仅代表了控制器,函数(在本例中editProduct)和其他数据就会被url的ID进行解析。

The filter can now perform any kind of processing it needs to, either synchronously or asynchronously.  In this case we're using our application's authenticate function to check that the user is currently logged in. This could entail an AJAX request to check the user's credentials on the server so it runs asynchronously - if the authentication was successful we continue the action by calling action.resume(), if not we tell the user that they need to log in first. 

现在，过滤器可以执行任何类型的处理需要,同步或异步处理。在本例中我们使用我们的应用程序的验证函数来检查当前登录的用户。这意味着在服务器上会运行一个异步的AJAX请求,以检查用户的凭证——如果认证成功我们继续执行通过调用action.resume(),如果告诉我们不是我们的用户,那么他们就必须先登录。

Before filters can also be used to load additional classes before certain actions are performed.  For example, if some actions are rarely used you may wish to defer loading of their source code until they are needed so that the application boots up faster.  To achieve this you can simply set up a filter that uses Ext.Loader to load code on demand. 

在过滤器还可以用来在加载其他类之前进行某些操作。例如,如果有些处理在你的应用程序中是很被使用的，你希望延迟加载它们,直到需要它们的时候才会加载到应用程序中。要完成此操作,你可以简单地设置一个过滤器,它使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.Loader](Ext.Loader)加载程序来加载需求的代码。

Any number of before filters can be specified for each action, to use more than one filter just pass in an array: 

在每一个处理中都可以指定任意数量的前置过滤器，只要是在阵列中使用到的过滤器：

```javascript
Ext.define('MyApp.controller.Products', {
    config: {
        before: {
            editProduct: ['authenticate', 'ensureLoaded']
        },

        routes: {
            'product/edit/:id': 'editProduct'
        }
    },

    // this is not directly because our before filter is called first
    editProduct: function() {
        //... performs the product editing logic
    },

    // this is the first filter that is called
    authenticate: function(action) {
        MyApp.authenticate({
            success: function() {
                action.resume();
            },
            failure: function() {
                Ext.Msg.alert('Not Logged In', "You can't do that, you're not logged in");
            }
        });
    },

    // this is the second filter that is called
    ensureLoaded: function(action) {
        Ext.require(['MyApp.custom.Class', 'MyApp.another.Class'], function() {
            action.resume();
        });
    }
});
```

The filters are called in order, and must each call action.resume() to continue the processing. 

该过滤器是有秩序的,必须每次都要调用action.resume()方能继续处理。

###Profile-specific Controllers

Superclass, shared stuff:

```
Ext.define('MyApp.controller.Users', {
    extend: 'Ext.app.Controller',

    config: {
        routes: {
            'login': 'showLogin'
        },

        refs: {
            loginPanel: {
                selector: 'loginpanel',
                xtype: 'loginpanel',
                autoCreate: true
            }
        },

        control: {
            'logoutbutton': {
                tap: 'logout'
            }
        }
    },

    logout: function() {
        // code to close the user's session
    }
});
```

Phone Controller:

```javascript
Ext.define('MyApp.controller.phone.Users', {
    extend: 'MypApp.controller.Users',

    config: {
        refs: {
            nav: '#mainNav'
        }
    },

    showLogin: function() {
        this.getNav().setActiveItem(this.getLoginPanel());
    }
});
```
Tablet Controller:

```javascript
Ext.define('MyApp.controller.tablet.Users', {
    extend: 'MyApp.controller.Users',

    showLogin: function() {
        this.getLoginPanel().show();
    }
});
```
























