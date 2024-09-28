[![LSZH](media/LSZH.png)](https://www.zhihu.com/people/cui-zi-mu)

[LSZH](https://www.zhihu.com/people/cui-zi-mu)

320 人赞同了该回答

因为空格才是正统。这个问题培训班低端码农是很难理解的。

文本text和字符串string是两个不同的概念

- 文本是指无样式可打印字符在矩形lattice中的排版。是一个human-readable的概念。你可以明确说出某行某列的那个可打印字符是什么。
- 字符串是给IO设备描述「如何产生一段文本」的指令序列。是一个machine-readable的概念。

文本是平台无关的，字符串是平台相关的。在不同平台不同IO设备，描述同一段文本所需的字符串是不一定相同的。

比如下面这段文本，第一行第一列是a，第二行第一列是b

```text
a
b
```

在windows中可以用字符串

```text
"a\r\nb"
```

来描述。而在unix中可以用字符串

```text
"a\nb"
```

来描述。

思考题：json是javascript plain object的字符串表示还是文本表示？

答案：是字符串表示。因为json官网明确规定了哪些控制字符属于空白分隔符。

这也是为什么json的mime类型是application/json而不是text/json。

思考题：html5标准中的server sent event发送的是字符串还是文本？

答案：是文本。因为html5标准没有规定具体用哪个字符表示换行。因此[sse协议](https://www.zhihu.com/search?q=sse%E5%8D%8F%E8%AE%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)

- 只保证「发送端原字符串在发送端平台上所对应的文本」与「接收端收到的文本」相同
- 但并不保证「发送端原字符串在发送端平台上所对应的文本」与「接收端收到的文本在接收端平台上对应的字符串」相同。比如发送端源字符串中的换行符，到接收端产生的最终字符串中后，很可能已经变了。因此发送端和接收端手中是两个完全不同的字符串。

如果你用sse来发送json，发送端和接收端手中将会是两个不同的[json字符串](https://www.zhihu.com/search?q=json%E5%AD%97%E7%AC%A6%E4%B8%B2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)，只不过他们表示的javascript plain object恰好相同，这是一种特殊场景下的歪打正着。

因此严格来说，sse不能用于直接推送json。严谨的做法是发送端先把字符串用utf8等编码为二进制串，再用base64把二进制串编码为文本，再推送。

万一某个场景下，接收端A从sse拿到json字符串并没有转成object，而是把这个字符串hash一下作为json的key存到了某个[远程数据库](https://www.zhihu.com/search?q=%E8%BF%9C%E7%A8%8B%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)里。同时发送端也用sse向另一个不同平台的接收端B推送了同一个json，接收端B也把收到的json字符串hash一下存进那个远程数据库。本来期望判定key相同的，实际判定为不同。

遥远的某一天某个遥远的下游用户卡到这个bug，看到时候不得调死你。

**软件工程中的各种隐藏bug，就是一个个低端码农在这么一个个不起眼的「语义与实现的不一致」中累积起来的。**

现在你应该明白了为什么空格才是缩进的正统。因为「缩进」是一个文本层面的概念，描述不同行之间的列对齐结构。而'\t'是一个平台相关的[控制字符](https://www.zhihu.com/search?q=%E6%8E%A7%E5%88%B6%E5%AD%97%E7%AC%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)，只能出现在字符串中。

只不过对于缩进不敏感的编程语言，恰好没有影响，这也是一种特殊场景下的歪打正着。如果换成缩进敏感的编程语言，比如[haskell](https://www.zhihu.com/search?q=haskell&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)，就会出问题。

因此，只要你用的那个语言把源文件内容的语义定义为「[源代码](https://www.zhihu.com/search?q=%E6%BA%90%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2745791841%7D)的字符串」而不是定义为「源代码文本」，就可以用tab。只不过空格更正统，并不是更正确。

如果我这个回答你看不懂，可以看看 

[@黄亮anthony](https://www.zhihu.com/people/01f473c80d12789ec9658bc5e1571c17)

 的在另一个问题回答

[Unix/Linux/Mac 与 Windows 的换行符不统一的原因/目的是什么？30 赞同 · 2 评论回答](https://www.zhihu.com/answer/557595940)

[编辑于 2022-11-05 20:29](https://www.zhihu.com/question/555706148/answer/2745791841)・IP 属地黑龙江