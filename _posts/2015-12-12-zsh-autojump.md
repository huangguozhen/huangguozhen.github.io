---
layout: post
title: Ubuntu14.04使用zsh和autojump
date: 2015-12-12 20:19:38
categories: 系统工具
excerpt: 记录Linux下开发的一些工具
---

* content
{:toc}

## 一，使用Zsh的九大理由

1.  **完全兼容bash**  
    兼容bash意味着我们不需要太多学习成本就可以切换过来，意味这我以前在bash下积累的shell语法，基本操作都不会荒废。

2.  **全模式更方便**  
    zsh 中按两下tab键可以触发zsh的补全，所有补全项都可以通过键盘方向键或者<Ctrl-n/p/f/b>来选择。

3.  **支持命令选项补全**  
    zsh除了支持目录的补全，还支持命令选项的补全，例如ls -<TAB><TAB>会直接列出所有ls参数，再也不会出现一个命令打到
    一半忘记参数导致重开一个terminal man一把。

4.  **支持命令参数补全**  
    以前想 kill 掉一个进程，我的做法是 ps aux | grep "进程名" 然后记下 id，再 kill id。在 zsh 下，只需要 kill 进
    程名<TAB>，zsh 就会自动补全进程的 pid。

5.  **支持更加聪明的目录补全**  
    以前比如想进入一个比较深的目录，比如 /Users/pw/workspace/project/src/main/webapps/static/js，就得在 bash 下面
    打半天，不停的 tab 去补全一个正确的路径出来。在 zsh 下，只需要输入每个路径的头字母然后 tab 一下：
    cd /u/p/w/p/s/m/w/s/j<TAB>

6.  **强大的快速目录切换**  
    以前最苦逼的事情莫过于频繁在两个工作目录下切换，总要打一长串 cd 路径。也尝试过 popd 和 pushd 来解决这个问题，
    但往往是目录已经切换了才想起来没用 pushd。而 zsh 会记住你每一次切换的路径，然后通过 1 来切换到你上一次访问的路径，
    2 切换到上上次……一直到 9，还可以通过 d 查看目录访问历史。
    zsh 还可以配合 autojump 一起使用，autojump 会记录下每一个你访问过的目录，然后通过 j 来快速跳转。

7.  **支持全局alias和后缀名alias**  
    bash 的 alias 只能做命令的缩写，而 zsh 更进一步，使 alias 可以缩写命令的一部分，例如参数或环境变量设置。

8.  **有丰富多彩的命令行提示符**  
    bash 下通过设置 $PS1 已经可以实现很丰富的提示符了，而 zsh 更进一步，可以实现诸如多行提示符、提示符右对齐等功能。
    oh-my-zsh 配置文件中提供了非常丰富的提示符 theme 供选择，我使用的是 gentoo 主题，比较简洁，还可以显示当前 git 仓库的状态。

9.  **有更多优雅的语法**  
    例如修改 PATH，bash 下设置 $PATH 要求所有路径都要写在一行里，目录多了以后看起来就很难看。zsh 支持更加符合程序员审美观的设置方式。

## 二，安装zsh

    sudo apt-get -y install zsh
    # 设置为默认shell
    sudo chsh -s /bin/zsh

## 三，安装Oh My Zsh
Oh My Zsh 是一个开源的，社区驱动的Zsh配置管理框架。

### 依赖条件
* 必须安装 curl 或者 wget
* 必须安装 git

通过curl安装:

    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

通过wget安装:

    sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

## 四，使用Oh My Zsh

### 使用插件
通过配置~/.zshrc文件可以添加你需要的zsh插件。例如：

    plugins=(git ubuntu autojump)

阅读各个插件的Readme可以获取更多关于插件的信息。

### 使用主题
默认主题是Oh My Zsh作者Robby的主题。在~/.zshrc文件中你可以看到像以下的环境设置：

    ZSH_THEME="robbyrussell"

这套主题我并不喜欢。如果你想改变主题，只要改变一下ZSH_THEME的值为你喜欢的主题即可。例如:

    ZSH_THEME="agnoster" #我目前用的就是这个主题（需要powerline字库支持）

### 自定义主题和插件
如果你想重写默认zsh行为，只需要把相应.zsh后缀的文件添加到custom/目录即可。

如果想自己写插件，只需要把他写入到命名像XYZ.plugin.zsh格式的文件中，并他把它放到custom/plugins文件夹中并使用即可。

如果想重写Oh My Zsh提供的插件，只要命名为与他名字相同的自定义插件即可覆盖。

### 更新和卸载
默认情况下，Oh My Zsh每周都会提醒你更新。如果你想要不提醒自动更新。设置~/.zshrc

    DISABLE_UPDATE_PROMPT=true

如果不想自动更新，设置~/.zshrc

    DISABLE_AUTO_UPDATE=true

另外，可以使用tools目录下工具进行更新或者卸载管理

    chmod a+x tools/uninstall.sh
    tools/uninstall.sh # 卸载
    tools/upgrade.sh #更新

## 五，安装autojump

    git clone git://github.com/joelthelion/autojump.git

    cd autojump
    ./install.py or ./uninstall.py
