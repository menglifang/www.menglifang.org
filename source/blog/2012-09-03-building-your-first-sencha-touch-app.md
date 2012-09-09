---
title: 建立你的第一个Sencha Touch应用程序
date: 2012/09/03
tags: sencha touch, javascript
---

**声明**：本文翻译自Sencha Touch官方文档。

**英文原版地址**：[http://docs.sencha.com/touch/2-0/#!/guide/first_app](http://docs.sencha.com/touch/2-0/#!/guide/first_app)

###准备开始

本指南是建立在的[Sencha Touch 2入门指南](/blog/2012/08/31/getting-started-with-sencha-touch-2/)基础上，能帮助你快速的设置好SDK，并确保你的环境工作正常。如果你还没有设置好SDK，建议去阅读[Sencha Touch 2入门指南](/blog/2012/08/31/getting-started-with-sencha-touch-2/)（只需要花费你几分钟的时间），然后你就可以回到这里来创建你的第一个应用程序。

###我们要构建的是什么

今天，我们要构建一个简单的应用程序类似于移动网站，它可用于你公司的移动网站。我们将添加一个主页，联系方式和一个简单的列表来获取我们最近的博客文章，并让我们的访客可以在他们的手机进行阅读。


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

我们需要做的第一件事情就是建立我们的应用程序，就像我们在[Sencha Touch 2 入门指南](/blog/2012/08/31/getting-started-with-sencha-touch-2/)中学习的一样。应用程序使用一个[标签面板](http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel])，面板中放置4个页面，现在让我们开始：

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

如果你在浏览器中运行这个程序，在屏幕的顶部应该会出现一个[标签面板](http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel])。让我们给它添加一些内容，并将标签栏放在底部，使得这个主页更受欢迎。我们通过配置[tabBarPosition](http://docs.sencha.com/touch/2-0/#!/api/Ext.tab.Panel-cfg-tabBarPosition)，并增加了一点的HTML内容来实现：

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

现在你可以点击例子边上的预览按钮来查看（请访问原文出处查看），你应该能看到一些HTML内容，当它不太好看。我们将添加一个`cls`配置，把它添加到面板上。通过增加一个CSS类，我们可以让这个网页的内容看起来更好看。我们添加的CSS在SDK的`examples/getting_started/index.html`中。现在让我们刷新我们的页面，看现在变成什么样子了：

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

现在，我们有一个看上去很不错的主页，让我们继续下一个页面。为了让每一个页面的代码都遵守相同的规则，我们每次创建一个选项卡，到最后再将它们组合在一起。

现在，让我们删除第一个选项卡并用一个列表来取代它。我们将使用谷歌的[Feed API service](https://developers.google.com/feed/v1/reference)来获取资料。这将有更多的代码参与这项工作中，让我们先来看一看它是如何完成的：

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

你可以点击“代码编辑器”按钮来查看完整的例子代码（原文功能）。接下来，我们将一块一块来分析。我们使用[nestedlist](http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList)来取代了面板，并从`sencha.com/blog`中获取最新的博客。我们使用嵌套列表，这样我们通过点击博客题目就可以进入到博客的具体内容。

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

我们为嵌套列表做几个简单的配置 —— title、iconCls和displayField —— 和一个复杂一些的配置[store](http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-cfg-store)。Store配置告诉嵌套列表如何获取它的数据。回过头来，让我们看一store的配置:

* `type: tree`创建一个NestedList使用的[tree store](http://docs.sencha.com/touch/2-0/#!/api/Ext.data.TreeStore)。

* `fields`指定Store哪些字段是我们在博客中期待读取的数据(如标题、内容、作者等)。

* `proxy`指定Store在哪去读取数据。关于`proxy`我们会有更详细的讲解，在这就不做太多的解释了。

* `root`指定根节点，表明它不是一个叶节点。之前我们已经将叶节点的`defaultValue`设置为`true`，所以在此我们需要为根节点重新定义。

所有`Store`的配置，`proxy`将完成大部分工作。我们告诉`proxy`如何的去使用谷歌的[Feed API服务](https://developers.google.com/feed/v1/reference)上的资料，并以`JSON-P`的格式返回我们的博客上需要的数据。这让我们能够通过我们的应用程序查看并攫取任何博客上的数据（例如将Sencha的博客地址换成http://rss.slashdot.org/Slashdot/slashdot，就能获取Slashdot的资料)。

`proxy`定义的最后部分是一个阅读器。阅读器完成的工作是将从Google返回的数据解码成可用的数据。当谷歌传回我们需要的博客数据，他们会把它们放在一个JSON对象里面，它看起来像这样：

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

我们关心的是`entries`数组，所以我们只需要将阅读器的[rootProperty](http://docs.sencha.com/touch/2-0/#!/api/Ext.data.reader.Json-cfg-rootProperty)属性设置为“responseData.feed.entries'，让框架的完成其余的部分便可。

###数据的挖掘

现在，我们有了获取和显示数据的嵌套列表，我们需要做的最后一件事就是让用户点击一个标题进行阅读。我们只要添加另外两个配置，就能够让嵌套列表来完成这个功能：

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

这里我们刚刚建立了一个[detailCard](http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-cfg-detailCard)，这是嵌套列表一个很有用的功能，当一个用户点击一个标题的时候，会让你的嵌套列表来显示一个不同的试图。这里我们已经配置我们的`detailCard`为一个可滚动[Panel](http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel)，使用[styleHtmlContent](http://docs.sencha.com/touch/2-0/#!/api/Ext.Panel-cfg-styleHtmlContent)让文本看起来更友好。

最后的难题是如何为[itemtap](http://docs.sencha.com/touch/2-0/#!/api/Ext.dataview.NestedList-event-itemtap)添加事件一个侦听，每一次点击它都能够调用我们的函数。这个函数完成的功能是设置`detailCard`的HTML为你点击的博客的内容，如何使用友好的方式将这个详细卡放入试图，并显示这篇博客则由框架来处理。这是为了让这个博客阅读器正常工作，我们必须写的一行代码。

###添加联系方式

The final thing we're going to do for our app is create a contact form. We're just going to take the user's name, email address and a message, and use a FieldSet to make it look good. The code for this one is simple:

最后，我们要为我们的应用程序添加一个联系方式。我们需要的是用户的姓名，电子邮件地址和一个消息，并使用[FieldSet](http://docs.sencha.com/touch/2-0/#!/api/Ext.form.FieldSet)使其更加美观。下面是具体的代码部分：

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

这一次，我们创建一个[Form](http://docs.sencha.com/touch/2-0/#!/api/Ext.form.Panel)，它包含一个[fieldset](http://docs.sencha.com/touch/2-0/#!/api/Ext.form.FieldSet)。这个`fieldset`包含三个字段：名字、电子邮件和消息。我们使用了[VBox](http://docs.sencha.com/touch/2-0/#!/api/Ext.layout.VBox)布局，将这些控件垂直排列。

在屏幕的底部，有一个与点击处理器绑定好的[Button](http://docs.sencha.com/touch/2-0/#!/api/Ext.Button)。里面通过调用[up](http://docs.sencha.com/touch/2-0/#!/api/Ext.Container-method-up)方法，获取到这个按钮所在的表单面板的对象。然后，我们只需要调用表单的[submit](http://docs.sencha.com/touch/2-0/#!/api/Ext.form.Panel-method-submit)方法提交表单，就把表单发送到我们上面指定的URL（“contact.php”）。

###代码的集合

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

你也可以在下载的Sencha 2.0 SDK的`examples/getting_started`文件夹中找到这个入门应用的完整源代码。



