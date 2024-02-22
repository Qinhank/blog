---
title: 折腾一个好看的windows终端
date: 2021-04-16 14:28:00
tags:
categories:
---
#  阅读前介绍
为了更好的阅读，对以后的文章进行多平台同步并且统一采用markdown语法的方式。

当前文章支持阅览的地址有：
1.  微信公众号
2.  https://blog.hankqin.com/2021/04/16/折腾一个好看的windows终端
3.  https://telegra.ph/折腾一个好看的windows终端
#  正文
> 任何文章的初衷都可以用是什么、为什么、怎么办来解释。
##  是什么？
前几天逛v2ex的时候碰到一个说关于有什么好的命令行工具的帖子，于是进去阅读，没想阅读完去搜索，居然还挺好看，于是本着我不折腾谁折腾的精神，我开启了我漫长的定制之路。

至于命令行工具是什么我并不能解释清楚，但假如你是一位程序员你一定知道我在表达什么。
##  为什么？
我的第一代命令行工具使用的是`git bash`，它挺不错，但是也并非完美，我在使用过程中就遇到过乱码、无法打开很多个窗口等问题，而且它长得还非常中规中矩，让人喜欢不起来。

接着我又使用了`cmder`，它解决了多窗口的问题，也多了很多功能和样式，可以说还算是一个好工具。但依旧长得中规中矩。

于是我就在想，是不是有一款可以即强大又可以足够美观的工具。

于是就有了我的这一次折腾。
##  怎么办？
###  折腾的前提
你需要有足够的耐心，非常足够的耐心，你可能会中途遇到很多莫名其妙的问题，你可能会因为一个问题引发另一个问题，最后要一路解决完才能继续教程的下一步，这都是非常正常的，这也是为什么说需要有足够的耐心。

当然，为了这样一个工具是否值得花费自己那么大精力和时间也需要自己去衡量。
###  正式开始
1.  在Microsoft Store商店里搜索下载`Windows Terminal`
2.  下载完成后打开它，你将看到一个非常中规中矩的命令行界面
3.  安装`posh-git`和`oh-my-posh`
```javascript
// 安装方式：
posh-git： Install-Module posh-git -Scope CurrentUser 
oh-my-posh：Install-Module oh-my-posh -Scope CurrentUser
注：有可能遇到不允许安装脚本，可输入set-executionpolicy remotesigned解决
```
4.  创建或更改$PROFILE配置
```javascript
// 如果之前没有配置文件，就新建一个 PowerShell 配置文件
if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
// 用记事本打开配置文件，也可以用vscode打开：code $PROFILE
notepad $PROFILE
```
5.  对$PROFILE配置新增数据
```cmd
Import-Module posh-git 
Import-Module oh-my-posh 
Set-Theme spaceship
```
6.  下载字体
> 前往[nerdfonts](https://www.nerdfonts.com/font-downloads)下载**Caskaydia Cove Nerd Font**字体，打开后可以看到后缀为ttf的字体，点击安装
7.  设置字体
```cmd
1.  打开Windows Terminal
2.  点击下箭头，选择设置
3.  选择打开JSON文件
4.  找到profiles => list
5.  在第一个数组对象里添加字段："fontFace": "CaskaydiaCove NF"
6.  保存
```
### 基本完成
自此整个工具就安装完成了，但是要达到美化还有一步，就是设置背景和样式。