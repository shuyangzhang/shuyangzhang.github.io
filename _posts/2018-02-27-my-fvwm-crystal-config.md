---
layout:     post
title:      "我的fvwm-crystal设置"
subtitle:   "调教了半年的fvwm，终于用起来顺心了，记录一下踩过的坑"
date:       2018-02-27 10:57:00
author:     "Zhang Shuyang"
header-img: "img/post-bg-2015.jpg"
tags:
    - fvwm
    - fvwm-crystal
    - wm
    - windows manager
    - archlinux
    - linux
---

#### 前言

archlinux一直是最简洁但又定制性最强的linux发行版，其自身避免了一切不必要的添加、修饰，它的一切完全由用户自己决定，这也是我一直坚持使用arch的原因，就是很多地方可以“折腾”，并且在“折腾”的过程中能学习到linux的知识，也能因为自己DIY系统而感受到乐趣与成就感。
是否选择配置桌面环境一直是arch用户津津乐道的话题，而选用什么样的桌面环境，如何优化调教也是大家经常讨论的。之前我体验过重量级的Gnome、KDE，也用过略微轻量级的xfce、lxde，都觉得这并不是我最希望或者最期待的，自从我了解到了平铺式窗口管理器（即只使用窗口管理器windows manager，而放弃使用桌面化境desktop environment），我真的觉得打开了新世界的大门。而在平铺式窗口管理器中，使用人数最多的应该是i3wm和awesome，我是通过朋友的推荐，选择了一款并不是很出名但是非常轻量级的windows manager——fvwm。

#### fvwm与fvwm-crystal

[fvwm](http://www.fvwm.org/)是一款历史悠久的平铺式窗口管理器，而[fvwm-crystal](http://fvwm-crystal.sourceforge.net/)是根据fvwm定制的一款易用的、养眼的wm分支。其有很多默认的设置很方便，比如影音方面的设置，但是有一些地方的设置并不是满足每个人的胃口，所以相比较定制简单纯粹的fvwm而言，更改fvwm-crystal这种本身就预设了一些设置就显得更加麻烦，或者有很多坑和弯路，以下就记录了一些我认为不太满意的设置与更改方法。

#### emacs类输入习惯

由于fvwm-crstal的作者是一个vim用户，所以在设计的时候并没有考虑emacs用户的输入习惯。虽然我也是一个vim粉丝，但是有些emacs类的操作还是很方便的，比如在terminal中使用alt+b，alt+f进行以单词为单位的光标回退与前进，是非常高效的，但是fvwm-crystal自身在alt与其他字母键上绑定了很多功能导致该功能失效。
fvwm-crystal作者已经考虑到了这个问题，并且在FAQ中提供了解决方案，就是在preferences->key binding modifiers editor中，将Mod1 改为 4（即left windows），将SelectOnRelease改为Super_L(也是left windows)就行，这么改我尝试过，的确可以，但是等于将所有的alt快捷键全部挪到了windows键上，这个其实我可以适应，但是有一个小bug，就是在切换窗口时，本来的热键是alt+tab，现在变成了windows+tab，在松开windows键后并不能自动切换到选择的窗口，需要再点击一次，这个bug我在google上搜了很久，自己也尝试了很多种方法都未能解决，但是只要改回alt键，就没问题了，很奇怪，但是改回alt后，emacs类的操作习惯就无法被满足，最后的解决方案是，直接在fvwm的全局keybinding设置中将alt+b,alt+f等常用热键解绑，完美解决此问题。
具体方法是修改 ~/.fvwm-crystal/userconfig 文件，在其中加入两行

`Key F A M -`
`Key B A M -`

之前我尝试在 ~/.fvwm-crystal/preferences/CustomKeyModifiers 中修改并没有用，应该是配置文件优先级的问题。

#### 部分中文显示

由于fvwm的作者是english speaker所以在设计之初并没有考虑到其他语言用户的习惯问题，所以很多默认的设置是不支持中文字体的，比如窗口的标题栏与windows list栏，汉字的显示均是方块，解决此问题，需要在 ~/.fvwm-crystal/userconfig 文件，在其中加入一行

`Style * Font "xft:Noto Sans CJK SC"`

并且在 preferences -> Character fonts 中将 Noto Sans CJK SC light选为默认字体即可。
其中 Noto是google 的免费开源字体，sans指黑体，CJK代表ChineseJapaneseKorean，SC是simplified chinese。

#### 窗口焦点捕捉

fvwm默认的窗口焦点捕捉逻辑是move to focus即鼠标悬停的位置所在的窗口立即获得焦点，而作为一个多年的windows用户加之使用linux的这七年一直依赖桌面环境的我而言，是非常不习惯的，我还是习惯传统的click to focus即点击一下窗口才获得焦点的操作逻辑，针对这个问题，只需要修改 ~/.fvwm-crystal/userconfig 文件，在其中加入一行

`Style * ClickToFocus`

即可解决。

#### 写在最后

由于现在是研三了，即将毕业，没有那么多时间用于“折腾”，才选择的相对易用一些的fvwm-crystal而没有选择fvwm，因为时间真的很紧张，但是我依然非常非常喜欢折腾，喜欢调教自己的桌面环境，让它的每一个设置都符合我的操作习惯，每次遇到解决不了的问题，总会感到痛苦，甚至吃饭睡觉时候这个问题都会在脑子附近围绕，但是每次解决了问题的时候，体会到的愉悦感和成就感是无法言表的，总之，我爱archlinux，我爱折腾！



