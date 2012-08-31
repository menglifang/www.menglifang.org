---
title: "搭建Ruby集成开发环境"
description: "介绍Ubuntu-12.04 LTS X64下ruby环境的配置。"
category: Technologies
tags: [ruby, rvm, vim, zsh, janus, ubuntu, javascript, rake, oh-my-zsh, git, github]
---

###安装Ubuntu

* 开机启动插入光盘，按F12进入启动设备选择，然后选择从光盘启动。
* 选择语言为英语，然后检查准备情况，要求磁盘空间足够，直接点“继续”按钮。
* 选择安装类型为删除全部并重新安装。
* 接下来进入磁盘分区，此次安装共分四个分区，其各分区属性如下：     

READMORE

```
挂载点     主分区/逻辑分区     文件类型      分区容量（MB）
/根分区       主分区           ext4         50000
/swap分区     主分区           swap         8000
/boot分区     主分区           ext2         200 
/home分区     主分区           ext4      余下所有容量
```

然后进行具体操作，双击“free space”（空闲空间），点击“New partition”（新建分区），每个分区按此步骤利用上述信息进行分区，点击“OK”即可，全部设置完毕后点击“继续”。

* 此时询问地区，点击“继续”或选择从地图上选择地区。
* 此时是键盘布局，选择“美式键盘”，点击继续。
* 这一步输入用户的姓名及密码，输入完成点击“继续”。
* 然后继续安装过程，此时Ubuntu会给出相应的系统介绍。
* 此时安装完毕，选择重启系统。

###安装RVM

RVM，是Ruby Version Manager的首字母简写，是个ruby版本管理器。如ubuntu系统之前安装了一个ruby，那在你安装了RVM之后还可以使用RVM来安装另一版本的Ruby（可以装很多个不同版本的ruby），然后RVM可以不同版本之间进行切换使用。开始安装rvm：

* 安装依赖的应用；

```bash
sudo apt-get install curl git-core
```

* 安装RVM；

```bash
curl -L https://get.rvm.io | bash -s stable
```

###安装Ruby

* 安装Ruby依赖；通过运行`rvm
   requirements`来查看待安装发行版ruby的依赖安装方法，并安装；
* 安装指定ruby发行版；如：

```bash
# 安装Ruby MRI 1.9.3
rvm install 1.9.3
```

* 设置默认的ruby版本。

```bash
rvm --default use 1.9.3
```

**注意：**

* 如果安装ruby的过程出现rubygems下载不成功（可能因为目标地址被墙），需要修改`~/.rvm/config/db`文件，找到`rubygems`下载路径配置项，将其修改为`http://rubyforge.org/frs/download.php/76073/rubygems-1.8.24.tgz`。如果rubygems版本不一样，请做相应修改。

* 如果执行`rvm`命令，出现如下提示：

```
  RVM is not a function, selecting rubies with 'rvm use ...' will not work
```

需要修改~/.bashrc，将`source ~/.rvm/scripts/rvm`加入到文件最后，并执行`source .bashrc`让配置生效。

###安装Janus

* 安装vim；

```bash
sudo apt-get install vim
```

* 安装工具；

```bash
sudo apt-get install ack-grep ctags
```

* 安装Janus。

```bash
curl -Lo- <https://bit.ly/janus-bootstrap> | bash
```

###安装oh-my-zsh

* 首先安装zsh；

```bash
sudo apt-get install zsh
```

* 安装oh-my-zsh；

```bash
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

* 配置启用的插件。修改~/.zshrc，设置：

```bash
plugins=(git git-flow bundler github heroku rails rails3 ruby rvm gem rake vi-mode)
```
