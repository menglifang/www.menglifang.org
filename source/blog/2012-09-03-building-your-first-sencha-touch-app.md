---
title: 建立你的第一个sencha touch应用程序
date: 2012/09/03
tags: sencha touch, javascript
---

###准备开始

This guide builds on the Getting Started Guide, which quickly gets you set up with the SDK and ensures your environment is fully functional. If you haven't been through this yet we suggest reading that guide first (it only takes a few minutes), then come back here to create your first app.

本指南是建立在[http://docs.sencha.com/touch/2-0/#!/guide/getting_started](入门指南)的基础上能让你很快设置好SDK,并确保你的环境是搭配好的能够正常使用。如果你还没有设置好SDK，建议去阅读入门指南(它只需要发费你几分钟的时间),然后你就可以回到这里来创建你的第一个应用程序。

What we're going to build

###我们要构建的是什么

Today we're going to build a simple mobile website-like app that could be used for your company's mobile site. We'll add a home page, a contact form and a simple list to fetch our recent blog posts and allow our visitor to read them right there on the phone.

今天，我们要构建一个简单的应用程序类似于移动网站，它可用于一个公司的移动网站。我们将添加一个主页，联系方式和一个简单的列表来获取我们最近的博客文章，并让我们的游客有权在他们的手机上阅读。

This is what we're going to build (it's interactive, try it yourself):

这是我们要构建的（你可以自己尝试，这是很有必要的）
```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        //The whole app UI lives in this tab panel
        Ext.Viewport.add({
            xtype: 'tabpanel',
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                // This is the home page, just some simple html
                {
                    title: 'Home',
                    iconCls: 'home',
                    cls: 'home',
                    html: [
                        '<img height=260 src="http://staging.sencha.com/img/sencha.png" />',
                        '<h1>Welcome to Sencha Touch</h1>',
                        "<p>Building the Getting Started app</p>",
                        '<h2>Sencha Touch (2.0.0)</h2>'
                    ].join("")
                },

                // This is the recent blogs page. It uses a tree store to load its data from blog.json
                {
                    xtype: 'nestedlist',
                    title: 'Blog',
                    iconCls: 'star',
                    cls: 'blog',
                    displayField: 'title',

                    store: {
                        type: 'tree',

                        fields: ['title', 'link', 'author', 'contentSnippet', 'content', {
                            name: 'leaf',
                            defaultValue: true
                        }],

                        root: {
                            leaf: false
                        },

                        proxy: {
                            type: 'jsonp',
                            url: 'https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.feedburner.com/SenchaBlog',
                            reader: {
                                type: 'json',
                                rootProperty: 'responseData.feed.entries'
                            }
                        }
                    },

                    detailCard: {
                        xtype: 'panel',
                        scrollable: true,
                        styleHtmlContent: true
                    },

                    listeners: {
                        itemtap: function(nestedList, list, index, element, post) {
                            this.getDetailCard().setHtml(post.get('content'));
                        }
                    }
                },

                // This is the contact page, which features a form and a button. The button submits the form
                {
                    xtype: 'formpanel',
                    title: 'Contact Us',
                    iconCls: 'user',
                    url: 'contact.php',
                    layout: 'vbox',

                    items: [
                        {
                            xtype: 'fieldset',
                            title: 'Contact Us',
                            instructions: 'Email address is optional',

                            items: [
                                {
                                    xtype: 'textfield',
                                    label: 'Name',
                                    name: 'name'
                                },
                                {
                                    xtype: 'emailfield',
                                    label: 'Email',
                                    name: 'email'
                                },
                                {
                                    xtype: 'textareafield',
                                    label: 'Message',
                                    name: 'message',
                                    height: 90
                                }
                            ]
                        },
                        {
                            xtype: 'button',
                            text: 'Send',
                            ui: 'confirm',

                            // The handler is called when the button is tapped
                            handler: function() {

                                // This looks up the items stack above, getting a reference to the first form it see
                                var form = this.up('formpanel');

                                // Sends an AJAX request with the form data to the url specified above (contact.php).
                                // The success callback is called if we get a non-error response from the server
                                form.submit({
                                    success: function() {
                                        // The callback function is run when the user taps the 'ok' button
                                        Ext.Msg.alert('Thank You', 'Your message has been received', function() {
                                            form.reset();
                                        });
                                    }
                                });
                            }
                        }
                    ]
                }
            ]
        });
    }
});
```

