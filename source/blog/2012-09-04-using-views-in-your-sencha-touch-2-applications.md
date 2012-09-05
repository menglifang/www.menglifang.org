---
title: 在你的Sencha Touch 2应用程序中使用视图
date: 2012/09/04
tags: sencha touch, javascript
---

**声明**：本文翻译自Sencha Touch官方文档。

**英文原版地址**：[http://docs.sencha.com/touch/2-0/#!/guide/views](http://docs.sencha.com/touch/2-0/#!/guide/views)


From the user's point of view, your application is just a collection of views.  Although much of the value of the app is in the Models and Controllers, the Views are what the user interacts with directly.  In this guide we're going to look at how to create the views that build your application. 

从用户的角度来看,你的应用程序应该是一个视图的集合。虽然大部分有用的应用是在模型和控制器,但视图是直接用户交互的。在构建你的应用程序的时候，这个指南会帮助我们创建视图。

###使用现有的组件

The easiest way to create a view is to just use Ext.create with an existing Component.  For example, if we wanted to create a simple Panel with some HTML inside we can just do this:
 
创建一个视图最简单的方法就是使用[http://docs.sencha.com/touch/2-0/#!/api/Ext-method-create](Ext.create)与现有的组件。例如,如果我们想要创建一个简单的面板和包含一些HTML内容在里面的页面，我们可以这样做:

```javascript
Ext.create('Ext.Panel', {
    html: 'Welcome to my app',
    fullscreen: true
});
```

This simply creates a Panel with some html and makes it fill the screen.  You can create any of our built-in components this way but best practice is to create a subclass with your specializations and then create that.  Thankfully that's simple too: 

这只是创建一个在全屏幕上显示的包含一些HTML内容的面板。在我们的内置组件中，你可以创建很多像这样的,但最佳方法是创建你专门用来做这样的事的一个子类,然后再创建。值得庆幸的是,这也是很简单的:

```javascript
Ext.define('MyApp.view.Welcome', {
    extend: 'Ext.Panel',

    config: {
        html: 'Welcome to my app',
        fullscreen: true
    }
});

Ext.create('MyApp.view.Welcome');
```

The outcome is the same, but now we have a brand new component that we can create any number of times.  This is the pattern we'll normally want to follow when building our app - create a subclass of an existing component then create an instance of it later.  Let's take a look through what we just changed: 

这样做和上面结果是相同的,但是现在我们有一个全新的组件,我们可以在任意时候创建任意数量。在构建我们的应用程序的时候我们通常的模式要遵循——创建一个子类的现有的组件之后去创建它的一个实例。让我们来看一看我们改变了什么:

* [http://docs.sencha.com/touch/2-0/#!/api/Ext-method-define](Ext.define) allows us to create a new class, extending an existing one (Ext.Panel in this case) 

* Ext.define让我们创建一个新的类,来扩展现有的([http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel](ext.Panel)在本例中)

We followed the MyApp.view. MyView convention for our new view class.  You can name it whatever you like but we suggest sticking with convention 

* 我们用myapp.view来约束我们的新建的MyView类。你可以用你喜欢的方式来命名，但还是推荐使用公约。

We defined config for the new class inside a config object 

* 我们使用config对象为新类定义它自己的参数

Any of the config options available on Ext.Panel can now be specified in either our new class's config block or when we come to create our instance.  When defining a subclass be sure to use the config object, when creating just pass in an object 

当我们创建我们实例的所有的配置选项的时候，都可以在[http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel](Ext.Panel)中作为我们新类的配置快。当定义一个子类在创建仅有一个传统的对象时就一定要使用的配置对象。

For example, here's the same code again but with additional configuration passed in with our Ext.create call: 

举例来说,下面是的相同的代码,而且通过使用额外的配置在我们的ext.create被调用时:

```javascript
Ext.define('MyApp.view.Welcome', {
    extend: 'Ext.Panel',

    config: {
        html: 'Welcome to my app',
        fullscreen: true
    }
});

Ext.create('MyApp.view.Welcome', {
    styleHtmlContent: true
});
```

###一个真实的例子

This is one of the view classes from our Twitter app: 

这里其中一个视图类是从我们Twitter应用程序中得到的:

```javascript
Ext.define('Twitter.view.SearchBar', {
    extend: 'Ext.Toolbar',
    xtype : 'searchbar',
    requires: ['Ext.field.Search'],

    config: {
        ui: 'searchbar',
        layout: 'vbox',
        cls: 'big',

        items: [
            {
                xtype: 'title',
                title: 'Twitter Search'
            },
            {
                xtype: 'searchfield',
                placeHolder: 'Search...'
            }
        ]
    }
});
```

This follows the same pattern as before - we've created a new class called Twitter.view. SearchBar, which extends the framework's Ext.Toolbar class.  We also passed in some configuration options, including a layout and an items array. 

这还和像以前一样要遵循相同的模式——我们已经创建了一个新类,叫作Twitter.view. SearchBar,它是扩展框架的Ext.Toolbar的类。我们还通过一些配置选项,增添了包括一个布局和一个项目的数组。

We've used a couple of new options this time:

这次我们使用二个新的参数

requires - because we're using a searchfield in our items array, we tell our new view to require the Ext.field. Search class.  At the moment the dynamic loading system does not recognize classes specified by xtype so we need to define the dependency manually 

* requires——由于我们在items数组中使用了一个searchfield，因此要告诉视图require（引用）Ext.field.Search类。之所以要这样做是因为此时的系统会自动加载功能还无法通过xtype识别这个类（Twitter.view.SearchBar），所以我们不得不手动定义这个依赖项。

xtype - gives our new class its own xtype, allowing us to easily create it in a configuration object (just like we did with searchfield above) 

* xtype——给我们的新类一个自己的xtype,让我们轻松地通过它的配置参数来创建它

This allows us to create instances of our new view class in a couple of ways: 

以后我们就可以通过两种方式来创建新视图（Twitter.view.SearchBar）的实例了。

```
//creates a standalone instance
Ext.create('Twitter.view.SearchBar');

//alternatively, use xtype to create our new class inside a Panel
Ext.create('Ext.Panel', {
    html: 'Welcome to my app',

    items: [
        {
            xtype: 'searchbar',
            docked: 'top'
        }
    ]
});
```

###自定义配置和处理

Sencha Touch 2 makes extensive use of the configuration system to provide predictable APIs and keep the code clean and easily testable.  We strongly suggest you do the same in your own classes. 

Sencha Touch 2大量使用的配置系统来提供可预测的API，并让代码的整洁和易于测试。我们强烈建议在你自己的类也用这样的方法。

Let's say you want to create a image viewer that pops up information about the image when you tap on it.  Our aim is to create a reusable view that can be configured with the image url, title and description, and displays the title and description when you tap on it. 

比方说，你要创建一个图像浏览器。当你点击它的时候要能弹出有关图像的信息。我们的目的是要创建一个重复是使用的视图,你可以配置图片url、标题、描述属性，并且当点触的时候会自动把标题和描述弹出来。

Most of the work around displaying images is taken care of for us by the Ext.Img component, so we'll subclass that: 

我们的[http://docs.sencha.com/touch/2-0/#!/api/Ext.Img](Eex.Img)组件大多数工作在围绕图片的显示,因此我们只要继承它就可以了:

```javascript
Ext.define('MyApp.view.Image', {
    extend: 'Ext.Img',

    config: {
        title: null,
        description: null
    },

    //sets up our tap event listener
    initialize: function() {
        this.callParent(arguments);

        this.element.on('tap', this.onTap, this);
    },

    //this is called whenever you tap on the image
    onTap: function() {
        Ext.Msg.alert(this.getTitle(), this.getDescription());
    }
});

//creates a full screen tappable image
Ext.create('MyApp.view.Image', {
    title: 'Orion Nebula',
    description: 'The Orion Nebula is rather pretty',

    src: 'http://apod.nasa.gov/apod/image/1202/oriondeep_andreo_960.jpg',
    fullscreen: true
});
```
We're adding two more configurations to our class - title and description - which both start off as null.  When we create an instance of our new class we pass the title and description configs in just like any other configuration. 

我们为我们的类新添两个配置,标题和描述——这两个开始是null。当我们创建新类的一个实例,像其他的配置一样可以把二个参数传递给标题和描述。

Our new behavior happens in the initialize and onTap functions.  The initialize function is called whenever any component is instantiated, so is a great place to set up behavior like event listeners.  The first thing we do is use this.callParent(arguments) to make sure the superclass' initialize function is called.  This is very important, omitting this line may cause your components not to behave correctly.
 
新定义的动作在初始化和点击的时候发生，每当组件实例化的时候都会被调用initialize函数，所以这个地方用来设置事件侦听器是最合适的。在这里我们做的第一件事是使用this.callParent(arguments)来确保父类的初始化函数已经被执行。这是很重要的，忽略这一步可能会导致你的组件无法正常的工作。

After callParent, we add a tap listener to the component's element, which will call our onTap function whenever the element is tapped.  All components in Sencha Touch 2 have an element property that you can use in this way to listen to events on the DOM objects, add or remove styling, or do anything else you'd normally do to an Ext.dom.Element.
 
callParent之后,我们将利用组件的元素来添加一个侦听器,当元素被窃听的时候它将调用我们的onTap函数。Sencha Touch 2中所有的组件都有一个元素属性,你可以用这种方式去侦听DOM对象的事件,添加或删除样式,或者用[http://docs.sencha.com/touch/2-0/#!/api/Ext.dom.Element](Ext.dom.Element)来做你以前做的其他事情。

The onTap function itself is pretty simple, it just uses Ext.Msg. alert to quickly pop up some information about the image.  Note that our two new configs - title and description - both receive generated getter functions (getTitle and getDescription respectively), as well as generated setter functions (setTitle and setDescription). 

这个onTap函数本身非常简单,它只使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.Msg-method-alert](Ext.Msg.alert)来迅速弹出一些图像信息。请注意,我们的两个新的configs——标题和描述——两个接收的getter函数(getTitle和getDescription),以及生成两个setter函数(setTitle和setDescription)。


###高级配置


When you create a new configuration option to a class, the getter and setter functions are generated for you so a config called 'border' is automatically given getBorder and setBorder functions: 

当你为一个类来创建一个新的配置选项,生成getter和setter函数。因此一个叫做border的配置参数会自动被给予getBorder和setBorder方法。

```javascript
Ext.define('MyApp.view.MyView', {
    extend: 'Ext.Panel',

    config: {
        border: 10
    }
});

var view = Ext.create('MyApp.view.MyView');

alert(view.getBorder()); //alerts 10

view.setBorder(15);
alert(view.getBorder()); //now alerts 15
```

The getter and setter aren't the only functions that are generated, there are a couple more that make life as a component author much simpler - applyBorder and updateBorder:
 
getter和setter并不是唯一生成的函数,还会生成一对让开发者感到实用性很强的方法，applyBorder和updateBorder：

```javascript
Ext.define('MyApp.view.MyView', {
    extend: 'Ext.Panel',

    config: {
        border: 0
    },

    applyBorder: function(value) {
        return value + "px solid red";
    },

    updateBorder: function(newValue, oldValue) {
        this.element.setStyle('border', newValue);
    }
});
```

Our applyBorder function is called internally any time the border configuration is set or changed, including when the component is first instantiated.  This is the best place to put any code that transforms an input value.  In this case we're going to take the border width passed in an return a CSS border specification string. 

applyBorder函数会在每次border参数设置或者改变的时候被调用，包括组件第一次被实例化时。因此这个地方很适合放置改变参数值格式的代码，在这个例子里我们可以把传入的border宽度值变成css字符串格式。

This means that when we set the view's border config to 10, our applyBorder function will make sure that we transform that value to '10px solid red'.  The apply function is totally optional but note that you must return a value from it or nothing will happen. 

这意味着当我们设置视图的边界配置到10,我们的applyBorder函数将确保我们改变这个值的“10px solid red”应用到我们的视图。apply函数用或者不用是完全可选的,但是请注意,你必须要返回一个值,或是没有什么会发生。

The updateBorder function is called after the applyBorder function has had a chance to transform the value, and is usually used to modify the DOM, send AJAX requests or perform any other kind of processing.  In our case we're just getting the view's element and updating the border style using setStyle.  This means that every time setBorder is called our DOM will immediately be updated to reflect the new style.
 
updateBorder函数被函数applyBorder调用后也有机会改变值,通常是用来修改DOM,发送AJAX请求或执行其他类型的处理。在我们的例子中我们刚刚使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.dom.Element-method-setStyle](setStyle)来活却视图的元素和更新边框样式。这意味真我们的DOM在每次setBorder后将会被立即执行，并反映在视图中去。

Here's an example of the new view in action.  Click the Code Editor button to see the source - basically we just create an instance of the new view and dock a spinner field at the top, allowing us to change the border width by tapping the spinner buttons.  We hook into the Spinner's spin event and call our view's new setBorder function from there: 

这里是一个新视图的示例代码。单击代码编辑器按钮来查看源代码——基本上我们只是创建一个新视图的实例和顶部码头spinner字段在,使我们能够利用微调控制的按钮来改变边框宽度。我们侦听spinner的spin事件，在里面调用视图setBorder方法:

```javascript
//as before
Ext.define('MyApp.view.MyView', {
    extend: 'Ext.Panel',
 
    config: {
        border: 0
    },
 
    applyBorder: function(value) {
        return value + "px solid red";
    },
 
    updateBorder: function(newValue, oldValue) {
        this.element.setStyle('border', newValue);
    }
});
 
//create an instance of MyView with a spinner field that updates the border config
var view = Ext.create('MyApp.view.MyView', {
    border: 5,
    fullscreen: true,
    styleHtmlContent: true,
    html: 'Tap the spinner to change the border config option',
    items: {
        xtype: 'spinnerfield',
        label: 'Border size',
        docked: 'top',
        value: 5,
        minValue: 0,
        maxValue: 100,
        increment: 1,
        listeners: {
            spin: function(spinner, value) {
                view.setBorder(value);
            }
        }
    }
});
```

###使用MVC模型

We recommend that most Sencha Touch applications should follow the MVC conventions so that your code remains well organized and reusable.  As the "V" in MVC, views also fit into this structure.  The conventions around views in MVC are very simple and follow directly from the naming convention we used above. 

我们建议大多数Sencha Touch的应用程序应该遵循[http://docs.sencha.com/touch/2-0/#!/guide/apps_intro](MVC conventions),这样你的代码可以看起来很有条理性和可重用性。作为MVC中“V”、视图也融入这个结构。在视图的约定中MVC是很简单的,可以直接按照上面使用的约定来命名。

Our MyApp.view. MyView class should be defined inside a file called app/view/MyView. js - this allows the Application to find and load it automatically.  If you're not already familiar with the file structure for MVC-based Sencha Touch apps, it's pretty simple - an app is just an html file, an app.js and a collection of models, views and controllers inside the app/model, app/view and app/controller directories: 

我们'app/view/MyView'类应该定义在一个文件名为'app/view/MyView. js' -这样能允许应用程序自动查找并加载它。如果你不熟悉这个文件结构基于mvc Sencha Touch应用程序的结构,这也是相当简单的——一个应用程序只是一个html文件,一个app.js和一个集合的模型、视图和控制器在app /model,app/view和app / controller目录:

```javascript
index.html
app.js
app/
    controller/
    model/
    view/
        MyView.js
```

You can create as many views as you want and organize them inside your app/view directory.  By specifying your application's views inside your app.js they'll be loaded automatically:
 
你可以创建许多视图,在你想要的app/view目录。通过你指定的应用程序的视图来自动加载你的app.js:

```javascript
//contents of app.js
Ext.application({
    name: 'MyApp',
    views: ['MyView'],

    launch: function() {
        Ext.create('MyApp.view.MyView');
    }
});
```

By following the simple view naming conventions we can easily load and create instances of our view classes inside our application.  The example above does exactly that - loads the MyView class and creates an instance of it in the application launch function.  To find out more about writing MVC apps in Sencha Touch please see the intro to apps guide.
 
通过下面的简单视图命名约定我们可以很容易地加载,并创建实例的视图类在我们的应用程序。上面的例子中就是这样做的——在应用程序启动时加载MyView类,并创建一个实例。想了解更多关于MVC应用程序在Sencha Touch 2 请参阅[http://docs.sencha.com/touch/2-0/#!/guide/apps_intro](入门应用指南)。






















