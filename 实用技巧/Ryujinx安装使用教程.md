-   [](https://www.bilibili.com)

[主站](https://www.bilibili.com)-   [番剧](https://www.bilibili.com/anime/)
-   [游戏中心](https://game.bilibili.com/platform/)
-   [直播](https://live.bilibili.com)
-   [会员购](https://show.bilibili.com/platform/home.html?msource=pc_web)
-   [漫画](https://manga.bilibili.com?from=bill_top_mnav)
-   [赛事](https://www.bilibili.com/match/home/)
-   [放放假](https://www.bilibili.com/blackboard/activity-rNcxJPitgl.html)
-   [
    
    MSI
    
    ![menuName](media/menuName.png)
    
    MSI
    
    
    
    ](https://www.bilibili.com/blackboard/activity-eZKDp2LJND.html)
-   [](https://app.bilibili.com)

-   [下载客户端](https://app.bilibili.com)

登录

大会员

消息

动态

收藏

历史记录

[创作中心](https://member.bilibili.com/platform/home?from_spm_id=333.976.top_bar.creation)

投稿

[专栏](https://www.bilibili.com/read/home?from=articleDetail)/Ryujinx安装使用教程

# Ryujinx安装使用教程

2020-03-08 02:4725.0万阅读 · 3016喜欢 · 583评论

[

![](media/6353d683518f1bedbd2c4e7396375a834848a71c.jpg@96w_96h_1c_1s.webp)

](https://space.bilibili.com/66958652)

[一花の一界](https://space.bilibili.com/66958652)

粉丝：3291文章：6

关注

# 前排说明

  

评论区现在放不了链接了，所有的文件都去下面的专栏下载  

评论区现在放不了链接了，所有的文件都去下面的专栏下载

评论区现在放不了链接了，所有的文件都去下面的专栏下载

[

yuzu模拟器 Ryujinx模拟器 分享

制作教程及不定期更新需要大量时间和实验，请大家多多三连投币，现在收藏比投币和点赞加起来还多，不要白嫖呀 /(ㄒoㄒ)/~~零、必看前排说明0.1 写这个文章的目的原来的文章的评论区吞链接，我发的分享链接在评论区不显示，只有我一个人能看到，所以新开一个文章把链接放到这里来。除以上原因为，我还把大家经常问的问题在本篇文章里做了一个总结，建议大家看一看，应该是很有用的。0.2 两款模拟器的区别，必看目前主要有两款模拟器，一款是yuzu，一款是Ryujinx。在以前，这两款模拟器的区别就是：Ryujinx兼容性更

文章 一花の一界 1084 52 24



](https://www.bilibili.com/read/cv23421642?from=articleDetail)

![](media/4aa545dccf7de8d4a93c2b2b8e3265ac0a26d216.png@progressive.webp)

#   
零.懒人必看，解压即用

    如果你嫌教程麻烦，看不懂，评论区置顶分享了解压即用版（只提供Windows版，其他系统请按照下面的正式教程自己配置），直接解压即可使用。解压即用版本的配置文件在压缩包内的portable目录中，你玩游戏的存档、安装的DLC和update、我帮你安装好的固件、我帮你配置好的key文件全在这个portable目录中。

![](media/fd4a69f27b834ba78d7a83aa4391b5a4710d5fa4.png@942w_717h_progressive.webp)

配置目录

    如果想更新模拟器，记得备份portable目录，可以直接去官网下载最新的模拟器，解压之后，直接把原来的portable目录放到最新模拟器与ryujinx.exe同级目录下即可，不需要再重新进行任何配置，之前的游戏和存档都还在。

    以下开始正式教程，如果懒得看，想直接玩，可以直接跳过下面的教程，去评论区下载解压即用版即可。如何添加游戏可以查看下面的第五步。其他常用设置，如安装DLC和update、配置手柄等，可以看我另一篇文章。

  

![](media/02db465212d3c374a43c60fa2625cc1caeaab796.png@progressive.webp)

  

# 一.Ryujinx介绍

    Ryujinx（或许可以译为龙神？）是由gdkchan用C＃编写的switch开源模拟器，相对于另一个模拟器yuzu，它的兼容性更好，但运行速度更慢，目前以支持Vulkan，运行流畅，且软件支持中文。关于Ryujinx的更多内容可以进入官网查看：https://ryujinx.org/

前排提示，必看！！！  

  

  

1.  文章所用到的所有文件请到上面的文章查看，会不定期更新分享最新的模拟器
    
2.  本教程的环境是windows系统，但Linux和Mac也可以用，教程通用
    
3.  AMD的显卡可能会有问题
    
4.  如果教程中某些网页打不开，直接去评论置顶找相应的文件即可
    
5.  附另一个模拟器yuzu安装教程
    
    yuzu的配置要求相对低一点，Ryujinx兼容性更好，目前也支持中文和Vulkan，运行也很流畅，大家都可以试试。
    

# 二.官网下载

    1.进入官网：https://ryujinx.org/

    2.在最上方点击DOWNLOAD

    3.下载对应的版本即可

官网下载

# 三.安装  

    1.把下载的压缩包解压到一个没有中文的路径下

    2.进入目录，在Ryujinx.exe的同级目录下，新建portable目录

新建portable目录

    3.双击打开Ryujinx.exe，有弹窗点ok和yes就行了  

    4.进入第2步创建的portable目录，可以看到生成了一些目录和文件  

       进入system目录，把key文件（一般包括prod.keys和title.keys，请到评论置顶下载key文件）放进去

放入key文件

    5.关闭目录，关闭模拟器，重新打开模拟器(这时应该不会再有弹窗了)  

# 四.安装Firmware(固件)

    1.去评论区置顶下载Firmware固件

    2.在模拟器最上方依次点击Tools→Install Firmware→Install a firmware from XCI or ZIP

    3.在弹出的窗口中选择你刚刚下载的固件Firmware x.x.x.zip，然后点open  

    4.安装成功可以在模拟器右下角看到固件版本号，如下图片所示

    5.若出现报错，如下图所示，是因为步骤三的key文件与Firmware不匹配，key文件要与Firmware匹配才可以安装成功，因此你若想安装新的firmware，则需要去找对应的新的key文件

安装失败

# 五.选择游戏目录  

    1.依次点击Options→Settings，打开设置

    2.在General选项卡，点击Add

    3.在弹出的窗口中选择你的游戏目录，然后点击右下角的Add

    4.点击save，然后应该就可以看到你的游戏了

# 六.常用设置说明（最好看一下）

常用设置请看我的另一篇文章，包括软件设置、如何安装dlc、如何安装update、手柄设置等

# **七.END**

    到这里教程就结束了，感谢大家慢慢看完。

    作者一开始也是一个小白，什么也不懂，慢慢看贴吧的大佬的教程，照着做，但是还是遇到了很多问题，总是云里雾里，东找西找，很是麻烦，因此特地写这个教程，希望能给小白一点帮助。

    大家遇到的问题可以在评论区说，也可以去Ryujinx贴吧去问。当然，最好确定自己照着教程的步骤做了，发现有问题再问。

    最后感谢Ryujinx制作组们和网友的无私奉献，若有写的不对的地方，欢饮大家在评论区指出来，若是帮助到了你，也请大家给我点个赞，投个币，一键三连支持一下，(*^▽^*)  

    制作本教程以及定期分享不易，拜托大家多多投币三连

  

本文为我原创

[Ryujinx](https://search.bilibili.com/article?keyword=Ryujinx&from_source=article) [Ryujinx模拟器](https://search.bilibili.com/article?keyword=Ryujinx模拟器&from_source=article) [模拟器](https://search.bilibili.com/article?keyword=模拟器&from_source=article) [switch模拟器](https://search.bilibili.com/article?keyword=switch模拟器&from_source=article)

投诉或建议

推荐文章

[更多精彩内容](https://www.bilibili.com/read/home)

[

可以不玩，但不能没有！限时0折的诱惑谁能挡得住？！【本周steam史低游戏推荐】

白给游戏： 00:18 太空堡垒卡拉狄加僵局 05:11 省流表格 01:04 小偷模拟器（新史低11 01:34 畸战求生（平史低17 02...

陈皮橘皮西瓜皮

全部笔记 8670 571 0

](https://www.bilibili.com/read/cv22910123?from=articleDetail)

评论

-   全部评论

-   按时间排序

目录

3016

1812

5535

562

登录哔哩哔哩，高清视频免费看！

![](https://s1.hdslb.com/bfs/seed/jinkela/header-v2/images/login-tv-man.svg)

更多登录后权益等你解锁

立即登录