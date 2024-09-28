[**xkww3n's site**](https://www.xkww3n.cyou/)

-    [首页](https://www.xkww3n.cyou/)
-    [归档](https://www.xkww3n.cyou/archives/)
-    [分类](https://www.xkww3n.cyou/categories/)
-    [标签](https://www.xkww3n.cyou/tags/)
-    [关于](https://www.xkww3n.cyou/about/)
-    [友链](https://www.xkww3n.cyou/links/)
-    [订阅](https://www.xkww3n.cyou/atom.xml)
-     
-     

从 pixiv 免代理直连看 SNI 阻断及其绕过方法——域前置

 2020年5月1日 晚上

 1.7k 字  15 分钟

最近在使用某 pixiv 第三方客户端时发现这个客户端可以直连 pixiv。然而，众所周知，使用 hosts 访问被屏蔽的网站的方式很早就行不通了。所以我相当好奇，就乘着五一假期做点研究。

# [](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#pixiv-%E5%8F%97%E5%B1%8F%E8%94%BD%E6%83%85%E5%86%B5)[](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#pixiv-%E5%8F%97%E5%B1%8F%E8%94%BD%E6%83%85%E5%86%B5)pixiv 受屏蔽情况

目前 GFW 屏蔽“违规网站”所用的方法有两种——DNS污染和TCP重置抢答。所以如果只用 hosts 解析到正确的 IP 地址，连接会被 GFW 抢先发送的 RST 报文重置：

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20155913.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20155913.png)

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20174547.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20174547.png)

# [](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E7%9B%B4%E8%BF%9E%E6%96%B9%E6%A1%88)[](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E7%9B%B4%E8%BF%9E%E6%96%B9%E6%A1%88)直连方案

那么那些可以直连的软件是如何做到直连的呢？我开启直连功能后抓了下这个软件发送的包：

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20164837.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20164837.png)

欸这个Client Hello怎么和我之前看到的不一样呢？太短了吧这也！？（以下是访问百度的Client Hello）

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20165101.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20165101.png)

和前面的包一对比就可以看出来，这个软件在访问 pixiv 时把数据包里的 `Server Name` 字段搞乱了（变成了 `?124`），这样GFW**理论上**就无法通过这个字段来探知连接到的域名。

~~但我还是不明白，GFW 的动态路由投毒大法（又称 IP 黑洞）是拿来干嘛用的？直接 ban 掉这个 IP 不就好了？`210.140.131.0/24`这个 IP 段属于日本的 IDC Frontier Inc.，隶属于 SoftBank。目前这个IP段似乎只有 pixiv 一家在使用。按理来说屏蔽了也没什么影响。~~查了一下，发现这家是个云计算服务提供商，价格死贵：[https://www.idcf.jp/english/cloud/](https://www.idcf.jp/english/cloud/)

那么为什么即使不发送有效的 `Server Name` 字段，依然能正常完成 SSL Handshake 呢？这就要谈到 pixiv 服务器所用的技术之一—— **SNI**

# [](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#SNI)[](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#SNI)SNI

Server Name Indication，**SNI**，服务器名称指示；简单来说，是用于在一台服务器的相同端口上部署不同证书的方法，我们熟知的 Cloudflare 在启用 CDN 后提供的证书就是基于 SNI 部署的。当然，pixiv 也是。 实现 SNI 的重要过程之一，就是在客户端与服务器建立连接的时候就发送要连接的域名，以便服务器可以返回一张颁发给指定域名的证书。

这个过程中，如果**用户请求了一个不存在的域名后，服务器会发送一张默认证书给客户端以便连接能够继续。**有趣的是，pixiv 服务器设置的默认证书正好是**颁发给 `*.pixiv.net` 的泛域名证书**。所以无论请求什么东西，返回来的证书总是能用的。  
下图是 pixiv 返回的默认证书，使用者是`*.pixiv.net`：

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20172533.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20172533.png)

这张则是正常访问主页时，使用 SNI 技术签发的证书，使用者是 `www.pixiv.net`：

[![](media/%E6%89%B9%E6%B3%A8%202020-05-01%20172845.png)](https://cdn.xkww3n.cyou/%E6%89%B9%E6%B3%A8%202020-05-01%20172845.png)

事实上，GFW 目前检测某个数据包是否“违规”的手段就是基于深度数据包检测的 SNI 阻断，即通过 Server Name 字段来检查域名，判断是否阻止对这个IP的连接。而这种通过混淆 Server Name 来绕过阻断的方法也已经不是什么新鲜技术，这叫做**域前置**，Domain fronting. [维基百科](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%89%8D%E7%BD%AE)对这一技术有介绍。

这种方法的最大局限性在于其实用性取决于服务器如何响应无效的 Server Name. 如果 pixiv 服务器在收到这样的数据包后没有返回这个泛域名证书而是拒绝连接，那这种方法显然会失效。

# [](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E9%A2%98%E5%A4%96%E8%AF%9D)[](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E9%A2%98%E5%A4%96%E8%AF%9D)题外话

前期的审查还不算严格，但是到了现在，已经不能用“严格”来形容了，完全到了**变态**的地步。举个例子，B站手机APP的启动图曾一度长这样

（此图片来自[萌娘共享](https://commons.moegirl.org/)，采用[知识共享(Creative Commons) 署名-非商业性使用-相同方式共享 3.0 协议](https://commons.moegirl.org/%E8%90%8C%E5%A8%98%E5%85%B1%E4%BA%AB:%E7%89%88%E6%9D%83%E4%BF%A1%E6%81%AF)授权）

由于授权协议冲突，现停止从萌娘共享直接引用图片，请点击下面链接查看： [https://zh.moegirl.org.cn/File:B站原起始页面.png](https://zh.moegirl.org.cn/File:B%E7%AB%99%E5%8E%9F%E8%B5%B7%E5%A7%8B%E9%A1%B5%E9%9D%A2.png)

但后来就变成了这样（此图片来自[萌娘共享](https://commons.moegirl.org/)，采用[知识共享(Creative Commons) 署名-非商业性使用-相同方式共享 3.0 协议](https://commons.moegirl.org/%E8%90%8C%E5%A8%98%E5%85%B1%E4%BA%AB:%E7%89%88%E6%9D%83%E4%BF%A1%E6%81%AF)授权）

由于授权协议冲突，现停止从萌娘共享直接引用图片，请点击下面链接查看： [https://zh.moegirl.org.cn/File:B站新起始页面.png](https://zh.moegirl.org.cn/File:B%E7%AB%99%E6%96%B0%E8%B5%B7%E5%A7%8B%E9%A1%B5%E9%9D%A2.png)

> B站对内容审查已不是一天两天，但这次竟把代表了“哔哩哔哩干杯”这句口号，代表了众多还喜欢这个网站的用户心中情怀的两杯人畜无害的啤酒都抹消掉；结合前文所述的被央视钦点和下架番剧自审等风波，可见国内网站审查环境之严峻、生存空间之狭窄。（摘自[萌娘百科](https://zh.moegirl.org/Bilibili/%E4%BA%89%E8%AE%AE%E5%92%8C%E5%BD%B1%E5%93%8D#%E5%95%A4%E9%85%92%E5%8F%98%E5%B0%8F%E7%94%B5%E8%A7%86)）

最后，放两个视频，自己体会。

  

---

[笔记](https://www.xkww3n.cyou/categories/%E7%AC%94%E8%AE%B0/)

 [#某科学的上网方式](https://www.xkww3n.cyou/tags/%E6%9F%90%E7%A7%91%E5%AD%A6%E7%9A%84%E4%B8%8A%E7%BD%91%E6%96%B9%E5%BC%8F/) [#网络](https://www.xkww3n.cyou/tags/%E7%BD%91%E7%BB%9C/)

从 pixiv 免代理直连看 SNI 阻断及其绕过方法——域前置

https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/

作者

xkww3n

发布于

2020年5月1日

许可协议

 [](https://creativecommons.org/licenses/by-sa/4.0/)[](https://creativecommons.org/licenses/by-sa/4.0/)

[回归](https://www.xkww3n.cyou/2021/10/23/alive/ "回归")

[中文 VOCALOID 调声常用教程](https://www.xkww3n.cyou/2020/04/02/cn-vocaloid-tutorials/ "中文 VOCALOID 调声常用教程")

 目录

1.  [pixiv 受屏蔽情况](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#pixiv-%E5%8F%97%E5%B1%8F%E8%94%BD%E6%83%85%E5%86%B5 "pixiv 受屏蔽情况")
2.  [直连方案](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E7%9B%B4%E8%BF%9E%E6%96%B9%E6%A1%88 "直连方案")
3.  [SNI](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#SNI "SNI")
4.  [题外话](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#%E9%A2%98%E5%A4%96%E8%AF%9D "题外话")

[](https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/#)

[Hexo](https://hexo.io/)  [Fluid](https://github.com/fluid-dev/hexo-theme-fluid) [萌ICP备20220309号](https://icp.gov.moe/?keyword=20220309)