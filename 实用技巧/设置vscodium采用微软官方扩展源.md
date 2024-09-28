vscodium默认使用的扩展源是`open-vsx.org`，部分扩展没有加入此扩展源，需要使用微软官方扩展源才能直接安装和自动更新。 

修改`安装根目录/resources/app/product.json`文件中的`extensionsGallery`键对应的值如下：

```text
"extensionsGallery": {
    "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
    "itemUrl": "https://marketplace.visualstudio.com/items"
}
```

重启vscodium。扩展里就可以搜索安装微软官方扩展源里的扩展。

发布于 2020-11-06 15:49

[  
习佑年](https://www.zhihu.com/people/251f541197bce8dc76248af9465b5ebd)

给后来人探个雷！即使这样能装vscode的扩展，它也不是“vscode”，比如remote套装“生产力”，微软故意没有开源(其实就是脚本里特地没有改，还是vscode的标识)导致根本没法用！狗曰的！

2022-04-22

​回复​5

[![冯道龙](https://pica.zhimg.com/a431d797cb1da30f21f4e7a1e494cc34_l.jpg?source=06d4cd63)](https://www.zhihu.com/people/698b485834df3f28354851468ae679fb)

[冯道龙](https://www.zhihu.com/people/698b485834df3f28354851468ae679fb)

我用的是 code-oss，据说是没有开启某些 API 的扩展，比如用这样的方式启动 code-oss --enable-proposed-api ms-vscode-remote.remote-ssh ，就可以看到 ssh 远程资源管理器了，但不知道还需要做什么，我这里连接ssh总是失败。![[衰]](https://pic1.zhimg.com/v2-d6d4d1689c2ce59e710aa40ab81c8f10.png)

2022-09-19

​回复​喜欢

[![JackMaOct](https://pica.zhimg.com/4877f2cc7848565832267c0f4e241413_l.jpg?source=06d4cd63)](https://www.zhihu.com/people/1df419bb06ea7a7dacb8d8f30892ba4c)

[JackMaOct](https://www.zhihu.com/people/1df419bb06ea7a7dacb8d8f30892ba4c)

![](https://picx.zhimg.com/v2-4812630bc27d642f7cafcd6cdeca3d7a.jpg?source=88ceefae)

找了半天终于找到换源的方法了，谢谢答主

2021-03-05