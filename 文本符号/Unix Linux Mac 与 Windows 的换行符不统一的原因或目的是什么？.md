**更新：**  
很多人第一反应是我对Unix/Linux 有偏爱，所以一切要以Unix/Linux 为准。  
其实并没有。  
我喜欢Unix/Linux 上的GNU 工具，但是也喜欢Windows 上的很多东西。  
我的初衷就是遇到了换行符不一致导致的问题，然后思考不统一起来的原因是什么，未来有没有可能统一？  
至于说都改成"\n"，只是因为根据了解，两个字符的换行在计算机上并没有严格要求，基于避繁就简的简单想法。  
结论：只要改成一样，都是欣然接受，热烈鼓掌的。  
==================================
假设1，Windows 先制定了规则，
然后Unix/Linux 和Mac 觉得两个字符浪费，决定用一个字符，为什么后者不沿用前者方案（最开始Unix/Linux是"\n"，Mac是"\r"）？  
**既然后来****Mac 可以改成跟**Unix/Linux **一样的“\n”，**那为什么Windows 不试着改成一样的“\n”？  
  
**假设2，**Unix/Linux **先制定规则，**  
那Windows 为什么要用两个字符，故意弄不一样？  
=================================  
关于“回车”（carriage return）和“换行”（line feed）这两个概念的来历和区别：  

> 在计算机还没有出现之前，有一种叫做电传打字机（Teletype Model 33）的玩意，每秒钟可以打10个字符。但是它有一个问题，就是打完一行换行的时候，要用去0.2秒，正好可以打两个字符。要是在  
> 这0.2秒里面，又有新的字符传过来，那么这个字符将丢失。  
> 于是，研制人员想了个办法解决这个问题，就是在每行后面加两个表示结束的字符。一个叫做“回车”，告诉打字机把打印头定位在左边界；另一个叫做“换行”，告诉打字机把纸向下移一行。  
> 这就是“换行”和“回车”的来历，从它们的英语名字上也可以看出一二。  
>   
> 后来，计算机发明了，这两个概念也就被般到了计算机上。那时，存储器很贵，一些科学家认为在每行结尾加两个字符太浪费了，加一个就可以。于是，就出现了分歧。  
> Unix/Linux 系统里，每行结尾只有“<换行>”，即“\n”；Windows系统里面，每行结尾是“ <回车><换行>”，即“\r\n”；Mac系统里，每行结尾是“<回车>”，即“\r”（现在已改成跟Unix/Linux一样的"\n"）。  
>   
> 一个直接后果是，Unix/Linux/Mac系统下的文件在Windows里打开的话，会出现换行丢失，整个文本会乱成一团。（类似UltraEdit 这种编辑器可以正确识别）； 而反过来就会出现^M的符号（这是Unix/Linux 等系统下规定的特殊标记，占一个字符大小，不是^和M的组合，打印不出来的，可以通过ctrl+V, ctrl+M 输入）。Unix/Linux 下很多文本编辑器（命令行）会在显示这个标记之后，补上一个自己的换行符，以避免内容混乱（只是用于显示，补充的换行符不会写入文件，有专门的命令将Windows换行符替换为Unix/Linux 换行符）。 Unix/Linux 系统下的换行符在Windows系统的文本编辑器中会被忽略，整个文本会乱成一团。  
> Windows 的换行是\r\n，十六进制数值是：0D0A。  
> Unix/Linux/OS X 的换行是\n，十六进制数值是：0A。