###开始我们的应用程序

The first thing we need to do is set up our application, just like we did in the Getting Started Guide. The app is using a tab panel that will hold the 4 pages so we'll start with that:

我们需要做的第一件事情就是建立我们的应用程序，就像我们在[http://docs.sencha.com/touch/2-0/#!/guide/getting_started](“入门指南”)中学习的一样。应用程序正在使用一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel](标签面板)，它将有的4个页面，现在让我们开始：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            items: [
                {
                    title: 'Home',
                    iconCls: 'home',
                    html: 'Welcome'
                }
            ]
        });
    }
});
```

If you run this in the browser (click the Preview button), a TabPanel should appear on top of the screen. The home page could be a bit more welcoming, so let's add some content to it and reposition the tab bar at the bottom of the page. We do that with the tabBarPosition config and by adding a bit of HTML:

如果你运行这个浏览器（点击“预览”按钮），在屏幕的顶部应该会出现一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel](Tab Panel)。这是一个很受欢迎的主页，让我们给它添加一些内容，并重新定位在页面底部的标签栏。我们可以这样配置[http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel-cfg-tabBarPosition](tabBarPosition)，并增加了一点的HTML内容：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    title: 'Home',
                    iconCls: 'home',
                    html: [
                        '<img src="http://staging.sencha.com/img/sencha.png" />',
                        '<h1>Welcome to Sencha Touch</h1>',
                        "<p>You're creating the Getting Started app. This demonstrates how ",
                        "to use tabs, lists and forms to create a simple app</p>",
                        '<h2>Sencha Touch (2.0.0)</h2>'
                    ].join("")
                }
            ]
        });
    }
});
```

Click the Preview button next to the example to have a look: you should see some HTML content but it won't look very good. We'll add a cls config and add it to the panel, adding a CSS class that we can target to make things look better. All of the CSS we're adding is in the examples/getting_started/index.html file in the SDK download. Here's how our home page looks now:

现在你可以点击预览按钮旁边的按钮查看这个例子，你应该能看到一些网页内容，当它不会很好看。我们将添加一个CLS的配置，并把它添加到面板上，增加一个CSS类。我们做的这些事情是为了让这个网页的内容看起来更好看。包括我们添加CSS的例子我们可以到/ getting_started / index.html中去下载。现在让我们刷新我们的页面：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    title: 'Home',
                    iconCls: 'home',
                    cls: 'home',

                    html: [
                        '<img src="http://staging.sencha.com/img/sencha.png" />',
                        '<h1>Welcome to Sencha Touch</h1>',
                        "<p>You're creating the Getting Started app. This demonstrates how ",
                        "to use tabs, lists and forms to create a simple app</p>",
                        '<h2>Sencha Touch 2</h2>'
                    ].join("")
                }
            ]
        });
    }
});
```
###增加博客页面

Now that we have a decent looking home page, it's time to move on to the next screen. In order to keep the code for each page easy to follow we're just going to create one tab at a time and then combine them all together again at the end.

现在,我们有一个看上去很不错的主页,让我们继续下一个页面。为了让每一个页面的代码都去遵守相同的规则，我们仅需要创建一个页面的选项卡,结束时再将它们组合在一起。

For now, we'll remove the first tab and replace it with a List. We're going to be using Google's Feed API service to fetch the feeds. There's a bit more code involved this time so first let's take a look at the result, then we can see how it's accomplished:

现在，让我们删除第一个选项卡用一个列表来取代它。我们将使用谷歌的[https://developers.google.com/feed/v1/reference](Feed APL service)来获取资料。这将有更多的代码参与这项工作让我们先来看一看，我们将可以看到它是如何完成的：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    xtype: 'nestedlist',
                    title: 'Blog',
                    iconCls: 'star',
                    displayField: 'title',

                    store: {
                        type: 'tree',

                        fields: [
                            'title', 'link', 'author', 'contentSnippet', 'content',
                            {name: 'leaf', defaultValue: true}
                        ],

                        root: {
                            leaf: false
                        },

                        proxy: {
                            type: 'jsonp',
                            url: 'https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.feedburner.com/SenchaBlog',
                            reader: {
                                type: 'json',
                                rootProperty: 'responseData.feed.entries'
                            }
                        }
                    },

                    detailCard: {
                        xtype: 'panel',
                        scrollable: true,
                        styleHtmlContent: true
                    },

                    listeners: {
                        itemtap: function(nestedList, list, index, element, post) {
                            this.getDetailCard().setHtml(post.get('content'));
                        }
                    }
                }
            ]
        });
    }
});
```

