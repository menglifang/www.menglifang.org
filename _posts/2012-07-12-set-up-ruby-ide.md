---
layout: post
title: "搭建Ruby集成开发环境"
description: "介绍Ubuntu-12.04 LTS X64下ruby环境的配置。"
category: Technologies
tags: [ruby, rvm, vim, zsh, janus, ubuntu, javascript, rake, post, oh-my-zsh, git, github]
---
{% include JB/setup %}
####安装Ubuntu
1 开机启动插入光盘，按F12进入启动设备选择，然后选择从光盘启动。

2 选择语言为英语，然后检查准备情况，要求磁盘空间足够，直接点“继续”按钮。

3 选择安装类型为删除全部并重新安装。

4 接下来进入磁盘分区，此次安装共分四个分区，其各分区属性如下：     

***    

    挂载点     主分区/逻辑分区     文件类型      分区容量（MB）

    /根分区       主分区           ext4         50000

    /swap分区     主分区           swap         8000

    /boot分区     主分区           ext2         200 

    /home分区     主分区           ext4      余下所有容量

***
然后进行具体操作，双击“free space”（空闲空间），点击“New partition”（新建分区），每个分区按此步骤利用上述信息进行分区，点击“OK”即可，全部设置完毕后点击“继续”。

5 此时询问地区，点击“继续”或选择从地图上选择地区。

6 此时是键盘布局，选择“美式键盘”，点击继续。

7 这一步输入用户的姓名及密码，输入完成点击“继续”。

8 然后继续安装过程，此时Ubuntu会给出相应的系统介绍。

9 此时安装完毕，选择重启系统。

####安装RVM
RVM，是Ruby Version Manager的首字母简写，是个ruby版本管理器。如ubuntu系统之前安装了一个ruby，那在你安装了RVM之后还可以使用RVM来安装另一版本的Ruby（可以装很多个不同版本的ruby），然后RVM可以不同版本之间进行切换使用。开始安装rvm

1 安装RVM前我们要安装curl：*sudo apt-get install curl*。

2 由链接<https://rvm.io/rvm/>进入官网，安装稳定版：user$ *curl -L https://get.rvm.io | bash -s stable*。

3 此时安装完成，同时已将Git安装上了

####安装Ruby
1 通过命令可*rvm requirementsvp*可得出安装的依赖包，在安装Ruby前要将这些依赖包全部装上。

2 基于rvm安装ruby，即*rvm install 1.9.3*（注意，此时可能会出错“RVM is not a function, selecting rubies with 'rvm use ...' will not work”，解决方法为修改.bashrc,将source ~/.rvm/scripts/rvm加入到.bashrc最后一行，最后输入“*source .bashrc*”这样使得刚添加的内容得以生效）。安装以后加入“source ~/.rvm/scripts/rvm”到“.bashrc”最后一行，此时可以进行版本检测输入“*rvm version*”。

3 将ruby1.9.3设为默认的ruby版本，其命令为$ *rvm --default use 1.9.3*。

####安装Janus
1 安装vim： *sudo apt-get install vim* 。

2 安装Janus前要确保已经安装ack-grep, ctags, git, ruby和rake。

***
        安装ack-grep：sudo apt-get install ack-grep
        安装ctags：sudo apt-get install ctags
        git, ruby, rake 已安装    
***
3 键入$*curl -Lo- <https://bit.ly/janus-bootstrap> | bash*进行安装Janus。

####安装oh-my-zsh
1 若没有安装zsh要首先安装zsh：*sudo apt-get install zsh*。

2 键入*curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh*进行安装oh-my-zsh。修改plugins=(git)为plugins=(git git-flow bundler github heroku rails rails3 ruby rvm gem rake vi-mode)。

