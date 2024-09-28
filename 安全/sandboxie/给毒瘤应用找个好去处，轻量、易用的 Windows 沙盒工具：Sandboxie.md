[![化学心情下2](media/化学心情下2.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

2020 年 03 月 29 日

**编注：**目前 Sandboxie 项目已经不再维护，出于安全性和功能性考虑，如果你有类似的需求，不妨跟随这份 [指南](https://sspai.com/link?target=https%3A%2F%2Fcomparite.ch%2Fsandboxing) 查找替代产品。

---

一直以来我们都希望为 Windows 系统中的软件创造一个隔离环境：从系统安全的角度出发，我们可以将软件的权利限制在一个隔离的空间当中，避免其对主系统运行造成影响；另一方面，创造隔离环境还可以实现一些新的功能特性，比如「双开」某些软件，或者是打造安全的上网环境临时提供给其它成员使用等。

不过目前 Windows 上可以实现软件隔离的方案并不算多，此前我们曾经 [介绍过](https://sspai.com/post/58668) 使用 Windows 自带权限工具，通过建立账号的形式来实现软件隔离，好处是对于系统性能的影响不大，但上手操作等较为复杂，另一个方案则是通过虚拟机来实现，虽然操作较为容易，但是对设备硬件有要求（设备最好支持硬件虚拟化）并且运行效率不高，如果只是为了解决一些日常使用需求还显得有些大材小用。

![](media/276ebc4fe26aed0bebc476f888f07ff2.png)

虚拟机对于日常使用场景而言显得过于笨重

因此最为合适的解决方案显然就是「沙盒」了。微软曾在 2019 年 5 月更新中加入了原生的沙盒功能，但这个系统沙盒「用后即焚」，关闭后也无法保存数据以供下次使用，每次启动都必须重新安装；同时它本质上也采用了虚拟化技术，因此对系统版本（需要专业版/企业版 Windows 10）和环境（需要虚拟化支持）也有要求。

![](media/f565d87222a1f405a7edbfcac438b09d.png)

Windows 沙盒需要虚拟化支持

所以如果你需要一个性能开销小、更加简单易用的 Windows 隔离方案，最近刚刚宣布转为免费软件的老牌沙盒软件 Sandboxie 就值得重新考虑一下了。

## 为什么选择 Sandboxie

作为老牌的沙盒软件，Sandboxie 相比上文提到的隔离方案有着相当大的优势。

首先 Sandboxie 实现方式无需虚拟化，它的工作原理类似于在真实操作系统的运行层之上新建了一个安全运行区域，因此沙盒内软件的启动和运行速度几乎都与真实系统环境下无异。

![](media/e00a52fbc6f2f0f4ff9747271fca6e3c.png)

请输入图片标题

其次，Sandboxie 对于系统版本的要求也较为宽松，除了目前主流的 Windows 10 之外，早期的 Windows 8.1、Windows 7 等版本都可以使用。更重要的是 Sandboxie 支持将沙盒中的数据拷贝回真实系统，在 Sandboxie 关闭或重启后，下一次依然可以继续使用此前的环境和数据，无需反复配置。

## Sandboxie 的基本运行原理

和很多安全软件相同，安装 Sandboxie 时软件会提示将在系统中安装驱动组件等，并且在第一次启动时通过引导页再次说明软件的实现原理以及一些基础的使用方法。

![](media/bda7f87ad2a0219c16d79759d57facb2.gif)

请输入图片标题

安装完成 Sandboxie 之后，真实系统的桌面上便会出现「使用 Sandboxie 打开默认浏览器」的快捷方式，双击这个快捷方式就会直接在沙盒中启动网页浏览器。

![](media/f0bf9edeeb4875f23bb00b481a248401.png)

请输入图片标题

整个在沙盒上使用网页浏览器其实和真实系统中使用基本没有什么差别，因为并非是基于虚拟机所以几乎看不到卡顿和延迟，唯一的区别是只有当鼠标移动到沙盒中运行的软件的窗口边缘时才会出现——沙盒中运行的浏览器在窗口边缘会出现黄色的边框。当然为了出现可能的误操作，还可以在 Sandboxie 的设置中让黄色边框一直显示。

![](media/b7df3146b00423b83ec600b632b2a750.png)

请输入图片标题

默认安装完成之后会生成一个默认沙盒，当然你也可以根据需要（比如针对软件类型设置不同）新建沙盒，并且这些沙盒环境均可以独立运行，并不会和真实系统冲突，当然我们使用沙盒最重要的当然是运行软件，在 Sandboxie 运行软件大致有这么两种方式：

首先最为直接的方式就是直接在真实系统中，通过右键菜单的形式运行在沙盒中，如果设置多个沙盒还可以进行选择。

![](media/4b9a1fe5c3df35adc300a774b71d2679.png)

请输入图片标题

另外一种方式则需要回到 Sandboxie 控制器中，每一个沙盒环境都映射了完整的系统环境，包括资源管理器，文件目录以及开始菜单等。点击希望运行软件的沙盒，在右键选择-在沙盒中运行：之后会出现多个选项，比如运行系统默认的浏览器、邮件客户端，或者是从开始菜单、资源管理器或者是手动选择软件。

![](media/933318cf1e0182c92b1b3a254664de86.png)

请输入图片标题

另一方面，在沙盒中下载或者保存文件等操作则与真实的系统无关。比如说你在沙盒中通过浏览器下载了一个文件，看上去文件被下载到了 Download 目录中，但你在真实系统中通过资源管理器是看不到下载文件的。当然 Sandboxie 可以通过取回的方式将文件转移到真实系统对应的文件夹中。

这也有效地限制了一些软件后台下载行为——下载的文件数据只有经过我们的认可和需要才能被拷贝到真实系统目录当中。  
 

![](media/4a93e3e15d63c29a9798ec388e1c1197.png)

下载的文件可以被取回到真实系统对应的目录中

默认情况下，每一个沙盒中的软件数据等都会被保存在沙盒中，但由于其行为被严格被限制，因此里面运行的软件配置等都不会对真实的系统产生影响，我们在运行完软件之后可以通过终止软件的形式清空沙盒中所有软件进程，而如果不再需要某个沙盒也可以通过删除沙盒来实现。

## Sandboxie 使用场景

前面大致介绍了 Sandboxie 的原理和使用方法，那么 Sandboxie 适用于哪些使用场景呢？

### 使用 Sandboxie 打造纯净浏览环境

这个场景其实主要是针对家中对电脑技术并不精通的老人，相信不少朋友都有过给家中长辈清理各种流氓软件的经验，而归根到底都是他们在不知情的情况下载安装来路不明的软件结果，因此Sandboxie 在真实系统的桌面自动生成的「在沙盒中运行网页浏览器」就非常管用了。

![](media/25542f377a7b36c460ae1629c47e22cb.png)

改名改图标：打造长辈专用网络浏览器

我们甚至还可以进一步对常用应用的图标、名称做一些手脚，比如说将真实系统中桌面上的浏览器快捷方式移除，然后将「在沙盒中运行网页浏览器」改成一个平时使用的浏览器名称，顺便将图标也更换一下，这样神不知鬼不觉地将家中长辈的网页浏览行为限制在一个小小的沙盒中，下载、安装软件等行为都完全被限制在沙盒中，真实系统再也不会被无心装上一大堆流氓软件了。

### 在 Sandboxie 中安装某某云下载文件

现在百度云对下载的限制越来越多，文件体积稍大就必须通过客户端下载，但桌面客户端往往又会夹带「私货」，因此将其安装在 Sandboxie 这样的沙盒系统中并直接在沙盒中运行最为安全，毕竟我们只需要其下载文件，下载完成后再将下载的文件转到真实系统的文件目录即可。

![](media/9295ddd250ad473de927048db8fabb89.png)

请输入图片标题

在真实系统中下载安装包然后使用右键「在沙盒中运行」，之后在沙盒环境中完成安装，这样百度云客户端就只安装在沙盒中了，客户端的运行也必须通过沙盒——通过内置的菜单从「从开始菜单运行」，当然你在真实系统中是看不到这个百度云的安装目录的。

![](media/3d2af3915dd23b04132c8a9263608103.png)

请输入图片标题

其他使用步骤和正常使用百度云基本无差，唯一的区别是最后需要将下载的文件保存到真实系统目录中。当然也就不用担心软件安装之后出现的驻留系统进程、添加启动项、修改浏览器首页等一系列问题了，因为百度云没有安装在真实的系统中，而仅在沙盒中运行。

### 使用 Sandboxie 双开应用

因为沙盒属于一种轻量化的软件隔离环境，所以我们根据这个特性突破一些原本系统或者软件的限制，比如说多开或者双开应用，例如打开多个微信 PC 客户端。

![](media/66699896c3b4d314839aca4415fcd85b.png)

请输入图片标题

在真实的系统中安装好微信 PC 客户端之后，除了在真实系统通过双击直接运行微信之外，还可以在桌面通过「右键菜单」- 「在沙盒中运行」让其运行在沙盒中，如果你有多个微信号需要登录，则可以通过新建多个沙盒环境然后在各个沙盒运行来实现。

## 结语

Sandboxie 从去年开始逐渐转变成一款免费软件，作为目前 Windows 平台上功能最全面的沙盒工具之一，它低系统资源占用、强系统版本兼容性的优势非常适合用来对付一些喜欢夹带「私货」的软件。它既能避免对真实系统数据和安全性的干扰，又能方便地满足隔离、多开和数据转存等需求。

你可以在 [Sandboxie 官网](https://sspai.com/link?target=https%3A%2F%2Fwww.sandboxie.com%2F) 下载到这款免费应用。

> 下载少数派 [客户端](https://sspai.com/page/client)、关注 [少数派公众号](https://sspai.com/s/J71e) ，了解更多有趣的应用 🚀

[](https://sspai.com/s/JYjP)

205

36

为什么选择 Sandboxie

Sandboxie 的基本运行原理

Sandboxie 使用场景

使用 Sandboxie 打造纯净浏览环境

在 Sandboxie 中安装某某云下载文件

使用 Sandboxie 双开应用

结语

© 本文著作权归作者所有，并授权少数派独家使用，未经少数派许可，不得转载使用。

[#Windows](https://sspai.com/tag/Windows)

[#应用推荐](https://sspai.com/tag/%E5%BA%94%E7%94%A8%E6%8E%A8%E8%8D%90)

[#信息安全](https://sspai.com/tag/%E4%BF%A1%E6%81%AF%E5%AE%89%E5%85%A8)

 205

[![少数派_896366](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/o23k4ujj/updates)

[![少数派_937990](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/44agobj5/updates)

[![getdancing](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/vxjle82g/updates)

[![RenatoHong](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/RenatoHong/updates)

[![yanjing](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/xpq5k88p/updates)

[少数派_896366](https://sspai.com/u/o23k4ujj/updates)、[少数派_937990](https://sspai.com/u/44agobj5/updates)、[getdancing](https://sspai.com/u/vxjle82g/updates) 等 205 人为本文章充电

[![化学心情下2](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)![](media/zishenzuozhe_.png)

曾经电商人和媒体人，现在的产品人，热爱摇滚乐，只混 IT 圈

关注

全部评论(36)

热门排序

[![Dundant](media/Dundant.png)](https://sspai.com/u/5j6bo9u9/updates)

写下尊重、理性、友好的评论，有助于彼此更好地交流～

[![中南路](media/中南路.png)](https://sspai.com/u/jme0moex/updates)

[中南路](https://sspai.com/u/jme0moex/updates)

2020 年 03 月 30 日

2020年对于会自己加驱动的国产流氓还是老老实实虚拟机吧😋

25

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 31 日

现在国产流氓应用都会这么玩了😅

0

[![少数派_969585](media/少数派_969585.png)](https://sspai.com/u/anaj30c3/updates)

[少数派_969585](https://sspai.com/u/anaj30c3/updates)

2020 年 04 月 04 日

所以Qq 阿里旺旺 百度网盘 都是虚拟机

1

[![十二月是你的名字](media/十二月是你的名字.png)](https://sspai.com/u/gajrd88u/updates)

[十二月是你的名字](https://sspai.com/u/gajrd88u/updates)

2020 年 03 月 29 日

双开应用是真的好用

14

[![少数派_707117](media/少数派_707117.png)](https://sspai.com/u/ebb2fwsj/updates)

[少数派_707117](https://sspai.com/u/ebb2fwsj/updates)

2020 年 03 月 29 日

确实没想到还能哪来多开，对付钉钉微信这种不让多开的app简直好评

1

[![少数派_960734](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://sspai.com/u/ehxtof29/updates)

[少数派_960734](https://sspai.com/u/ehxtof29/updates)

2020 年 03 月 29 日

sandboxie里运行的软件可以读取用户硬盘的资料吗？我担心在隐私这方面，可能会不会无法起到和虚拟机一样的隔离保护作用

21

[![山鬼谣](media/山鬼谣.png)](https://sspai.com/u/shanguiyao/updates)

[山鬼谣](https://sspai.com/u/shanguiyao/updates)

![Matrix 作者](media/Matrix_作者.png)

2020 年 03 月 29 日

同问

0

[![LBHarry](media/LBHarry.png)](https://sspai.com/u/littleboyharry/updates)

[LBHarry](https://sspai.com/u/littleboyharry/updates)

2020 年 03 月 29 日

可以，在沙箱管理面板右键沙箱 -> 沙箱设置 -> 资源访问 -> 文件访问

3

[![Limerence](media/Limerence.png)](https://sspai.com/u/limerence/updates)

[Limerence](https://sspai.com/u/limerence/updates)

2021 年 10 月 22 日

使用了之后快捷键失效了咋办？

00

[![土豆没营养](media/土豆没营养.png)](https://sspai.com/u/6iqx84no/updates)

[土豆没营养](https://sspai.com/u/6iqx84no/updates)

2020 年 04 月 05 日

太棒了！非常好用！

00

[![Kaishan9O](media/Kaishan9O.png)](https://sspai.com/u/d15kczzv/updates)

[Kaishan9O](https://sspai.com/u/d15kczzv/updates)

2020 年 03 月 31 日

有没有办法把沙箱的存储位置转移到其他路径，c盘满了

00

[![lssence](media/lssence.png)](https://sspai.com/u/vohyzj7z/updates)

[lssence](https://sspai.com/u/vohyzj7z/updates)

2020 年 03 月 31 日

同时出现"SBIE 1222"以及"SBIE 2314"消息提醒，要如何处理么！

30

[![lssence](media/lssence.png)](https://sspai.com/u/vohyzj7z/updates)

[lssence](https://sspai.com/u/vohyzj7z/updates)

2020 年 03 月 31 日

解决了

0

[![布菜克先生](media/布菜克先生.png)](https://sspai.com/u/6vvmo6qe/updates)

[布菜克先生](https://sspai.com/u/6vvmo6qe/updates)

回复

[lssence](https://sspai.com/u/vohyzj7z/updates)

2020 年 04 月 07 日

我也出现了，请问是如何解决的？

0

[![lssence](media/lssence.png)](https://sspai.com/u/vohyzj7z/updates)

[lssence](https://sspai.com/u/vohyzj7z/updates)

回复

[布菜克先生](https://sspai.com/u/6vvmo6qe/updates)

2020 年 04 月 14 日

去官网下载最新版即刻，其他网站很多放的都不是新版！

1

[![牛头嘿嘿](media/牛头嘿嘿.png)](https://sspai.com/u/1tqcti3l/updates)

[牛头嘿嘿](https://sspai.com/u/1tqcti3l/updates)

2020 年 03 月 31 日

这个软件很早之前用过，没想到现在还在更新。回去给我的PC安一个，非常感谢作者介绍。

00

[![Wolf_G](media/Wolf_G.png)](https://sspai.com/u/gdfuadg4/updates)

[Wolf_G](https://sspai.com/u/gdfuadg4/updates)

2020 年 03 月 30 日

可是 Sandboxie 里面的程序好像不能输入中文（不能使用输入法）...

有办法解决吗？💔

10

[![5462323](media/5462323.png)](https://sspai.com/u/6at8vv0p/updates)

[5462323](https://sspai.com/u/6at8vv0p/updates)

2020 年 04 月 11 日

你装进去沙盒里一个输入法不就是了

1

[![爱喝板蓝根](media/爱喝板蓝根.png)](https://sspai.com/u/ttjmhyy8/updates)

[爱喝板蓝根](https://sspai.com/u/ttjmhyy8/updates)

2020 年 03 月 30 日

不知道安卓平台有没有类似的沙盒软件,这样就不用老是给父母清理毒瘤了

20

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 31 日

我记得有～找到回复你～

2

[![少数派_978092](media/少数派_978092.png)](https://sspai.com/u/7566lmz8/updates)

[少数派_978092](https://sspai.com/u/7566lmz8/updates)

2020 年 05 月 25 日

ISLAND

1

[![少数派_913495](media/少数派_913495.png)](https://sspai.com/u/7xlm19o6/updates)

[少数派_913495](https://sspai.com/u/7xlm19o6/updates)

2020 年 03 月 30 日

请问在沙盘中安装的百度云，每次打开都会提示uac怎么办啊？看记录是baidunetdiskhost.exe交给sandbox start请求的，有没有办法不弹窗呢？

10

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 30 日

我也遇到这个情境，虽然弹窗但是正常运行，估计是百度云太流氓了~

0

[![邻羟基苯甲酸](media/邻羟基苯甲酸.png)](https://sspai.com/u/p2y787m4/updates)

[邻羟基苯甲酸](https://sspai.com/u/p2y787m4/updates)

2020 年 03 月 30 日

windows真的需要更严格的权限管理系统。

20

[![LeoTian](media/LeoTian.jpg)](https://sspai.com/u/0pzq4kuz/updates)

[LeoTian](https://sspai.com/u/0pzq4kuz/updates)

2020 年 03 月 31 日

要想安全就别用Windows了，防不胜防，我朋友就是觉得自己掌控不了Windows才转的Linux。

0

[![少数派_969585](media/少数派_969585.png)](https://sspai.com/u/anaj30c3/updates)

[少数派_969585](https://sspai.com/u/anaj30c3/updates)

2020 年 04 月 04 日

比如阿里旺旺 需要你windows 所有权限

0

[![少数派_917007](media/少数派_917007.png)](https://sspai.com/u/bgufbznx/updates)

[少数派_917007](https://sspai.com/u/bgufbznx/updates)

2020 年 03 月 30 日

sandboxie自从开源之后还更新吗？

10

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 30 日

会，他们会继续更新~

0

[![三点一四](media/三点一四.png)](https://sspai.com/u/8fa9gqq2/updates)

[三点一四](https://sspai.com/u/8fa9gqq2/updates)

2020 年 03 月 30 日

好像不能在sandboxie里再安装个不同版本的office

10

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 30 日

嗯，有限制，这个好像不行～

0

[![时间旅行者](media/时间旅行者.png)](https://sspai.com/u/s895a1a2/updates)

[时间旅行者](https://sspai.com/u/s895a1a2/updates)

2020 年 03 月 29 日

太感动了，这真是XP时代病毒横行流行过的沙盒软件，居然在10多年后还在更新～

10

[![化学心情下2](media/化学心情下2-1.jpg)](https://sspai.com/u/liuxiaofengone/updates)

[化学心情下2](https://sspai.com/u/liuxiaofengone/updates)

![资深作者](media/资深作者.png)

2020 年 03 月 30 日

对的~老软件真的到现在真不容易~

0

[![少数派_858275](media/少数派_858275.png)](https://sspai.com/u/8d5ciumd/updates)

[少数派_858275](https://sspai.com/u/8d5ciumd/updates)

2020 年 03 月 29 日

太好了，实在受不了某某晕了

00

[![雪碧漱口](media/雪碧漱口.png)](https://sspai.com/u/s2olb3n0/updates)

[雪碧漱口](https://sspai.com/u/s2olb3n0/updates)

2020 年 03 月 29 日

Mac 有这种东西吗

10

[![少数派_964682](media/少数派_964682.png)](https://sspai.com/u/sx54vhvu/updates)

[少数派_964682](https://sspai.com/u/sx54vhvu/updates)

2020 年 03 月 29 日

MAS的软件本来就是基于沙盒的，再有其他需求虚拟机就可以了