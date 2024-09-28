# win10任务栏图标空白的解决方案

![](media/reprint.png)

[慢慢慢时光](https://blog.csdn.net/wjl31802 "慢慢慢时光") ![](media/newCurrentTime2.png) 于 2020-06-07 16:13:10 发布 ![](media/articleReadEyes2.png) 8030 ![](media/tobarCollect2.png) 收藏 18

分类专栏： [技术杂谈](https://blog.csdn.net/wjl31802/category_8857334.html) 文章标签： [任务栏图标](https://so.csdn.net/so/search/s.do?q=%E4%BB%BB%E5%8A%A1%E6%A0%8F%E5%9B%BE%E6%A0%87&t=all&o=vip&s=&l=&f=&viparticle=) [空白](https://so.csdn.net/so/search/s.do?q=%E7%A9%BA%E7%99%BD&t=all&o=vip&s=&l=&f=&viparticle=) [win10](https://so.csdn.net/so/search/s.do?q=win10&t=all&o=vip&s=&l=&f=&viparticle=)

版权

 [![](media/20201014180756913.png) 技术杂谈 专栏收录该内容](https://blog.csdn.net/wjl31802/category_8857334.html "技术杂谈")

8 篇文章 2 订阅

订阅专栏

> 引用自[百度知道](https://zhidao.baidu.com/question/494555309826097932.html)，有时候会碰到该问题，记录和分享下

## 原因

在 Windows 10 系统中，为了加速图标的显示，当第一次bai对图标进du行显示时，系统会对文件或程序的图标进行缓zhi存。  
之后，当我们再次显示该图标时，系统会直接从缓存中读取数据，从而大大加快显示速度。  
当缓存文件出现问题时，就会引发系统图标显示不正常。  
因此，我们只需要将有问题的图标缓存文件删除掉，让系统重新建立图标缓存即可。

## 解决方案

> 前提：图标缓存文件是隐藏文件，我们需要在[资源管理器](https://so.csdn.net/so/search?q=%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8&spm=1001.2101.3001.7020)中将设置改为“显示所有文件”，显示隐藏的项目。

1.  按下快捷键 Win+R，在打开的运行窗口中输入 %localappdata%，回车。
2.  在打开的文件夹中，找到 Iconcache.db，将其删除。
3.  在任务栏上右击鼠标，在弹出的菜单中点击“任务管理器”。
4.  在任务管理器中找到“Windows资源管理器”，右击鼠标，选择“重新启动”即可重建图标缓存。