You can click the 'Code Editor' button above the example to see the full code, but we'll go over it piece by piece. Instead of a panel we're using a nestedlist this time, fetching the most recent blog posts from sencha.com/blog. We're using Nested List so that we can drill down into the blog entry itself by simply tapping on the list.

你可以点击“代码编辑器”上面的按钮来查看完整的例子代码，它看起来是一块一块的。而不是一个面板，这个时候我们使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList](nestedlist_)，从sencha.com/blog中获取最新的博客。我们使用嵌套列表，这样我们通过点击博客题目就可以进入到博客的具体内容。

Let's break down the code above, starting with just the list itself:

让我们分析一下上面的代码，从列表本身开始：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    xtype: 'nestedlist',
                    title: 'Blog',
                    iconCls: 'star',
                    displayField: 'title',

                    store: {
                        type: 'tree',

                        fields: [
                            'title', 'link', 'author', 'contentSnippet', 'content',
                            {name: 'leaf', defaultValue: true}
                        ],

                        root: {
                            leaf: false
                        },

                        proxy: {
                            type: 'jsonp',
                            url: 'https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.feedburner.com/SenchaBlog',
                            reader: {
                                type: 'json',
                                rootProperty: 'responseData.feed.entries'
                            }
                        }
                    }
                }
            ]
        });
    }
});
```

We're giving the Nested List a few simple configurations - title, iconCls and displayField - and a more detailed one called store. The Store config tells the nested list how to fetch its data. Let's go over each store config in turn:

我们为嵌套列表做几个简单的配置——title、iconCls和displayField——和一个更详细的一种叫做[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-cfg-store](Store)。Store配置告诉嵌套列表如何获取它的数据。反过来让我们看一下每个存储配置:

type: tree creates a tree store, which NestedList uses

* type:创建树的[http://docs.sencha.com/touch/2-0/#!/api/Ext.data.TreeStore](tree store)，使用NestedList

fields tells the Store what fields we're expecting in the blog data (title, content, author etc)

* 字段告诉Store哪些字段是我们在博客中期待读取的数据(如标题、内容、作者等)

proxy tells the Store where to fetch its data from. We'll examine this more closely in a moment

* 数据代理，将告诉Store在那去读取数据。关于数据代理我们会有更详细的讲解，在这就不做太多的解释

root tells the root node it is not a leaf. Above we'd set the leaf defaultValue to true so we need to override that for the root

* 在这我们设置根节点为叶子节点，并将defaultValue设置为true隐藏根节点

Of all the Store configurations, proxy is doing the most work. We're telling the proxy to use Google's [https://developers.google.com/feed/v1/reference](Feed API service) to return our blog data in JSON-P format. This allows us to easily grab feed data from any blog and view it in our app (for example try swapping the Sencha blog url for http://rss.slashdot.org/Slashdot/slashdot above to see it fetch Slashdot's feed).

所有的存储配置,代理所做的大部分工作。是告诉代理如何的去使用谷歌的 Feed API服务上的资料，并以JSON-P的格式返回我们的博客上需要的数据。这让我们能够通过我们的应用程序查看并攫取任何博客上的数据(例如试着交换Sencha blog http://rss.slashdot.org/Slashdot/slashdot上面的url看到获取Slashdot的资料)。

The last part of the proxy definition was a Reader. The reader is what decodes the response from Google into useful data. When Google sends us back the blog data, they nest it inside a JSON object that looks a bit like this:

代理定义的最后部分是一个阅读器。读者该怎样解码谷歌的传回来的响应，将传来的数据转化为有用的数据。当谷歌传回我们需要的博客数据，他们会把它们放在一个JSON对象里面，它看起来像这样：

```javascript
{
    responseData: {
        feed: {
            entries: [
                {author: 'Bob', title: 'Great Post', content: 'Really good content...'}
            ]
        }
    }
}
```

The bit we care about is the entries array, so we just set our Reader's rootProperty to 'responseData.feed.entries' and let the framework do the rest.

我们关心的是数组，所以我们只是将我们的读者[http://docs.sencha.com/touch/2-0/#!/api/Ext.data.reader.Json-cfg-rootProperty](rootProperty)属性设置为“responseData.feed.entries'，让框架的继续完成其余的部分。

###数据的挖掘

Now that we have our nested list fetching and showing data, the last thing we need to do is to allow the user to tap on an entry to read it. We're just going to add two more configurations to our Nested List to finish this off:

现在，我们有了获取和显示数据的嵌套列表，我们需要做的最后一件事就是让用户点击一个标题进行阅读。我们只要添加几个这样的配置，我们就能够让嵌套列表来完成这个功能：

```javascript
{
    xtype: 'nestedlist',
    //all other configurations as above

    detailCard: {
        xtype: 'panel',
        scrollable: true,
        styleHtmlContent: true
    },

    listeners: {
        itemtap: function(nestedList, list, index, element, post) {
            this.getDetailCard().setHtml(post.get('content'));
        }
    }
}
```

Here we've just set up a detailCard, which is a useful feature of Nested List that allows you to show a different view when a user taps on an item. We've configured our detailCard to be a scrollable Panel that uses styleHtmlContent to make the text look good.

这里我们刚刚建立了一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-cfg-detailCard](detailCard),这是一个很有用的功能,当一个用户点击一个标题的时候会让你的嵌套列表来显示一个不同的观点。这里我们已经配置我们的detailCard是一个可滚动[http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel](Panel),使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel-cfg-styleHtmlContent](styleHtmlContent)这让文本看起来更友好。

The final piece of the puzzle is adding an itemtap listener, which just calls our function whenever an item is tapped on. All our function does is set the detailCard's HTML to the content of the post you just tapped on and the framework takes care of the rest, animating the detail card into view to make the post appear. This was the only line of code we had to write to make the blog reader work.

最后的难题是如何为[http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-event-itemtap](itemtap)添加事件一个侦听,每一次点击它都能够调用我们的函数。我们所有的功能是设置detailCard的HTML的内容后，你就能点击和让框架负责完成其余,让用户选择点击的博客动态的显示到页面中来。这是作为一个博客系统读取的开发着，是我们不得不写的一行代码。

###建立联系方式

The final thing we're going to do for our app is create a contact form. We're just going to take the user's name, email address and a message, and use a FieldSet to make it look good. The code for this one is simple:

最后，我们要为我们的应用程序创建一个联系方式。我们需要的是用户的姓名，电子邮件地址和一个信息，并使用[http://docs.sencha.com/touch/2-0/#!/api/Ext.form.FieldSet](自定义字段)来管理。下面是具体的代码部分：

```javascript
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    title: 'Contact',
                    iconCls: 'user',
                    xtype: 'formpanel',
                    url: 'contact.php',
                    layout: 'vbox',

                    items: [
                        {
                            xtype: 'fieldset',
                            title: 'Contact Us',
                            instructions: '(email address is optional)',
                            items: [
                                {
                                    xtype: 'textfield',
                                    label: 'Name'
                                },
                                {
                                    xtype: 'emailfield',
                                    label: 'Email'
                                },
                                {
                                    xtype: 'textareafield',
                                    label: 'Message'
                                }
                            ]
                        },
                        {
                            xtype: 'button',
                            text: 'Send',
                            ui: 'confirm',
                            handler: function() {
                                this.up('formpanel').submit();
                            }
                        }
                    ]
                }
            ]
        });
    }
});
```

This time we're just creating a form that contains a fieldset. The fieldset contains three fields - one for name, one for email and one for a message. We've using a VBox layout, which just arranges the items vertically on the page one above the other.

这一次,我们创建一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.form.Panel](表单),它包含一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.form.FieldSet](自定义字段)。这个自定义字段包含三个字段——一个名字,一个用于电子邮件和一个用于消息。我们使用一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.layout.VBox](VBox layout),vbox layout用于垂直方向的布局


At the bottom we have a Button with a tap handler. This employs the useful up method, which returns the form panel that the button is inside of. We then just call submit to submit the form, which sends it to the url we specified above ('contact.php').

在屏幕的底部有一个[http://docs.sencha.com/touch/2-0/#!/api/Ext.Button](按钮)和 [http://docs.sencha.com/touch/2-0/#!/api/Ext.Button-cfg-handler](处理)程序。采用这种[http://docs.sencha.com/touch/2-0/#!/api/Ext.Container-method-up](有用的方法)，能让该方法在单击按钮时返回的是表单面板的内部的内容。然后，我们只需要调用[http://docs.sencha.com/touch/2-0/#!/api/Ext.form.Panel-method-submit](提交)表单的功能，并将其发送到我们上面指定的URL（“contact.php”）。


###代码的集合

Now that we've created each view individually it's time to bring them all together into our completed app.

现在，我们要将我们在上面建立的单独的视图放在一起，这样就能完成我们的应用程序。

```javascript
//We've added a third and final item to our tab panel - scroll down to see it
Ext.application({
    name: 'Sencha',

    launch: function() {
        Ext.create("Ext.tab.Panel", {
            fullscreen: true,
            tabBarPosition: 'bottom',

            items: [
                {
                    title: 'Home',
                    iconCls: 'home',
                    cls: 'home',
                    html: [
                        '<img width="65%" src="http://staging.sencha.com/img/sencha.png" />',
                        '<h1>Welcome to Sencha Touch</h1>',
                        "<p>You're creating the Getting Started app. This demonstrates how ",
                        "to use tabs, lists and forms to create a simple app</p>",
                        '<h2>Sencha Touch 2</h2>'
                    ].join("")
                },
                {
                    xtype: 'nestedlist',
                    title: 'Blog',
                    iconCls: 'star',
                    displayField: 'title',

                    store: {
                        type: 'tree',

                        fields: [
                            'title', 'link', 'author', 'contentSnippet', 'content',
                            {name: 'leaf', defaultValue: true}
                        ],

                        root: {
                            leaf: false
                        },

                        proxy: {
                            type: 'jsonp',
                            url: 'https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.feedburner.com/SenchaBlog',
                            reader: {
                                type: 'json',
                                rootProperty: 'responseData.feed.entries'
                            }
                        }
                    },

                    detailCard: {
                        xtype: 'panel',
                        scrollable: true,
                        styleHtmlContent: true
                    },

                    listeners: {
                        itemtap: function(nestedList, list, index, element, post) {
                            this.getDetailCard().setHtml(post.get('content'));
                        }
                    }
                },
                //this is the new item
                {
                    title: 'Contact',
                    iconCls: 'user',
                    xtype: 'formpanel',
                    url: 'contact.php',
                    layout: 'vbox',

                    items: [
                        {
                            xtype: 'fieldset',
                            title: 'Contact Us',
                            instructions: '(email address is optional)',
                            items: [
                                {
                                    xtype: 'textfield',
                                    label: 'Name'
                                },
                                {
                                    xtype: 'emailfield',
                                    label: 'Email'
                                },
                                {
                                    xtype: 'textareafield',
                                    label: 'Message'
                                }
                            ]
                        },
                        {
                            xtype: 'button',
                            text: 'Send',
                            ui: 'confirm',
                            handler: function() {
                                this.up('formpanel').submit();
                            }
                        }
                    ]
                }
            ]
        });
    }
});
```
You can find the full source code of the getting started app in the examples/getting_started folder of the Sencha Touch 2.0 SDK download.

你也可以在开始下载的Sencha 2.0中examples/getting_started文件夹中找到完整的源代码



