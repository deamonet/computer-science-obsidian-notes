
[车小胖](https://www.zhihu.com/people/chexiaopang)  [出处​](https://www.zhihu.com/question/48509984) 计算机网络话题下的优秀答主

318 人赞同了该回答

不是弃用，而是这两层从来没有独立实现过，都是和应用层在一起实现。

以TCP/IP协议五层模型的[计算机网络](https://www.zhihu.com/search?q=%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)率先出现，硬件接口实现“**[物理层](https://www.zhihu.com/search?q=%E7%89%A9%E7%90%86%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)**”、“**[数据链路层](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)**”，[操作系统内核](https://www.zhihu.com/search?q=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%86%85%E6%A0%B8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)里的TCP/IP[协议栈](https://www.zhihu.com/search?q=%E5%8D%8F%E8%AE%AE%E6%A0%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)实现“**网络层**”、“**传输层**”，所有依赖于TCP/IP协议栈的应用程序实现广泛意义上的“**应用层**”，这个广泛意义的“应用层”既实现了会话ID、心跳keepalive，又实现了诸如文字、图片、音频、视频、文件的不同表示。

后来才有以TCP/IP为现实素材的OSI七层[参考模型](https://www.zhihu.com/search?q=%E5%8F%82%E8%80%83%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)，希望将“会话层”、“表示层”从广泛意义上的应用层里独立出来，这样可以让应用程序瘦身，**核心目标是：让千千万万不同应用程序共享“会话层”、“表示层”[软件代码](https://www.zhihu.com/search?q=%E8%BD%AF%E4%BB%B6%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)。**

很遗憾的是，迄今为止这个美好的愿望依然没有实现，究其根本原因是，不同的应用程序有大同小异的会话、表示需求，**这些代码不完全能够抽象到独立的会话层、表示层**，或者说，现有的应用层已经比较完美实现了会话层、表示层，对于[七层模型](https://www.zhihu.com/search?q=%E4%B8%83%E5%B1%82%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)需求没有动力。

**安全层**

但是，**有心栽花花不成，[无心插柳柳成荫](https://www.zhihu.com/search?q=%E6%97%A0%E5%BF%83%E6%8F%92%E6%9F%B3%E6%9F%B3%E6%88%90%E8%8D%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A159305039%7D)**，一个提供安全加密服务层出现了，很多人都使用过，只是一直没有人想去划分层次结构，它的名字叫**SSL/TLS**，有了它的加入，我们可以将TCP/IP五层结构看成六层：

应用层

**安全层（TLS）**

TCP/UDP

IP层

数据链路层

物理层

有了安全层提供的服务，位于应用层的HTTP/SMTP/FTP，都可以在其名字后加一个S（Security），比如**HTTPS**，其实这个世界压根**不存在HTTPS协议**，只有**HTTP**协议，加上S的后缀只是告诉大家HTTP使用的是六层结构，有了SSL/TLS的安全保护。

[编辑于 2017-04-24 22:18](https://www.zhihu.com/question/58798786/answer/159305039)


不过看评论也有观点认为表示层包括了安全层