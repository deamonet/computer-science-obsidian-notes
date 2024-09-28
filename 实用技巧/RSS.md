# [RSS介绍、RSS 2.0规范说明和示例代码](https://www.cnblogs.com/tuyile006/p/3691024.html)

RSS是一种消息来源格式规范，用以发布经常更新资料的网站，例如博客、新闻的网摘。RSS文件，又称做摘要、网摘、更新、频道等，包含了全文或节选文字，再加上一定的属性数据。RSS让发布者自动发布信息，也使读者能够聚合和定期更新不同网站的网摘。RSS可以通过以网页或桌面为架构的软件来阅读，即RSS阅读器、新闻聚合器等，并进行定期更新检查、自动下载。

RSS可以是以下三种解释中任一种的缩写，

-   Really Simple Syndication
-   RDF (Resource Description Framework) Site Summary
-   Rich Site Summary

RSS规范的主要版本有0.91、1.0和2.0等。

-   1.0版和0.91版内容不同、风格不同、制定标准的人也不同，称为RDF分支；
-   2.0版和0.91版一脉相承，称为2.*分支。

早期版本的RSS阅读器不支持RDF分支，而当前主流的RSS阅读器都同时兼容两种分支。

RSS 2.*规范由美国人Dave Winer个人进行维护，在全球大部分网站得到使用。可能是版权方面的原因，RSS 2.*规范并没有在[W3C](http://www.w3.org/)、[IETF](http://www.ietf.org/)或者[ISO](http://www.iso.org/)等国际标准化组织发布，而是在[RSS Advisory Board](http://www.rssboard.org/)发布。

# RSS 2.0规范说明

![](http://www.bobopo.com/images/00000/00000236.png)

RSS标志

本规范是RSS 2.0规范的2.0.10版本，2007-10-15在[RSS Advisory Board](http://www.rssboard.org/)发布，[波波坡原创翻译](http://www.bobopo.com/article/code/rss_2_0_spec.htm)。点击[这个链接](http://www.rssboard.org/rss-specification)可以获得RSS的最新版本的英语规范。

RSS是一个[Xml格式](http://woyouxian.net/xml/xml_tutorials/what_is_xml.html)的数据或文件，根节点是一个带有版本号的**<rss>**节点，根节点一下是一个单一的**<channel>**节点极其子节点。对于具体的Rss订阅项目，例如一篇文章、网志，由<channel>节点下多个**<item>**节点来表示。一个<channel>可以有任意多个<item>。

## <channel>

必须的子节点：

项目

说明

举例

**<title>**

频道名称。

程序员的波波坡。

**<link>**

与频道关联的Web站点或者站点区域的Url。

http://www.bobopo.com

**<description>**

简要介绍该频道是做什么的。

包含编程、休闲、知识、杂记的程序员站点。

可选的子节点：

项目

说明

举例

**<language>**

频道内容使用的语言。详见[常用HTML、RSS语言代码列表](http://www.bobopo.com/article/code/html_rss_language_code.htm)。

zh-cn

**<copyright>**

频道内容的版权说明。

Copyright 2008,2009 bobopo.com

**<managingEditor>**

责任编辑的Email地址。

rosbicn@hotmail.com

**<webMaster>**

频道相关网站管理员的Email地址。

rosbicn@hotmail.com

**<pubDate>**

频道内容发布日期。遵循[RFC 822](http://www.w3.org/Protocols/rfc822/)。

Wed, 04 Mar 2009 00:00:01 GMT

**<lastBuildDate>**

频道内容最后的修改日期。遵循[RFC 822](http://www.w3.org/Protocols/rfc822/)。

Wed, 04 Mar 2009 09:42:31 GMT

**<category>**

频道所属的一个或几个类别。详见后文。

Html

**<generator>**

生成该频道的程序名字符串。

Bobopo Site Generator 2009

**<docs>**

解释当前RSS文件的文档的Url。(给不知道啥是RSS的某人看:)

http://www.bobopo.com/code/rss.htm

**<cloud>**

允许进程注册为“cloud”，频道更新时通知它，为 RSS 提要实现了一种轻量级的发布-订阅协议。详见后文。

详见后文。

**<ttl>**

内容有效期，一个数字，指明该频道可被缓存的最长分钟数。

60

**<image>**

指定一个 GIF或JPEG或PNG图片，用以与频道一起显示。详见后文。

详见后文。

**<rating>**

內容分级，主要指成人、限制、儿童等，多数情况不用，如果要用参见[PICS](http://www.w3.org/PICS/)。

 

**<textInput>**

定义可与频道一起显示的输入框。多数情况不用。详见后文。

 

**<skipHours>**

提示新闻聚合器，哪些小时时段它可以跳过。可包含最多24个<hour>子节点，它的值是0～23中的一个数字。

<hour>2</hour>  
<hour>3</hour>

**<skipDays>**

提示新闻聚合器，那些天它可以跳过。可包含最多7个<day>子节点，它的值是Monday、Tuesday、Wednesday、Thursday、Friday、Saturday或Sunday之一。

<day>Saturday</day>  
<day>Sunday</day>

## <item>

<item>的任何一个子节点都是可选的，但是<title>和<description>至少要被包含一个。

项目

说明

举例

**<title>**

项目的名称。

RSS简介。

**<link>**

项目的Url。

http://www.bobopo.com/article/rss.htm

**<description>**

项目的摘要。

RSS（简易资讯聚合）是一种消息来源格式规范，用以发布经常更新资料的网站，例如部落格文章、新闻、音讯或视讯的网摘。RSS文件（或称做摘要、网络摘要、或频更新，提供到能道）包含了全文或是节录的文字，……

**<author>**

作者的Email地址。通常忽略。

rosbicn@hotmail.com

**<category>**

频道所属的一个或几个类别。详见后文。

Html

**<comments>**

与此项目相关的评论的Url。

http://www.bobopo.com/comments/rss.htm

**<enclosure>**

此项目相关的多媒体附件。属性url表示附件网址，属性length表示附件字节数，属性type表示附件的MIME类型。

<enclosure url="http://www.bobopo.com/video/rss.mp3" length="16131450" type="audio/mpeg" />

**<guid>**

项目的唯一识别码。详见后文。

http://www.bobopo.com/article/rss.htm

**<pubDate>**

项目的发布日期。遵循[RFC 822](http://www.w3.org/Protocols/rfc822/)。

Wed, 04 Mar 2009 00:00:01 GMT

**<source>**

项目来源于哪个Rss频道。如果一个Rss是从其他Rss转贴过来，可以用这个。必须包含属性url，指向另外一个rss。

<source url="http://www.blabla.cn/rss.xml">Blabla</source>

## <item>中支持Html格式的<description>

<item>的子节点<description>可以只包含项目的摘要，也可以是项目全文。它的值是Text类型，或者是一个实体编码的HTML类型(entity-encoded HTML)。所谓实体编码的HTML类型，指的是HTML的保留字(实体)都进行了编码处理。举例如下：

示例1：对HTML标记进行编码。

<description>this is &lt;b&gt;bold&lt;/b&gt;</description> 

结果：this is **bold**

示例2：HTML标记放在CDATA段中编码。

<description><![CDATA[this is <b>bold</b>]]></description> 

结果：this is **bold**

示例3：对尖括号进行编码

<description>5 &amp;lt; 8, ticker symbol&amp;lt;BIGCO&amp;gt;</description>

结果：5 < 8, ticker symbol <BIGCO>

示例4：尖括号放在CDATA段中编码。

<description><![CDATA[5 &lt; 8, ticker symbol &lt;BIGCO&gt;]]></description>

结果：5 < 8, ticker symbol <BIGCO>

## <category>

<category>是<channel>的可选子节点，也是<item>的可选子节点，用来表示频道或者内容的类别。一个节点表示一个类别，如果是多个类别，可以用多个<category>节点来表示。如果该类别有专门的Url表示，可以用属性domain来表示。

示例如下，

<category>编程</category>  
<category domain="http://www.bobopo.com/html/">Html</category>

<category>有点类似很多网站中的标签(Tag)。

## <guid>

guid是全球唯一识别码的意思，用来唯一确定Rss项目的字符串。现在，一个新闻聚合器往往用这个字符串来判断某个项目是不是新的。

guid并没有什么语法上面的规定，新闻聚合器肯定把它视作字符串。建立这个字符串的唯一性取决于项目的内容。往往用项目相关的Url来做guid。

如果<guid>包含属性isPermaLink，并且属性值是true，新闻聚合器会把<guid>的值当做当前项目的永久连接，也就是能够在浏览器中打开，连接到项目全文的Url。isPermaLink是可选属性，缺省值是true，也就是说，如果包含了isPermaLink，只有明确写明值是false，才表示<guid>的值不是Url。

<guid isPermaLink="true">http://www.bobopo.com/article/rss.htm</guid>

## <cloud>

这个东西主要是给HTTP-POST、XML-RPC或者SOAP 1.1这类的Web Service用的。如果真的要用，请看[这里](http://www.rssboard.org/rsscloud-interface)。

项目

说明

举例

**<domain>**

必须。cloud程序所在机器的域名或IP地址。

rpc.bobopo.com

**<port>**

必须。访问cloud程序所通过的端口。

80

**<path>**

必须。程序所在路径，不一定需要是真实路径。

/RPC2

**<registerProcedure>**

必须。 注册的可提供的服务或过程。

cloud.rss

**<protocol>**

必须。协议，http-post、xml-rpc、soap之一。

xml-rpc

示例如下，

<cloud domain="rpc.bobopo.com" port="80" path="/RPC2" registerProcedure="cloud.rss" protocol="xml-rpc" />

## <image>

项目

说明

举例

**<url>**

必须。表示该频道的Gif、Jpeg或Png图像的Url。

http://www.bobopo.com/style/images/bobopo.gif

**<title>**

必须。图象描述。当频道以Html呈现时用作<img>标签的alt属性。

程序员的波波坡。

**<link>**

必须。站点Url，当频道以Html呈现时，该图像会链接到此。

http://www.bobopo.com

**<width>**

可选。 数字，图象的像素宽度，最大值144，默认值为88。

120

**<height>**

可选。 数字，图象的像素高度，最大值400，默认值为31。

120

**<description>**

可选。 当频道以Html呈现时，作为围绕着该图像形成的链接Tag的title属性。

包含编程、休闲、知识、杂记的程序员站点。

上述<title>和<link>多数情况下会与<channal>的<title>和<link>相同。

## <textInput>

用户可以用<textInput>让读者进行一次搜索引擎的搜索，或者提交一个反馈。不过大多数情况都是忽略这个东西。

项目

说明

**<title>**

必须。输入框中Submit按钮上的文字。

**<description>**

必须。输入框的解释。

**<name>**

必须。输入框对象的名字。

**<link>**

可选。 输入框提交的Url。

# RSS 2.0代码示<?xml version="1.0"?><rss version="2.0">

  __<channel>  
    <title>Liftoff News</title>  
    <link>http://liftoff.msfc.nasa.gov/</link>  
    <description>Liftoff to Space Exploration.</description>  
    <language>en-us</language>  
    <pubDate>Tue, 10 Jun 2003 04:00:00 GMT</pubDate>  
    <lastBuildDate>Tue, 10 Jun 2003 09:41:01 GMT</lastBuildDate>  
    <docs>http://blogs.law.harvard.edu/tech/rss</docs>  
    <generator>Weblog Editor 2.0</generator>  
    <managingEditor>editor@example.com</managingEditor>  
    <webMaster>webmaster@example.com</webMaster>  
    <item>  
      <title>Star City</title>  
      <link>http://liftoff.msfc.nasa.gov/news/2003/news-starcity.asp</link>  
      <description>How do Americans get ready to work with Russians aboard the International Space Station? They take a crash course in culture, language and protocol at Russia's &lt;a href="http://howe.iki.rssi.ru/GCTC/gctc_e.htm"&gt;Star City&lt;/a&gt;.</description>  
      <pubDate>Tue, 03 Jun 2003 09:39:21 GMT</pubDate>  
      <guid>http://liftoff.msfc.nasa.gov/2003/06/03.html#item573</guid>  
    </item>  
    <item>  
      <description>Sky watchers in Europe, Asia, and parts of Alaska and Canada will experience a &lt;a href="http://science.nasa.gov/headlines/y2003/30may_solareclipse.htm"&gt;partial eclipse of the Sun&lt;/a&gt; on Saturday, May 31st.</description>  
      <pubDate>Fri, 30 May 2003 11:06:42 GMT</pubDate>  
      <guid>http://liftoff.msfc.nasa.gov/2003/05/30.html#item572</guid>  
    </item>  
    <item>  
      <title>The Engine That Does More</title>  
      <link>http://liftoff.msfc.nasa.gov/news/2003/news-VASIMR.asp</link>  
      <description>Before man travels to Mars, NASA hopes to design new engines that will let us fly through the Solar System more quickly.  The proposed VASIMR engine would do that.</description>  
      <pubDate>Tue, 27 May 2003 08:37:32 GMT</pubDate>  
      <guid>http://liftoff.msfc.nasa.gov/2003/05/27.html#item571</guid>  
    </item>  
    <item>  
      <title>Astronauts' Dirty Laundry</title>  
      <link>http://liftoff.msfc.nasa.gov/news/2003/news-laundry.asp</link>  
      <description>Compared to earlier spacecraft, the International Space Station has many luxuries, but laundry facilities are not one of them.  Instead, astronauts have other options.</description>  
      <pubDate>Tue, 20 May 2003 08:56:02 GMT</pubDate>  
      <guid>http://liftoff.msfc.nasa.gov/2003/05/20.html#item570</guid>  
    </item>  
  </channel>  
</rss>__

我的另一篇博文有讲怎么用C# 3.5、4.0来生成和解析rss 详见：_[用C#实现RSS的生成和解析，支持RSS2.0和A](http://www.cnblogs.com/tuyile006/p/3710305.html)_