[![黄亮anthony](https://picx.zhimg.com/5613e87e5fc036eeedf3a80fb331acbb_l.jpg?source=1940ef5c)](https://www.zhihu.com/people/anthonyh)

[黄亮anthony](https://www.zhihu.com/people/anthonyh)


编程，语言，数学，逻辑

30 人赞同了该回答

为什么大家已经说到了。我说说为什么在使用中会有问题。

这个问题，让我们认识到文件是一个操作系统抽象，离开操作系统谈文件会发现这个抽象有很多问题。我们并没有一个跨操作系统的文件，我们无论下载，上传，还是通过文件共享，其实都经过了一种通信协议，在我们的操作系统上重建了文件，这本来就应该由通信协议完成本地化转换。

大部分遇到这个问题的人要么有意忽视了这个问题（强行原样复制文件并在不同系统中打开，比如通过打包，传输，再解包的方式），要么就是协议不支持，比如windows的scp。

FTP协议中区分了文本和非文本，git中支持修改或不修改换行符，都是这个原因。

见过不少同事在window下强行用"b"模式打开文本，再强迫自己用"\r\n"写行结束，实在太辛苦。

windows和unix下都通用的作法是用不加"b"的模式打开文本，用"\n"写行结束。

异种系统传递文件内容时，记得区分文本和非文本，按需重建文件。

[发布于 2018-12-22 22:37](https://www.zhihu.com/question/46542168/answer/557595940)



[![hhhhhhhhh](https://picx.zhimg.com/v2-8061d0d00b25f9316626a9d07f6fe2ff_l.jpg?source=1940ef5c)](https://www.zhihu.com/people/chantisnake)

[hhhhhhhhh](https://www.zhihu.com/people/chantisnake)

玩完了就跑

234 人赞同了该回答

泻药。。。事情是这样的。。。

[换行符](https://www.zhihu.com/search?q=%E6%8D%A2%E8%A1%8C%E7%AC%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A101979873%7D)是最开始模拟打字机的大家也都知道了，所以当时的情况是怎么样的呢。

在最早的 ASCII 标准（1963-1968）中，有两套标准，一套是 ISO 出的认为 CRLF 和 LF 都是标准的；然而另一套是 ASA 出的只认为 CRLF 是符合标准的。

所以 MS-DOS（1981）设计的时候是采用了在两种方法中都符合标准的 CRLF，一方面是满足了两个标准，另一方面是兼容了当时大量采用 CRLF 的计算机。

然而 unix 作为一个连 e 都懒得加的系统，你让人家加一个 CR？科科。。。然后 Linux 的很多设计上也兼容了 unix，[mac os x](https://www.zhihu.com/search?q=mac%20os%20x&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A101979873%7D) 也投奔了 unix。

就有了现在三大 PC 只有 windows 有 CRLF 这样的尴尬场面。

其实真正异类的是老 mac os 这种用 CR 的系统。。。大家快点去干他们。。。

  

大家可以看一下：

[Newline](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Newline)  

-------------------------------------------------------------------------------------------------------------

其实，从真正实际使用的角度来说，

大部分 editor （比如 sublime text）不管是在 unix 上还是 Linux 上都默认采用的是 [windows line ending](https://www.zhihu.com/search?q=windows%20line%20ending&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A101979873%7D)，这样兼容了所有的系统（除了老 mac os）。

至于 vim 和 emacs 也有 oneliner 来改动 line ending

[File format - Vim Tips Wiki](https://link.zhihu.com/?target=http%3A//vim.wikia.com/wiki/File_Format)[EmacsWiki: End Of Line Tips](https://link.zhihu.com/?target=https%3A//www.emacswiki.org/emacs/EndOfLineTips)  

而且 windows 上基本除了 [notepad](https://www.zhihu.com/search?q=notepad&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A101979873%7D) 都支持 unix line ending ，包括比较新版本的 vs。

所以相对来说因为 line ending 出现的错误还是比较少的。

----------------------------------------------------------------------------------------------------------

至于为什么 mac 可以改，因为 mac os x 和 mac os 简直就是超越了雷锋和[雷峰塔](https://www.zhihu.com/search?q=%E9%9B%B7%E5%B3%B0%E5%A1%94&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A101979873%7D)的区别。。。

不得不说，比起 mac os x， mac os 和蟑螂长得更像一些。。。

[编辑于 2016-05-22 23:55](https://www.zhihu.com/question/46542168/answer/101979873)